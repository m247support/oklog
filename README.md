# OK Log

[OK Log](https://github.com/oklog/oklog) is a distributed and coördination-free log management system for big ol' clusters. It's an on-prem solution that's designed to be a sort of building block: easy to understand, easy to operate, and easy to extend.

## Dockerfiles

Tag, version, base image, Dockerfile link:

-	[`latest`, `v0.1.3`, `alpine:3.5` (*Dockerfile*)](https://github.com/m247suppport/oklog/blob/master/Dockerfile)
-	[`v0.1.3`, `v0.1.3`, `alpine:3.5` (*v0.1.3/Dockerfile*)](https://github.com/m247suppport/oklog/blob/master/v0.1.3/Dockerfile)
-	[`v0.1.2`, `v0.1.2`, `alpine:3.5` (*v0.1.2/Dockerfile*)](https://github.com/m247suppport/oklog/blob/master/v0.1.2/Dockerfile)
-	[`v0.2.0`, `v0.2.0`, `alpine:3.5` (*v0.2.0/Dockerfile*)](https://github.com/m247suppport/oklog/blob/master/v0.2.0/Dockerfile)

(Note that the latest denotes the most recent stable release, not the most latest release of OK Log.)

(Original Dockerfile modified from [lendico/oklog](https://hub.docker.com/r/lendico/oklog). All credits to [lendico-seong](https://github.com/lendico-seong). See notes for change requests.)

## Docker

Quick start:

Quickly create a store:

```
docker run -d \
	-p 7650:7650 \
	-p 7651:7651 \
	-p 7653:7653 \
	-p 7659:7659 \
	--name oklog \
	oklog/oklog:latest ingeststore -store.segment-replication-factor 1
```

(To mount the data directory please use `-v /path/to/dir:/data`).

Quickly forward logs (requires a binary exectuable from the [releases](https://github.com/oklog/oklog/releases)):

```
./myservice | oklog forward <ingeststore>
```

(Since v0.1.3 (latest) you can use `-prefix <tag> -prefix <label>` repeatable flag to prepend annotations to logs.)


Integrations:

Please see [Integrations](https://github.com/oklog/oklog/wiki/Integrations) within the OK Log [wiki](https://github.com/oklog/oklog/wiki).

Quick query:

```
docker exec -it oklog /bin/ash
```

```
./oklog query -stats
./oklog query -from 2h -q "e"
```


Quick stream:

Since v0.2.0 (currently unstable) you can register a steamed query to OK Log process.

Users can register a query via a long-lived HTTP/1.1 connection to an OK Log container running in store or injeststore modes, e.g.:

```
curl -iv localhost:7650/store/stream?q=e
```

Alternatively, you can run a OK Log container in stream mode:


```
docker run -d \
	--name oklogstream \
	oklog/oklog:v0.2.0 stream -store http://localhost:7650 -q "e"
```

(Be careful of log looping when pumping logs from the Docker daemon, as this will burn through storage!)

Please see [PR #34](https://github.com/oklog/oklog/pull/34) for more information.


# Notes

These are unofficial Docker builds for OK Log. All credits for OK Log should go to [peterbourgon](https://github.com/peterbourgon) and the OK Log contributors / maintainers.

Any problems, suggestions or enhancements, please raise a new issue [here](https://github.com/m247suppport/oklog/issues/new). Alternatively, alterations or additions can be included in a new PR [here](https://github.com/m247suppport/oklog/pulls). All feedback welcome.