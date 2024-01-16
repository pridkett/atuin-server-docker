atuin-server-docker
===================

Patrick Wagstrom &lt;160672+pridkett@users.noreply.github.com&gt;

January 2024

Overview
--------

Sometimes things are a lot more annoying than they appear. That was the case with trying to set up a simple atuin server for use on Raspberry Pi machines. Primary challenges were as follows:

* `atuin` is not available as a binary for Raspberry Pis
* The installation script for `atuin` does not work for Raspberry Pis
* When the installation script attempts to install rust, it breaks on Raspberry Pis
* The docker containers for `atuin` are not availabe for Raspberry Pis
* The base containers to build your own container are not available for Raspberry Pis
* Even once I got over all that, it still didn't like my Let's Encrypt certificate, which turned out to be PKCS#1 instead of PKCS#8

Luckily, this should get over all of that. At the expense of this setup being Raspberry Pi specific right now.

Usage
-----

First, you'll need to set some variables in your `.env` file. Specifically, these will be `ATUIN_DB_USERNAME`, `ATUIN_DB_NAME`, and `ATUIN_DB_PASSWORD`. These are used by both the Postgres instance and atuin in order to connect to the database.

Next, you'll want to customize `config/atuin/server.toml` according to the [atuin self-hosting docs](https://atuin.sh/docs/self-hosting/). Specificaly, pay attention to `open_registration`, `host`, and setting up the `tls` section if needed. Here you can see that I've got TLS working.

```bash
docker compose up --build
```

The first time you run this it will take a VERY VERY long time. Why? Because Raspberry Pis are very slow and compiling on them is super duper mega slow - especially for the 500+ packages that get compiled when compiling `atuin` for the first time.

After this point, you can configure your `sync_address` in `~/.config/atuin/config.toml` and you should be able to register and start syncing your stuff across machines.

Bonus Feature - Statically Linked atuin Binary for Raspberry Pis
----------------------------------------------------------------

I spent some extra time to make it so the binary your get out of this is a statically linked binary. This means it should work on your base image of your Raspberry Pi, which means that now you've got a way to run `atuin` on your Raspberry Pi interactive sessions, instead of just using this as a server.

```bash
sudo docker cp atuin_atuin_1:/usr/local/bin/atuin /usr/local/bin
```

License
-------

Copyright (c) 2024 Patrick Wagstrom

Licensed under terms of the MIT License
