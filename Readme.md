# Heroku Secrets

## Heroku, Inc. Secrets

## "Heroku"

* Hero: the engineer that can jump in to save the day
* Haiku: simple, well-designed, elements of nature

> Cross referenced with 6 letter TLDs

## Starter Projects

* Test tech chops on real problems
* Test culture and attitude fit
* Present something to the team

> Style guide for Heroku CLI, conversion rate analysis, pylibmc

## Maker Day and Workshops

* Paul Graham's [Maker's Schedule, Managers Schedule](http://www.paulgraham.com/makersschedule.html)
* Demo new software, discuss new tools and techniques

> "nextgen framework workshops" June 2006
> "The time has come to escalate our discussion on next generation
> frameworks... From now on I would like us to always do a workshop that day.  If we
> don't have anyone who wants to do a specific presentation, then we can
> just all get together and talk about what new cool tools or techniques
> we've come across recently."

## dev@heroku.com Secrets

## Continuous Deployment

* One codebase -- many deploys
* Enable new paths via feature flags

## Culture - Twelve-Factor

* Adam Wiggin's [The Twelve-Factor App]
* Modern software design for software-as-a-service
* Heroku enforces and enables these patterns

> Codebase, dependencies, config, backing services, build release run, processes, port binding, concurrency, disposability, dev/prod parity, logs, admin processes

## Operations

## heroku.com Secrets

## Statistics

* X million deploys
* X million processes

## Build

* [Anvil](https://github.com/ddollar/anvil)
* Transport files over HTTPS
* `Heroku run` a shared-nothing build dyno
* Store slug tgz in S3

## Build

```bash
$ heroku build .
Checking for app files to sync... done, 2 files needed
Uploading: 100.0% (ETA: 0s)
Launching build process... done 
Fetching buildpack... done 
Compiling app... 
Success, slug is https://api.anvilworks.org/slugs/cd447c86-528a-4a4b-90e0-3cea29b645e8.tgz
```

> Work in progress. All in userspace, open source, make your own build and test services!
> Don't need a monolithic `git push` service

## Release

* Release API and 

```
$ curl -vX POST https://cisaurus.herokuapp.com/v1/apps/heroku-secrets/release             \
  -H "Content-Type: text/json"                                                            \
  -u ":$HEROKU_API_KEY" \
  -d '{"app":"heroku-secrets", "description":"foo", "slug_url":"https://api.anvilworks.org/slugs/cd447c86-528a-4a4b-90e0-3cea29b645e8.tgz"}'

< HTTP/1.1 202 Accepted
```

> Work in progress. Wrapper around poorly documented public API.

## Rollback

```
$ heroku releases --app heroku-secrets
=== heroku-secrets Releases
v10  foo              noah@heroku.com  2013/02/27 12:41:01 (~ 12s ago)
...

$ heroku rollback
Rolling back heroku-secrets... done, v9
 !    Warning: rollback affects code and config vars; it doesn't add or remove addons. To undo, run: heroku rollback v10

$ heroku releases
=== heroku-secrets Releases
v11  Rollback to v9   noah@heroku.com  2013/02/27 12:44:00 (~ 23s ago)
```

## Run

```
$ heroku run bash
~ $ du -sh
5.2M  .
~ $ hostname
eb34173c-733e-4d38-9b30-7f9d356fc554
~ $ echo $PORT
22728
~ $ /sbin/ifconfig | sed -n 's/.*inet addr:\([0-9.]\+\)\s.*/\1/p' | head -1
10.29.141.197
~ $ curl ifconfig.me/host
ec2-54-234-58-59.compute-1.amazonaws.com
~ $ bundle exec irb
```

## Profile Scripts

```
$ heroku run bash
Running `bash` attached to terminal... up, run.8679
Sometimes I wonder if I'm in my right mind.  Then it passes off and I'm
as intelligent as ever.
    -- Samuel Beckett, "Endgame"
~ $ 
$ cat $HOME/.profile.d/fortune
~ $ cat $HOME/.profile.d/fortune.sh 
LD_LIBRARY_PATH=/app/vendor/usr/lib /app/vendor/usr/games/fortune /app/vendor/usr/share/games/fortunes/
```

> Buildpacks should use these for appending to PATH or setting GEM_PATH, etc.

## Data

* [Heroku Postgres](https://postgres.heroku.com/)
* [Postgres 9.2](https://blog.heroku.com/archives/2012/12/6/postgres_9_2_the_database_you_helped_build)
* pg_stat_statements extension
* [Dataclips](https://dataclips.heroku.com/xqzzcwmlubhblavdipydzzqmlmbm)
* [Dataclips -> Google Docs](https://docs.google.com/a/heroku.com/spreadsheet/ccc?key=0AuBDxqx7T2vodDhfZk1YR0xIXzl2ckJRaFA5RUZjU0E&rm=full#gid=2)

> =ImportData("https://dataclips.heroku.com/xqzzcwmlubhblavdipydzzqmlmbm.csv")

## Data

```
$ heroku addons:add heroku-postgresql:dev --version=9.2
Attached as HEROKU_POSTGRESQL_COPPER_URL
$ heroku pg:psql COPPER
=> \i schema.sql
CREATE TABLE
=> create extension pg_stat_statements;
CREATE EXTENSION
=> SELECT query, calls, total_time from pg_stat_statements WHERE query != '<insufficient privilege>';
```

## Platform - Important Apps

* Bundler API - 6 web dynos, [1 datascope worker](http://datascope.herokuapp.com/), 4 databases
* Shogun - 13 process types, 200 dynos
* Codon - ~800 detached processes

