Docker LetsEncrypt Certbot Get Certs Automatically Demo
=======================================================

使用`certbot`从<https://letsencrypt.org/>自动获取证书。

注：由于letsencrypt会对域名进行验证，所以这个命令必须放在相应域名对应ip的服务器上运行。

原理：

1. certbot会向letsencrypt发出验证请求
2. certbot会产生相应的token放在指定的server目录，供letsencrypt远程验证
3. 验证成功后，certbot会将生成的keys放在指定目录
4. certbot运行时会先检查keys是否已经存在，是否过期

- https://gist.github.com/harryfinn/e36e41cdbfba5a6e1d69d6498a4fc5ee
- https://www.humankode.com/ssl/how-to-set-up-free-ssl-certificates-from-lets-encrypt-using-docker-and-nginx

根据web server和服务器提供相应的命令：

https://certbot.eff.org/lets-encrypt/ubuntubionic-other

注意：
- `--standalone`: 让web server停止运行
- `--webroot`: 不让web server停止运行

我们这个命令为了简单起见，使用的是`--standalone`，需要先把web server关掉，释放80端口

```
npm run up
```

Output:

```
certbot_1  | Saving debug log to /var/log/letsencrypt/letsencrypt.log
certbot_1  | Plugins selected: Authenticator webroot, Installer None
certbot_1  | Registering without email!
certbot_1  | Obtaining a new certificate
certbot_1  | Performing the following challenges:
certbot_1  | http-01 challenge for onlinegeniuses.net
certbot_1  | http-01 challenge for www.onlinegeniuses.net
certbot_1  | Using the webroot path /data/letsencrypt for all unmatched domains.
certbot_1  | Waiting for verification...
nginx_1    | 18.224.20.83 - - [04/Feb/2020:12:33:28 +0000] "GET /.well-known/acme-challenge/VjJdeKCeuOQ5GMi4SvjRghAXCghIk4XZezd8_GujSIk HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)" "-"
nginx_1    | 18.224.20.83 - - [04/Feb/2020:12:33:28 +0000] "GET /.well-known/acme-challenge/DVDAwV8Kr0W8LSJmdA3SSAze4gm68JXNRVw6ulZrqtU HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)" "-"
nginx_1    | 52.58.118.98 - - [04/Feb/2020:12:33:28 +0000] "GET /.well-known/acme-challenge/DVDAwV8Kr0W8LSJmdA3SSAze4gm68JXNRVw6ulZrqtU HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)" "-"
nginx_1    | 52.58.118.98 - - [04/Feb/2020:12:33:28 +0000] "GET /.well-known/acme-challenge/VjJdeKCeuOQ5GMi4SvjRghAXCghIk4XZezd8_GujSIk HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)" "-"
nginx_1    | 34.211.60.134 - - [04/Feb/2020:12:33:28 +0000] "GET /.well-known/acme-challenge/DVDAwV8Kr0W8LSJmdA3SSAze4gm68JXNRVw6ulZrqtU HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)" "-"
nginx_1    | 34.211.60.134 - - [04/Feb/2020:12:33:28 +0000] "GET /.well-known/acme-challenge/VjJdeKCeuOQ5GMi4SvjRghAXCghIk4XZezd8_GujSIk HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)" "-"
nginx_1    | 66.133.109.36 - - [04/Feb/2020:12:33:28 +0000] "GET /.well-known/acme-challenge/DVDAwV8Kr0W8LSJmdA3SSAze4gm68JXNRVw6ulZrqtU HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)" "-"
nginx_1    | 66.133.109.36 - - [04/Feb/2020:12:33:28 +0000] "GET /.well-known/acme-challenge/VjJdeKCeuOQ5GMi4SvjRghAXCghIk4XZezd8_GujSIk HTTP/1.1" 200 87 "-" "Mozilla/5.0 (compatible; Let's Encrypt validation server; +https://www.letsencrypt.org)" "-"
certbot_1  | Cleaning up challenges
certbot_1  | IMPORTANT NOTES:
certbot_1  |  - Congratulations! Your certificate and chain have been saved at:
certbot_1  |    /etc/letsencrypt/live/onlinegeniuses.net/fullchain.pem
certbot_1  |    Your key file has been saved at:
certbot_1  |    /etc/letsencrypt/live/onlinegeniuses.net/privkey.pem
certbot_1  |    Your cert will expire on 2020-05-04. To obtain a new or tweaked
certbot_1  |    version of this certificate in the future, simply run certbot
certbot_1  |    again. To non-interactively renew *all* of your certificates, run
certbot_1  |    "certbot renew"
certbot_1  |  - Your account credentials have been saved in your Certbot
certbot_1  |    configuration directory at /etc/letsencrypt. You should make a
certbot_1  |    secure backup of this folder now. This configuration directory will
certbot_1  |    also contain certificates and private keys obtained by Certbot so
certbot_1  |    making regular backups of this folder is ideal.
docker-letsencrypt-certbot-get-certs-automatically-demo_certbot_1 exited with code 0
^CGracefully stopping... (press Ctrl+C again to force)
Stopping docker-letsencrypt-certbot-get-certs-automatically-demo_nginx_1   ... done

$ tree .
.
├── Dockerfile
├── README.md
├── docker-compose.yml
├── package.json
└── volumes
    ├── _common
    │   └── usr
    │       └── share
    │           └── nginx
    │               └── html
    │                   └── index.html
    ├── certbot
    │   └── etc
    │       └── letsencrypt
    │           ├── accounts
    │           │   └── acme-staging-v02.api.letsencrypt.org
    │           │       └── directory
    │           │           └── 4b3da4be4e4a5624a430e14abc6167f2
    │           │               ├── meta.json
    │           │               ├── private_key.json
    │           │               └── regr.json
    │           ├── archive
    │           │   └── onlinegeniuses.net
    │           │       ├── cert1.pem
    │           │       ├── chain1.pem
    │           │       ├── fullchain1.pem
    │           │       └── privkey1.pem
    │           ├── csr
    │           │   └── 0000_csr-certbot.pem
    │           ├── keys
    │           │   └── 0000_key-certbot.pem
    │           ├── live
    │           │   ├── README
    │           │   └── onlinegeniuses.net
    │           │       ├── README
    │           │       ├── cert.pem -> ../../archive/onlinegeniuses.net/cert1.pem
    │           │       ├── chain.pem -> ../../archive/onlinegeniuses.net/chain1.pem
    │           │       ├── fullchain.pem -> ../../archive/onlinegeniuses.net/fullchain1.pem
    │           │       └── privkey.pem -> ../../archive/onlinegeniuses.net/privkey1.pem
    │           ├── renewal
    │           │   └── onlinegeniuses.net.conf
    │           └── renewal-hooks
    │               ├── deploy
    │               ├── post
    │               └── pre
    └── nginx
        └── etc
            └── nginx
                └── conf.d
                    └── default.conf

28 directories, 22 files
```

然后把`/volumes/certbot/etc/letsencrypt/live`下产生的文件copy到真正的地方
