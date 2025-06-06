# ================= Jenkinsfile for Deploying Docker Container with .env.production or .env.development =================
# This Jenkinsfile defines a pipeline to build, stop, remove, and run a Docker container for the xt-bo-uat application.

pipeline {
    agent any

    environment {
        IMAGE_NAME = 'xt-bo-uat'
        TAG = 'uat'
        PORT = '8800'
        INTERNAL_PORT = '3000'
        NETWORK = 'docker-stack_default'
    }

    stages {

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:${TAG} ."
                }
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                script {
                    sh "docker stop ${IMAGE_NAME} || true"
                    sh "docker rm ${IMAGE_NAME} || true"
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    sh """
                        docker run -d \
                        --name ${IMAGE_NAME} \
                        --network ${NETWORK} \
                        -p ${PORT}:${INTERNAL_PORT} \
                        -e TZ=Asia/Phnom_Penh \
                        -v /etc/timezone:/etc/timezone:ro \
                        -v /etc/localtime:/etc/localtime:ro \
                        ${IMAGE_NAME}:${TAG}
                    """
                }
            }
        }
    }
}



#======================== Jenkinsfile for Deploying Docker Container with Environment Variables ========================


pipeline {
    agent any

    environment {
        IMAGE_NAME = 'xt-bo-staging'
        TAG = 'staging'
        PORT = '9900'
        INTERNAL_PORT = '3000'
        NETWORK = 'docker-stack_default'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        docker build \
                          --build-arg NEXT_PUBLIC_SITE_NAME=${NEXT_PUBLIC_SITE_NAME} \
                          --build-arg NEXT_PUBLIC_BE_API_URL=${NEXT_PUBLIC_BE_API_URL} \
                          --build-arg NEXTAUTH_SECRET=${NEXTAUTH_SECRET} \
                          --build-arg NEXTAUTH_URL=${NEXTAUTH_URL} \
                          -t ${IMAGE_NAME}:${TAG} .
                    """
                }
            }
        }

        stage('Stop & Remove Old Container') {
            steps {
                script {
                    sh "docker stop ${IMAGE_NAME} || true"
                    sh "docker rm ${IMAGE_NAME} || true"
                }
            }
        }

        stage('Run New Container') {
            steps {
                script {
                    sh """
                        docker run -d \
                          --name ${IMAGE_NAME} \
                          --network ${NETWORK} \
                          -p ${PORT}:${INTERNAL_PORT} \
                          -e NEXT_PUBLIC_SITE_NAME=${NEXT_PUBLIC_SITE_NAME} \
                          -e NEXT_PUBLIC_BE_API_URL=${NEXT_PUBLIC_BE_API_URL} \
                          -e NEXTAUTH_SECRET=${NEXTAUTH_SECRET} \
                          -e NEXTAUTH_URL=${NEXTAUTH_URL} \
                          ${IMAGE_NAME}:${TAG}
                    """
                }
            }
        }
    }
}
