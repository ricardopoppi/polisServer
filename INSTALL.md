# PolisServer install guide

This instructions assume you are running Debian Stretch.

Dependencies:

* postgresql server
* curl
* npm >= 3.3.8
* git
* mongodb server

Run as root:

   # apt-get install npm curl git postgresql postgresql-server-dev-all libkrb5-dev mongodb

Following the [nodejs v6.x](https://github.com/nodesource/distributions#deb)
run as root.

   # curl -sL https://deb.nodesource.com/setup_6.x | bash -
   # apt-get install -y nodejs

Then install dependencies listed on `package.json` with npm.

   $ npm install

Create the [Intercom API
token](https://app.intercom.io/a/apps/_/settings/personal-access-token).
Follow [these
instructions](https://developers.intercom.com/docs/personal-access-tokens) and
create an Intercom APP ID and ACCESS TOKEN, copy these values to `.env_dev`
file.

Create the postgresql database:

    $ sudo su - postgres
    $ createuser -d -R -S -P -E polisserver (set password as: polis)
    $ createdb -O polisserver polisserver

Check if database connection is ok:

    $ psql -h localhost -U polisserver -W polisserver (password: polis)
    polisserver=> \q

Create the database tables (execute SQL file twice):

    $ psql -h localhost -U polisserver -W polisserver < postgres/db_setup_draft.sql
    $ psql -h localhost -U polisserver -W polisserver < postgres/db_setup_draft.sql

Run polisServer:

    $ . .env_dev
    $ nodejs --max_old_space_size=400 --gc_interval=100 --harmony app.js
