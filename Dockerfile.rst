###############################################################################
Dockerfile
###############################################################################

- Dockerfile reference:
  https://docs.docker.com/engine/reference/builder/

- Best practices for writing Dockerfiles:
  https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/



ENTRYPOINT & CMD
===============================================================================

The ``ENTRYPOINT`` specifies a command that will always be executed when the
container starts. It makes your dockerized application behave like a binary.
You can pass arguments to the ``ENTRYPOINT`` during docker run and not worry
about it being overwritten (unlike ``CMD``). 

Use ``ENTRYPOINT`` if you don't want developers to change the executable that
is run when the container starts. You can think of your container as an
"executable wrapper". A good strategy is to define a "stable" combination of
executable plus parameters as the ``ENTRYPOINT``. Then you can (optionally)
specify a default ``CMD`` that developers can easily override.

.. code-block:: dockerfile

    FROM alpine
    ENTRYPOINT ["ping"]
    CMD ["www.google.com"]

Use only ``CMD`` (with no ``ENTRYPOINT``) if you want developers the ability to
easily override the executable that is being run. If entrypoint is defined you
can still override the executable using ``--entrypoint``, but it is a much
easier for developers to append the command they want at the end of docker run.

.. code-block:: dockerfile

    FROM alpine
    CMD ["ping", "www.google.com"]



EXPOSE-ing ports
===============================================================================

With Dockerfiles you have the ability to map the private and public ports,
however, you should never map the public port in a Dockerfile. By mapping to
the public port on your host you will only be able to have one instance of your
dockerized app running.

.. code-block:: dockerfile

    # Private and Public mapping
    EXPOSE 80:8080

    # Private only
    EXPOSE 80

If the consumer of the image cares what public port the container maps to they
will pass the ``-p`` option when running the image, otherwise, docker will
automatically assign a port for the container.
