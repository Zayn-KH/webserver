Using Jenkins pipeline script to automate Docker container deployment and management.
# Connection internal with docker-compose and Dockerfile
In the build stage as mention below
```
-f /opt/ci/nextjs-deploy/Dockerfile.stage-production \
```

We must create or move # Dockerfile.stage-production and # .env.production as required in the path /opt/ci/nextjs-deploy/ with the content:
```
# Step 1: Build
FROM node:18-alpine as builder
WORKDIR /app

# Use package-lock.json instead of yarn.lock
COPY package.json package-lock.json ./
RUN npm ci
COPY . .

# Accept build arguments
ARG NEXT_PUBLIC_SITE_NAME
ARG NEXT_PUBLIC_BE_API_URL
ARG NEXT_PUBLIC_DOMAIN_URL
ARG NEXTAUTH_SECRET
ARG NEXTAUTH_URL
ARG NEXT_PUBLIC_CUSTOMER_SHEETS_ID
ARG BASE64_SA_JSON
ARG NEXT_PUBLIC_WS_URL
ARG NEXT_PUBLIC_AUTO_TRANSFER_DURATION
ARG NEXT_PUBLIC_REFRESH_TIME
ARG NEXT_PUBLIC_FORMAT_DATE_TIME

# Export them as environment variables for build
ENV NEXT_PUBLIC_SITE_NAME=$NEXT_PUBLIC_SITE_NAME
ENV NEXT_PUBLIC_BE_API_URL=$NEXT_PUBLIC_BE_API_URL
ENV NEXT_PUBLIC_DOMAIN_URL=$NEXT_PUBLIC_DOMAIN_URL
ENV NEXTAUTH_SECRET=$NEXTAUTH_SECRET
ENV NEXTAUTH_URL=$NEXTAUTH_URL
ENV NEXT_PUBLIC_CUSTOMER_SHEETS_ID=$NEXT_PUBLIC_CUSTOMER_SHEETS_ID
ENV BASE64_SA_JSON=$BASE64_SA_JSON
ENV NEXT_PUBLIC_WS_URL=$NEXT_PUBLIC_WS_URL
ENV NEXT_PUBLIC_AUTO_TRANSFER_DURATION=$NEXT_PUBLIC_AUTO_TRANSFER_DURATION
ENV NEXT_PUBLIC_REFRESH_TIME=$NEXT_PUBLIC_REFRESH_TIME
ENV NEXT_PUBLIC_FORMAT_DATE_TIME=$NEXT_PUBLIC_FORMAT_DATE_TIME

RUN npm run build

# Step 2: Runtime
FROM node:18-alpine as runner
WORKDIR /app

COPY --from=builder /app/package.json ./
COPY --from=builder /app/package-lock.json ./
COPY --from=builder /app/next.config.js ./
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

EXPOSE 3000
```

