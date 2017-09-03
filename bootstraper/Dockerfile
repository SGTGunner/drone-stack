FROM ubuntu:16.04
RUN apt-get update \
    && apt-get install software-properties-common -y \
    && add-apt-repository ppa:certbot/certbot -y \
    && apt-get update \
    && apt-get install python-certbot-nginx nginx -y

# This script does the following:
#  - Customizes default sites-enabled config to work correctly with certbot
#  - Runs certbot (which will require interactive mode to answer prompts)
#  - Symlinks certs to make them easier to find by other containers
#  - Generates a strong dh parameter for strong SSL
CMD sed -i "s/\DOCKERFILE_WILL_REPLACE_WITH_DRONE_HOST/$DRONE_HOST/" /etc/nginx/sites-enabled/default \
    && nginx \
    && certbot --nginx -d $DRONE_HOST \
    && ln -s /etc/letsencrypt/live/$DRONE_HOST /etc/letsencrypt/live/default \
    && sudo openssl dhparam -out /etc/drone-stack/certs/dhparam.pem 2048
