# letsencrypt-nginx-sidecar

Run letsencrypt and nginx in a docker-compose side car, providing automatic certificate renewal and SSL for your web app.

This is based on https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion, but with the nginx + letsencrypt containers decoupled from your webapp containers.

This is my workaround for a letsencrypt certificate renewal exhaustion issue that some users encounter: [#374](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion/issues/374)

## Use

- Configure your webapp container(s) in [docker-compose.yml](docker-compose.yml). Make sure the webapp listens on port 80. The sidecar will listen on the container host port 443 and proxy traffic using a LetsEncrypt certificate that it will get.
- Start your webapp with `docker-compose up -d`
- Now start the sidecar with `cd sidecar && docker-compose up -d`

Now you can bring your webapp up and down as much as you need to, to update it, and have no issue with certificate renewal exhaustion, as the letsencrypt container stays up all the time.
