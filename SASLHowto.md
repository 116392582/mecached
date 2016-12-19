## Introduction

If running memcached in a mostly trusted network; such as within a cloud or a
corporate internal cloud, you might want to restrict access to the service.

SASL is not implemented as end-ot-end encryption. While this allow safely
restricting access to the daemon, it does not hide communications and isn't
suitable for using over the internet.

[SASL](http://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) (as described in [RFC2222](http://tools.ietf.org/html/rfc2222)) is a standard for adding authentication mechanisms to protocols in a way that is protocol independent.

## Getting Started

In order to deploy memcached with SASL, you'll need two things:

  # A memcached server with SASL support (version 1.4.3 or greater built with `--enable-sasl`)
  # A client that supports SASL

### Configuring SASL

For the most part, you just do the normal SASL admin stuff.

```
# Create a user for memcached.
saslpasswd2 -a memcached -c cacheuser
```

### Running Memcached

In order to enable SASL support in the server you must use the `-S` flag.

The `-S` flag does a few things things:

  # Enable all of the SASL commands.
  # Require binary protocol _only_.
  # Require authentication to have been successful before commands may be issued on a connection.

### Further Info

Read more about memcached's [[SASL auth protocol|SASLAuthProtocol]].
