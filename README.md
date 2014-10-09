Dockerized version of [Kojak](https://github.com/sbadakhc/kojak).

## Features Specific to the Dockerized Version

* Avahi is used to publish 'koji.local' from the koji server, to make it easier to use without modifying your `/etc/hosts`. (Or, so goes the theory.)

## Development Setup

Assuming the containers haven't been pushed to your Docker register (or the hub) yet, OR you've made local changes you want to try out.

### Step 1: Build `kojak-common`

    $ cd <git-root>/kojak-common
    $ docker build --tag kojak/kojak-common .

### Step 2: Build `kojak-koji-db` (PostgreSQL DB)

    $ cd <git-repo>/kojak-koji-db
    $ docker build --tag kojak/kojak-koji-db .

### Step 3: Build `kojak-koji-server` (Monolithic Koji Install)

    $ cd <git-repo>/kojak/koji-server
    $ docker build --tag kojak/kojak-koji-server .

### Step 4: Start `kojidb` (PostgreSQL) Container

    $ docker run -P -d --name kojidb kojak/kojak-koji-db

### Step 5: Start `koji` (Server) Container

    $ docker run -P --name koji --link kojidb:kojidb kojak/kojak-koji-server

Once we have everything setup and working correctly, this should allow you to stand up a test instance of these containers.

## TODO

### Document envars for containers

1. Git repo to pull for koji sources
2. host/port for koji server