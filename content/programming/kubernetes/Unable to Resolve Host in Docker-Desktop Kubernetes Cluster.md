I once ran into a quite confusing situation in which a pod running in the single node Kubernetes cluster provided by Docker-Desktop was not able to connect to Amazon S3 because it could not resolve the bucket's host name. The error was something along the lines of

```
ERROR: Cannot resolve host 'some-very-long-bucket-name.s3.amazonaws.com'
```

It turns out this was caused by a bug in the CoreDNS service used by the Kubernetes cluster. In the CoreDNS pods' logs, I saw this error:

```
dns: buffer size too small
```

After some searching around, I found [this helpful github issue](https://github.com/docker/for-mac/issues/7088#issuecomment-1880516515) which suggested editing the CoreDNS's configmap to include this:

```
      ...
      forward . /etc/resolv.conf {
          force_tcp
          max_concurrent 1000
      }
      ...
```

The `force_tcp` was missing in my configuration. I made the change by manually editing the `kube-system/coredns` configmap. I suspect that if I were to recreate my Kubernetes cluster or uninstall/reinstall Docker-Desktop, I would need to do this again to fix the DNS service.

#docker #kubernetes #dns #troubleshooting