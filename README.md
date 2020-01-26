Docker Apache Https Demo
==================

没找到直接支持https的apache image，按照<https://hub.docker.com/_/httpd>的做法，
弄了一个Dockerfile对apache的配置文件进行了修正，并使用了手动生成的证书。

```
npm run up
```

```
npm run down
```
