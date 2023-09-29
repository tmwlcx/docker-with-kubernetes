
# Build
#   Select the node image from local cache or docker hub
FROM node

#   set the working directory to the /app folder
#   COPY and RUN commands will execute relative to this folder
WORKDIR /app

#   copy the files in the local . folder to the /app folder in the docker image
COPY . /app

#   run the `npm install` command in the working directory folder
RUN npm install

# expose the port we want to our local system (in this case, 80)
EXPOSE 80

# Start server
CMD ["node", "server.js"]