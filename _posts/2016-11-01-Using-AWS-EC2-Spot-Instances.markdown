---
title: Using AWS EC2 Spot Instances
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

# AWS EC2 Spot Instance
They're usually 60% cheaper than the normal instances. If one simply bids for a spot instance and has it, he only has it for the period when his bid price is higher than the current spot price and he'll lose it instantly when the spot price surges above his bid price. A good way to use the spot instances, however, is to estimate for how long and how many instances you're going to use and choose the "Reserve for Duration" option when booking them.

# AWS CLI
As we usually bring online a swarm of spot instances, the only practical way to manage them is through CLI. 

## Installation
AWS CLI is written in Python and supports Python 2/3.

```bash
pip3 install -U awscli
```

After installation, one will have the command `aws` in the path.

## Configuration for Your User Account
We have to first set up `~/.aws/config` and `~/.aws/credentials`. One can use `aws configure` to auto generate them. 

## Example Use
The command below list the Private IPs of all the instances currently running which are spawned from a given AMI image.

```bash
aws ec2 describe-instances --filters "Name=image-id,Values=ami-037a826c" --output text --query "Reservations[*].Instances[*].PrivateIpAddress" 
```

Specifying `text` for the output format is very important so that we could query into the structured ouput with the `query` option.

