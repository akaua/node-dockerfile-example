FROM node:8-alpine
MAINTAINER Softplan - Akaua da Costa
ARG PORT=3000

# For handling Karnel signal properly.
RUN apk add --no-cache tini

# Non previlage mode for better security (this user comes with official NodeJS image).
USER node

# Create necessary directories.
RUN mkdir -p /home/node/app
WORKDIR /home/node/app

# Copy necessary files
COPY --chown=node:node ["package*.json","*.lock","server.js", "./"]

# Install dependencies
RUN npm install --silent && npm cache clean --force

EXPOSE $PORT
ENTRYPOINT ["/sbin/tini", "--"]
CMD ["node", "server.js"]
