# Docker に Jenkins をのせる
参考：https://dev.classmethod.jp/tool/jenkins/jenkins-on-docker/

参考サイトに記載の通りに手順を踏んでいくとビルドで詰まった。

```
λ docker build -t jenkins-master .
Sending build context to Docker daemon  3.072kB
Step 1/5 : FROM jenkinsci/jenkins:2.11
2.11: Pulling from jenkinsci/jenkins
5c90d4a2d1a8: Pull complete
ab30c63719b1: Pull complete
c6072700a242: Pull complete
5f444d070427: Pull complete
620b5227cf38: Pull complete
3cfd33220efa: Pull complete
864a98a84dd2: Pull complete
734cc28150de: Pull complete
7abfef30d047: Pull complete
a3ed95caeb02: Pull complete
d6d17acf6d8c: Pull complete
4fe88aba41f8: Pull complete
cef8a93fdd78: Pull complete
27895d8c43ea: Pull complete
828cf6248824: Pull complete
dee1f141ee47: Pull complete
1d68e5cafb4c: Pull complete
4129a1370db4: Pull complete
8f0a0499e340: Pull complete
Digest: sha256:c4c04de0e9341fa477752923ec94014d28a13254bebe49fcefd4098e71e8344f
Status: Downloaded newer image for jenkinsci/jenkins:2.11
 ---> fb476c24b4d7
Step 2/5 : USER root
 ---> Running in 8b827a6567ba
 ---> aed9aaaada67
Removing intermediate container 8b827a6567ba
Step 3/5 : COPY plugins.txt /usr/share/jenkins/plugins.txt
 ---> 3cb85116ad16
Step 4/5 : RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
 ---> Running in bc0c67713126
Analyzing war: /usr/share/jenkins/jenkins.war
Downloading javadoc:1.4
curl: (3) Illegal characters found in URL
The command '/bin/sh -c /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt' returned a non-zero code: 3
```

plugins.txt を CRLF で作ってたから LF に修正。

```
λ docker- build -t jenkins-master .
Sending build context to Docker daemon  3.072kB
Step 1/5 : FROM jenkinsci/jenkins:2.11
 ---> fb476c24b4d7
Step 2/5 : USER root
 ---> Using cache
 ---> aed9aaaada67
Step 3/5 : COPY plugins.txt /usr/share/jenkins/plugins.txt
 ---> 3e053dfbdd4a
Step 4/5 : RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
 ---> Running in 846f005675d8
Analyzing war: /usr/share/jenkins/jenkins.war
Downloading javadoc:1.4
curl: (6) Could not resolve host: updates.jenkins.io
The command '/bin/sh -c /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt' returned a non-zero code: 6
```

ホスト解決できない。。。ブラウザから以下にはアクセスできる。
http://updates.jenkins.io/

またプロキシか。。。。
Dockerfileにプロキシ設定を追記して再実行

```
λ docker build -t jenkins-master .
Sending build context to Docker daemon  3.072kB
Step 1/8 : FROM jenkinsci/jenkins:2.11
 ---> fb476c24b4d7
Step 2/8 : ENV HTTP_PROXY http://oskproxy.intra.tis.co.jp:8080
 ---> Using cache
 ---> 35cece25ee44
Step 3/8 : ENV HTTPS_PROXY http://oskproxy.intra.tis.co.jp:8080
 ---> Using cache
 ---> cfc38f8ab914
Step 4/8 : ENV NO_PROXY 192.178.99.*
 ---> Using cache
 ---> c97163d5a802
Step 5/8 : USER root
 ---> Using cache
 ---> da5e9da2153c
Step 6/8 : COPY plugins.txt /usr/share/jenkins/plugins.txt
 ---> Using cache
 ---> ac622cdea330
Step 7/8 : RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
 ---> Running in 5ce02481e454
Analyzing war: /usr/share/jenkins/jenkins.war
Downloading javadoc:1.4
curl: (6) Could not resolve host: mirrors.jenkins-ci.org
The command '/bin/sh -c /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt' returned a non-zero code: 6
```

違うホストの名前解決に失敗。


