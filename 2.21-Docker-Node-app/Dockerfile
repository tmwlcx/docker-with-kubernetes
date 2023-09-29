
# Build
#   Select the node image from local cache or docker hub
FROM node

#   set the working directory to the /app folder
#   COPY and RUN commands will execute relative to this folder
WORKDIR /app

# copy the package.json file first since this won't change
COPY package.json /app

#   run the `npm install` command in the working directory folder
RUN npm install

#   copy the files in the local (.) folder to the /app folder in the docker image
COPY . /app

# expose the port we want to our local system (in this case, 80) -- this is a best practice
# but doesn't actually do anything!! you do this when you run the container!
EXPOSE 80

# Start server
CMD ["node", "server.js"]