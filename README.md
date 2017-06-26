# Dommage, Dockerised Mirage

`dommage` is a shell script that wraps the [Mirage] CLI to make use of Docker
containers. This has a handful of notable benefits:

  * you can cache the OPAM build artefacts in the container image which speeds
    up local builds;
  * by publishing said build container image, you can re-use it in Travis
    builds, speeding those up considerably; and
  * you can now easily test build `-t xen` targets on OSX.

I've tried to minimise interference with the normal operation of [Mirage] CLI so
simply replacing `mirage` with `dommage` is supposed to work. To publish the
resulting container image, `dommage publish <image>`. See [my
website][mor1-www] [Makefile] and [.travis.yml][travis-yml] for extended
examples.

Issues, comments, suggestions and bug fixes all welcome!

[mirage]: https://mirage.io
[mor1-www]: https://github.com/mor1/mor1.github.io
[makefile]: https://github.com/mor1/mor1.github.io/blob/master/Makefile
[travis-yml]: https://github.com/mor1/mor1.github.io/blob/master/.travis.yml

## Operation

To start, `dommage` provides a few management commands to manipulate the build
container:
  * `dommage init BASE-IMAGE` creates a new container, based off `BASE-IMAGE`
    from the [Docker Hub][hub]
  * `dommage publish IMAGE` commits the current container and pushes it to
    [Docker Hub][hub] as `IMAGE`
  * `dommage destroy` stops and removes the current build container
  * `dommage run ...` executes a command inside the current build container

In addition, it wraps the main [Mirage][] CLI commands:
  * `dommage configure ...` runs `mirage configure ... && make depends` inside
    the build contianer
  * `dommage build ...` runs `mirage build ...` inside the build container
  * `dommage clean ...` runs `mirage clean ...` inside the build container

[hub]: https://hub.docker.com
