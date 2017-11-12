---
layout: doc_modules
title: Greylisting module
---

# Greylisting module

This module is intended to delay messages that have spam score above `greylisting` action threshold.

## Principles of work

Greylisting module saves 2 hashes for each message in Redis:

* **meta** hash is based on triplet `from`:`to`:`ip`
* **data** hash is taken from the message's body if it has enough length for it

IP address is stored with certain mask applied: it is `/19` for IPv4 and `/64` for IPv6 accordingly. Each hash has its own timestamp and Rspamd checks for the following times:

* `greylisting` time - when a message should be temporary rejected
* `expire` time - when a greylisting hash is stored in Redis

The hashes lifetime is depicted in the following scheme:

<img class="img-responsive" width="75%" src="{{ site.baseurl }}/img/greylisting.png">

This module produces `soft reject` action on greylisting which **SHOULD** be treated as temporary rejection by MTA (usually via Milter interface).  Exim can recognise it with configuration - refer to the [integration guide]({{ site.baseurl }}/doc/integration.html#integration-with-exim-mta) for details. Haraka supports it from v2.9.0.

## Module configuration

First of all, you need to setup Redis server for storing hashes. This procedure is described in detail in the [following document]({{ site.baseurl }}/doc/configuration/redis.html). Thereafter, you can modify a couple of options specific for greylisting module. It is recommended to define these options in `local.d/greylist.conf`:

* **`expire`**: setup hashes expire time (1 day by default)
* **`greylist_min_score`**: messages with scores below this threshold are not greylisted (default unset)
* **`ipv4_mask`**: mask to apply for IPv4 addresses (19 by default)
* **`ipv6_mask`**: mask to apply for IPv6 addresses (64 by default)
* **`key_prefix`**: prefix for hashes to store in Redis (`rg` by default)
* **`max_data_len`**: maximum length of data to be used for body hash (10kB by default)
* **`message`**: a message for temporary rejection reason (`Try again later` by default)
* **`timeout`**: defines greylisting timeout (5 min by default)
* **`whitelisted_ip`**: map of IP addresses and/or subnets to skip greylisting for
* **`whitelist_domains_url`**: map of hostnames and/or eSLDs of hostnames to skip greylisting for

If you need to skip greylisting based on other conditions disabling the `GREYLIST_CHECK` and `GREYLIST_SAVE` symbols with [settings module]({{ site.baseurl }}/doc/configuration/settings.html) might suffice.

To enable the module with default settings you need to define at least [redis]({{ site.baseurl }}/doc/configuration/redis.html) servers to store greylisting data:

~~~ucl
# local.d/greylist.conf
servers = "127.0.0.1:6379";
~~~

Adding servers to store greylisting data enables greylisting in Rspamd.
