# Let's call this docker image as "tech-magic/node-js"

FROM node:argon
# Inherited from the official docker image for node

MAINTAINER Wimal Perera <wimal.perera@ebuilder.com>

# Create the application directory within the docker image
RUN mkdir -p /usr/src/node-app

# Set the application directory as the work directory
WORKDIR /usr/src/node-app

# Copy the node-app in your folder to node-app in docker image
COPY node-app /usr/src/node-app

# This applies if you're using AWS
ADD credentials /root/.aws/credentials

# Install npm and bower dependencies
RUN npm install --production && \
    npm install -g bower && \
    bower install --allow-root --config.interactive=false

# Expose the desired port
EXPOSE 8080

# Start node js app
CMD ["npm", "start"]
