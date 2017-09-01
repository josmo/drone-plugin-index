---
date: 2016-01-01T00:00:00+00:00
title: Webhook
author: drone-plugins
tags: [ notifications ]
repo: drone-plugins/drone-webhook
logo: webhook.svg
image: plugins/webhook
---

Drone plugin to send build status notifications via Webhook. The below pipeline configuration demonstrates simple usage:

```yaml
pipeline:
  webhook:
    image: plugins/webhook
    urls: 
      - https://hooks.slack.com/services/...
    headers:
      - "Authorization=pa55word"
```

Example configuration with custom username and password for http basic auth:

```diff
pipeline:
  webhook:
    image: plugins/webhook
    urls: 
      - https://hooks.slack.com/services/...
    headers:
      - "Authorization=pa55word"
+   username: username
+   password: password
```

Example configuration with GET instead of POST:

```diff
pipeline:
  webhook:
    image: plugins/webhook
    urls: 
      - https://hooks.slack.com/services/...
    headers:
      - "Authorization=pa55word"
+   method: GET
```

# Parameter Reference

urls
: list of urls for the webhook to hit

method
: HTTP verb to use with the hook, default to `POST`

headers
: list of customer headers to use 

username
: username for basic auth, if set must have password as well

password
: password for basic auth, if set must have username as well 

debug
: Print out each URL request and response information, default to `false`

skip-verify
: Skips verification of TLS certificates, defaults to `false`

content-type
: customer content type, default application/json

template
: overwrite the default message template

# Template Reference

repo.owner
: repository owner

repo.name
: repository name

build.status
: build status type enumeration, either `success` or `failure`

build.event
: build event type enumeration, one of `push`, `pull_request`, `tag`, `deployment`

build.number
: build number

build.commit
: git sha for current commit

build.branch
: git branch for current commit

build.tag
: git tag for current commit

build.ref
: git ref for current commit

build.author
: git author for current commit

build.link
: link the the build results in drone

build.created
: unix timestamp for build creation

build.started
: unix timestamp for build started

# Template Function Reference

uppercasefirst
: converts the first letter of a string to uppercase

uppercase
: converts a string to uppercase

lowercase
: converts a string to lowercase. Example `{{lowercase build.author}}`

datetime
: converts a unix timestamp to a date time string. Example `{{datetime build.started}}`

success
: returns true if the build is successful

failure
: returns true if the build is failed

truncate
: returns a truncated string to n characters. Example `{{truncate build.sha 8}}`

urlencode
: returns a url encoded string

since
: returns a duration string between now and the given timestamp. Example `{{since build.started}}`
