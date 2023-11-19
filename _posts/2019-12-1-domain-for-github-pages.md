---
layout: post
date: 2019/12/1
tag:
  - takehomessage
author: Qiang
---

# How to configure a domain for github pages?

## 1. Custom domain in GitHub setting
Go to: Github repository - Setting - Custom domain

Add `takehomessage.com` (without www.) and save. Enable https later.

## 2. DNS management from your domain register

| Type  | Name | Value             |
|-------|------|-------------------|
| CNAME | www  | hutuben.github.io |
| A     | @    | 185.199.108.153   |
| A     | @    | 185.199.109.153   |
| A     | @    | 185.199.110.153   |
| A     | @    | 185.199.111.153   |

That's it. Both `takehomessage.com` and `www.takehomessage.com` go to `hutuben.github.io`.

Ref:

1. <https://medium.com/@hossainkhan/using-custom-domain-for-github-pages-86b303d3918a>
2. <https://help.github.com/en/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site>
