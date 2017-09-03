# drone-stack

A docker-compose stack for drone.io with SSL proxy and automated certificate management 

### drone.io

The drone.io configuration used here is based off of their documentation samples. See their docs for more information. 

### letsencrypt SSL certificates

This repo includes a docker container to help create letsencrypt SSL certificates as well as a container that runs periodically to renew them.

### nginx

The nginx configuration will use the SSL certificates obtained from letsencrypt, and then function as a proxy in front of the drone.io app.

# Setup

1. Configure a hostname on your DNS that points to the public IP of the server that will host this stack.

2. Configure `config/settings` and `config/secrets`

3. Once your DNS has updated, generate a new SSL certificate by running the bootstrapper interactive script.

```
./bootstrapper/create-initial-cert
``` 

4. Ensure that your config is loaded on startup by adding this to your `/etc/bash.rc`. (This is not an ideal approach, but suggestions to improve it are welcome.)  

```
# Add to bottom of /etc/bash.bashrc replacing [PATH_TO_REPO_ROOT]

source [PATH_TO_REPO_ROOT]/config/secrets
source [PATH_TO_REPO_ROOT]/config/settings
```

5. Start the docker-compose stack by running 

```
./start
```

5. The app should be accessible by simply visiting https://[YOUR_HOST_NAME]
