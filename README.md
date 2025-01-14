# smtprelay

[![Go Report Card](https://goreportcard.com/badge/github.com/decke/smtprelay)](https://goreportcard.com/report/github.com/decke/smtprelay)

Simple Golang based SMTP relay/proxy server that accepts mail via SMTP
and forwards it directly to another SMTP server.


## Why another SMTP server?

Outgoing mails are usually send via SMTP to an MTA (Mail Transfer Agent)
which is one of Postfix, Exim, Sendmail or OpenSMTPD on UNIX/Linux in most
cases. You really don't want to setup and maintain any of those full blown
kitchensinks yourself because they are complex, fragile and hard to
configure.

My use case is simple. I need to send automatically generated mails from
cron via msmtp/sSMTP/dma, mails from various services and network printers
via a remote SMTP server without giving away my mail credentials to each
device which produces mail.


## Main features

* Simple configuration with ini file or environment variables
* Supports SMTPS/TLS (465), STARTTLS (587) and unencrypted SMTP (25)
* Checks for sender, receiver, client IP
* Authentication support with file (LOGIN, PLAIN)
* Enforce encryption for authentication
* Forwards all mail to a smarthost (any SMTP server)
* Small codebase
* IPv6 support
* Docker support

## Using docker

Running smtprelay in a container is supported. You can simply use the included
`Dockerfile` to build and run the server. When building the container the
current configuration file `smtprelay.ini` will be copied inside the container,
so you must either configure the server before building or use a volume to mount
a different configuration to `/smtprelay.ini`.

As you may decide to use a different port than 25 to listen on, you must uncomment
the `EXPOSE` statement in the Dockerfile and configure your own port
(or use docker-compose).

**Important**: Keep in mind that docker does not support IPv6 by default, so you
either enable support or remove any IPv6 listening directives (`listen = ... [::1]:25`)
from `smtprelay.ini` before running.
