###############################################################################
Docker Compose
###############################################################################

- `Subcommands short overview`_
- `docker-compose.yml`_
- `Install/Uninstall`_
- `Difference between up/run/start`_



===============================================================================
Subcommands short overview
===============================================================================
- CLI reference:
  https://docs.docker.com/compose/reference/
- CLI environment variables:
  https://docs.docker.com/compose/reference/envvars/


``docker-compose [SUBCOMMAND] --help``
    More information on a command.

``docker-compose images``
    List images used by the created containers.
``docker-compose top``
    Display the running processes.

``docker-compose config``
    Validate and view the Compose file.

``docker-compose build``
    Builds or rebuilds services (images).
``docker-compose create`` (**v1.17+: This command is deprecated.**)
    Use the ``up`` command with ``--no-start`` instead. Creates containers for
    a services (images), EXCEPT NETWORKS, so use ``up``.

``docker-compose start``
    Start existing containers. Don't create any new containers.
``docker-compose stop``
    Stop running containers without removing them.

``docker-compose up``
    Build the images if required and start the containers, also starts any
    linked services.
``docker-compose up --no-deps``
    Don't start linked services.
``docker-compose up --no-start`` (**v1.17+**)
    Don't start the services after creating them.
``docker-compose up -d``
    Starts the containers in the background and leaves them running,
    print new container names.
``docker-compose down``
    Stops containers and removes containers, networks, volumes, and images
    created by ``up``. Networks and volumes defined as ``external`` are never
    removed.



===============================================================================
docker-compose.yml
===============================================================================

- Compose file version 3 reference:
  https://docs.docker.com/compose/compose-file/

- Compose file versions and upgrading:
  https://docs.docker.com/compose/compose-file/compose-versioning/




===============================================================================
Install/Uninstall
===============================================================================

For the latest version check the Compose repository release page on GitHub:
https://github.com/docker/compose/releases

Download the Docker Compose binary::

    sudo curl -L https://github.com/docker/compose/releases/download/1.17.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

Apply executable permissions to the binary::

    sudo chmod +x /usr/local/bin/docker-compose

Optionally, install command completion for the bash to
``/etc/bash_completion.d/``::

    sudo curl -L https://raw.githubusercontent.com/docker/compose/1.17.1/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose

Test the installation::

    docker-compose --version

To uninstall::

    sudo rm /usr/local/bin/docker-compose



===============================================================================
Difference between up/run/start
===============================================================================

Typically, you want ``docker-compose up``. Use ``up`` to start or restart all the
services defined in a ``docker-compose.yml``. In the default “attached” mode,
you’ll see all the logs from all the containers. In “detached” mode (-d),
Compose exits after starting the containers, but the containers continue to run
in the background.

The ``docker-compose run`` command is for running “one-off” or “adhoc” tasks.
It requires the service name you want to run and only starts containers for
services that the running service depends on. Use ``run`` to run tests or
perform an administrative task such as removing or adding data to a data volume
container. The ``run`` command acts like ``docker run -ti`` in that it opens an
interactive terminal to the container and returns an exit status matching the
exit status of the process in the container.

The ``docker-compose start`` command is useful only to restart containers that
were previously created, but were stopped. It never creates new containers.
