---
layout: doc_modules
title: Antivirus module
---

# Antivirus module

Antivirus module (new in Rspamd 1.4) provides integration with virus scanners. Currently supported are [ClamAV](http://www.clamav.net), [F-Prot](http://www.f-prot.com/products/corporate_users/unix/linux/mailserver.html), [Sophos](https://www.sophos.com/en-us/medialibrary/PDFs/partners/sophossavdidsna.pdf) (via SAVDI) and [Avira](https://www.avira.com/de/oem-antivirus) (via SAVAPI).

### Configuration

By default, given [Redis]({{ site.baseurl }}/doc/configuration/redis.html) is configured globally and `antivirus` is not explicitly disabled in redis configuration, results are cached in Redis according to message checksums.

Settings should be added to `/etc/rspamd/local.d/antivirus.conf`:

~~~ucl
# multiple scanners could be checked, for each we create a configuration block with an arbitrary name
first {
  # If set force this action if any virus is found (default unset: no action is forced)
  # action = "reject";
  # if `true` only messages with non-image attachments will be checked (default true)
  attachments_only = true;
  # If `max_size` is set, messages > n bytes in size are not scanned
  #max_size = 20000000;
  # symbol to add (add it to metric if you want non-zero weight)
  symbol = "CLAM_VIRUS";
  # type of scanner: "clamav", "fprot", "sophos" or "savapi"
  type = "clamav";
  # If set true, log message is emitted for clean messages
  #log_clean = false;
  # Prefix used for caching in Redis: scanner-specific defaults are used. If Redis is enabled and
  # multiple scanners of the same type are present, it is important to set prefix to something unique.
  #prefix = "rs_cl_";
  # For "savapi" you must also specify the following variable
  #product_id = 12345;
  # servers to query (if port is unspecified, scanner-specific default is used)
  # can be specified multiple times to pool servers
  # can be set to a path to a unix socket
  servers = "127.0.0.1:3310";
  # if `patterns` is specified virus name will be matched against provided regexes and the related
  # symbol will be yielded if a match is found. If no match is found, default symbol is yielded.
  patterns {
    # symbol_name = "pattern";
    JUST_EICAR = '^Eicar-Test-Signature$';
  }
  # In version 1.7.0+ patterns could be a list for ordered matching
  #patterns = [{SANE_MAL = 'Sanesecurity\.Malware\.*'}, {CLAM_UNOFFICIAL = 'UNOFFICIAL$'}];
  # `whitelist` points to a map of IP addresses. Mail from these addresses is not scanned.
  whitelist = "/etc/rspamd/antivirus.wl";
}
~~~

### SAVAPI specific details ###

The default SAVAPI configuration has a listening unix socket. You must change this to a TCP socket. The option "ListenAddress" in savapi.conf shows some examples. Per default this module expects the socket at 127.0.0.1:4444. You can change this by setting it in the "servers" variable as seen above.

You must also set the "product_id" which must match with the id for your HBEDV.key file. If you leave this, the default value is "0" and checking will fail with a log message that the given id was invalid.
