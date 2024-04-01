## DO188 Practice Tasks

---

### Task 1

Install podman and podman-compose

---

## Task 2

As root, deploy a local registry with the following parameters

- Container 1:
    - Use image docker.io/registry:2
    - Name the container as registry
    - Run the container in detached mode
    - Create directory /srv/registry/data for container and mount it as /var/lib/registry
    - Bind container port 5000 to local port 5000
    - Test functionality downloading the alpine image (tag different than latest), tag it as &lt;host_fqdn&gt;:5000/alpine:local and upload it to local registry
    - If required update configuration in /etc/containers/registries.conf

- Container 2:
    - Use image docker.io/konradkleine/docker-registry-frontend:v2
    - Name the container as frontend
    - Run the container in detached mode
    - Set following environment variables
        - ENV_DOCKER_REGISTRY_HOST: &lt;host_fqdn&gt;
        - ENV_DOCKER_REGISTRY_PORT: 5000
    - Bind container port 80 to local port 8080
    - Test functionality browsing to local port 8080

---

### Task 3

Create a rootless container with the following requirements

- Use image docker.io/nginx with a tag different than latest
- Name the container as mynginx
- Run the container in detached mode
- Bind container port 80 to local port 8081
- Create directory /web inside container
- Copy index.html file from /tasks/nginx to /web in the container
- Copy default.conf file from /tasks/nginx to /etc/nginx/conf.d/default.conf in container
- Restart nginx in the container with command nginx -s reload
- Verify app is working, browse to http://localhost:8081

---

### Task 4

Create a rootless container with the following requirements

- Use the docker.io/tomcat image with tag 7-jre7-alpine
- Name the container as tomcat-app
- Run the container in detached mode
- Inspect the image and locate the CATALINA_HOME variable
- Create a tomcat directory, this will be mounted as $CATALINA_HOME/webapps
- Download the java app from following url:
    - https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war
- Copy the app to the tomcat directory
- Bind container port 8080 to local port 8082
- Verify app is working, browse to http://localhost:8082/sample/

- Extra:
    - Make the container start as a user service called tomcat-app.service

---

### Task 5

Create a container image with the following requirements

- Use image docker.io/fedora with a  tag different than latest
- Name the Containerfile as Containerfile-fortune
- Use an ARG to create a user during build, “fortune” must be the default user
- Install the fortune-mod, cowsay, and lolcat packages
- Copy the script fortune.sh from /tasks/fortune to /usr/local/bin in the container
- Make it executable
- Change its ownership to the created user
- Use the script as the entrypoint for the container, it should be run by the created user
- Tag the container as fortune:1.0
- Verify image is working running a rootless container with the -it and --rm flags

---

### Task 6

Create a container image with the following requirements:

- Use image docker.io/mysql with a tag different than latest
- Name the Containerfile as Containerfile-world
- Download sample sql file from following url:
    - https://downloads.mysql.com/docs/world-db.tar.gz
- Uncompress the tar.gz file
- Copy the world-db/world.sql file to /docker-entrypoint-initdb.d directory in the container
- Set the following environment variables
    - MYSQL_ROOT_PASSWORD: world.Pw0rd
    - MYSQL_USER: traveler
    - MYSQL_PASSWORD: world.Pw0rd
    - MYSQL_DATABASE: world
- Name the image as &lt;host_fqdn&gt;:5000/mysql:world
- Push the image to local registry 

- Extra:
    - Remove the local image, create a test container and verify the database world is present

---

### Task 7

Create a rootless container image with the following requirements

- Copy the /tasks/node directory to $HOME
- Edit the Containerfile and complete the required commands
- Build the image with tag mynodeapp:1.0
- Verify creating a container from this image
    - Name the container as nodeapp1
    - Run the container in detached mode
    - Bind container port 8080 to local port 8083
    - Check container logs
    - Verify app is working, browse to http://localhost:8083
    - Export container to tar file nodeapp1.tar

- Extra:
    - Modify app.js, change port to 8084
    - Modify Containerfile, change port to 8084
    - Create new image with tag mynodeapp:2.0
    - Verify creating a rootless container from this image
        - Name the container as nodeapp2
        - Run the container in detached mode
        - Bind container port 8084 to local port 8084
        - Check container logs
        - Verify app is working, browse to http://localhost:8084
        - Save image to tar file mynodeapp-2.0.tar

---

### Task 8

Build the following rootless containers, use a tag different than latest

- Create network: proxy
- Copy directory /tasks/proxy to $HOME

- Container 1:
    - Use image docker.io/nginx
    - Name the container as websvr1
    - Connect to network proxy
    - Run the container in detached mode
    - Mount the directory $HOME/proxy/websvr1 as /usr/share/nginx/html

- Container 2:
    - Use image docker.io/nginx
    - Name the container as websvr2
    - Connect to network proxy
    - Run the container in detached mode
    - Mount the directory $HOME/proxy/webvr2 as /usr/share/nginx/html

- Container 3:
    - Use image docker.io/haproxy
    - Name the container as lb
    - Connect to network proxy
    - Bind container port 8080 to local port 8085
    - Run the container in detached mode
    - Mount the directory $HOME/proxy/haproxy as /usr/local/etc/haproxy

- Verify containers are working, browse to localhost:8085 a couple times

---

### Task 9

Build the following rootless containers, use a tag different than latest

- Create network: wordpress

- Container 1:
    - Use image registry.access.redhat.com/rhscl/mysql-57-rhel7 for the DB
    - Name the container as wpdb
    - Mount volume ~/wordpress/db for /var/lib/mysql/data
    - Connect to network wordpress
    - Set the following environment variables
        - MYSQL_ROOT_PASSWORD: WP.pw0rd
        - MYSQL_USER: wpuser
        - MYSQL_PASSWORD: Test.pw0rd
        - MYSQL_DATABASE: wpdatabase

- Container 2:
    - Use image docker.io/library/wordpress for the APP
    - Name the container as wpapp
    - Mount volume ~/wordpress/app for /var/www/html
    - Bind container port 80 to local port 8086
    - Connect to network wordpress
    - Set the following environment variables
        - WORDPRESS_DB_HOST: wpdb
        - WORDPRESS_DB_NAME: wpdatabase
        - WORDPRESS_DB_USER: wpuser
        - WORDPRESS_DB_PASSWORD: Test.pw0rd

- Container 3:
    - Use image docker.io/bitnami/phpmyadmin
    - Name the container as wpmya
    - Bind container port 8080 to local port 8087
    - Connect to network wordpress
    - Set the following environment variables
        - DATABASE_HOST: wpdb
        - DATABASE_USER: wpuser
        - DATABASE_PASSWORD: Test.pw0rd

- Extra 1: 
    - Remove the containers, run them again as part of a pod named wp-pod

- Extra 2: 
    - Save pod as a kubernetes yaml file (kube), remove pod and containers, run them again using the yaml file.

---

### Task 10

Create the following rootless containers

- Create network: gitea

- Create secret:
    - gitea-db-user-pwd with value: Gitea.pw0rd

- Container 1:
    - Use image docker.io/postgres:alpine
    - Name the container as gitea-db
    - Mount local directory ~/gitea/postgres to /var/lib/postgresql/data
    - Connect to network gitea
    - Set the following environment variables
        - POSTGRES_USER: gitea
        - POSTGRES_PASSWORD_FILE: &lt;use gitea-db-user-pwd secret&gt;
        - POSTGRES_DB: gitea

- Container 2:
    - Use image docker.io/gitea/gitea:latest-rootless
    - Name the container as gitea-web
    - Mount local directory ~/gitea/data for /var/lib/gitea
    - Mount local directory ~/gitea/config for /etc/gitea
    - Mount file /etc/localtime as read only
    - Connect to network gitea
    - Bind container port 3000 to local port 3000 
    - Bind container port 2222 to local port  2222
    - Set the following environment variables
        - RUN_MODE: prod
        - DISABLE_SSH: false
        - DOMAIN: git.example.com
        - SSH_DOMAIN: git.example.com
        - SSH_PORT: 2222
        - GITEA__database__DB_TYPE: postgres
        - GITEA__database__HOST: gitea-db:5432
        - GITEA__database__NAME: gitea
        - GITEA__database__USER: gitea
        - GITEA__database__PASSWD__FILE: &lt;use gitea-db-user-pwd secret&gt;
        - TZ: &lt;use the timezone for your location&gt;

- Extra:
    - Remove the containers, rebuild them using podman-compose (docker-compose.yml), set container dependencies.
