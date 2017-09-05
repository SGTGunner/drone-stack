# drone-stack

A docker-compose stack for drone.io with SSL proxy and automated certificate management 

### drone.io

The basic drone.io configuration is based on examples from the drone.io documentation. See their docs for more information. 

### letsencrypt SSL certificates

This repo includes a docker script to simplify letsencrypt SSL certificate generation. Another service that runs periodically to renew the certificate is also included as part of the main docker-compose deployment.

### nginx

The nginx service is configured as a proxy that uses the SSL certificates obtained from letsencrypt.

# Setup

1. Configure a hostname on your DNS that points to the public IP of the server that will host this stack.

2. Once your DNS has updated to resolve to your host's IP address, you can now generate a new SSL certificate by running the bootstrapper interactive script.

    ```
    # Example
    ./bootstrapper/create-initial-cert example.com
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

3. Before starting the stack, make sure that any required environment variables are set properly.  See  docker-compose.yaml to see which variables are needed.  Refer to the drone.io documentations for more information.  Customize docker-compose.yaml as needed by adding or removing environment variables.

4. Start the docker-compose stack. 

    ```
    # Example
    docker-compose up --force-recreate -d
    ```

5. The app should be accessible by simply visiting https://[YOUR_HOST_NAME]
