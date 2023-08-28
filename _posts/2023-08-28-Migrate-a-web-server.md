---
title: Migrate a web server with SSL certificates
layout: post
---

## Introduction
Suppose the domain is `example.com`, associated with a static IP address, for which one had generated a SSL certficate before. We will migrate to a new server, at the end of the process, still bearing the domain name `example.com`.

As these are two machine instances, at one point of time, one'd need to revoke the SSL certificate on the old server, and issue a new certificate for `example.com` on the new server. The risk lies after the old certificate is revoked and before the new one is issued -- one got to be confident enough he can get one issued on the new server, otherwise disaster strikes, especially for web domains of high commerical value.

## A Plan for Migration
1. In [AWS](https://aws.amazon.com), create a new Virtual Machine (VM) instance and a new Elastic IP (static public IP address). Associate the Elastic IP address to the new VM instance.
2. Now go to your DNS ([Domain Name System](https://en.wikipedia.org/wiki/Domain_Name_System)) registrar, create a new DNS entry: `stage1` with the new Elastic IP address. The new VM lives for `stage1.example.com`.
3. Set up everything on `stage1.example.com`. Get it running. Generate an SSL certificate for `stage1.example.com` as a rehearsal, refer to [Use acme.sh to install SSL certficates](/2023/08/28/acme-SSL-certificates.html).
4. `stage1.example.com` should be fully functional by now. Leave it running for some time. When ready to complete the migration, shut down the web service on both the old and new servers, revoke the SSL certificates on both servers with `certbot revoke` or `acme.sh --revoke`. Now go into the AWS Management Console, disassociate the Elastic IP address from the new VM instance, and associate it with the Elastic IP address always configured for `example.com`. The VM is now `example.com` instead of `stage1.example.com`.
5. Start the new VM. With the SSL certificate for `stage1.example.com` already revoked, issue an SSL certificate for `example.com` on it. Re-configure the web server for `example.com` and run it.
6. Go to the DNS setting, and delete the entry for subdomain `stage1`. Back in AWS, the Elastic IP address created at the beginning of these steps should be free of association now, release it. It is **important** to clean up and release these resources, uncleaned DNS configurations may become a security risk.
