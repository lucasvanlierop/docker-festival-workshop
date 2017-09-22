# Docker festival workshop
A workshop centered around Docker meant to help all attendees get to their personal next step (rather than a fixed program).

Goals are to 
- Contribute to container adoption
- Warn for pitfalls and drawbacks
Not just about Docker
Not just about containers


You have to choose a project to to work on during the workshop. We don't mind whether that's a personal or professional project. If you don't have a project you could start with with default framework-of-your-choice project OR you could help containerize existing projects like for example the [DomCode rafflers](https://travis-ci.org/domcode/rafflers/builds/240328946?utm_source=github_status&utm_medium=notification)

This workshop will be or has been given at:
- [Dutch PHP Conference 2017](https://www.phpconference.nl/)
- [DomCode](https://www.meetup.com/DomCode/events/243139488/)

# WhyGoal of this workshop is to:


# Pipeline/Timeline

The following checklist can be used to see where you/your Dockerized application currently stand(s) and what next steps could be .

## Fiddle around with a side project
This workshop assumes you have fiddled around with Docker already and will not cover any basics.
*Note some of the following points are very opiniated ;)*

## General:
- [ ] On Linux: services do not run as root (unless you know why you want that)
- [ ] File mounts are read only unless you need to write to them (e.g. to store file uploads).
- [ ] Containers run as read only unless this is not feasible. (Tip: for writable dirs take a look at [`tmpfs` volumes](https://docs.docker.com/engine/admin/volumes/tmpfs/))
- [ ] Environment specific config is organized in the project, e.g: `env/dev/docker-compose.yml, or `env/dev/app/.env` or `/env/dev/db/data/{lots of binary files}`.
- [ ] Environment specific config values are passed as environemnt variables.  
 
## Docker images
- [ ] There is a base image (or maybe multiple) for your application that includes everything the application needs to run. Think: Operating System, runtimes, libraries, configuration etc.
- [ ] Images include nothing more than required.
- [ ] Base images are bases on official images whenever possible *(Stand on the shoulds of giants)*
- [ ] Different responsibilities (e.g. running (Java, PHP, Ruby etc.) application code, handling web requests  with e.g. Nginx) are handled by different images.
- [ ] Images, configuration scripts for different services are organized in the project, e.g: `docker/service/app/web/my-custom-webserver.conf`
- [ ] Images are configured to handle stop signals (Tip take a look at `bash trap/tini/docker run --init/Dockerfile: STOPSIGNAL`)
- [ ] Services that need to resolve other services are configured to use the embedded Docker DNS (`127.0.0.11`) otherwise services like Nginx will try to resolve services outside docker which results in DNS resolve conflicts.
- [ ] Containerized CLI tools have an entrypoint configured
- [ ] The main process in entrypoint scripts takes over the process id using `exec`
- [ ] [Best practices for Dockerfiles](https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/) are taken into consideration
- [ ] Applications are configured to log to `stdout`/`stderr` (which can be aggregated by Docker)

## Development environment
- [ ] There is an image (or maybe multiple) which contains ALL tools for e.g. dependency management, compiling, debugging, testing etc. This might extend your base image.
- [ ] Different languages and thus different test and build tools run in different containers (e.g. PHP backend, JS frontend)
- [ ] There is service orchestration (e.g. [`docker-compose`](https://docs.docker.com/compose/)) that runs the development images + services it depends on like databases, key-value stores, queues etc.
- [ ] There is task orchestration (e.g. [GNU Make](https://www.gnu.org/software/make/manual/make.html))
that creates base/dev images and starts the project. 
- [ ] Source code is mounted from the local machine into the dev container so it can be compiled or hot-reloaded after a change. 
- [ ] Local configuration files/cache dirs are mounted into the dev container so it runs just as before.
- [ ] Helper scripts exist for containerized tools that are often run by hand (e.g. dependency managers, unit-testing frameworks)
- [ ] When the application has dependencies on databases etc: The application knows how to wait for services to be ready before attempting to run database migrations etc e.g. by polling them. (Tip: Symfony developers take a look at: - LIIP Monitor bundle etc.)
- [ ] When the application has dependencies on databases etc: The application runs database migrations before it starts. 
- [ ] When the application has a web interface: The application is accessible via a domain name (on standard ports like `80` or `443`)
- [ ] A pre-commit/pre-push hook is configured for version control to run tests.
 
Related info:
- [Run Test/build tools in a container](http://lucasvanlierop.nl/blog/2017/06/28/running-cli-tools-in-docker-part-1-composer/) 
- [Access your application via a proxy](http://lucasvanlierop.nl/blog/2017/06/25/accessing-your-docker-app-via-a-domain-name-using-traefik/)

## CI
- [ ] There is service orchestration similar to the one for development that runs the services required to build and test the application. 
- [ ] There is task orchestration that runs CI tasks like code inspection, tests and build/compile steps.
- [ ] Tests and code inspections are run on the source code
- [ ] The application is built/compiled
- [ ] Built version of the application is combined with the base image into an Docker image that is production ready.
- [ ] When the application has separate application run time and web server images: Backend code/binaries are packed in application run time image and front-end assets are packed with web server image.
- [ ] There is service orchestration similar to the one for development that runs the built versions of the application and the services it depends on for testing.
- [ ] There is an end-to-end test system that runs against the built application e.g. Selenium.
    - [ ] End-to-end test system knows how to determine if the application is ready to be test (fully started) e.g. by polling them with `curl --fail`.
- [ ] Production images are stored in a repository
- [ ] Production images can be deployed to a production (like) environment
- [ ] When the project is not under active development: the above process is scheduled to prevent running old (possibly vulnerable) images in production
- [ ] Images are scanned for vulnerabilities

Related info:
- https://hub.docker.com/r/selenium/standalone-chrome/

## Staging/Production
- [ ] Services have a health check configured that helps the orchestrator decide if a service should be added or removed to 
 the 'pool'.
- [ ] Services have a deployment configuration that prevent things like running concurrent database migrations.
- [ ] Services have a restart policy configured
- [ ] Sensitive configuration is stored in a secure store rather than exposed via Environment variables (e.g. [Docker Swarm Secrets](https://docs.docker.com/engine/swarm/secrets/)
- When running multiple instances of a service:
    - [ ] Persistent data like sessions are shared between instances (e.g. by using [Redis](https://redis.io/)).
- When running a cluster:
    - [ ] Files that need to be persisted are synced accross hosts ()e.g. by using [GlusterFS](https://www.gluster.org/)).- Dangling images, networks and volumes are [removed](https://docs.docker.com/engine/reference/commandline/system_prune) on a scheduled basis.
- [ ] When running a database with persistent storage: extra care is taken to maintain integrity (read the docs!)
    
## Optimized images
- [ ] Number of layers is reduced by concatenating all installation commands 
- [ ] Cache from build steps, OS package managers is removed in the same layer as the were created
- [ ] A small base image like [Alpine Linux](https://hub.docker.com/_/alpine/) is used when possible
- [ ] Documentation, tests, dev config files etc. are left out.
     

## Continuous Delivery
- Monitoring is configured to aggregate and visualize the logs from all containers e.g: with [Prometheus/Grafana](https://prometheus.io/docs/visualization/grafana/) or [ELK Stack](https://www.elastic.co/webinars/introduction-elk-stack)
- Services can be auto rolled back upon error.

# Resources

### Books

#### [The DevOps 2.1 Toolkit: Docker Swarm](https://leanpub.com/the-devops-2-1-toolkit)
 
Highly recommended: contains a lot of practical suggestions for working with Swarm, setting up clusters in a fully automated fashion. This book is full of practical suggestions for setting up continous delivery, monitoring, logging and databases.

#### [Docker in Action](https://www.manning.com/books/docker-in-action)

A proper introduction to Docker and Docker Compose.

### Articles

- [Security in a containerized world](https://docs.google.com/presentation/d/1QnakgUC8AaNydPZCmKGYYja8gs2WoHbHRSjioIVdD9g/edit)
- [Unprivileged containers](https://docs.google.com/presentation/d/1FGhm7S7bm8AwzF6eD8hb9m6cjf68Vr3ej-HzfYpGiNA/edit#slide=id.p4)
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

- [](https://nschoe.com/articles/2016-05-26-Docker-Taming-the-Beast-Part-1.html#what-is-docker)
- [Self Paced Docker Training](http://training.play-with-docker.com/)
- [BASH Programming - Introduction HOW-TO](http://tldp.org/HOWTO/Bash-Prog-Intro-HOWTO.html)

### By one of the trainers
- [Accessing your Docker app via a domain name using Træfɪk](http://lucasvanlierop.nl/blog/2017/06/25/accessing-your-docker-app-via-a-domain-name-using-traefik/)
- [Containerizing a static website with Docker](https://php-and-symfony.matthiasnoback.nl/2017/01/containerizing-a-static-website-with-docker)
- [Docker build patterns](https://php-and-symfony.matthiasnoback.nl/2017/04/docker-build-patterns)
- [Making a Docker image ready for use with Swarm Secrets](https://php-and-symfony.matthiasnoback.nl/2017/06/making-a-docker-image-ready-for-swarm-secrets)
- [Microservices for everyone](https://leanpub.com/microservices-for-everyone/)
- [Run CLI tools in a container](http://lucasvanlierop.nl/blog/2017/06/28/running-cli-tools-in-docker-part-1-composer/)
- [A Collection of 'Dockerized' PHP extensions](https://github.com/lucasvanlierop/docker-php-extensions)
