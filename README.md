# seafile-server

Helm chart for Seafile Server

[![Artifact HUB](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/dr300481)](https://artifacthub.io/packages/search?repo=dr300481)

# install

```
helm repo add seafile https://300481.github.io/charts/
helm repo update
helm upgrade --install seafile seafile/seafile --version 0.5.0 --values YOUR-VALUES.yaml
```

# upgrade

## from 0.4.4 to 0.5.0

There are major changes:

* Seafile changes from Deployment to StatefulSet with dynamic provisioned volume
* MariaDB is deployed with a Sub Helm Chart (Bitnami)
* Memcached is deployed with a Sub Helm Chart (Bitnami)

The trick is now to get the state from your deployment volumes into the new dynamic provisioned.

You can achieve this by mounting the previous volumes with extraVolumes and extraVolumeMounts in the values.yaml

for the Seafile StatefulSet and for the MariaDB Statefulset.

Additionally you must replace the container images by an image of your choice.

For MariaDB you must deactivate the livenessProbe and readinessProbe.

Then copy/move the state from the previous volumes to the new dynamic provisioned ones.

After moving the state, reset the values (container images, volume mounts, probes etc.)

and restart your application. Cross the fingers!

Don't forget to set `mariadb.auth.rootPassword` to your already existing password. Otherwise the MariaDB container will not start.

And, never ever do this directly on production!

Play with it on a test system first, monday to thursday, not friday 3pm ;-)

## from 0.3.0 to 0.4.4

nothing to do

## from 0.2.5 to 0.3.0

run `mysql_upgrade -u root -p` inside the mysql container because of upgrade from mariadb:10.1 to mariadb:10.5

# contribution

If you have any ideas, just create a pull request from your fork or open an issue here.

I will try to work on it as soon as possible, but cannot give any guarantee because this is my hobby project.

If you want to get in contact, visit my [site](https://300481.tk) and find me on social medias.
