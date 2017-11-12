---
layout: doc_main
title: Rspamd Documentation
---

## Installation and Administration

Here are the main introduction documents that are recommended for reading if you are going to use Rspamd in your mail system.

* **[Quick start]({{ site.baseurl }}/doc/quickstart.html)** - learn how to install, setup and perform initial configuring of Rspamd
* **[MTA integration](integration.html)** - describes how to integrate Rspamd into your mail infrastructure
* **[Upgrading](migration.html)** - the list of incompatible changes between versions of Rspamd
* **[Frequently asked questions](faq.html)** - common questions about Rspamd
* **[Migrating from SA](./tutorials/migrate_sa.html)** - the guide for those who want to migrate an existing SpamAssassin system to Rspamd

## Reference guide

This section contains documents about various configuration details.

* **[Configuration structure](./configuration/index.html)** explains the basic principles of Rspamd configuration files
  + **[Users settings description](./configuration/settings.html)** could be useful if you need to setup per-user configuration or want to process mail in different ways, for example, for inbound and outbound messages.
  + **[Top-level options]({{ site.baseurl }}/doc/configuration/options.html)** are used to set the global options for Rspamd, such as DNS resolver setup and so on
  + **[Logging setup]({{ site.baseurl }}/doc/configuration/logging.html)** is required if you need customized logging
  + **[Metrics, actions and symbols]({{ site.baseurl }}/doc/configuration/metrics.html)** are very important as they allow to set custom rules scores and thresholds for different actions
  + **[Composite symbols]({{ site.baseurl }}/doc/configuration/composites.html)** are useful to combine rules into meta-rules
  + **[Statistical module]({{ site.baseurl }}/doc/configuration/statistic.html)** describes the Bayes classifier and its backends
* **[Modules documentation](./modules/)** gives the detailed description of each Rspamd module
* **[Workers documentation](./workers/)** contains information about different Rspamd worker processes: scanners, controller, fuzzy storage and so on
* **[Redis configuration page]({{ site.baseurl }}/doc/configuration/redis.html)** describes how to configure Redis cache for Rspamd
* **[Redis replication](./tutorials/redis_replication.html)** is a step-by-step guide on establishing Bayesian statistics and fuzzy storage replication
* **[Rspamadm]({{ site.baseurl }}/doc/rspamadm.html)** - describes utilities contained in the `rspamadm` command
* **[Stunnel setup](./tutorials/stunnel_setup.html)** is a step-by-step guide to secure communications between Redis instances with [stunnel](https://www.stunnel.org)
* **[Upstreams article]({{ site.baseurl }}/doc/configuration/upstream.html)** depicts how Rspamd connects to external servers (e.g. Redis or DNS)
* **[Getting feedback from users with IMAPSieve](./tutorials/feedback_from_users_with_IMAPSieve.html)** - how to get a copy of the message moved by user from or into the `Junk` folder

## Developers documentation

This section contains details about Rspamd internals, documents about writing new rules for Rspamd and, in particular, Rspamd Lua API.

* **[Writing Rspamd rules](./tutorials/writing_rules.html)** is a step-by-step guide that describes how to write rules for Rspamd
* **[Lua API reference](./lua/)** provides the extensive information about all Lua modules available in Rspamd
* **[Creating your fuzzy storage]({{ site.url }}{{ site.baseurl }}/doc/fuzzy_storage.html)** provides information about how to make your own hashes storage and how to learn it efficiently
* **[Architecture information](./architecture/index.html)** provides an overview of the Rspamd internal structure
* **[Protocol documentation](./architecture/protocol.html)** describes Rspamd protocol which is used to communicate with external tools, such as Rmilter or `rspamc` client utility
* **[Contributing to rspamd.com web-site](./tutorials/site_contributing.html)** describes general conventions and how to test web-site changes

## External resources

* [Own mail server based on Dovecot, Postfix, MySQL, Rspamd and Debian 9 Stretch](https://thomas-leister.de/en/mailserver-debian-stretch/) - a good example of all-in-one tutorial about how to setup your own mail server
* [A guide to self-hosting your email on FreeBSD using Postfix, Dovecot, Rspamd, and LDAP.](https://www.c0ffee.net/blog/mail-server-guide) - similar to the previous guide but uses a different technologies stack
* [Training Rspamd with Dovecot antispam plugin, part 1](https://kaworu.ch/blog/2014/03/25/dovecot-antispam-with-rspamd/)<sup>[1](#fn1)</sup> - this tutorial describes how to train Rspamd automatically using the `antispam` plugin of the `Dovecot` IMAP server
* [Training Rspamd with Dovecot antispam plugin, part 2](https://kaworu.ch/blog/2015/10/12/dovecot-antispam-with-rspamd-part2/)<sup>[1](#fn1)</sup> - continuation of the previous tutorial

<a name="fn1">1.</a> antispam plugin is deprecated, use [IMAPSieve](https://wiki.dovecot.org/HowTo/AntispamWithSieve) instead
