# dockerfile-nginx-php55-laravel

A Dockerfile that installs the latest nginx, php-5.5 with opcache and php-fpm. If you place this Dockerfile inside your Laravel root directory it will insert the contents of that directory into your Docker image. After that just run the container and it will have your Laravel project running in there.

## Installation

```
$ git clone https://github.com/Joostvanderlaan/dockerfile-nginx-php55-laravel
$ cd dockerfile-nginx-php55-laravel
$ sudo docker build -t dockerfile-nginx-php55-laravel .
```

## Usage

To spawn a new instance of wordpress:

```bash
$ sudo docker run -p 80 -d dockerfile-nginx-php55-laravel
```

You'll see an ID output like:
```
6742949a1bae
```

Use this ID to check the port it's on:
```bash
$ sudo docker port 674 80 # Make sure to change the ID to yours!
```

This command returns the container ID, which you can use to find the external port you can use to access Laravel from your host machine:

```
$ docker port <container-id> 80
```

You can the visit the following URL in a browser on your host machine to get started:

```
http://127.0.0.1:<port>
```