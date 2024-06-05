# Use an official Node.js runtime as a parent image
FROM node:14

# Set the working directory
WORKDIR /200970_frontend

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application
COPY . .

# Build the app for production with minification
RUN npm run build

# Use a minimal web server to serve the static files
FROM nginx:alpine
COPY --from=0 /200970_frontend/build /usr/share/nginx/html

# Expose the port the app runs on
EXPOSE 200970

# No need to specify CMD as nginx will run by default
