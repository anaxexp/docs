# Varnish stack documentation

Varnish can be configured with the following [environment variables](https://github.com/anaxexp/varnish#environment-variables)

## Headers

* `X-Varnish-Cache`: HIT or MISS, corresponds to when the cache was found or not
* `Age: 34`: age of the cache in seconds
* `X-Varnish: 65658 65623` - the first number is the ID of a request, the second is the ID of cache inside of Varnish. When operating normally the first number changes with every request of the same page and the second stays the same.

Set header `Cache-Control:no-cache` on backend to tell Varnish to not cache this page.

## CLI

Grouped list with the most usual entries from different logs:
```bash
varnishtop
```

A histogram that shows the time taken for the requests processing:
```bash
varnishhist
```

Varnish stats, shows how many contents on cache hits, resource consumption, etc..:
```bash
varnishstat
```

Log showing requests made to the web backend server:
```bash
varnishlog
```

## Troubleshooting 503 (guru meditation) errors

You can get more details on 503 responses by filtering the logs:
```bash
varnishlog -q 'RespStatus == 503' -g request
```

A few reasons why you may get 503:

* A problem on backend, see backend container's logs
* Varnish may cache non-broken page from backend when backend gives 5xx, in this cases you will sometimes get 503 (fetch from backend) and sometimes 200 OK (from cache)
* Broken backend headers that prevent from parsing backend response's body, e.g. gzip encoding header when the body in fact is not gzipped (you should not gzip pages on your backend, we already do that on Nginx)  
* Timeouts on varnish are too low (unlikely, the defaults are high enough for 95% cases), you can increase them via environment variables 

## Changelog

### 1.2.0

Environment variable `VARNISHD_STORAGE_SIZE` has been dropped, we no longer add a predefined secondary storage. You can now add your custom secondary storage via `VARNISHD_SECONDARY_STORAGE` https://github.com/anaxexp/varnish/pull/4

### 1.1.0

* Default [memory request](../config.md#resources) set to 16m
* The following environment variables changed names (old version no longer supported), DEPRECATED > NEW:
```
VARNISHD_THREAD_POOLS > VARNISHD_PARAM_THREAD_POOLS
VARNISHD_THREAD_POOL_ADD_DELAY > VARNISHD_PARAM_THREAD_POOL_ADD_DELAY
VARNISHD_THREAD_POOL_MIN > VARNISHD_PARAM_THREAD_POOL_MIN
VARNISHD_THREAD_POOL_MAX > VARNISHD_PARAM_THREAD_POOL_MAX
```
* Changed default values:
```
VARNISHD_PARAM_THREAD_POOL_ADD_DELAY from 2 to 0.000
VARNISHD_PARAM_THREAD_POOLS from 1 to 2
VARNISHD_PARAM_THREAD_POOL_MAX from 1000 to 5000
```
* Added additional env vars that control varnishd params (https://github.com/anaxexp/varnish/issues/1)


### 1.0.3

* Varnish updated to 4.1.9
* Health check timeout increased to 30 seconds

### 1.0.2

* Varnish daemon env vars now start from VARNISHD_ to avoid collisions

### 1.0.1

* Health check improvement, now uses varnishadm instead of curl

### 1.0.0

Initial release