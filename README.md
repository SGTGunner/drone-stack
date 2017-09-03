# drone-stack

A docker-compose stack for drone.io with SSL proxy and automated certificate management 

### drone.io

The basic drone.io configuration is based on their documentation samples. See their docs for more information. 

### letsencrypt SSL certificates

This repo includes a docker script simplify letsencrypt SSL certificate generation. A service that runs periodically to renew the certificate is also included as part of the main docker-compose deployment.

### nginx

The nginx service is configured as a proxy that uses the SSL certificates obtained from letsencrypt.

# Setup

1. Configure a hostname on your DNS that points to the public IP of the server that will host this stack.

2. Configure `config/settings` and `config/secrets`.  Documentation is provided within those files. 

3. Once your DNS has updated to resolve to your host's IP address, you can now generate a new SSL certificate by running the bootstrapper interactive script.

    ```
    ./bootstrapper/create-initial-cert
    ``` 

    All prompts should be straightforward. The last prompt regarding updating the nginx config is irrelevant because the bootstrapping container is isolated from the main proxy server.

    ```
    # TIP: You can choose 1 for this prompt. (It does not matter, as explained above.)
    
    Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
    -------------------------------------------------------------------------------
    1: No redirect - Make no further changes to the webserver configuration.
    2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
    new sites, or if you're confident your site works on HTTPS. You can undo this
    change by editing your web server's configuration.
    -------------------------------------------------------------------------------
    Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 1
    ```

4. Ensure that your config is loaded on startup by adding this to your `/etc/bash.rc`. (This is not an ideal approach, but suggestions to improve it are welcome.)  

    ```
    # Add to bottom of /etc/bash.bashrc replacing [PATH_TO_REPO_ROOT]
    
    source [PATH_TO_REPO_ROOT]/config/settings
    source [PATH_TO_REPO_ROOT]/config/secrets
    ```

5. Start the docker-compose stack by running 

    ```
    ./start
    ```

5. The app should be accessible by simply visiting https://[YOUR_HOST_NAME]
