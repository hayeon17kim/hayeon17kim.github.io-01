---
title: "git clone 에러: certificate verification failed"
categories: error
tags: error, linux
---

https 프로토콜을 사용해 git clone을 하던 중 다음과 같은 인증서 에러가 발생했다.
```
$ git clone https://github.com/{username}/{reponame}.git
Cloning into '{reponame}'
fatal: unable to access 'https://github.com/{username}/{reponame}.git' server
certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile:none
```
이러한 경우 SSL Verify 옵션을 Off 해주면 다시 clone 하였을 때 잘 다운받아진다.

```
git config --global http.sslVerify false
```
git의 ssl veryfy 옵션을 끈다. --global을 주어 전역적으로 설정하고, [http] 섹션에 sslVerify = false를 추가한다.