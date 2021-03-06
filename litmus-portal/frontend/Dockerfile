# Docker Image which is used as foundation to create
# a custom Docker Image with this Dockerfile
FROM node:12.18.0 as react-build

# Add Maintainer Info
LABEL maintainer="LitmusChaos"

# A directory within the virtualized Docker environment
# Becomes more relevant when using Docker Compose later
WORKDIR /frontend

# Copies package.json and package-lock.json to Docker environment
COPY package.json ./
COPY package-lock.json ./

# Installs all node packages except Cypress
RUN CYPRESS_INSTALL_BINARY=0 npm install

# Copies everything over to Docker environment
COPY . ./

# Finally runs the application
RUN npm run build

# Stage 2: the production environment
FROM nginx:alpine
RUN rm /etc/nginx/conf.d/default.conf
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf
COPY  --from=react-build /frontend/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]