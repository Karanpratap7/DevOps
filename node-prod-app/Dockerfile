# ---------- Stage 1: Builder ----------
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# ---------- Stage 2: Production ----------
FROM node:18-alpine

RUN addgroup -S nodejs && adduser -S nodejs -G nodejs

WORKDIR /app

COPY --from=builder /app/node_modules ./node_modules
COPY . .

USER nodejs

EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s \
  CMD node healthcheck.js

ENV NODE_ENV=production

CMD ["node", "server.js"]