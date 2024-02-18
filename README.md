
# Self Signed Certified Nginx on Docker

The below project helps us setting up a nginx with the self signed certificate on docker.



## Steps to Perform
Make sure open ssl is installed. Generate Self signed certificate

```bash
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout nginx-selfsigned.key -out nginx-selfsigned.crt
```


Create a Dockerfile in your project directory to build the NGINX Docker image

```bash
    FROM nginx:latest

    COPY nginx-selfsigned.crt /etc/nginx/ssl/nginx-selfsigned.crt
    COPY nginx-selfsigned.key /etc/nginx/ssl/nginx-selfsigned.key

    COPY nginx.conf /etc/nginx/nginx.conf
```

Create an nginx.conf file in the same directory as your Dockerfile

```bash
    events {
        worker_connections 1024;
    }

    http {
        server {
            listen 443 ssl;
            server_name localhost;

            ssl_certificate /etc/nginx/ssl/nginx-selfsigned.crt;
            ssl_certificate_key /etc/nginx/ssl/nginx-selfsigned.key;

            location / {
                root /usr/share/nginx/html;
                index index.html;
            }
        }
    }
```
## Building and Running the Docker Container

Build the Docker image

```bash
  docker build -t nginx-selfsigned .
```

Run the Docker container

```bash
  docker run -p 443:443 -d nginx-selfsigned
```


## Running the localhost

When you go to localhost, your browser might show a security warning. You can proceed to the site, but keep in mind that this certificate won't be trusted by default.

```bash
  https://localhost
```

