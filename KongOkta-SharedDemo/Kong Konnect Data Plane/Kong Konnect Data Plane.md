# Kong and Okta Demo Environment Control Planes

To get access to both Control Planes please use the following links:

### Okta


[https://demo-kong.oktapreview.com/](https://demo-kong.oktapreview.com/)


### Kong

[http://konnect.konghq.com](http://konnect.konghq.com)

UID: oktaprise-support@okta.com


# 


# Kong Data Plane - Amazon Elastic Kubernetes Service (EKS) Cluster

## Pre-requisites
Before getting started make sure you have the following tools installed:

* [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)
* [eksctl](https://eksctl.io/introduction/#installation)


## Creating the Cluster

The Kong+Okta Shared Demo Environment comprehends an existing Amazon EKS Cluster for the Kong Data Plane. The EKS Cluster and Kong Data Plane were created with the process described in this section.

If you are not interested in creating a new Kong Data Plane, skip to the following section, "Accessing the Kong Data Plane Cluster", to use this existing Cluster.


### Creating the EKS Cluster
To create the Kubernetes Cluster, we're going to use [eksctl](https://eksctl.io/), the official CLI for Amazon Elastic Kubernetes Service.

```
$ eksctl create cluster --name kong-okta --version 1.21 --region us-east-1 --nodegroup-name standard-workers --node-type t3.large --nodes 1
```


In case you need to delete it, run the following command:


```
eksctl delete cluster --name kong-okta
```


Update the local kubeconfig file:


```
aws eks update-kubeconfig --name kong-okta
```



### Checking the EKS Cluster


```
$ kubectl get pod --all-namespaces
NAMESPACE     NAME                       READY   STATUS    RESTARTS   AGE
kube-system   aws-node-jcnww             1/1     Running   0          2m40s
kube-system   coredns-66cb55d4f4-7lsj8   1/1     Running   0          10m
kube-system   coredns-66cb55d4f4-wk6pm   1/1     Running   0          10m
kube-system   kube-proxy-v5mjm           1/1     Running   0          2m40s

$ kubectl get service --all-namespaces
NAMESPACE     NAME         TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)         AGE
default       kubernetes   ClusterIP   10.100.0.1    <none>        443/TCP         11m
kube-system   kube-dns     ClusterIP   10.100.0.10   <none>        53/UDP,53/TCP   11m
```



### Kong Certificates and Key

Login to [Konnect](http://konnect.konghq.com) and go to "Runtimes" -> "Configure Runtime" -> "Kubernetes":

![konnect_runtime](images/KonnectRuntime.png)


Click on "Generate Certificate" and copy them. For example:



* Cluster Certificate. Save it as `tls.crt`


```
-----BEGIN CERTIFICATE-----
MIIDhTCCAm2gAwIBAgIULr9j9itp3e62N3OLqM6W5lgP3IEwDQYJKoZIhvcNAQEL
BQAwFDESMBAGA1UEAxMJdmF1bHQuZWtzMB4XDTIyMDMwMTIxNDgxNloXDTIyMDgz
MTA5NDg0NlowNzE1MDMGA1UEAxMsZDEub2t0YXByaXNlMmU0ZjYyLnAua29ubmVj
dC1wcm9kLmtvbmdocS5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
AQC4v1UgU/NR89ubxHhfaTGgCYrTOqe5C62zztMJeYCvdfRsfAeJ79oEggj0xs9u
wch3ek7tV/wuU7OUh8K5Ia1wnpLkkjWrM0DLY3TNl71EyfrxlrpVsvxrN9QmH+m4
Yb5S0puEwFrRSwYYbWEaidd5Swan/3q+fPMf8K99kTj+HrjqD6DThZMq4l8mV6ye
4KnNalu2KYmJ7PIJb5lxlqj2OGEHIP/2D+No6w893pn+a/1ew4CJZFAmxfMIlZ7f
Bd2Pb5jrlZKSq93+xcTjMSuOqxcexrAkhozHCKRucQ9XsNLiA1A43T58mKloAxxz
QTYuGjyoxPqBQSUodXB8zZ1lAgMBAAGjgaswgagwDgYDVR0PAQH/BAQDAgOoMB0G
A1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAdBgNVHQ4EFgQUmC3gXWJ6mqpj
qbB24ri6Bn4g5ZkwHwYDVR0jBBgwFoAUPGPdIVjPBnnUO18llh43+gKI9mQwNwYD
VR0RBDAwLoIsZDEub2t0YXByaXNlMmU0ZjYyLnAua29ubmVjdC1wcm9kLmtvbmdo
cS5jb20wDQYJKoZIhvcNAQELBQADggEBACVcJgZ9R//qy220fQHs9q1YWeDGPlfe
bR80FvzG3Cj2hPmH61oKT0np7We/KprbtUT5jzGTGhivWznO67SW9i0gETHpeQX0
GBcODmfeOv9yTjFTTs7dt2ejwc6EP3FEwduOkWQ/qirenHRYzsLkjxd4kZ3dmAk0
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIDLzCCAhegAwIBAgIUeJtyByZMnCjS+jf4QVxp99wrZ8swDQYJKoZIhvcNAQEL
BQAwFDESMBAGA1UEAxMJdmF1bHQuZWtzMB4XDTIxMDEyNjIyNDEyNloXDTMxMDEy
NDIyNDE1NlowFDESMBAGA1UEAxMJdmF1bHQuZWtzMIIBIjANBgkqhkiG9w0BAQEF
AAOCAQ8AMIIBCgKCAQEA3zKm6yK5q7extSOBcV6VFqxjEw/moeeSNr78Al8v8fJc
JS2/9guL0gpolhsS0kuAXxiKLXUbHwPY86zT2LlNAjuiGMs/ckuogju+I0k3tuY6
NX5vvxEJ215cG6SmOxjOGFo31Pkq4u0mH80MbkyyzmoGGw2bcWjV3cspRYhJ3nXb
7ix/MXHDoJwh2Oq9tTnXx+9aKISV8l9YenT+hWy33VlqcbDRALC8CXXDxTLVqrYa
WvuKB1z/p/ERq/Mk4lSfb+pS2BJ35AOXs4zJKyOtJpjJwxBXLxyZpff9oDIVXPh4
4Fo9hG0FqJl1MbPUav3mkK6q7JRhDxqiTDZV2L1OGwIDAQABo3kwdzAOBgNVHQ8B
Af8EBAMCAQYwDwYDVR0TAQH/BAUwAwEB/zAdBgNVHQ4EFgQUPGPdIVjPBnnUO18l
lh43+gKI9mQwHwYDVR0jBBgwFoAUPGPdIVjPBnnUO18llh43+gKI9mQwFAYDVR0R
BA0wC4IJdmF1bHQuZWtzMA0GCSqGSIb3DQEBCwUAA4IBAQAd7tzev3wi9bEXf11y
WeO4P0jztV8VOvTLPTtYFfOXpGgtMyQJ/BHOPVqDC0yBpT7ZmimmYRskp+SxkpTC
lcG2H/QXPxhDLEygQ5sjwPvLIIqgfpuTShqLjVLTvpKPI21WQ0V4guCEombkpwb4
-----END CERTIFICATE-----

```



* Certificate Key. Save it as `tls.key`


```
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAuL9VIFPzUfPbm8R4X2kxoAmK0zqnuQuts87TCXmAr3X0bHwH
ie/aBIII9MbPbsHId3pO7Vf8LlOzlIfCuSGtcJ6S5JI1qzNAy2N0zZe9RMn68Za6
VbL8azfUJh/puGG+UtKbhMBa0UsGGG1hGonXeUsGp/96vnzzH/CvfZE4/h646g+g
04WTKuJfJlesnuCpzWpbtimJiezyCW+ZcZao9jhhByD/9g/jaOsPPd6Z/mv9XsOA
iWRQJsXzCJWe3wXdj2+Y65WSkqvd/sXE4zErjqsXHsawJIaMxwikbnEPV7DS4gNQ
ON0+fJipaAMcc0E2Lho8qMT6gUElKHVwfM2dZQIDAQABAoIBAE4xXPwzy/ZRug8x
lqj7Qu+zWjiQyNA/KyPBU/zcmiSpTexkimRNSf9Ccjv3sW/1QL2g3KsD5E5PSuCA
nV/x6xCRbSak42Y0aSYsUBF8ObVVr4p0+ySufrt6g6YnN6q8Y+t2DDChoU6X7FqG
YOT0lmhV4tlT/kWFaPmJyxzgTDjxW8avGJ0y8Q3MZcnjzDTArN0wsXPLjyJGZMns
ITYB1tTFFMv/ifaEzRonZARTkovDY/McNIcikvAHvXAFIURtn2DaFF11e1SYfP8p
Yh0m223n3HAOm3iND6Li9t59OgbPOaBDvcQLG2BbI6HfABQr+4IzyZtBEf4k3DYO
0LJTcIECgYEA2iQTwD/mIpmuvX2ZCdwCARfzZU1ULfCgo4w66zXRvZsAntmUuNzm
kFZn+MsaqyAH8Q4P+74ndgkBidFTwLB/GWrb6sxQvsW9UkpGTDasmqYX/5ESQLmU
JqWDXCsKEe6FBnVo62V8wPESSGMTmBRWiOGcm7E47xwF+NMZIpSPsBECgYEA2M+X
unBVmojrPccmR2x+n5K64jYjIhC/OA8l6/5++P3RpoRpBcFBXGKPzdElqZT2uLGk
vKMBsSt+ehbNWK0M/2iVLrnu+ErIeEDMBnK75ik0P9lbNiYluuk3hOd/M9zk1x3K
m2QLyj6oxGPhRIMhr59zDcnNAbZGNUJD27/tbBUCgYAxky1h7aOxjVem/NpA4V0b
PoQ0K4TMA/md8YqaVdraylUzxKMF+D31440xf4cKCPeFEvsS7T4ulxFgbtAn5HSe
JCjijZh0IqBKCVM8kthbNRATJrcNOp+uwMK/uZBKTviwZRJ5HNFjT+YlyU50q3qX
FR7wSzBzNt0slV0K/NRC4QKBgF0+2wiqK4btAA9vyTzKoqLKnmmP0whhn3vit/KJ
X96Ce1pcdDavaQbstOBV5RbMJFhGvo0Kpj2mcRzdFa/frUljl1WjTe2nbBSb+aOE
UN+qNPiI6LufKHb8uo4EaibKB/mkrHCsVI3E6RydwFKQ+znnH7p8YhksZYGeYETw
-----END RSA PRIVATE KEY-----

```



* Root CA Certificate. Save it as `ca.crt`


```
-----BEGIN CERTIFICATE-----
MIIDLzCCAhegAwIBAgIUeJtyByZMnCjS+jf4QVxp99wrZ8swDQYJKoZIhvcNAQEL
BQAwFDESMBAGA1UEAxMJdmF1bHQuZWtzMB4XDTIxMDEyNjIyNDEyNloXDTMxMDEy
NDIyNDE1NlowFDESMBAGA1UEAxMJdmF1bHQuZWtzMIIBIjANBgkqhkiG9w0BAQEF
AAOCAQ8AMIIBCgKCAQEA3zKm6yK5q7extSOBcV6VFqxjEw/moeeSNr78Al8v8fJc
JS2/9guL0gpolhsS0kuAXxiKLXUbHwPY86zT2LlNAjuiGMs/ckuogju+I0k3tuY6
NX5vvxEJ215cG6SmOxjOGFo31Pkq4u0mH80MbkyyzmoGGw2bcWjV3cspRYhJ3nXb
7ix/MXHDoJwh2Oq9tTnXx+9aKISV8l9YenT+hWy33VlqcbDRALC8CXXDxTLVqrYa
WvuKB1z/p/ERq/Mk4lSfb+pS2BJ35AOXs4zJKyOtJpjJwxBXLxyZpff9oDIVXPh4
4Fo9hG0FqJl1MbPUav3mkK6q7JRhDxqiTDZV2L1OGwIDAQABo3kwdzAOBgNVHQ8B
Af8EBAMCAQYwDwYDVR0TAQH/BAUwAwEB/zAdBgNVHQ4EFgQUPGPdIVjPBnnUO18l
lh43+gKI9mQwHwYDVR0jBBgwFoAUPGPdIVjPBnnUO18llh43+gKI9mQwFAYDVR0R
BA0wC4IJdmF1bHQuZWtzMA0GCSqGSIb3DQEBCwUAA4IBAQAd7tzev3wi9bEXf11y
WeO4P0jztV8VOvTLPTtYFfOXpGgtMyQJ/BHOPVqDC0yBpT7ZmimmYRskp+SxkpTC
lcG2H/QXPxhDLEygQ5sjwPvLIIqgfpuTShqLjVLTvpKPI21WQ0V4guCEombkpwb4
/LOnZK+wd5yexbhmIQWNYQg0GhUHxXM0iakEcg9DdEchhCioxs09oz3+b+bGD7po
-----END CERTIFICATE-----
```



### Injecting Key and DCs


```
kubectl create namespace kong-dp

kubectl create secret tls kong-cluster-cert --cert=./tls.crt --key=./tls.key -n kong-dp

kubectl create secret generic kong-cluster-ca --from-file=ca.crt=./ca.crt -n kong-dp
```



### Kong Enterprise Secrets

Create a secret with your license file


```
kubectl create secret generic kong-enterprise-license -n kong-dp --from-file=./license
```



### Deploying the Data Plane


```
helm install kong-dp kong/kong -n kong-dp \
--set ingressController.enabled=false \
--set image.repository=kong/kong-gateway \
--set image.tag=2.7.1.2-alpine \
--set admin.enabled=false \
--set env.role=data_plane \
--set env.database=off \
--set env.anonymous_reports=off \
--set env.vitals_ttl_days=732 \
--set env.cluster_mtls=pki \
--set env.cluster_control_plane=kong-cpoutlet-oktaprise2e4f62.cp.konnect.konghq.com:443 \
--set env.cluster_server_name=kong-cpoutlet-oktaprise2e4f62.cp.konnect.konghq.com \
--set env.cluster_telemetry_endpoint=kong-telemetry-oktaprise2e4f62.tp.konnect.konghq.com:443 \
--set env.cluster_telemetry_server_name=kong-telemetry-oktaprise2e4f62.tp.konnect.konghq.com \
--set env.cluster_ca_cert=/etc/secrets/kong-cluster-ca/ca.crt \
--set env.cluster_cert=/etc/secrets/kong-cluster-cert/tls.crt \
--set env.cluster_cert_key=/etc/secrets/kong-cluster-cert/tls.key \
--set env.lua_ssl_trusted_certificate=/etc/secrets/kong-cluster-cert/tls.crt \
--set proxy.enabled=true \
--set proxy.type=LoadBalancer \
--set enterprise.enabled=true \
--set enterprise.license_secret=kong-enterprise-license \
--set enterprise.portal.enabled=false \
--set enterprise.rbac.enabled=false \
--set enterprise.smtp.enabled=false \
--set manager.enabled=false \
--set portal.enabled=false \
--set portalapi.enabled=false \
--set env.status_listen=0.0.0.0:8100 \
--set secretVolumes[0]=kong-cluster-cert \
--set secretVolumes[1]=kong-cluster-ca
```



## Accessing the Kong Data Plane Cluster

If you don't have your local kubeconfig set, run:


```
$ aws eks update-kubeconfig --name kong-okta
Added new context arn:aws:eks:us-east-1:959189502117:cluster/kong-okta to /Users/claudio/.kube/config

$ kubectl config get-contexts
CURRENT   NAME                                                   CLUSTER                                                AUTHINFO                                               NAMESPACE
*         arn:aws:eks:us-east-1:959189502117:cluster/kong-okta   arn:aws:eks:us-east-1:959189502117:cluster/kong-okta   arn:aws:eks:us-east-1:959189502117:cluster/kong-okta
```



### Checking the EKS Cluster


```
$ kubectl get deployments --all-namespaces
NAMESPACE     NAME           READY   UP-TO-DATE   AVAILABLE   AGE
kong-dp       kong-dp-kong   1/1     1            1           46s
kube-system   coredns        2/2     2            2           55m

$ kubectl get pod --all-namespaces
NAMESPACE     NAME                            READY   STATUS    RESTARTS   AGE
kong-dp       kong-dp-kong-7755cf4fc6-9bnkw   1/1     Running   0          55s
kube-system   aws-node-jcnww                  1/1     Running   0          47m
kube-system   coredns-66cb55d4f4-7lsj8        1/1     Running   0          55m
kube-system   coredns-66cb55d4f4-wk6pm        1/1     Running   0          55m
kube-system   kube-proxy-v5mjm                1/1     Running   0          47m

$ kubectl get service --all-namespaces
NAMESPACE     NAME                 TYPE           CLUSTER-IP      EXTERNAL-IP                                                               PORT(S)                      AGE
default       kubernetes           ClusterIP      10.100.0.1      <none>                                                                    443/TCP                      55m
kong-dp       kong-dp-kong-proxy   LoadBalancer   10.100.24.228   aa028f3bf482240a1b02280e14647be2-1862770018.us-east-1.elb.amazonaws.com   80:31552/TCP,443:30792/TCP   61s
kube-system   kube-dns             ClusterIP      10.100.0.10     <none>                                                                    53/UDP,53/TCP                55m
```



### Next Steps

[Kong Fundamentals](https://github.com/Kong/kong-okta/blob/main/Kong%20Fundamentals/Kong%20Fundamentals.md)