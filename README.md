# bordel

Helper scripts to manage, build, and package OpenXT builds.

## Setting up a Docker container

NOTE: these instructions assume that you already have a basic build tree set up.
Follow the Quick Start instructions at https://github.com/apertussolutions/openxt-manifest/
to set this up.

- Use `./bordel docker list` and note the desired Dockerfile name.
- Run `./bordel docker create {my_container_name} {Dockerfile_name}` to create the container.

## Building OpenXT with Docker

- First, the build must be configured with `./bordel config [-t template_name] [-b branch]`.
    * The available template names can be found in `./templates` (the template names match the
      directory names in this dir).
    * You can optionally provide a build id using `./bordel -i BUILD_ID config ...`; the default
      build id is YYMMDD, today's date. Note that this build id must be passed in for the rest
      of the steps if you elect to use your own.
- Next, generate your certs with `./bordel [-i BUILD_ID] certs`.
- To run the full build, run `./bordel [-i BUILD_ID] docker build`.
- You can optionally run partial build steps manually from inside the container:
    * Use `./bordel docker enter {my_container_name} [user]` to enter the container as user [user].
      The default user is `build`.
    * Navigate to `/home/<user>/openxt/<build-BUILD_ID>` to access your build tree.
    * Source the build_env file to get access to necessary OE variables (`. build_env`)
    * Build a single package by running `MACHINE=<machine_name> bitbake <package_name>`
    * You can run the full build if desired by running `~/openxt/openxt/bordel [-i BUILD_ID] build`
