# Docker festival workshop
A workshop centered around Docker meant to help all attendees get to their personal next step (rather than a fixed program)

This workshop will be given at the [Dutch PHP Conference](https://www.phpconference.nl/)

## Steps

### Side project
- Learn about Docker

### Development environment
- Create an image for your app
- Create docker-compose config for your app
- [Run Test/build tools like composer in container](http://lucasvanlierop.nl/blog/2017/06/28/running-cli-tools-in-docker-part-1-composer/)
- Mount project code into conainer
- Access your application via a proxy like [Traefik](http://lucasvanlierop.nl/blog/2017/06/25/accessing-your-docker-app-via-a-domain-name-using-traefik/)
- Write tests
- Maybe add a pre-commit hook

### CI
- Add CI specific config so your project can run in the CI too
- Create built version of app
- Consider to split app in backend (e.g. PHP-FPM) and front-end (e.g. nginx) 
- If you want to run database migrations: Create a wait script to make sure databases etc. are available before running migrations (you can use curl, LIIP Monitor bundle etc.)
- If you want to run system tests e.g. Behat/Selenium: Create a wait script to make sure databases etc. are available before running tests.

### Staging/Production environment
- Add Staging/production config
- Deploy!
- Restart on error
- Docker Swarm secrets

### Continuous Delivery
- Support multiple instances of your app (handle sessions)
- Add health checks that can be used for rolling migrations/monitoring
- Create a rolling migration
- Rolling updates including db migrations
- Monitoring
- Auto rollback

### Blue/green, resilience

## Projects

You have to choose a project to to work on during the workshop. We don't mind whether that's a personal or professional project. If you don't have a project you could start with with default Symfony/Laravel/Drupal/Wordpress/x project or something which isn't PHP based OR you could help containerize existing projects like for example the [DomCode rafflers](https://travis-ci.org/domcode/rafflers/builds/240328946?utm_source=github_status&utm_medium=notification)

## Resources

### Books

#### [The DevOps 2.1 Toolkit: Docker Swarm](https://leanpub.com/the-devops-2-1-toolkit)
 
Highly recommended: contains a lot of practical suggestions for working with Swarm, setting up clusters in a fully automated fashio. This book is full of practical suggestions for setting up continous delivery, monitoring, logging and databases.

#### [Docker in Action](https://www.manning.com/books/docker-in-action)

A proper introduction to Docker and Docker Compose.

### Articles

- [Awesome Docker - A curated list of Docker resources and projects](https://github.com/veggiemonk/awesome-docker)
- [Forwarding Logs From All Containers Running Anywhere Inside A Docker Swarm Cluster](https://technologyconversations.com/2016/10/24/forwarding-logs-from-all-containers-running-anywhere-inside-a-docker-swarm-cluster/)
- [A monitoring solution for Docker hosts, containers and containerized services](https://stefanprodan.com/2016/a-monitoring-solution-for-docker-hosts-containers-and-containerized-services/)
- [Tips & Tricks with Alpine + Docker](http://blog.zot24.com/tips-tricks-with-alpine-docker/)
- [REX-Ray - container storage orchestration engine](https://rexray.codedellemc.com/)
- [The Twelve-Factor App - a methodology for building software-as-a-service apps](https://12factor.net/)
- [What even is a container](https://jvns.ca/blog/2016/10/10/what-even-is-a-container/)
- [Is Docker insecure? (Hint:No)](http://blog.wercker.com/docker-security-issues)
- [Using Docker Secrets during Development](https://blog.mikesir87.io/2017/05/using-docker-secrets-during-development/)
- [3 ways to get Docker for Mac faster on your Symfony app](http://blog.michaelperrin.fr/2017/04/14/docker-for-mac-on-a-symfony-app/)
- [Test-drive Docker Healthcheck in 10 minutes](https://blog.alexellis.io/test-drive-healthcheck/)
- [10 things to avoid in docker containers](https://developers.redhat.com/blog/2016/02/24/10-things-to-avoid-in-docker-containers/)
- [Monitor your applications with Prometheus](https://blog.alexellis.io/prometheus-monitoring/)

### Training & tutorials

- [Self Paced Docker Training](http://training.play-with-docker.com/)
- [BASH Programming - Introduction HOW-TO](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html)

### By your trainers

- [Accessing your Docker app via a domain name using Træfɪk](http://lucasvanlierop.nl/blog/2017/06/25/accessing-your-docker-app-via-a-domain-name-using-traefik/)
- [Containerizing a static website with Docker](https://php-and-symfony.matthiasnoback.nl/2017/01/containerizing-a-static-website-with-docker)
- [Docker build patterns](https://php-and-symfony.matthiasnoback.nl/2017/04/docker-build-patterns)
- [Making a Docker image ready for use with Swarm Secrets](https://php-and-symfony.matthiasnoback.nl/2017/06/making-a-docker-image-ready-for-swarm-secrets)
- [Microservices for everyone](https://leanpub.com/microservices-for-everyone/)
- [Run CLI tools in a container](http://lucasvanlierop.nl/blog/2017/06/28/running-cli-tools-in-docker-part-1-composer/)
- [A Collection of 'Dockerized' PHP extensions](https://github.com/lucasvanlierop/docker-php-extensions)
