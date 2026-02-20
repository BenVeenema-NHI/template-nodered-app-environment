# Select the base image, specify a specific version of the image before going to production!
FROM nodered/node-red:latest
# FROM nodered/node-red:4.0.2

### Install linux packages
USER root
    # Update the package list
    RUN apk update
    # Install poppler-utils: PDF manipulation utilities
    RUN apk add poppler-utils
    # Install zip: allows to zip files 
    RUN apk add zip
USER node-red


# Install pallette dependencies
RUN npm install @flowfuse/node-red-dashboard
RUN npm install node-red-contrib-mssql-plus
RUN npm install @nhimm/node-red-contrib-data-forge
RUN npm install node-red-contrib-filesystem
RUN npm install node-red-contrib-postgresql
RUN npm install node-red-contrib-spreadsheet-in