# docker-festival-workshop
A workshop centered around Docker meant to help all attendees get to their personal next step (rather than a fixed program)

This workshop will be given at the [Dutch PHP Conference](https://www.phpconference.nl/)

## Steps

### Side project (volgende stap: containerizen)
### Development environment (volgende stap: tests, CI configurereren)
- Mount volumes from shared filesystem
- Use Ingress proxy like Traefik
- Run as non root user
- Test/build tools in container
- Pre-commit hook
- Wait with running tests until all services are available (using curl, LIIP Monitor bundle etc.)
### CI (volgende stap: deployment)
- Split app in backend (e.g. PHP-FPM) and front-end (e.g. nginx) (including resolving app by web container)  
- Create built version of app
- Health checks

### Staging/Production environment (volgende stap: pipeline)
- Support multiple instances (handle sessions)
- Restart on error
- Docker Swarm secrets

### Continuous Delivery (volgende stap: monitoring, automatische rollback, etc.)
- Rolling updates including db migrations

### Blue/green, resilience

## Installing PHP Extensions
Take a look at this [collection of 'Dockerized' PHP extensions](https://github.com/lucasvanlierop/docker-php-extensions)

## Projects

You have to choose a project to to work on during the workshop. We don't mind whether that's a personal or professional project. If you don't have a project you could start with with default Symfony/Laravel/Drupal/Wordpress/x project or something which isn't PHP based OR you could help containerize existing projects like for example the [DomCode rafflers](https://travis-ci.org/domcode/rafflers/builds/240328946?utm_source=github_status&utm_medium=notification)
