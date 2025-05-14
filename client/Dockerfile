# # Use an official Node runtime as a parent image
# FROM node:14-slim AS build


# # Set the working directory in the container
# WORKDIR /usr/src/app

# # Copy the package.json and package-lock.json files to the container
# COPY package*.json ./

# # Install application dependencies
# # RUN npm install
# RUN npm install --legacy-peer-deps

# # Copy the rest of the application code to the container
# COPY . .

# FROM alpine:3.16.7


# WORKDIR /app

# RUN apk update && apk add npm

# COPY --from=build /usr/src/app /app

# # Expose the port the app runs on
# EXPOSE 3000

# # Define the command to run your app
# CMD ["npm", "start"]



# First stage: Build
FROM node:14-alpine AS build
WORKDIR /usr/src/app

# Copy dependencies files first for efficient caching
COPY package*.json ./
RUN npm install --legacy-peer-deps

# Copy the rest of the application code
COPY . .
RUN npm run build

# Second stage: Runtime
FROM node:14-alpine AS runtime
WORKDIR /app

# Copy built files from the first stage
COPY --from=build /usr/src/app /app

# Ensure the app binds to all network interfaces inside the container
ENV HOST=0.0.0.0

# Expose port for serving the app
EXPOSE 3000

# Use serve to run the built React frontend
CMD ["npx", "serve", "-s", "build"]


