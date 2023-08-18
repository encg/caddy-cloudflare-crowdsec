# caddy-cloudflare-crowdsec

[Caddy Docker image](https://hub.docker.com/_/caddy) with the following modules included:
* mholt/caddy-l4
* caddyserver/transform-encoder
* hslatman/caddy-crowdsec-bouncer
* caddy-dns/cloudflare

## Why?
I've been using Docker for years, but have never built an image. Also couldn't find a Caddy image with the aforementioned modules included.

## Usage
#### docker-compose.yml
```
version: "3.8"
services:
  caddy:
    image: encg/caddy-cloudflare-crowdsec
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - $PWD/Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config/config

volumes:
  caddy_data:
    external: true
  caddy_config:
```

#### .env
```
CF_API_TOKEN=cloudflare_api_token
CS_LOCAL_API_KEY=crowdsec_local_api_key
```

#### Caddyfile
```
{
	acme_dns cloudflare {$CF_API_TOKEN}
	crowdsec {
		api_url http://crowdsec:8080
		api_key {$CS_LOCAL_API_KEY}
		ticker_interval 15s
		#disable_streaming
		#enable_hard_fails
	}
}
```

Refer to the respective projects for further documentation:
* [caddy](https://hub.docker.com/_/caddy)
* hslatman/caddy-crowdsec-bouncer/#usage
* caddy-dns/cloudflare