<!-- README.md generated by helm-docs from README.md.gotmpl -->
# ziti-browzer-bootstrapper

![Version: 0.0.1](https://img.shields.io/badge/Version-0.0.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 0.61.1](https://img.shields.io/badge/AppVersion-0.61.1-informational?style=flat-square)

Deploy OpenZiti browZer bootstrapper as kubernetes service

## Add the OpenZiti Charts Repo to Helm

```bash
helm repo add openziti https://docs.openziti.io/helm-charts/
```

## Minimal Installation

This chart deploys a pod running `ziti-browzer-bootstrapper`, the [OpenZiti browzer bootstrapper](https://github.com/openziti/ziti-browzer-bootstrapper/).

After adding the charts repo to Helm then you may install the chart.

```bash
helm install \
  --namespace ziti-browzer-bootstrapper --create-namespace --generate-name \
  openziti/ziti-browzer-bootstrapper \
    -f browzer-values.yaml
```

A `browzer-values.yaml` file with some samples values:

```yaml
zitiBrowzer:
  bootstrapper:
    logLevel: debug
    host: browzer.[mydomain.tld]
    targets:
      - vhost: test1.browzer.[mydomain.tld]
        # Service name to connect to
        service: ziti-service
        path: /
        scheme: http
        idp_issuer_base_url: https://[my.idsp.issuer.url/with/path]
        idp_client_id: [clientId]
  runtime:
    logLevel: debug
    # see https://openziti.discourse.group/t/browzer-setup-error-1014-origintrial-subdomain-mismatch/2481
    # get token from https://developer.chrome.com/origintrials/#/register_trial/1603844417297317889
    originTrailToken: [originTrailToken]
  controller:
    host: ziti-controller-client.openziti
    port: 443
  loadBalancer:
    # TODO: validate this - maybe set to zitiBrowzer.bootstrapper.host ?
    host: [mydomain.tld]

ingress:
  ingressClassName: nginx
  tlsSecret: [tlsSecretName]

# This is a workaround for
extraVolumeMounts:
- name: tmp
  subPath: log
  mountPath: /home/node/ziti-browzer-bootstrapper/log
extraVolumes:
- name: tmp
  emptyDir: {}

```

<!-- README.md generated by helm-docs from README.md.gotmpl -->