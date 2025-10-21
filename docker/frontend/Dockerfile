# client/Dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app

# Install build deps
RUN apk add --no-cache git python3 make g++ && npm config set fund false

# Copy package files first for caching
COPY client/package*.json ./
RUN npm ci --no-audit --no-fund

# Copy source and build
COPY client/. .
# Ajuste se o comando do projeto for diferente (ex: ng build --prod)
RUN npx ngcc --yes || true
RUN npm run build -- --configuration production

# Production stage: nginx serving static files
FROM nginx:1.25-alpine
COPY --from=builder /app/dist/browser /usr/share/nginx/html
# Optional: custom nginx config (uncomment if you add one)
# COPY docker/nginx/default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
STOPSIGNAL SIGTERM

# keep default nginx entrypoint
