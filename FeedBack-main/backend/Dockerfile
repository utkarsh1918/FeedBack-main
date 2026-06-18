FROM node:18-alpine
WORKDIR /app

# Install dependencies including dev dependencies for Prisma
COPY package*.json ./
RUN npm ci

# Copy source code
COPY . .

# Generate Prisma client
RUN npx prisma generate

# Remove dev dependencies to reduce image size
RUN npm prune --production

# Expose correct port
EXPOSE 5000

# Health check for production
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node -e "require('http').get('http://localhost:5000/api/health', (res) => { process.exit(res.statusCode === 200 ? 0 : 1) })"

# Start production server
CMD ["node", "src/app.js"]
