# AEM & Docker getting started guide

Getting started guide for development with [Adobe Experience Manager](https://www.adobe.com/nl/marketing-cloud/experience-manager.html) together with Docker. The configuration contains an AEM author, publisher and dispatcher environment, running in three separate containers. Docker images also have support for installing AEM packages during build.

## Prerequisites

This tutorial assumes running on a Mac. Installation on Windows might differ for certain steps. The following items are required:

- [Docker](https://www.docker.com) with at least 8GB memory allocated
- AEM installation file, named `AEM_6.5_Quickstart.jar` (other versions might work, but are not tested)
- AEM license file, named `license.properties`
- Oak runnable jar named `oak-run-*.jar`, where the `*` contains the version number. Make sure the Oak version is compatible with the AEM version.
- Recommended: [Homebrew](https://brew.sh) package manager

## Getting started: running AEM

1. Clone this repository to a local directory and put the AEM installation file, license file and Oak runnable jar file in the root.
2. Put AEM packages that need to be installed during build in `./author/packages/` and `./publisher/packages/`. Order of installation will be alphabetically, based on package file name.
3. Add license and aem quickstar jar files to root folder.
4. Build the Docker images with `docker build -t aem-base -f base/Dockerfile . && docker-compose build`. This takes a couple of minutes (or more, depending on the number of packages), as the author and publisher are started during build to be able to install packages.
5. Start the Docker containers with `docker-compose up`. This will also mount the `./logs` directory on your local system to the containers, so you have easy access to the logs of all containers.
6. Wait until AEM has fully started. To check for the author, open the [bundles page](http://localhost:4502/system/console/bundles) and when all bundle statusses are either `Active` or `Fragment` the AEM environment has fully started.
7. Navigate to [http://localhost:4502](http://localhost:4502) and you'll see a login screen. Login with username `admin` and password `admin`. Navigate to [http://localhost](http://localhost) to see the published site via the dispatcher. The publisher runs on [http://localhost:4503](http://localhost:4503).

Starting and stopping containers preserves AEM content. Images need to be rebuild when changing packages in the `packages` directories. After stopping a container, or after a system reboot, you can be quickly up-and-running again by starting the containers with `docker-compose up`.
