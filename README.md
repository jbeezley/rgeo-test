# resonantgeo

## Quick Start Instructions

```bash
git clone git://github.com/kitware/resonantgeo
cd resonantgeo
git submodule update --init --recursive
cd devops/docker

# build options
export MAKE_PARALLELISM="-j1" # default: -j4

# runtime options
export GIRDER_WORKER_CONCURRENCY="1"   # default: 4
export GIRDER_ADMIN_NAME="myadminname" # default: admin
export GIRDER_ADMIN_PASS="myadminpass" # default: nimdaredrig
export GIRDER_USER_NAME="myusername"   # default: girder
export GIRDER_USER_PASS="myuserpass"   # default: redrig
export GIRDER_PORT="10080"             # default: 8080

docker-compose up -d # will build images if necessary
docker-compose logs -f provision
```

The last command will show the progress of the `provision` service, which is a
service that runs after the web server and database come up.  The `provision`
service provisions the web server using a post-install routine, and then exits
cleanly.  You can tell when the routine is complete by observing a log entry
resembling the one below:

```
provision_1  |
provision_1  | Done.
provision_1  | PROVISION COMPLETE
resonantgeo_provision_1 exited with code 0
```

You will also notice that the container is listed as no longer running.
(`docker-compose ps`).

At this time, you should be able to point your browser to the host running the
docker stack and the port designated for the web server
(e.g.: `localhost:10080`).  You should be greated with the Minerva UI.

## Making Changes

For now, changes you make to the source code under `modules` or to the
Dockerfile under `devops/docker/assets` are applied to the running environment
by rebuilding the main docker image and recreating the corresponding containers.

```bash
# open your favorite text editor and make changes...

docker-compose build # will rebuild the main docker image
docker-compose up -d # will recreate the web, worker, and provision containers.
```

Again, when the provisioner completes, you should be able to browse to your
host's site and your changes should be reflected in the application.

Coming soon: check back for updates that will accomodate live code editing
features.  These updates will allow you to work on the code and have your
changes automatically reflected in the application environment without having to
rebuild images or recreate containers.
