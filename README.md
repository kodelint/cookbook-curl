[![Master Branch](http://34.238.246.129:8080/buildStatus/icon?job=cookbook-curl/master)](http://34.238.246.129:8080/job/cookbook-curl/job/master/)
[![Production Branch](http://34.238.246.129:8080/buildStatus/icon?job=cookbook-curl/production)](http://34.238.246.129:8080/job/cookbook-curl/job/production/)

Description
============

Installs/Configures curl

Requirements
============

* Chef 12
* Ruby 2.1.0/2.2.0

Attributes
==========

* `default['curl']['libcurl_packages']` - A list of libcurl packages to install.

Usage
=====

```json
"run_list": [
    "recipe[curl]"
]
```

default
-------

Installs/Configures curl

libcurl
-------

```json
"run_list": [
    "recipe[curl::libcurl]"
]
```

Install/Configure libcurl packages

Chef Upload
============
**Cookbook uploads** - **Branch:** `production`

License and Author
==================

Author:: Kodelint (<kodelint@gmail.com>)
