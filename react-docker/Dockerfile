#set the base image to create the image for the react app
FROM node:22-alpine

# create new user only the permission to run the app
# -S => create system user
# -G => add user to a group - To avoid running the app as root that has access to everything.
# if the app is run as root, any vulnarability can in the app can be exploited to gain aceess to the host system.
# best practice to run the app as a non-root user
RUN addgroup app && adduser -S -G app app

# setting the user to run the app 
USER app

# setting workdir to /app
WORKDIR /app

# copy package.json and the package-lock.json to the working dir
# this is done to take advantage of the docker's cache
# if the package.json & package-lock.json hasn't changed, docker will utilize the cached dependencies
COPY package*.json ./

# sometimes the ownership of the files in workdir are changed to root
# when that happens the app can't access the files and throws and error -> EACCESS: permission denied
# to avoid this, change the ownership of the files to the root user
USER root

# change ownership of the /app dir to the app user
# chown -R <user>:<group> <dir>
# chown cmd changes the user and/or the group ownership of the file
RUN chown -R app:app .

# change the user to run the app again
USER app

# install the json dependencies 
RUN npm install

# copy the rest of the workdir
COPY . .

# expose port 5173 to tell Docker that the container listens on the specified network ports at runtime
EXPOSE 5173

# cmd to run the app
CMD npm run dev