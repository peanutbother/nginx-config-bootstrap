# NGINX Example Configuration Bootstrap

## What's in it?

- PHP 7 FPM configuration (mimetypes, file handling)
- example.com domain configuration
- modular snippets for [easy domain configuration](#snippets)
- letsencrypt auto-renewal [configuration](#letsencrypt)
- nodejs / other service loopback
- domain-folder / subdomain-file [file structure](#file-structure)

## What's required?

- NGINX
- Letsencrypt / certbot

### Optional

- PHP 7 FPM

You can use any version of PHP which is compatible with your NGINX, keep in mind you might need to alter configuration snippets for it to work.

- NodeJS

You can use any version of NodeJS with your NGINX, keep in mind you might need to alter configuration snippets for it to work.

## What's further possible?

### File Structure

Domains are organized by folders of their names.
Each domain folder contains a file called `default` which is responsible for that domain.
Every other file in that directory is named after the subdomain it handles.

```
|_
|  example.com
|    |— default
|    |— subdomain1
|    |— subdomain2
|    |— subdomain3
|_
|  example2.com
|    |— default
|    |— subdomain1
|    |— subdomain2
|    |— subdomain3
|    |— [...]
|_
|   [...]
```

### Snippets

For easy and fast configuration, there are several snippets to get you started.
| File | Snippet |
| ---- | ---- |
| `fastcgi-php.conf` | Fastcgi bootstrap configuration |
| `html.conf` | simple HTML configuration |
| `php.conf` | simple fastcgi / PHP configuration |
| `snakeoil.conf` | development test certificate (self-signed) |
| `spa-prerender.conf`* | SPA configuration for prerendered Apps (uses cache and `Node fallback`) |
| `spa.conf`* | Same configuration as `spa-prerender.conf` but without `Node fallback` |
| `static-cache.conf` | simple cache configuration e.g. used in `spa.conf` |
| `transparent-proxy.conf` | simple proxy for loopback or local proxies, e.g. `Node` backends |

`*` For these configuration snippets, there are additional SSL configuration in `ssl` subdirectory

#### SSL

| File | Snippet |
| ---- | ------- |
| `bootstrap.conf` | Bootstraps SSL configuration for Letsencrypt |
| `redirect-cf.conf` | Redirects HTTP to HTTPS on Cloudflare or other transparent proxies |
| `redirect.conf` | Redirects HTTP to HTTPS on regular connections |
| `spa-prerender.conf`* | Shortcut for `SPA-Prerender` with SSL |
| `spa.conf`* | Shortcut for `SPA` with SSL |
| `well-known.conf` | Bootstrap of certbot for authenticating certificate requests |

`*` These Shortcuts basically just import SSL configuration and the original snippet from above

### Letsencrypt

Domains are secured by letsencrypt certificates. For this to work, we need to set up letsencrypt for our domains we want to secure and run it with the following arguments:

```
$ certbot certonly --email you@email.com --agree-tos --expand --webroot -w /var/letsencrypt/
```

To update the certificates we run

```
$ certbot renew
```

Every time we update or create certificates we need to update nginx' configuration:

```
$ service nginx reload
```

For further information about certbot and letsencrypt visit [https://letsencrypt.org/](https://letsencrypt.org/getting-started/)
