api-gateway-hmac
================

Adds HMAC support to Lua with multiple algorithms, via OpenSSL and FFI.
Inspired from https://github.com/openresty/lua-resty-string
See: https://github.com/openresty/lua-resty-string/pull/18

Table of Contents
=================

* [Status](#status)
* [Description](#description)
* [Synopsis](#synopsis)
* [Developer Guide](#developer-guide)


Status
======

This library is considered production ready.

Description
===========

This library requires an nginx build with OpenSSL,
the [ngx_lua module](http://wiki.nginx.org/HttpLuaModule), and [LuaJIT 2.0](http://luajit.org/luajit.html).


Synopsis
========

```lua
    # nginx.conf:

    lua_package_path "/path/to/lua-resty-string/lib/?.lua;;";

    server {
        location = /test {
            content_by_lua_file conf/test.lua;
        }
    }

    -- conf/test.lua:

    -- HMAC-SHA-1
    local resty_hmac_sha1 = require "api-gateway.resty.hmac"
    local hmac_sha1 = resty_hmac_sha1:new()

    local digest = hmac_sha1:digest("sha1","secret-key","Hello world")
    ngx.say("hmac_sha1: ", digest)

    -- HMAC-SHA-224
    local resty_hmac_sha224 = require "api-gateway.resty.hmac"
    local hmac_sha224 = resty_hmac_sha224:new()

    local digest = hmac_sha224:digest("sha224","secret-key","Hello world")
    ngx.say("hmac_sha224: ", digest)

    -- HMAC-SHA-226
    local resty_hmac_sha256 = require "api-gateway.resty.hmac"
    local hmac_sha256 = resty_hmac_sha256:new()

    local digest = hmac_sha256:digest("sha256","secret-key","Hello world")
    ngx.say("hmac_sha256: ", digest)

    -- HMAC-SHA-384
    local resty_hmac_sha384 = require "api-gateway.resty.hmac"
    local hmac_sha384 = resty_hmac_sha384:new()

    local digest = hmac_sha384:digest("sha384","secret-key","Hello world")
    ngx.say("hmac_sha384: ", digest)

    -- HMAC-SHA-512
    local resty_hmac_sha512 = require "api-gateway.resty.hmac"
    local hmac_sha512 = resty_hmac_sha512:new()

    local digest = hmac_sha512:digest("sha512","secret-key","Hello world")
    ngx.say("hmac_sha512: ", digest)



```
[Back to TOC](#table-of-contents)

Developer guide
===============

## Install the api-gateway first
 Since this module is running inside the `api-gateway`, make sure the api-gateway binary is installed under `/usr/local/sbin`.
 You should have 2 binaries in there: `api-gateway` and `nginx`, the latter being only a symbolik link.

## Update git submodules
```
git submodule update --init --recursive
```

## Running the tests
The tests are based on the `test-nginx` library.
This library is added a git submodule under `test/resources/test-nginx/` folder, from `https://github.com/agentzh/test-nginx`.

Test files are located in `test/perl`.
The other libraries such as `Redis`, `test-nginx` are located in `test/resources/`.
Other files used when running the test are also located in `test/resources`.

## Build locally
 ```
sudo LUA_LIB_DIR=/usr/local/api-gateway/lualib make install
 ```

To execute the test issue the following command:
 ```
 make test
 ```

 If you want to run a single test, the following command helps:
 ```
 PATH=/usr/local/sbin:$PATH TEST_NGINX_SERVROOT=`pwd`/target/servroot TEST_NGINX_PORT=1989 prove -I ./test/resources/test-nginx/lib -r ./test/perl/hmac.t
 ```
 This command only executes the test `hmac.t`.
