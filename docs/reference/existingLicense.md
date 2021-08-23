---
title: Mount Existing Product License
---
## Mount Existing Product License

You can pass the license file to a container via mounting to the container's `/opt/in` directory.

Note: If you have provided license files via the volume mount and a DevOps User/Key, it will ignore the DevOps User/Key.

The `/opt/in` directory overlays files onto the products runtime filesystem, the license must be named correctly and mounted in the following locations for new deployments.

### License File Mount Paths

|  Product | File Name  |  Mount Path |
|---|---|---|
| **PingFederate**  | pingfederate.lic  |  /opt/in/instance/server/default/conf/pingfederate.lic |
| **PingAccess** | pingaccess.lic  | /opt/in/instance/conf/pingaccess.lic  |
| **PingDirectory** | PingDirectory.lic  | /opt/in/pd.profile/server-root/pre-setup/PingDirectory.lic  |
| **PingDataSync** | PingDirectory.lic  | /opt/in/instance/PingDirectory.lic  |
| **PingAuthorize** | PingAuthorize.lic  | /opt/in/instance/PingAuthorize.lic  |
| **PingAuthorize PAP** | PingAuthorize.lic  | /opt/in/instance/PingAuthorize.lic  |
| **PingCentral** | pingcentral.lic  | /opt/in/instance/conf/pingcentral.lic  |

### Helm

Create a Kubernetes secret from the license file

```sh
kubectl create secret generic pingfederate-license \
  --from-file=./pingfederate.lic
```

Add the secretVolumes within your values.yaml deployment file

```yaml
pingfederate-admin:
  ...
  secretVolumes:
    pingfederate-license:
      items:
        pingfederate.lic: /opt/in/instance/server/default/conf/pingfederate.lic
```

### Kubernetes Manifest

Create a Kubernetes secret from the license file

```sh
kubectl create secret generic pingfederate-license \
  --from-file=./pingfederate.lic
```

Then mount it to the pod

```yaml
spec:
  containers:
  - name: pingfederate
    image: pingidentity/pingfederate
    volumeMounts:
      - name: pingfederate-license-volume
        mountPath: "/opt/in/instance/server/default/conf/pingfederate.lic"
        subPath: pingfederate.lic
  volumes:
  - name: pingfederate-license-volume
    secret:
      secretName: pingfederate-license
```

