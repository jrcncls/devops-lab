# --- Stage 1: Build the static files using Node.js ---
FROM node:20-alpine AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy package files first to leverage Docker caching
COPY package*.json ./

# Install project dependencies
RUN npm ci

# Copy the rest of your website application code
COPY . .

# Generate the static production build (creates a /dist or /build folder)
RUN npm run build


# --- Stage 2: Serve the static files using Nginx ---
FROM nginx:alpine

# Clean out any default static files from Nginx
RUN rm -rf /usr/share/nginx/html/*

# Copy the static assets from the builder stage into the Nginx container
# Adjust "dist" to "build" if your framework outputs a different folder
COPY --from=builder /app/dist /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]