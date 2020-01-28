# Security-engineer.local

PHP-FPM 7.3 & Nginx 1.16 setup for Docker, build on [Alpine Linux](http://www.alpinelinux.org/). The image is only 35MB large.

-   Built on the lightweight and secure Alpine Linux distribution
-   Very small Docker image size (+/-35MB)
-   Uses PHP 7.3 for better performance, lower cpu usage & memory footprint
-   Optimized for 100 concurrent users
-   Optimized to only use resources when there's traffic (by using PHP-FPM's ondemand PM)
-   The servers Nginx, PHP-FPM and supervisord run under a non-privileged user (nobody) to make it more secure
-   The logs of all the services are redirected to the output of the Docker container (visible with `docker logs -f <container name>`)

![php 7.3](https://img.shields.io/badge/php-7.3-brightgreen.svg)
![License MIT](https://img.shields.io/badge/license-MIT-blue.svg)
![nginx 1.16.1](https://img.shields.io/badge/nginx-1.16-brightgreen.svg)


# Security Features!

  - minimal image that bundle only the necessary system tools and libraries required to run your project, you are also minimizing the attack surface for attackers and ensuring that you ship a secure OS.
  - To minimize exposure, opt-in to create a dedicated user and a dedicated group in the Docker image for the application; use the USER directive in the Dockerfile to ensure the container runs the application with the least privileged access possib
  - Sign and verify image to mitigate MITM attacks
  - Find, fix and monitor for open source vulnerabilities

### Tech

I use a number of open source projects to work properly:

* [Anchore](https://anchore.com/) - Enable High Velocity, Policy-Based Container Workflows Without Compromise
* [Snyk](https://snyk.io/) - Securing open source and containers
* [Notary](https://github.com/theupdateframework/notary#building-notary) - Notary aims to make the internet more secure by making it easy for people to publish and verify content.
* [Docker](https://www.docker.com/) - Securely build, share and run any application, anywhere
* [PHP](https://www.php.net/) - PHP is a popular general-purpose scripting language that is especially suited to web development.
* [Nginx](https://www.nginx.com/) - Improve the performance, reliability, and security of your applications


### Installation



Start the Docker container:

```sh
$ docker run -p 80:8080 diiegg/security-engineer.local
```
See the PHP info on http://localhost, or the static html page on http://localhost/test.html

Verify docker images

Docker defaults allow pulling Docker images without validating their authenticity, thus potentially exposing you to arbitrary Docker images whose origin and author aren’t verified.

Make it a best practice that you always verify images before pulling them in, regardless of policy. To experiment with verification, temporarily enable Docker Content Trust with the following command:

```sh
$ export DOCKER_CONTENT_TRUST=1
```
Get the Verify image

```sh
$ docker pull diiegg/security-engineer.local:latest
```

inspect the signing details of our image

```sh
$ docker trust inspect --pretty diiegg/security-engineer.local
```
![lo](https://user-images.githubusercontent.com/12648295/73222087-4d6be480-415a-11ea-9a96-80233e59bbd4.png)

Run the verify image 

```sh
$ docker run -p 80:8080 diiegg/security-engineer.local
```

Scan a Docker image for known vulnerabilities with these commands:

```sh
# fetch the image to be tested so it exists locally
$ docker pull node:10
# scan the image with snyk
$ snyk test --docker node:10 --file=path/to/Dockerfile
# need snuk installed
```

Monitor a Docker image for known vulnerabilities so that once newly discovered vulnerabilities are found in the image, Snyk can notify and provide remediation advice:

```sh
$ snyk monitor --docker node:10
```
![enter image description here](https://user-images.githubusercontent.com/12648295/73222275-c0755b00-415a-11ea-9a5d-b6df74a06789.png)

Analysing Images with Anchor

```sh
$ anchore-cli image add diiegg/security-engineer:latest
```
![enter image description here](https://user-images.githubusercontent.com/12648295/73225792-c4f34100-4165-11ea-955a-8b9996ca0e03.png)

![enter image description here](https://user-images.githubusercontent.com/12648295/73223097-ddab2900-415c-11ea-86e8-d1f626e13724.png)

For run This image in production please visit

* [Docker-bench-security](https://github.com/docker/docker-bench-security) - The Docker Bench for Security is a script that checks for dozens of common best-practices around deploying Docker containers in production



License
----

MIT
