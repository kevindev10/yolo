# # Use an official Node runtime as a parent image
# FROM node:14 AS build


# # Set the working directory in the container
# WORKDIR /usr/src/app

# # Copy the package.json and package-lock.json files to the container
# COPY package*.json ./

# # Install application dependencies
# RUN npm install

# # Copy the rest of the application code to the container
# COPY . .

# #Add multi-stage build
# FROM alpine:3.16.7


# WORKDIR /app

# RUN apk update && apk add --update nodejs

# COPY --from=build /usr/src/app /app

# # Expose the port the app runs on
# EXPOSE 5000

# # Define the command to run your app
# CMD ["node", "server.js"]






#The `node:current-alpine3.17` Docker image is a lightweight Node.js image based on Alpine Linux 3.17. It's designed for environments where minimal size and efficiency are key. 
#This image is particularly useful for deploying Node.js applications in containerized setups, as it combines the latest Node.js version with the compact Alpine Linux base.

FROM node:current-alpine3.17


# Set the working directory in the container

WORKDIR /usr/src/app


# Copy the package.json and package-lock.json files to the container

COPY package*.json ./


# Install application dependencies

RUN npm install



# Copy the rest of the application code to the container

COPY . .



# Expose the port the app runs on

EXPOSE 5000



# Define the command to run your app

CMD ["node", "server.js"]