# dokku-rethinkdb-plugin

This project provides a plugin for [Dokku][] to create linked [RethinkDB][]
containers for apps.

[Dokku]: https://github.com/progrium/dokku
[RethinkDB]: http://www.rethinkdb.com/

## Installation

```bash
cd /var/lib/dokku/plugins
git clone https://github.com/stuartpb/dokku-rethinkdb-plugin rethinkdb
dokku plugins-install
```

## Commands

```
$ dokku help
    rethinkdb:create <app>          Create a RethinkDB container
    rethinkdb:delete <app>          Delete specified RethinkDB container
    rethinkdb:info <app>            Display RethinkDB container information
    rethinkdb:logs <app>            Display last logs from RethinkDB container
```

## Simple usage

Create a new container for a named app:

```
$ dokku rethinkdb:create foo # or "ssh dokku@server -t" client-side

-----> Creating new container: rethinkdb/foo
```

Deploy an app to that name (client side):

```
$ git remote add dokku dokku@server:foo
$ git push dokku master
Counting objects: 155, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (70/70), done.
Writing objects: 100% (155/155), 22.44 KiB | 0 bytes/s, done.
Total 155 (delta 92), reused 131 (delta 80)
remote: -----> Building foo ...
remote:        Ruby/Rack app detected
remote: -----> Using Ruby version: ruby-2.0.0

... blah blah blah ...

remote: -----> Deploying foo ...
remote:
remote: -----> Starting linked container rethinkdb/foo ...
remote:        RETHINKDB_HOST: 172.16.0.104
remote:        RETHINKDB_PORT: 49187
remote:
remote: -----> Deploy complete!
remote: -----> Cleaning up ...
remote: -----> Cleanup complete!
remote: =====> Application deployed:
remote:        http://foo.server
```

These steps can be done in either order - if an app has been pushed to the
server, "rethinkdb:create" will re-deploy it with the RethinkDB container
linked.

## Advanced usage

Unlink and delete a RethinkDB container:

```
dokku rethinkdb:delete foo
```

RethinkDB logs (per container):

```
dokku rethinkdb:logs foo
```

RethinkDB information:

```
dokku rethinkdb:info foo
```
