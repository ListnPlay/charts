Migrated from [https://github.com/lotuc/presto-chart](https://github.com/lotuc/presto-chart)

# Presto Chart

"[Presto](http://prestosql.io/) is a high performance, 
distributed SQL query engine for big data."

#### Why not just use stable/presto helm chart?

While stable/presto was ok, it is no longer maintained, 
and its configuration was not ideal.
This chart has the catalog as simple configuration (thanks to [locut](https://github.com/lotuc) who created the base chart) 

And is the only one that is based on [prestosql](https://github.com/prestosql/presto) (and not the prestodb from Facebook) 
so you can get the latest versions in the 3xx ranges.

And the only one using the official [prestosql/presto](https://hub.docker.com/r/prestosql/presto) docker image directly 
(sorry eliran, bivas/presto is way out of date and still uses prestodb)


## Chart Details

This chart allows installation and configuration of a basic presto cluster 
of a coordinator and workers, allowing the user to add extra catalogs as needed.


## Installing the Chart

#### Add the helm repo

`helm repo add featurefm 'https://raw.githubusercontent.com/listnplay/charts/master/'`
    
#### TLDR
 `helm install --name presto featurefm/presto`
 
 (which will not give you much since no catalog is defined for your data)

#### Configurable Install

Create and modify your own values.yaml (see [my-values.yaml](my-values.yaml)
and Configuration section below)


`helm install --name my-relese featurefm/presto -f /path/to/your/values.yaml`

## Configuration

Configurable values are documented in the `values.yaml`.

You can add new catalog like this (`values.yaml`):

(see [Catalog Docs](https://prestosql.io/docs/current//connector.html) for more)

```yaml
...
server
  ...
  catalog:
    log.properties: |
      connector.name=localfile
      presto-logs.http-request-log.location=/presto/etc/data/var/log
      presto-logs.http-request-log.pattern=*.log
    dbmysql.properties: |
      connector.name=mysql
      connection-url=jdbc:mysql://example.com:port
      connection-user=user
      connection-password=password
...
```

To upgrade the release:

```bash
$ helm upgrade --recreate-pods -f /path/to/your/values.yaml my-release featurefm/presto
```


> **Tip**: You should use [my-values.yaml](my-values.yaml) as a template

### Usage/Admin

* make sure you have access to the presto service.
  One way to do this: `kubectl port-forward svc/presto 80:8080`

You can either connect to presto with the cli or use the JDBC driver:
* CLI: Download [presto-cli-326-executable.jar](https://repo1.maven.org/maven2/io/prestosql/presto-cli/326/presto-cli-326-executable.jar), 
rename it to `presto`, make it executable with `chmod +x presto`, then run it:

    `./presto --server localhost:8080` 

* JDBC see [Driver Docs](https://prestosql.io/docs/current/installation/jdbc.html).
    Similarly Connect to `jdbc:presto://localhost:8080`
    
    
* Use SQL to test thet your catalogs are working:
   
        SHOW CATALOGS [LIKE <pattern>]
        SHOW SCHEMAS [FROM <catalog>] [LIKE <pattern>]
        SHOW TABLES [FROM <schema>] [LIKE <pattern>]
        USE [<catalog>.]<schema>