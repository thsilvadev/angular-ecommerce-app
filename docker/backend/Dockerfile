# server/Dockerfile
FROM node:20-alpine

WORKDIR /app

# Install runtime deps needed for builds (if any)
RUN apk add --no-cache openssl curl

# Copy package files and install
COPY backend/package*.json ./
RUN npm ci --only=production --no-audit --no-fund

# Copy source
COPY backend/. .

# If your backend needs a build step (typescript), uncomment:
# RUN npm run build

# Healthcheck (adjust path if different)
HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1

EXPOSE 3000
CMD ["npm", "start"]