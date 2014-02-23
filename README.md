# dokku-rethinkdb-plugin

This project provides a plugin for [Dokku][] to create linked [RethinkDB][]
containers for apps.

[Dokku]: https://github.com/progrium/dokku
[RethinkDB]: http://www.rethinkdb.com/

## Features

- **No dependencies** (other than a recent development version of Dokku.)
  *Wow!*
- **Deletes cleanly with apps.** *Oh my!*
- **Survives reboots, cleanups, and redeploys.** *Holy cow!*
- **Can add before or after pushing an app.** *Am I dreaming?*
- **Allows access to admin ports through binding to host interfaces.** *Is this
  real life?*

![Vince McMahon reacts to dokku-rethinkdb-plugin](http://i.imgur.com/ef28TQS.gif)

**dokku-rethinkdb-plugin**: The [Gary Strydom][] of Dokku datastore plugins.

[Gary Strydom]: http://forum.bodybuilding.com/showthread.php?t=157841203&s=714b6c7c5685fc9feb106e0680a2fa1e&p=1155029043&viewfull=1#post1155029043

## Installation

```bash
cd /var/lib/dokku/plugins
git clone https://github.com/stuartpb/dokku-rethinkdb-plugin rethinkdb
dokku plugins-install
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
remote: -----> Starting linked container rethinkdb_foo ...
remote:        Container ID: 99797e35d7b9
remote:        Container IP: 172.16.0.104
remote: -----> Deploy complete!
remote: -----> Cleaning up ...
remote: -----> Cleanup complete!
remote: =====> Application deployed:
remote:        http://foo.server
```

These steps can be done in either order - if an app has been pushed to the
server, "rethinkdb:create" will re-deploy it with the RethinkDB container
linked.

The container will be exposed in the config/app environment as `RETHINKDB_HOST`
and `RETHINKDB_PORT`, as well as `RDB_HOST` and `RDB_PORT` (the names used by
the RethinkDB example apps).

## Advanced usage

*All of these commands are detailed when running `dokku help`.*

Bind a port on the host to the Web Admin UI for a RethinkDB container:

```
dokku rethinkdb:bind-webui <app> [host port]
```

Bind a port on the host to the cluster port (as used by the RethinkDB CLI) for
a RethinkDB container:

```
dokku rethinkdb:bind-cluster <app> [host port]
```

Release a RethinkDB container's bindings to host ports (does not affect links
to the associated app):

```
dokku rethinkdb:unbind <app>
```

Unlink and delete the RethinkDB container and data for an app:

```
dokku rethinkdb:delete <app>
```

Read app RethinkDB container logs:

```
dokku rethinkdb:logs <app>
```

Get RethinkDB container ID, IP, and any bound ports:

```
dokku rethinkdb:info <app>
```

(Re)start a RethinkDB container independently of a running app:

```
dokku rethinkdb:start <app>
```

Stop an app's running RethinkDB container:

```
dokku rethinkdb:stop <app>
```

(Re)set environment variables to direct to a linked RethinkDB container:

```
dokku rethinkdb:link <app>
```
