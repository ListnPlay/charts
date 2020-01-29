Forked from [https://github.com/helm/charts/tree/master/incubator/druid](https://github.com/helm/charts/tree/master/incubator/druid)

# Apache Druid

[Apache Druid](https://druid.apache.org/) is a high performance analytics data store for event-driven data.

#### Why not just use incubator/druid helm chart?

While the official chart is a great start (thanks to maver1ck, the maintainer) 
and we shamelessly copied many parts of it,
the official chart is not up to date with the newer version of druid,
and it does not support the apache-druid official docker image.

So we made some changes to match apache-druid 0.16+

## Install Chart

Add the featurefm helm repo:

```
helm repo add featurefm 'https://raw.githubusercontent.com/listnplay/charts/master/'
```

Create a `values.yaml` and make your modifications. 

**Note** The chart will not work without *deep storage* defined (e.g. in s3 bucket). 
See [my-values.yaml](my-values.yaml) file for example 


To install the Druid Chart into your Kubernetes cluster:

```bash
helm install --namespace "bi" --name "druid" featurefm/druid -f values.yaml
```

After installation succeeds, you can get a status of Chart

```bash
helm status "druid"
```

If you want to delete your Chart, use this command:

```bash
helm delete  --purge "druid"
```

### Services

This chart exposes the Druid services as kubernetes services:

- druid-router - the new druid management console
- druid-broker - for queries
- druid-coordinator (+Overlord)
- druid-historical
- druid-middle-manager (+Peons)

and also:
- druid-turnilo - [Allegro](https://github.com/allegro/turnilo)'s 
cool data exploration and visualization UI for druid.
(Note this is not a druid service and hence optional)


### Druid configuration

Druid configuration can be changed by supplying environment variables to the Docker container.
The `env` section in the `values.yaml` can be generic (for all) or service specific, 
and are converted (on each container startup)
to the configuration file of the service's `runtime.properties`



See the
[Druid Docker entry point](https://github.com/apache/druid/blob/master/distribution/docker/druid.sh)
of the official docker image for more details.

### Middle Manager and Historical Statefulset

_Middle Managers_ and _Historicals_ use a StatefulSet.
Persistence is enabled by default.

**Note** that while the stateful services can retrieve their copy of the data from the deep storge 
if restarted without persistence,
such recovery will take considerable time to complete.


## Helm chart Configuration


Full and up-to-date documentation can be found in the comments of the [values.yaml](values.yaml) file.
