The Dcycle Method
=====

Introduction
-----

The Dcycle method defines a development method. If you are using the Dcycle method for your project, you can enter this into your project's README.md file:

> Using the [Dcycle method](https://github.com/dcycle/dcycle-method) version x.

Each bullet point in this document has the method version in parentheses, for example `(v1-...)` means version 1 and over, `(v1-v3)` means version 1 to 3, etc. If this method does not suit your needs, feel free to fork this code and create your own.

Contents
-----

* Deployment
* Definition of done

Deployment
-----

* D.010 (v1-...) All deployment should be in Docker containers, except if done to managed hosting such as Pantheon or Acquia, or an existing legacy hosting system, in which case the project docker containers should mimic as best they can the production environment.
* D.015 (v1-...) Environment variables should be in a file or files in ~/.MYPROJECTNAME/ (project name should be similar to your repo name), with examples in PROJECT_ROOT/examples/env_files, and a PROJECT_ROOT/examples/env_files/README.md which explains how to set up the env_file. The deployment script (see D.050) should provide instructions on how to set up environment files if they do not exist; or it should be done automatically.
* D.020 (v1-...) The canonical deployment method, on dev, stage, or production (notwithstanding the above) should use Docker-compose and Docker containers exclusively. In cases where certain users cannot use Docker, for example if it's too slow on Mac OS, and prefer to use an alternative means such as DrupalVM or MAMP, the only supported method remains Docker.
* D.030 (v1-...) Docker-compose is used to set up the container structure.
* D.040 (v1-...) Target environments are: the dev environment, always applicable; the stage and production environments are applicable to websites, not projects such as Drupal modules.
* D.050 (v1-...) The README.md file should reference project environments if applicable: legacy, stage, prod, git, issue queue, continuous integration.
* D.040.010 (v1-...) The dev environment, deployed using `./scripts/deploy.sh dev`, should include all tools or information required or useful for development, such as the devel module for Drupal projects. The dev environment can contain unsafe passwords, for example for logging into a system.
* D.040.015 (v1-...) If a stage and production environment exist, a project Docker registry is required to store snapshots of images. 
* D.040.020 (v1-...) The stage environment, if applicable, should contain only tools or information required for production, and is built from scratch. For example, if your projects uses a base image and installs components via Composer, this is done when the stage environment is created or updated via `./scripts/deploy.sh up stage`. The Docker image(s) built during the stage deployment process should be sent to the Docker registry using the tag <branch>.
* D.040.025 (v1-...) A deployment system such as Jenkins should be available to deploy project branches to the stage environment. 
* D.040.027 (v1-...) It should be possible to see the current branch and commit number on the stage environment. 
* D.040.030 (v1-...) The production environment, if applicable, should be built by `./scripts/deploy.sh up prod` from Docker images taken from the stage environment and stored in the Docker registry using the tag <master>.
* D.050 (v1-...) `./scripts/deploy.sh up dev|stage|prod` is the only accepted means to deploy a project. `./scripts/deploy.sh down dev|stage|prod` or `./scripts/deploy.sh destroy dev|stage|prod` is the only accepted means to destroy an environment, the former keeps data and the latter destroys data.
* D.060 (v1-...) backups of production data, if applicable, should be performed daily by a continuous integration system, documented in the project README.md file.
* D.070 (v1-...) security updates must be automated.
* D.075 (v1-...) for local development, ports can be mapped to host ports (random if possible). For production, no ports are used, we will use [a reverse proxy to manage traffic](http://blog.dcycle.com/blog/170a6078/letsencrypt-drupal-docker/). For stage, a reverse proxy is used for acceptance testing, and ports are used for automated CI testing with no reverse proxy.
* D.080 (v1-...) new files should be linted.
* 0.b745d256 (draft) it should be possible to export data to a human-readable format, then re-import it.
* 0.a043cb0a (draft) it should be possible to rebuild any server and test that it works, using one command (for example a prod server may have two projects, and a reverse proxy).

Definition of done
-----

Each modification to the codebase must be manually and/or automatically reviewed with the following requirements.

* DOD.005 (v1-...) The version of the development method should be referenced, with a link to it, in the project README.md file.
* DOD.010 (v1-...) Directories, including the root directory, should contain README.md documents. Exceptions are directories which follow a standards tree structure.
* DOD.020 (v1-...) All changes to the code must be done in pull requests on separate branches from master.
* DOD.030 (v1-...) Pull request branches should be merged into master using the Squash method for clean git history.
* DOD.040 (v1-...) Pull requests with new features should contain at least one test. Pull requests which refactor features should not break existing tests. Unit tests should be favored (for javascript and PHP code), with a smaller number of GUI tests as per the principle of the "test pyramid".
* DOD.050 (v1-...) Tests need to pass on a continuous integration server whose address is accessible to developers at an address in the project README.md document
* DOD.060 (v1-...) No passwords or unique keys in code, with the exception of badge tokens such as the CirclCI badge in the README.md file.
* DOD.070 (v1-...) Deployment cannot require manual steps (for example, we need to install Drupal in the command line, not using the GUI).
* DOD.080 (v1-...) Deployment must be done within Docker containers, with no need to any additional software to servers other than Docker and Docker-compose.
* DOD.090 (v1-...) Containers should be immutable; that is, destroying and recreating a container should have no effect on the application.
* DOD.100 (v1-...) Initial and incremental deployments should happen in single command, `./scripts/deploy.sh up dev|stage|prod`, documented in the README.md file; the user should obtain everything needed in the output.
* DOD.110 (v1-...) Anything not required on production should be only on dev: ./deploy.sh can use a dev Dockerfile which is based on the production Dockerfile. For example, in the case of Drupal, Dockerfile-dev might contain a command to download and install the devel module.
* DOD.120 (v1-...) Code needs to be understandable just by reading it with minimal context. This may require extensive commenting.
* DOD.130 (v1-...) Relative paths need to be ./in/this/format, not/this/format.
* DOD.140 (v1-...) Tests on the CI server should run in 20 minutes or less.
* DOD.150 (v1-...) The master branch should always be-to-date with the production environment, if applicable. This should eliminate the need for hotfix branches.

