# Stage 1: Build the Vue.js application
FROM node:18-alpine as build-stage

# Set working directory
WORKDIR /app

# Install Yarn
RUN npm install -g yarn --force

# Copy package.json and yarn.lock files
COPY package*.json ./

# Install dependencies using Yarn
RUN yarn install

# Copy project files and build the project
COPY . .
RUN yarn build

# Stage 2: Serve the static files using Apache
FROM httpd:alpine

# Copy the build files from the build stage
COPY --from=build-stage /app/dist /usr/local/apache2/htdocs/

# Expose port 80
EXPOSE 80
