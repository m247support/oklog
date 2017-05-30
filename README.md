# OK Log

[OK Log](https://github.com/oklog/oklog) is a distributed and coördination-free log management system for big ol' clusters. It's an on-prem solution that's designed to be a sort of building block: easy to understand, easy to operate, and easy to extend.

## Dockerfiles

Tag, version, base image, Dockerfile link:

-	[`v0.2.1`, `v0.2.1`, `alpine:3.6` (*v0.2.1/Dockerfile*)](https://github.com/m247suppport/oklog/blob/master/v0.2.1/Dockerfile)
-	[`v0.2.0`, `v0.2.0`, `alpine:3.6` (*v0.2.0/Dockerfile*)](https://github.com/m247suppport/oklog/blob/master/v0.2.0/Dockerfile)
-	[`latest`, `v0.1.3`, `alpine:3.6` (*Dockerfile*)](https://github.com/m247suppport/oklog/blob/master/Dockerfile)
-	[`v0.1.3`, `v0.1.3`, `alpine:3.6` (*v0.1.3/Dockerfile*)](https://github.com/m247suppport/oklog/blob/master/v0.1.3/Dockerfile)
-	[`v0.1.2`, `v0.1.2`, `alpine:3.6` (*v0.1.2/Dockerfile*)](https://github.com/m247suppport/oklog/blob/master/v0.1.2/Dockerfile)

(Note that the latest denotes the most recent stable release, not the most recent release of OK Log.)

(Original Dockerfile modified from [lendico/oklog](https://hub.docker.com/r/lendico/oklog). All credits to [lendico-seong](https://github.com/lendico-seong). See notes for change requests.)

## Docker

Quickly create an ingeststore:

```
docker run -d \
	-p 7650:7650 \
	-p 7651:7651 \
	-p 7653:7653 \
	-p 7659:7659 \
	--name oklog_ingest_store \
	oklog/oklog:latest ingeststore -store.segment-replication-factor 1
```

(To mount the data directory please use `-v /path/to/dir:/data`).

Please see the OK Log [README.md](https://github.com/oklog/oklog/blob/master/README.md) for more advanced and useful deployments.

## Forward logs

Quickly forward logs (requires a binary exectuable from the [releases](https://github.com/oklog/oklog/releases)):

```
tail -f /var/log/*.log | oklog forward tcp://<store/ingeststore>:7651
```

(Since v0.1.3 (latest) you can use the `-prefix <tag> -prefix <label>` repeatable flag to prepend annotations to logs.)

### Integrations

Quickly forward docker daemon logs:

```
docker run -d \
	--volume=/var/run/docker.sock:/var/run/docker.sock \
	--name logspout \
	gliderlabs/logspout syslog+tcp://<store/ingeststore>:7651
```

Please see [Integrations](https://github.com/oklog/oklog/wiki/Integrations) within the OK Log [wiki](https://github.com/oklog/oklog/wiki).

### Query

Quick query:

```
docker exec -it oklog /bin/ash
./oklog query -stats
./oklog query -from 2h -q "e"
```
Or you can query a store or ingeststore with binary from [releases](https://github.com/oklog/oklog/releases):
```
oklog query -store tcp://<store/ingeststore>:7650 -from 1h -q "e"
```

### Stream

Since v0.2.0 (unstable) you can register a steamed query via a long-lived HTTP/1.1 connection to a OK Log process running in store or ingeststore modes:

```
curl -iv <store/ingeststore>:7650/store/stream?q=e
```

Or you can stream with binary executable from [releases](https://github.com/oklog/oklog/releases):

```
oklog stream -store <store/ingeststore>:7650 -from 1h -q "e"
```

(A note on running an OK Log container in stream mode: Be careful of log looping when pumping logs from the Docker daemon on the same host, as this will burn through storage!)

Please see [PR #34](https://github.com/oklog/oklog/pull/34) for more information.

### UI


Since v0.2.1 you can view a prototype web UI at `http://<store/ingeststore>:7650/ui/`.

# Notes

This represents a set of unofficial Dockerfiles and images containing the OK Log binary. All credits for OK Log should go to [peterbourgon](https://github.com/peterbourgon) and the OK Log contributors / maintainers.

If you have any problems with OK Log, then please raise an issue [here](https://github.com/oklog/oklog/issues/new). Any suggestions or enhancements to do with the Dockerfiles or images, then please raise a new issue [here](https://github.com/m247suppport/oklog/issues/new). Alternatively, alterations or additions can be included in a new PR [here](https://github.com/m247suppport/oklog/pulls). All feedback is welcome.
