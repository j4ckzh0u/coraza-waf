---
title: "Getting started with Coraza Web Application Firewall"
keywords: coraza waf
tags: [getting_started]
sidebar: mydoc_sidebar
permalink: index.html
summary: Coraza Web Application Firewall is a golang Modsecurity implementation with embedded reverse proxy capabilities.
---


## Coraza Web Application Firewall

<img src="{{ "images/company_logo.png" }}" alt="Company logo" style="width:440px;height:auto;"/>

Welcome to Coraza Web Application Firewall, this project is based on ModSecurity with the goal to become the first corporate-grade Open Source Web Application Firewall, extensible enough to serve as the baseline for many projects. 

## Build from source

Follow these instructions to build the WAF.

**Check Prerequisites**

* Linux distribution (Debian/Ubuntu is recommended)
* Golang compiler v1.13+
* libpcre-dev (``apt install libpcre++-dev`` for Ubuntu)

**Clone Project**

```
$ git clone https://github.com/jptosso/coraza-waf
```

**Build Coraza WAF**

```
# Installs libinjection for compilation, must be root
$ sudo make libinjection
$ make waf
```

As a result you will get a ./coraza-waf standalone binary.

## Install from source

You have to ways to install Coraza, you may create a installation package or install is using make.

To install it using make just type 
```
$ git clone https://github.com/jptosso/coraza-waf
$ cd coraza-waf/
$ sudo make libinjection
$ make
$ sudo make install
```

## Build installers

### Debian (.deb)

Keep in mind that this script requires the project dependencies plus dpkg tools.

Go to the project directory and run the following:
```
$ git clone https://github.com/jptosso/coraza-waf
$ cd coraza-waf/
$ ./scripts/debian/package.sh
```
As a result, you will get a /tmp/coraza-waf-build/corazawaf-version.deb file ready to be installed with ``dpkg -i corazawaf-version.deb``

### Centos/RHEL (.rpm)

There is no rpm package but you can create your own build using the `alien` command over a .deb package:
```
$ alien -r coraza-waf0.1-alpha1_amd64.deb
coraza-waf0.1.amd64.rpm generated
```

## Running the test suite

Run the go tests:
```
go test ./...
```

Run the standard test suite:
```
go run cmd/testsuite/main.go -path test/ -rules test/data/test-rules.conf
```

Run the test suite against OWASP CRS:
```
$ git clone https://github.com/jptosso/coraza-waf
$ git clone https://github.com/SpiderLabs/owasp-modsecurity-crs
# Create your OWASP CRS package owasp-crs.conf
$ cd coraza-waf/
$ go run cmd/testsuite/main.go -path ../owasp-modsecurity-crs -rules ../owasp-modsecurity-crs/owasp-crs.conf
```

## Deploy Coraza WAF with Docker

```
$ docker run --name my-waf -v /some/config/routes.eskip:/etc/coraza-waf/routes.eskip:ro -d -p 9090:9090 jptosso/coraza-waf
```

Alternatively, a simple Dockerfile can be used to generate a new image that includes the necessary content (which is a much cleaner solution than the bind mount above):

```
FROM jptosso/coraza-waf
COPY static-settings-directory /etc/coraza-waf
```

Place this file in the same directory as your directory of content ("static-settings-directory"), ``run docker build -t my-waf .``, then start your container:

```
$ docker run --name my-waf -d -p 9090:9090 some-waf-server
```
Then you can hit http://localhost:9090 or http://host-ip:9090 in your browser.

## Configure your installation

- **/etc/coraza-waf/skipper.yaml**: Contains the options that will be imported by Skipper by default.
- **/etc/coraza-waf/routes.eskip**:  Contains the routes that will be used by Skipper.
- **/etc/coraza-waf/profiles/default/rules.conf**: Placeholder file with default options.

## Differences with ModSecurity

### Deprecated directives
SecArgumentSeparator, SecAuditLog2, SecCacheTransformations, SecCookieFormat, SecCookieV0Separator, SecDebugLog, SecDebugLogLevel, SecDisableBackendCompression, SecGsbLookupDb, SecGuardianLog, SecPdfProtect, SecPdfProtectMethod, SecPdfProtectSecret, SecPdfProtectTimeout, SecPdfProtectTokenName, SecReadStateLimit, SecWriteStateLimit, SecRequestBodyInMemoryLimit, SecRequestBodyNoFilesLimit, SecRuleInheritance, SecStatusEngine, SecStreamInBodyInspection, SecUnicodeMapFile, SecUnicodeCodePage, 

### Custom directives

### Removed actions

Auditlog was removed and replaced with log to avoid confusion as Coraza WAF does not support other transaction log than audit log.

**Removed actions:** auditlog, noauditlog and setenv

### Removed Variables

### Removed Operators

### Custom Operators

**@validateNid:** Validates national ID for many countries, replaces validateSSN.

## Useful link

- [ModSecurity references](#)
- [Skipper Settings](#)
- [Skipper Routes (eskip)](#)

