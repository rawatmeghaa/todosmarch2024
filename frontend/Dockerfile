FROM node:18 AS builder

RUN groupadd -r appgroup && useradd -r -g appgroup -m appuser

WORKDIR /app

COPY package*.json ./
RUN chown -R appuser:appgroup /app

USER appuser

RUN npm install

# Copy all source files to builder stage master 
COPY . .

FROM node:18-slim

# Create the same user in final stage
RUN groupadd -r appgroup && useradd -r -g appgroup -m appuser

WORKDIR /app

# Copy node_modules from builder
COPY --from=builder /app/node_modules ./node_modules

# Copy package.json
COPY --from=builder /app/package*.json ./

# Copy all source files including public directory
COPY . .

# Change ownership to appuser
RUN chown -R appuser:appgroup /app

# Switch to non-root user as appuser master 
USER appuser

EXPOSE 3000

CMD ["npm", "start"]