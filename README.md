# rancher-sandbox
Rancher - A Complete Platform for Running Containers<br/>
This is a sample for running a wordpress. <a href="http://docs.rancher.com/rancher/v1.2/en/quick-start-guide/">Refer to this article</a>

### Start Rancher Server
```
$ docker run -d --restart=unless-stopped -p 8080:8080 rancher/server
# Tail the logs to show Rancher
$ docker logs -f containerid
# To access UI, open browser using below url
http://<ip-address>:8080
```
### Add Host
```
#For development environment, we will add the same host running the Rancher server as a host in Rancher.

```
### Create a Multi-Container Application using Rancher Compose
docker-compose.yml
```
mywordpress:
  tty: true
  image: wordpress
  links:
  - database:mysql
  stdin_open: true
wordpresslb:
  ports:
  - 80:80
  tty: true
  image: rancher/load-balancer-service
  links:
  - mywordpress:mywordpress
  stdin_open: true
database:
  environment:
    MYSQL_ROOT_PASSWORD: pass1
  tty: true
  image: mysql
  stdin_open: true
```
rancher-compose.yml
```
mywordpress:
  scale: 2
wordpresslb:
  scale: 1
  load_balancer_config:
    haproxy_config: {}
  health_check:
    port: 42
    interval: 2000
    unhealthy_threshold: 3
    healthy_threshold: 2
    response_timeout: 2000
database:
  scale: 1
```
###  Set up the environment variables needed for Rancher Compose
```
# Set the url that Rancher is on
# export RANCHER_URL=http://192.168.99.100:8080/
$ export RANCHER_URL=http://server_ip:8080/
# Set the access key, i.e. username
# export RANCHER_ACCESS_KEY=F8277CFC6872A08A2FD4
$ export RANCHER_ACCESS_KEY=<username_of_key>
# Set the secret key, i.e. password
# export RANCHER_SECRET_KEY=WsHxTJXAY4Twiebm61JbioRbKp74iaJKtFW53Nbp
$ export RANCHER_SECRET_KEY=<password_of_key>
```
### Run Rancher Compose
```
cd rancher-sandbox
$ rancher-compose -p NewWordpress up
```
