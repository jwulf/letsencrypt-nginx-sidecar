# letsencrypt-nginx-sidecar

Run letsencrypt and nginx in a docker-compose side car, providing automatic certificate renewal and SSL for your web apps.

This is based on https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion, but with the nginx + letsencrypt containers decoupled from your webapp containers.

This is my workaround for a letsencrypt certificate renewal exhaustion issue that some users encounter: [#374](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion/issues/374)

## Use

- Create the `letsencrypt` docker network:

```
docker network create letsencrypt
```
- Now start the sidecar with `cd sidecar && docker-compose up -d`. This starts an nginx reverse proxy with a Lets Encrypt sidecar that automatically provisions certificates for application servers that join the `letsencrypt` docker network.

- Use the [docker-compose.yml](docker-compose.yml) file as your template for your webapps. Set the `VIRTUAL_PORT` to the port that your container listens on. The nginx proxy will listen on the host machine's ports 80 and 443, and proxy traffic based on the `VIRTUAL_HOST` you set, using LetsEncrypt certificates that it will get.

- Start your webapp with `docker-compose up -d`

Now you can bring your webapp up and down as much as you need to, to update it, and have no issue with certificate renewal exhaustion, as the letsencrypt container stays up all the time.
