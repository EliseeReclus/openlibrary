# Welcome to the new Docker based Open Library development environment!

These current Dockerfiles are designed to be an alternative to the previous Vagrant based development environment.
The setup process and scripts are designed to *NOT* conflict with exisiting Vagrant provisioning, so you should be able to
chose to develop using the exisiting Vagrant method, or try the new Docker approach if you prefer, using the same code branch.

## Setup/Teardown Commands

All commands are from the docker directory:

```bash
# build images
docker build -t olbase:latest -f Dockerfile.olbase ..
docker build -t oldev:latest -f Dockerfile.oldev ..
docker build -t olsolr:latest -f Dockerfile.olsolr ..

# start the app
docker-compose up # Ctrl-C to stop
docker-compose up -d # detached (silent) mode

# stop the app
docker-compose down

# start specific service
docker-compose up --no-deps -d solr

# remove all volumes (i.e. reset all databases); perform WHILE RUNNING
docker-compose stop
docker-compose rm -v
```

Note: You must build `olbase` first before `oldev`. Currently (August 2018) the division is a bit arbitrary. More should be moved into `olbase` once we clarify
the production deployment requirements. Currently these docker images are only intented for development environments.

This exposes the following ports:

Port | Service
---- | -------
8080 | Open Library main site
7000 | Infobase
8983 | Solr

To access Solr admin:
http://localhost:8983/solr/admin/

If you are using Docker Toolbox on Windows, use the Docker Machine IP instead of `localhost`. For example, `192.168.99.100:8080`. To find the IP address, use the command `docker-machine ip`

You can customise the host ports by modifying the `-p` publish mapping in the `docker run` command to suit your development environment.

## Useful Runtime Commands

See the docs for more: https://docs.docker.com/compose/reference/overview

```bash
# Read a service's logs (replace `web` with service name)
docker-compose logs web # Show all logs (onetime)
docker-compose logs -f --tail=10 web # Show last 10 lines and follow

# Analyze a container
docker-compose exec web bash # Launch terminal in `web` service
```

## TODO
* Add dev volume mount to `run` command, and provide instructions for refreshing the application.
* Fix symlinks that are causing a problem in Windows, see issue https://github.com/internetarchive/openlibrary/issues/1051 

## Run tests in a temporary container
`docker-compose run --rm web make test`
