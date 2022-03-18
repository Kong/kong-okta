# Control Planes


### Okta

[https://demo-kong.oktapreview.com/](https://demo-kong.oktapreview.com/)


### Kong

[http://konnect.konghq.com](http://konnect.konghq.com)

UID: oktaprise-support@okta.com


# 


# Kong Data Plane - Amazon Elastic Kubernetes Service (EKS) Cluster


## Creating the Cluster

The Kong+Okta Shared Demo Environment comprehends an existing Amazon EKS Cluster for the Kong Data Plane. The EKS Cluster and Kong Data Plane were created with the process described in this section.

If you are not interested in creating a new Kong Data Plane, skip to the following section, "Accessing the Kong Data Plane Cluster", to use this existing Cluster.


### Creating the EKS Cluster


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

Login to Konnect ([http://konnect.konghq.com](http://konnect.konghq.com)) and go to "Runtimes" -> "Configure Runtime" -> "Kubernetes":



<p id="gdcalert2" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image2.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert3">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image2.png "image_tooltip")


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
GwZI66H9ZCZco6Tb/e5HYKnNBez3smpvaXW2ykpuQ4c/yeArYSNWRWItBQmP7W0R
MUN7QtJ2aDecrYJpfE6BQU2leRyqMND+peis2re/hcVL/0pdWtSGWviW1M/G9vVS
GA5TM/oJI5xyAjFApxEx/2pL92chZeTMquLXY3xMbuzgJayaqRInhFs=
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
/LOnZK+wd5yexbhmIQWNYQg0GhUHxXM0iakEcg9DdEchhCioxs09oz3+b+bGD7po
wz/r1qfxDlc6NoufzJXgmLcAnrZmqw2ja1MF3Q+VxHOq8JL3dGGpLvvZ1JaQ7eU0
iqUWqlrDUn2o+baeEy3MmwAoubQCcn7BiZhCUh0jJCCQiRiq/0PCapyvzYrSTIbW
Dckc
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
hJIJAoGAZuh5R1592rht1NlR2lyQ2DBHtvhJdM3z+VbJvvMAFXh4m01uhEXgVotN
F8XoERRAuW6A6T2Yr7r/qNs0mCiuxpwkXsoxNCVGaZPJaIsrUUsa6JTsOOga2Q8u
0nLSrM2MTC8otr9mrFI358TAcBdBryKEzp/2K3idc3I3Vt33Pto=
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
wz/r1qfxDlc6NoufzJXgmLcAnrZmqw2ja1MF3Q+VxHOq8JL3dGGpLvvZ1JaQ7eU0
iqUWqlrDUn2o+baeEy3MmwAoubQCcn7BiZhCUh0jJCCQiRiq/0PCapyvzYrSTIbW
Dckc
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



# 


# Kong Gateway Demo Script


## Kong Service and Route Creation


### Create the Service Package

Login to Konnect and click on "Add New Service"



<p id="gdcalert3" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image3.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert4">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image3.png "image_tooltip")


Create a new "httpbinservice" Service with "v1" version



<p id="gdcalert4" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image4.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert5">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image4.png "image_tooltip")


Click on "Create"



<p id="gdcalert5" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image5.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert6">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image5.png "image_tooltip")



### Create the Service's Implementation

Click on version "v1"



<p id="gdcalert6" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image6.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert7">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image6.png "image_tooltip")


Click on "New Implementation"



<p id="gdcalert7" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image7.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert8">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image7.png "image_tooltip")


Type "http://httpbin.org" for URL and click on "Next"



<p id="gdcalert8" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image8.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert9">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image8.png "image_tooltip")


Type "httpbinroute" for Name. Click on "+ Add Path" and type "/httpbin". Click on "Create".



<p id="gdcalert9" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image9.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert10">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image9.png "image_tooltip")



### Consume the Kong Route

Open a local terminal and send a request to the Data Plane using the AWS ELB already provisioned:


```
$ kubectl get service kong-dp-kong-proxy -n kong-dp -o json | jq -r .status.loadBalancer.ingress[].hostname
aa028f3bf482240a1b02280e14647be2-1862770018.us-east-1.elb.amazonaws.com

$ http aa028f3bf482240a1b02280e14647be2-1862770018.us-east-1.elb.amazonaws.com/httpbin/get
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 551
Content-Type: application/json
Date: Mon, 14 Feb 2022 13:57:46 GMT
Server: gunicorn/19.9.0
Via: kong/2.7.1.1-enterprise-edition
X-Kong-Proxy-Latency: 15
X-Kong-Upstream-Latency: 172

{
    "args": {},
    "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate",
        "Host": "httpbin.org",
        "User-Agent": "HTTPie/3.0.2",
        "X-Amzn-Trace-Id": "Root=1-620a5fda-39b8f04d6d00667c6dc0549d",
        "X-Forwarded-Host": "a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com",
        "X-Forwarded-Path": "/httpbin/get",
        "X-Forwarded-Prefix": "/httpbin"
    },
    "origin": "192.168.66.146, 15.236.158.176",
    "url": "http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/get"
}
```


Show the Konnect Traffic Dashboard:



<p id="gdcalert10" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image10.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert11">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image10.png "image_tooltip")



## 


## Rate Limiting Policy

As you can see the API has been exposed and can be consumed as many times as we want. Now it is time to define policies to control the exposure. The first policy we are going to create is Rate Limiting.


### Enable the Rate Limiting plugin to the Route

Generally speaking, a plugin can be enabled to specific objects like Services, Routes and Consumers or globally. Check the documentation for more information: [https://docs.konghq.com/konnect/manage-plugins/](https://docs.konghq.com/konnect/manage-plugins/)

Click on the Route "httpbinroute"



<p id="gdcalert11" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image11.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert12">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image11.png "image_tooltip")


Click on "Add a Plugin"



<p id="gdcalert12" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image12.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert13">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image12.png "image_tooltip")


Scroll down to the "Traffic Control" section:



<p id="gdcalert13" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image13.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert14">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image13.png "image_tooltip")


Click on "Rate Limiting":



<p id="gdcalert14" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image14.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert15">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image14.png "image_tooltip")


For the "Config.Minute" parameter type 5 and for "config.policy" choose "local". Click on "Create".



<p id="gdcalert15" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image15.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert16">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image15.png "image_tooltip")


As you can see we can define Rate Limiting policies by minute, hour and month. An external Redis infrastructure can be used to share the Rate Limiting counters across multiple Data Plane instances. Please check the Rate Limiting plugin page for more information: [https://docs.konghq.com/hub/kong-inc/rate-limiting/](https://docs.konghq.com/hub/kong-inc/rate-limiting/)


### Consume the Kong Route again

Send a new request to the Data Plane. As you can see new Headers have been added to the response showing the Rate Limiting policy status:


```
$ http aa028f3bf482240a1b02280e14647be2-1862770018.us-east-1.elb.amazonaws.com/httpbin/get
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 551
Content-Type: application/json
Date: Mon, 14 Feb 2022 14:24:35 GMT
RateLimit-Limit: 5
RateLimit-Remaining: 4
RateLimit-Reset: 25
Server: gunicorn/19.9.0
Via: kong/2.7.1.1-enterprise-edition
X-Kong-Proxy-Latency: 28
X-Kong-Upstream-Latency: 166
X-RateLimit-Limit-Minute: 5
X-RateLimit-Remaining-Minute: 4

{
    "args": {},
    "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate",
        "Host": "httpbin.org",
        "User-Agent": "HTTPie/3.0.2",
        "X-Amzn-Trace-Id": "Root=1-620a6623-75a05eef1229ef755b5821dd",
        "X-Forwarded-Host": "a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com",
        "X-Forwarded-Path": "/httpbin/get",
        "X-Forwarded-Prefix": "/httpbin"
    },
    "origin": "192.168.66.146, 15.236.158.176",
    "url": "http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/get"
}
```


Since we have defined a 5-request a minute policy, you should receive a specific 429 error code when trying to send more requests.


```
$ http aa028f3bf482240a1b02280e14647be2-1862770018.us-east-1.elb.amazonaws.com/httpbin/get
HTTP/1.1 429 Too Many Requests
Connection: keep-alive
Content-Length: 41
Content-Type: application/json; charset=utf-8
Date: Mon, 14 Feb 2022 14:24:46 GMT
RateLimit-Limit: 5
RateLimit-Remaining: 0
RateLimit-Reset: 14
Retry-After: 14
Server: kong/2.7.1.1-enterprise-edition
X-Kong-Response-Latency: 0
X-RateLimit-Limit-Minute: 5
X-RateLimit-Remaining-Minute: 0

{
    "message": "API rate limit exceeded"
}
```



## 


## Proxy Caching Policy

The second policy we are going to create is the Proxy Caching to get better performance and reduce latency times.


### Enable the Proxy Caching plugin to the Route

For the same Route we're going to enable the new policy based on the Proxy Caching plugin.

Click on the Route "httpbinroute" again:



<p id="gdcalert16" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image16.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert17">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image16.png "image_tooltip")


Click on "Add Plugin". Scroll down to the "Traffic Control" section again and choose the "Proxy Caching" plugin:



<p id="gdcalert17" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image17.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert18">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image17.png "image_tooltip")


Similarly to what we did before, we are going to set the Proxy Caching plugin with its specific parameters. For example, for "Config.Cache Ttl" parameter type 30 and for "Config.Strategy" choose "memory.

Check the Proxy Caching Plugin page for more information about it: [https://docs.konghq.com/hub/kong-inc/proxy-cache/](https://docs.konghq.com/hub/kong-inc/proxy-cache/)



<p id="gdcalert18" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image18.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert19">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image18.png "image_tooltip")



### Consume the Kong Route again

Send a new request to the Data Plane. Besides the Rate Limiting Headers, our response shows the Proxy Caching Status. The Status says the Gateway didn't have any data available to satisfy the request, so it had to go to the Upstream.


```
$ http aa028f3bf482240a1b02280e14647be2-1862770018.us-east-1.elb.amazonaws.com/httpbin/get
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 551
Content-Type: application/json
Date: Mon, 14 Feb 2022 14:50:33 GMT
RateLimit-Limit: 5
RateLimit-Remaining: 4
RateLimit-Reset: 27
Server: gunicorn/19.9.0
Via: kong/2.7.1.1-enterprise-edition
X-Cache-Key: 0de290ec1f2998c47e44881248bf3136
X-Cache-Status: Miss
X-Kong-Proxy-Latency: 23
X-Kong-Upstream-Latency: 166
X-RateLimit-Limit-Minute: 5
X-RateLimit-Remaining-Minute: 4

{
    "args": {},
    "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate",
        "Host": "httpbin.org",
        "User-Agent": "HTTPie/3.0.2",
        "X-Amzn-Trace-Id": "Root=1-620a6c39-76474f1d60fc17230786ec0e",
        "X-Forwarded-Host": "a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com",
        "X-Forwarded-Path": "/httpbin/get",
        "X-Forwarded-Prefix": "/httpbin"
    },
    "origin": "192.168.66.146, 15.236.158.176",
    "url": "http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/get"
}
```


For the second request, the Status says we got a "Hit" meaning that the Gateway had everything it needed to respond to the consumer. Moreover, the latency times are considerably reduced.


```
$ http aa028f3bf482240a1b02280e14647be2-1862770018.us-east-1.elb.amazonaws.com/httpbin/get
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Age: 9
Connection: keep-alive
Content-Length: 551
Content-Type: application/json
Date: Mon, 14 Feb 2022 14:50:33 GMT
RateLimit-Limit: 5
RateLimit-Remaining: 3
RateLimit-Reset: 27
Server: gunicorn/19.9.0
Via: kong/2.7.1.1-enterprise-edition
X-Cache-Key: 0de290ec1f2998c47e44881248bf3136
X-Cache-Status: Hit
X-Kong-Proxy-Latency: 0
X-Kong-Upstream-Latency: 0
X-RateLimit-Limit-Minute: 5
X-RateLimit-Remaining-Minute: 3

{
    "args": {},
    "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate",
        "Host": "httpbin.org",
        "User-Agent": "HTTPie/3.0.2",
        "X-Amzn-Trace-Id": "Root=1-620a6c39-76474f1d60fc17230786ec0e",
        "X-Forwarded-Host": "a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com",
        "X-Forwarded-Path": "/httpbin/get",
        "X-Forwarded-Prefix": "/httpbin"
    },
    "origin": "192.168.66.146, 15.236.158.176",
    "url": "http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/get"
}
```


If we wait for 30 seconds, since it's been our TTL for the Caching, the Gateway will purge all data from the Cache and report a "Miss" again.


```
$ http aa028f3bf482240a1b02280e14647be2-1862770018.us-east-1.elb.amazonaws.com/httpbin/get
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 551
Content-Type: application/json
Date: Mon, 14 Feb 2022 14:52:27 GMT
RateLimit-Limit: 5
RateLimit-Remaining: 4
RateLimit-Reset: 33
Server: gunicorn/19.9.0
Via: kong/2.7.1.1-enterprise-edition
X-Cache-Key: 0de290ec1f2998c47e44881248bf3136
X-Cache-Status: Miss
X-Kong-Proxy-Latency: 9
X-Kong-Upstream-Latency: 165
X-RateLimit-Limit-Minute: 5
X-RateLimit-Remaining-Minute: 4

{
    "args": {},
    "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate",
        "Host": "httpbin.org",
        "User-Agent": "HTTPie/3.0.2",
        "X-Amzn-Trace-Id": "Root=1-620a6cab-526b5a9d04cdb3727d3bca1e",
        "X-Forwarded-Host": "a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com",
        "X-Forwarded-Path": "/httpbin/get",
        "X-Forwarded-Prefix": "/httpbin"
    },
    "origin": "192.168.66.146, 15.236.158.176",
    "url": "http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/get"
}
```



# 


# Kong + Okta - Client Credentials Demo Script

The "Client Credentials" Flow is defined by OAuth to address scenarios like Application Authentication, Machine-to-Machine, Microservices Accounts, etc. For user-oriented flows, please go to the next "Authorization Code" section. Please refer to this link to learn more about "[Client Credentials](https://developer.okta.com/docs/guides/implement-client-creds/)".


### Okta Application Creation

Log in to Okta console using the Kong + Okta endpoint [https://demo-kong.oktapreview.com/](https://demo-kong.oktapreview.com/)



<p id="gdcalert19" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image19.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert20">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image19.png "image_tooltip")


Click on "Applications" -> "Applications" -> "Create App Integration". Create a "API Services" Application named KongClientCredentials:



<p id="gdcalert20" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image20.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert21">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image20.png "image_tooltip")


Click on the "eye" button to see the "Client secret". Save both "Client ID" and "Client secret". They'll be used to consume the Kong Route later on.


### Scope creation

Click on "Security" -> "API" -> "Authorization Servers"



<p id="gdcalert21" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image21.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert22">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image21.png "image_tooltip")


Click on the "default" Authorization Server. Take note of the Authorization Server issuer parameter, in our case: `https://demo-kong.oktapreview.com/oauth2/default`



<p id="gdcalert22" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image22.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert23">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image22.png "image_tooltip")


Click on "Scopes" and "Add Scope" to create a scope named "scope1"



<p id="gdcalert23" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image23.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert24">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image23.png "image_tooltip")



## 


### Enable the OIDC plugin to the Route

Let's apply the OIDC plugin to the Route with the specific Okta settings:



* Issuer Endpoint
* Scope

Click on "httpbinroute" again:



<p id="gdcalert24" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image24.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert25">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image24.png "image_tooltip")


Click on "Add Plugin". Choose "OpenID Connect" plug:



<p id="gdcalert25" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image25.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert26">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image25.png "image_tooltip")


Fill in the form with the following configuration:



* Config.Issuer: `https://demo-kong.oktapreview.com/oauth2/default`
* Config.Scopes: `scope1`

Click on "Create" and on the Route again:



<p id="gdcalert26" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image26.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert27">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image26.png "image_tooltip")



### Consume the Route with Client Id and Client Secret


```
$ http aa028f3bf482240a1b02280e14647be2-1862770018.us-east-1.elb.amazonaws.com/httpbin/get -a 0oa2jv4uoy4ed7zor1d7:7wQQl_MfQ2gEGLvYLMfSwNYRBBW9JP1NvNV3_hSb
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 1354
Content-Type: application/json
Date: Mon, 14 Feb 2022 15:22:44 GMT
RateLimit-Limit: 5
RateLimit-Remaining: 4
RateLimit-Reset: 16
Server: gunicorn/19.9.0
Set-Cookie: session=rqA_M1t6DOJh4tPBM74NBA|1644855762|9fRKkSUvWylJA7sJ7cqEx0k1MDqEjKzmU8M5rNObwER_kB0WO4m0VM7wNlfCwILEEdM2Goym6Pag5tfRHaia0z3yW3xC3uWipCliie6zcmt82DO9OqPCICwfzg4KhCUgwEw_PlHa3kAh9mtlgZD4_AEceEP1YuTVC8fS5tGcLiAuqLXumEWW0NiaWr9QWOLfbEiG6DHGqmTCSEBiEVdftmgKz26nUT2PfFMGIGtmjb0MSX0q9f-4iyWBMWeIjfuzPlF-BGSmaZuLUJR9ICbP3_7h19bOe-E79YI_yUTPTW8DsxY8cXQeTpEnDdRJVrockVkGDtJLmqb2QaYJsofQ0ufnS-9-K-XXonA6tZij1wVwidF86pxRIfSQmU3Xdo3ONT5NC8kuMX0yPFHYRXe0gr5SyHHQPzw4wZt9ft3764qgsCqmq8L4PFBqUlHfhXg6ZBIWLuib3CLLoulpC_haABFAgpaH0TBxlaX9TRepGVG8Jz2DXMviqFvS8yAEk2wlm87cm6KzIJaWX-lKi9gVwmTcsBsZWf062QC13E2hPVhDPpcz7U8TyXNXtyWiFkJxaD0zrSy6ldh5YBGaW0ir4c3nJVGnJvgdp4CpY8gspd2DXc_WoBgsx4uzBLiKS6pk0UX7f3tV9vRQUF2_Pl4167gKBnYEUwkljxGEolnrd4mXhBQZAl8hRNZZr-EJi_uLkuKUWhheaa7kRA0cBzwJfGizXUHU66EfRgot8Iqp5L2g9k0Rlbhslmo_8krmQO3UJKPixdc5onPba_3BfZbagys0C52OiRebYiLy9vtODHORuyJ5ZA4NRlc_yQnfdchV8X0jY_pTJ6kLEALG0nxbEz5wpVB-BxsOKL-1DoAygKNgF5FdMiJCNQwfrfrAuQMAnj-Hgqz4BeipIzd8Bpf9kbjmEZfFEjHaGmJlk1tPPJFVALka0rS_zBgumZkRpUm7vkJv2EgMoNSs3tKgjGYKsfjUSTgh-Z-tiW1Y0e1xBuOve7-wRSYJ_VY-hkkfckOTjEhNiaIllbSMTTIAwaHXRhQE7rqhSIO4HMK1xBqQkH_h2tTlksOScd52sCESOsm2XAljUpBEqwSCnQy2Tw4jS-TP1h9C-Qir0dJPj-6waIkXLez4bGyUOJg312-FoDofBPNGlY2zrC-ga7zEyg2VqtObI_hUmqeTR_P8ygRua20|X547CVhQUyrf1q7Iqq-0akHiiYA; Path=/; SameSite=Lax; HttpOnly
Via: kong/2.7.1.1-enterprise-edition
X-Cache-Key: 0de290ec1f2998c47e44881248bf3136
X-Cache-Status: Miss
X-Kong-Proxy-Latency: 1086
X-Kong-Upstream-Latency: 166
X-RateLimit-Limit-Minute: 5
X-RateLimit-Remaining-Minute: 4

{
    "args": {},
    "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate",
        "Authorization": "Bearer eyJraWQiOiJsWU9mb3FRNXVUa28xeURXbUVqc1RDaDdYSHkxaVJtb2pObFJjelFVZUl3IiwiYWxnIjoiUlMyNTYifQ.eyJ2ZXIiOjEsImp0aSI6IkFULlBhSVBFcV9mMDlydWlDWkYtQmk0VEFXVHJlR3k5YU03UXAzVUVqcE1Uck0iLCJpc3MiOiJodHRwczovL2RlbW8ta29uZy5va3RhcHJldmlldy5jb20vb2F1dGgyL2RlZmF1bHQiLCJhdWQiOiJhcGk6Ly9kZWZhdWx0IiwiaWF0IjoxNjQ0ODUyMTYzLCJleHAiOjE2NDQ4NTU3NjMsImNpZCI6IjBvYTJqdjR1b3k0ZWQ3em9yMWQ3Iiwic2NwIjpbInNjb3BlMSJdLCJzdWIiOiIwb2EyanY0dW95NGVkN3pvcjFkNyJ9.idJj6lsMdwaV9Dy8Cavgb_5RMkqk7NPeTnD2r1zLlwSDYfpZLoPCfGza9Q75DwNkwlbzMuArEJ2in0_Ooq2NIF7SQaEyM8Y_XWBBU5xHtcrTecwXa_ikpUYWReWORWJbmh0c0qRpQ_WgoymO8d3Ny--CrJpNWluKYG95WMNxz66nQN2FVA11iEfd-9TNhXhKHE2oqW1TC-tCQubXsKp_xRmd-GPKFTC_yiBD-uB1idqqE1q-2J4XW_HK5pKSm34kvXXkldzTDHsoeNoxU_butyn4RlRV32B1r9yfezSuzfOMm0T0AMwecF0YyN0y6qybqZHJ5OK5akycaEkCZ_3zjg",
        "Host": "httpbin.org",
        "User-Agent": "HTTPie/3.0.2",
        "X-Amzn-Trace-Id": "Root=1-620a73c4-3de6758416ee51a40758c3f8",
        "X-Forwarded-Host": "a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com",
        "X-Forwarded-Path": "/httpbin/get",
        "X-Forwarded-Prefix": "/httpbin"
    },
    "origin": "192.168.66.146, 15.236.158.176",
    "url": "http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/get"
}
```


The "`-a`" option is a shortcut for "`--auth`"

If you try to consumer the Route with invalid ClientId/ClientSecret pair you get a 401 error code:


```
$ http a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/httpbin/get -a 0oa2jv4uoy4ed7zor1d7:7wQQl_MfQ2gEGLvYLMfSwNYRBB
HTTP/1.1 401 Unauthorized
Connection: keep-alive
Content-Length: 26
Content-Type: application/json; charset=utf-8
Date: Mon, 14 Feb 2022 15:24:14 GMT
Server: kong/2.7.1.1-enterprise-edition
WWW-Authenticate: Bearer realm="demo-kong.oktapreview.com"
X-Kong-Response-Latency: 957

{
    "message": "Unauthorized"
}
```



### Check the JWT

Copy the token issued by Okta and paste it in [http://jwt.io](http://jwt.io)



<p id="gdcalert27" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image27.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert28">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image27.png "image_tooltip")



## 


## Upstream Header Injection

Now, we're going to update our OIDC plugin setting to add extra information to the tokens. That is, our coming request will inject not just the credentials (client_id and client_secret) but also new headers to be included in the routed request to the upstream service after authentication and validation.

For this example, we're going to inject a new header based on the "iss" field we have inside the token's payload: `https://demo-kong.oktapreview.com/oauth2/default`. So the new header should be:


```
"Issuer_Header": "https://demo-kong.oktapreview.com/oauth2/default",
```



### Update the OIDC plugin

Click on "httpbinroute" -> "openid-connect" plugin -> "Edit Plugin"

Fill in the form with the new configuration:



* Config.Upstream Headers Claims: `iss`
* Config.Upstream Headers Names: `Issuer_Header`

Click on "Update" and send a new request. As you can see there's a new Header as requested:


```
$ http a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/httpbin/get -a 0oa2jv4uoy4ed7zor1d7:7wQQl_MfQ2gEGLvYLMfSwNYRBBW9JP1NvNV3_hSb
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 1428
Content-Type: application/json
Date: Mon, 14 Feb 2022 15:31:31 GMT
RateLimit-Limit: 5
RateLimit-Remaining: 4
RateLimit-Reset: 29
Server: gunicorn/19.9.0
Set-Cookie: session=QF8ugeyUvYuVtLMfNUtjjQ|1644856290|lgqA3kXP2wGxwfuaMLKxIg5jhzqc31gwrDB5XalW_x0NqVW3zru3us1jwZCR_lMrXX9GpWtuNhfWOZ_cJqv6j99vjo_36oZwLQskjUcchE60iKY1-5sk2jOWl11sJvy7j7nj0jRSpD6qxpePB_FbvG8J86jFgV1UuMSGW5de-IqJqqnomp9aeoOMuxR4PSFycUA0FRpxn_PBI8McUeGoG9_9qP9sjJW8b9lxdXonfg3GvrBBmM0qgLFE3g98BYC1k60mvHJYStioji6WS9_RXzLZxHvhxws0kMOucvD7I2F2ogiqWOYgwMOpyMj1oRdnPIdVCEq0K6E8oEJ_XLiML7E02D8YlotFaF1YKit8qBzRJOGgIOu2TQcSCSUo1L6x5_4ZU-eWkZVG0OBVEztXEQGpKytR7SaxgD0frbkIudNW-SEYVbcyrvhdFnN-WJ5bhQVfgGNwWfbeSKqvjaieT0IMnDGcf06balq2NT8DNSFVpReeX3SqVW2h9JjHKwnEVRYtFLZ9rEUFj4ORVXQrPQoOUfGw7AxTv2ieyRuraKPfApkzlLAadVMPcPtNxLDBIOGDP8e4xWWJC2vPMf6KeKJ1A6LBumeqjvzqqtdexWnQixgTQBrlCz2UZ5aaRZghHy0-8JJhWAOCG7qlMPhAus9EXr0rPVLVGTo9Nn2ZfkzMe5feKXO1tvQvRfPorxd1eNvafeiOlFkdICVh-RI95tnVmuhXoCR444fomM6WbRYxaHphFtxYiGIU095kdIKo5AXUQXRdCp5ZwBTzXTLgABryhNjekxEPJ45Yw8XRb2327ZxdWKlO9NhZH2xX0Ygj8_Y4tFLihSCFR3_K855jhQgWFXz0Bexk7JkO_Xdpxx72OBwL6EeNxnqvz8ndp59eCkKqmgzNV2_q0RQ8usQWlY5NnoB951CTHZdyjQw327zB68RaqytetD7UQiAezYopFbH5aH6HrdWx4eoJ7pwoQrcoD-pLDLxoJuH4tkDsVM-An5vOsa-y4sLbOGSF7n4isupaoN_6s1InJjNOt4FpBN3t-19Vy7ZF9mTClY_gG4wi4HYtfQGyjWndJIXBXf8WqyTSxHY9pii8Qup3d1kGYCCrYad1JYjp4jIQ-hJS4ydcIq4ZcRbgmJY39E-YskOI6sN51arzyB1PTpHc35CxRXbzqkIx5L4734Dp5hFSyDc|XnfXRynXFXzSOa_mO97ZTJHZOlE; Path=/; SameSite=Lax; HttpOnly
Via: kong/2.7.1.1-enterprise-edition
X-Cache-Key: 0de290ec1f2998c47e44881248bf3136
X-Cache-Status: Miss
X-Kong-Proxy-Latency: 1133
X-Kong-Upstream-Latency: 165
X-RateLimit-Limit-Minute: 5
X-RateLimit-Remaining-Minute: 4

{
    "args": {},
    "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate",
        "Authorization": "Bearer eyJraWQiOiJsWU9mb3FRNXVUa28xeURXbUVqc1RDaDdYSHkxaVJtb2pObFJjelFVZUl3IiwiYWxnIjoiUlMyNTYifQ.eyJ2ZXIiOjEsImp0aSI6IkFULnFrWFhsdlpBLTIta0s4cXpsbDJrV0xBMWQyNDNJZVMyN2dwZFRfbzhYU0UiLCJpc3MiOiJodHRwczovL2RlbW8ta29uZy5va3RhcHJldmlldy5jb20vb2F1dGgyL2RlZmF1bHQiLCJhdWQiOiJhcGk6Ly9kZWZhdWx0IiwiaWF0IjoxNjQ0ODUyNjkxLCJleHAiOjE2NDQ4NTYyOTEsImNpZCI6IjBvYTJqdjR1b3k0ZWQ3em9yMWQ3Iiwic2NwIjpbInNjb3BlMSJdLCJzdWIiOiIwb2EyanY0dW95NGVkN3pvcjFkNyJ9.EvPTlK2fIeA0WKYchAJJkj-FTkDPd8hbuxpYHF5EgCKeF38RkBe0dpjQ4YNfqW0S5K06ASeYmReiFZbzmoKXXn-DHuLr8zjbMdgOgIerzZsbZa4eKCNQzVqGmD8Yjo2xRPRel9i6PCCt3Z7_Yv7s9DVdpQTYQcOWG2FN77dgApA5D_FSwDqA18HtoxEzUVqKWna39hc0bMVoG_5H5ZoRpEinTGS45EpwZ1ZJoex-JvRsHiqaI5CRDo8peIK5GYhhQ7MJlj1mBHH1GHZKyJ1ap_X62RmmibIEUA0e4zmnJbl88Gr2lBVgG3CX24gIUygiV9EGvRxP9av4Ue5kl_44xg",
        "Host": "httpbin.org",
        "Issuer-Header": "https://demo-kong.oktapreview.com/oauth2/default",
        "User-Agent": "HTTPie/3.0.2",
        "X-Amzn-Trace-Id": "Root=1-620a75d3-3f815be438d63db11d7c37ed",
        "X-Forwarded-Host": "a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com",
        "X-Forwarded-Path": "/httpbin/get",
        "X-Forwarded-Prefix": "/httpbin"
    },
    "origin": "192.168.66.146, 15.236.158.176",
    "url": "http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/get"
}
```



# 


# Kong Declaration (decK)


### Create a new Kong Route using decK and enable the OIDC plugin

You can install deck in MacOs with brew:


```
$ brew tap kong/deck
$ brew install kong/deck/deck
```


To get a full dump of the Kong objects run the following command. That will create a "konnect.yaml" file with all current Kong specification:


```
deck konnect dump --konnect-email oktaprise-support@okta.com --konnect-password 'Kongkong1!'
```



### kong.yaml

For the simplest Kong Service and Route creation uses the following declaration:


```
_format_version: "0.1"
service_packages:
- name: httpbinservice
  versions:
  - implementation:
      kong:
        service:
          url: http://httpbin.org
          id: 00000000-0000-0000-0000-000000000000
          retries: 5
          routes:
          - https_redirect_status_code: 426
            name: httpbinroute
            path_handling: v0
            paths:
            - /httpbin
            request_buffering: true
            response_buffering: true
            strip_path: true
          write_timeout: 60000
      type: kong-gateway
    version: v1

deck konnect sync --konnect-email oktaprise-support@okta.com --konnect-password 'Kongkong1!' -s kong.yaml
```



### kong_oidc.yaml


```
_format_version: "0.1"
service_packages:
- name: httpbinservice
  versions:
  - implementation:
      kong:
        service:
          connect_timeout: 60000
          host: httpbin.org
          id: 00000000-0000-0000-0000-000000000000
          port: 80
          protocol: http
          read_timeout: 60000
          retries: 5
          routes:
          - https_redirect_status_code: 426
            name: httpbinroute
            path_handling: v0
            paths:
            - /httpbin
            plugins:
            - config:
                minute: 5
              enabled: true
              name: rate-limiting
            - config:
                cache_ttl: 30
                strategy: memory
              enabled: true
              name: proxy-cache
            - config:
                issuer: https://demo-kong.oktapreview.com/oauth2/default
                scopes:
                - scope1
              enabled: true
              name: openid-connect
            preserve_host: false
            request_buffering: true
            response_buffering: true
            strip_path: true
          write_timeout: 60000
      type: kong-gateway
    version: v1
```



### Consume the Route with Client Id and Client Secret


```
http a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/httpbin/get -a 0oa2jv4uoy4ed7zor1d7:7wQQl_MfQ2gEGLvYLMfSwNYRBBW9JP1NvNV3_hSb
```



# 


# Kong + Okta - Authorization Code + PKCE (Proof Key for Code Exchange) Demo Script

If you are building a native application, then the Authorization Code flow with a Proof Key for Code Exchange (PKCE) is the recommended method for controlling the access between your application and a resource server. The Authorization Code flow with PKCE is similar to the standard Authorization Code flow with an extra step at the beginning and an extra verification at the end. Please refer to this link to learn more about "[Authorization Code with PKCE](https://developer.okta.com/docs/guides/implement-grant-type/authcodepkce/main/)".


### Okta Application Creation

Login to Okta console again and click on "Applications" -> "Applications" -> "Create App Integration". Create a "OIDC - OpenID Connect" -> "Native Application". 



* For "App integration name" type "KongPKCE".
* Turn "Refresh Token" on
* For "Sign-in redirect URIs" type "http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/httpbin2/get"
* For "Sign-out redirect URIs" type "http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com"
* Turn "Skip group assignment for now" on



<p id="gdcalert28" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image28.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert29">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image28.png "image_tooltip")


Save the Client Id, we're going to use it to configure the Kong OIDC plugin. In our case, `0oa2jw2dn1knwPbaf1d7`


### Assign User to Application

Click on "Assignments" -> "Assign" -> "Assign to People". Choose an user and click on "Save and Go Back". Click on "Done" again.



<p id="gdcalert29" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image29.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert30">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image29.png "image_tooltip")



### kong_oidc.yaml


```
_format_version: "0.1"
service_packages:
- name: httpbinservice
  versions:
  - implementation:
      kong:
        service:
          connect_timeout: 60000
          host: httpbin.org
          id: 00000000-0000-0000-0000-000000000000
          port: 80
          protocol: http
          read_timeout: 60000
          retries: 5
          routes:
          - https_redirect_status_code: 426
            name: httpbinroute
            path_handling: v0
            paths:
            - /httpbin
            plugins:
            - config:
                minute: 5
              enabled: true
              name: rate-limiting
            - config:
                cache_ttl: 30
                strategy: memory
              enabled: true
              name: proxy-cache
            - config:
                cache_ttl: 10
                issuer: https://demo-kong.oktapreview.com/oauth2/default
                scopes:
                - scope1
              enabled: true
              name: openid-connect
            preserve_host: false
            request_buffering: true
            response_buffering: true
            strip_path: true
          - https_redirect_status_code: 426
            name: httpbinroute2
            path_handling: v1
            paths:
            - /httpbin2
            plugins:
            - config:
                client_id: [0oa2jw2dn1knwPbaf1d7]
                auth_methods: [authorization_code]
                issuer: https://demo-kong.oktapreview.com/oauth2/default
                token_endpoint_auth_method: none
              enabled: true
              name: openid-connect
            preserve_host: false
            request_buffering: true
            response_buffering: true
            strip_path: true
          write_timeout: 60000
      type: kong-gateway
    version: v1
```



### Consume the Route

Redirect your browser to: 


```
http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/httpbin2/get
```




<p id="gdcalert30" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image30.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert31">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image30.png "image_tooltip")


Since you don't have an Identity Token, the Gateway will redirect you to Okta to get authenticated.



<p id="gdcalert31" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image31.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert32">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image31.png "image_tooltip")


Enter with the right credentials. After authentication, Okta will redirect you to the API Gateway with the Id Token injected inside the request.



<p id="gdcalert32" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image32.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert33">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image32.png "image_tooltip")



# 


# Kong + Okta - Access Control Demo Script


### Create a new Group

Click on "Directory" -> "Groups" -> "Add Group":



<p id="gdcalert33" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image33.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert34">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image33.png "image_tooltip")



### Create a new User

Click on "Directory" -> "People" -> "Add person":



<p id="gdcalert34" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image34.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert35">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image34.png "image_tooltip")


Add the new User to the Group we've just created:



<p id="gdcalert35" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image35.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert36">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image35.png "image_tooltip")


Assign the new user to the KongPKCE Application.


### Create a new Claim

Go to the "default" Authorization Server and create a new Claim:



<p id="gdcalert36" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image36.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert37">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image36.png "image_tooltip")



### Testing Token Issuing

Click on "Security" -> "API" -> "default" Authorization Server" -> "Token Preview".  and use the original user. As the user does not belong to the group the token won't have the "group" claim.



<p id="gdcalert37" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image37.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert38">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image37.png "image_tooltip")


If we use the just created User, the token will be issued with the "kong_claim":



<p id="gdcalert38" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image38.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert39">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image38.png "image_tooltip")



### kong_oidc.yaml


```
_format_version: "0.1"
service_packages:
- name: httpbinservice
  versions:
  - implementation:
      kong:
        service:
          connect_timeout: 60000
          host: httpbin.org
          id: 00000000-0000-0000-0000-000000000000
          port: 80
          protocol: http
          read_timeout: 60000
          retries: 5
          routes:
          - https_redirect_status_code: 426
            name: httpbinroute
            path_handling: v0
            paths:
            - /httpbin
            plugins:
            - config:
                minute: 5
              enabled: true
              name: rate-limiting
            - config:
                cache_ttl: 30
                strategy: memory
              enabled: true
              name: proxy-cache
            - config:
                cache_ttl: 10
                issuer: https://demo-kong.oktapreview.com/oauth2/default
                scopes:
                - scope1
              enabled: true
              name: openid-connect
            preserve_host: false
            request_buffering: true
            response_buffering: true
            strip_path: true
          - https_redirect_status_code: 426
            name: httpbinroute2
            path_handling: v1
            paths:
            - /httpbin2
            plugins:
            - config:
                client_id: [0oa2jw2dn1knwPbaf1d7]
                auth_methods: [authorization_code]
                issuer: https://demo-kong.oktapreview.com/oauth2/default
                token_endpoint_auth_method: none
                groups_required: [kong]
              enabled: true
              name: openid-connect
            preserve_host: false
            request_buffering: true
            response_buffering: true
            strip_path: true
          write_timeout: 60000
      type: kong-gateway
    version: v1

deck konnect sync --konnect-email oktaprise-support@okta.com --konnect-password 'Kongkong1!' -s kong.yaml
```



### Consume the Route

Redirect your browser to "http://a7e6a579326ec48c38020ca20d0fd990-217409592.eu-west-3.elb.amazonaws.com/httpbin2/get" and use the original username that is not included in the "kong_group".



<p id="gdcalert39" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image39.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert40">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image39.png "image_tooltip")


Since the user is not included in the "kong_group" we get a "Forbidden" error message"



<p id="gdcalert40" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image40.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert41">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image40.png "image_tooltip")



### Consume the Route with the new Username

This time, we're going to use the new username we have previously included in the "kong_group"



<p id="gdcalert41" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image41.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert42">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image41.png "image_tooltip")




<p id="gdcalert42" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image42.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert43">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image42.png "image_tooltip")


Check the issued token:



<p id="gdcalert43" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image43.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert44">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image43.png "image_tooltip")
