Docker image for building container of revealJS slides.

It includes :
- Socket.IO : Broadcast your slideshow

Docker HUB link : https://hub.docker.com/r/dkrpigeyre/revealjs-broadcast

# How to use it?

## Requirements

Get and install docket-toolbox : https://www.docker.com/products/docker-toolbox

## Build your slideshow

1. Download the subproject revealjs-workspace to get a project template

2. Add slides (.html or .md) in ./slides folder

3. Add images in ./images folder

4. Configure docker-compose.yml to set

- ports
  
  ```yml
  ports:
  # For Slide share
  - "**8080**:8080"
  # For Package download
  - "**8081**:8081"
  # ...
  ports:
  - "1948:1948" 
  ```
  
  - Volumes shared
  
  ```yml
  volumes:
  - **/c/Users/revealjs-builder/slides*:/project-builder/resources/slides
  - **/c/Users/revealjs-builder/images**:/project-builder/assets/site/images
  ```
  
  > For Windows Users configure your shared volumes starting /c/Users/revealjs-builder (/c/Users is the only folder which can be shared) (symlink doesn't work too)
  
  > For Linux/Unix Users path can be relative
  
  - Docker Host IP in order to connect your browser script to container backend for socketio broadcasting
  
  ```yml
  environment:
  # To configure for broadcasting : server host for containers
  - DOCKER_HOST_IP=192.168.99.100
  ```
  
5. Create and run the new container from the command line in the workspace folder
  
  ```yml
  docker-compose up
  ```
  
6. Open your browser to  http://<your_docker_host>:<your_port_mapping_for_8080_in_configuration_file>/. Adapt your slides, the browser will be automatically updated.

## Share your slideshow

1. Get built package from http://<your_docker_host>:<your_port_mapping_for_8081_in_configuration_file>/package.zip

2. Extract it and set your settings by editing docker-compose.yml :
  
  - Ports
  
  ```yml
  ports:
  - "**80**:80"   
  # ...
  ports:
  - "**1948**:1948" 
  ```
  
  - Environment variables
  
  ```yml
  environment:
  - MASTER_PASSWORD=yourmasterpassword
  # Must match with your socket io port exposed
  - DOCKER_HOST_PORT_SOCKETIO=1948
  ```
  
  > MASTER_PASSWORD is your password for master acces to slides
  
  - You can configure docker to use an existing socketio container by commenting socketio service and replacing existing link with external_links
  
3. Build and run containers by running from your command line (inside your extracted folder)
  
  ```yml
  docker-compose up
  ```
  
4. Access to your master presentation with http://<your_docker_host>:<your_port_mapping_for_80_in_configuration_file>/master.html
  
  > User is 'master', Password had been set in yout docker-compose.yml
  
  Client access http://<your_docker_host>:<your_port_mapping_for_80_in_configuration_file>/client.html is public
  
# How to sort slides

Slide order in ./slides folder controls slide order in the slideshow. It is an alphabetical order.

You can prefix your filename with numeric values (ex: "001 - ") to organize it by yourself.

# Slide Syntax

## HTML Syntax

Refer to : https://github.com/hakimel/reveal.js/tree/3.3.0#markup

## Markdown Syntax

Refer to : https://github.com/hakimel/reveal.js/tree/3.3.0#markdown

This is the configuration set for mardown separator :

- Slide separator : ---
- Slide vertical separator : --
- Slide note : Note: 