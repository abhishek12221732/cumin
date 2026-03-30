# ==========================================
# Stage 1: Build the React/Vite Application
# ==========================================
# Using a standard node image for the build environment
FROM node:18-alpine AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json first to leverage Docker cache
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the frontend source code
COPY . .

# Force the variable inline with the build command
RUN VITE_URL_BASE_URL="http://localhost:5000" npm run build

# Build the Vite project for production
# This generates the static HTML/CSS/JS files in the /dist folder
RUN npm run build

# ==========================================
# Stage 2: Serve with Nginx
# ==========================================
# Use a tiny Nginx image for the production server
FROM nginx:alpine

# Copy the built artifacts from the 'builder' stage into Nginx's serving directory
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 80 for internal cluster routing
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]