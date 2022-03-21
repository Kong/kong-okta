# Kong + Okta - Client Credentials Demo Script

The "Client Credentials" Flow is defined by OAuth to address scenarios like Application Authentication, Machine-to-Machine, Microservices Accounts, etc. For user-oriented flows, please go to the next "Authorization Code" section. Please refer to this link to learn more about "[Client Credentials](https://developer.okta.com/docs/guides/implement-client-creds/)".


### Okta Application Creation

Log in to Okta console using the Kong + Okta endpoint [https://demo-kong.oktapreview.com/](https://demo-kong.oktapreview.com/)

![clientcredentialsapp](images/clientcredentialsapp.png)


Click on "Applications" -> "Applications" -> "Create App Integration". Create a "API Services" Application named KongClientCredentials:

![clientcredentialsapp2](images/clientcredentialsapp2.png)


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


### Next Steps

[Authorization Code](https://github.com/Kong/kong-okta/blob/main/KongOkta-SharedDemo/Authorization%20Code/Authorization%20Code.md)


