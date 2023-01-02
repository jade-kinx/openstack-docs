# openstack image create

이미지를 생성하고 업로드한다.

!!! tip ""
    CLI 참조: [openstack image create](https://docs.openstack.org/python-openstackclient/zed/cli/command-objects/image-v2.html#image-create)

## OpenStack Client Command
``` bash title="python3-openstackclient command"
$ openstack image create \
  --disk-format qcow2 \
  --file cirros-0.6.1-x86_64-disk.qcow2 \
  --public \
  cirros-0.6.1-x86_64-disk
```

??? note ".vscode/launch.json"
    ``` json title="configuration .vscode/launch.json"
    {
        "name": "Python: openstack image create",
        "type": "python",
        "request": "launch",
        "program": "Scripts/openstack.exe",
        "args": ["image", "create", 
            "--disk-format", "qcow2", 
            "--file", "cirros-0.6.1-x86_64-disk.qcow2", 
            "--public", 
            "cirros-0.6.1-x86_64-disk"],
        "env": {
            "OS_AUTH_URL": "http://devstack-debug/identity",
            "OS_IDENTITY_API_VERSION": "3",
            "OS_USERNAME": "admin",
            "OS_PASSWORD": "asdf",
            "OS_PROJECT_NAME": "admin",
            "OS_USER_DOMAIN_NAME": "Default",
            "OS_PROJECT_DOMAIN_NAME": "Default"
        },
        "console": "integratedTerminal",
        "justMyCode": false
    },
    ```

위 커맨드를 실행했을 때의 시퀀스 다이어그램 및 HTTP Request/Response 내용은 다음과 같다.  

## Sequence Diagram

!!! note "variables"
    `image_id`: `5f0f89ee-9472-4df0-afdf-546157734351`  
    `image_endpoint_id`: `3195d06aa49541009838146ab9072997`  
    `service_id`: `3a16cadd069e4a70b95f71316ec6f3e8`  

``` mermaid
sequenceDiagram
  autonumber
  openstack->>keystone: GET /identity
  keystone-->>openstack: 300 MULTIPLE CHOICES /identity
  openstack->>keystone: POST /identity/v3/auth/tokens
  keystone-->>openstack: 201 CREATED /identity/v3/auth/tokens
  openstack->>glance-api: GET /image
  glance-api-->>openstack: 300 Multiple Choices /image
  openstack->>glance-api: POST /image/v2/images
    glance-api->>keystone: GET /identity
    keystone-->>glance-api: 300 MULTIPLE CHOICES /identity
    glance-api->>keystone: POST /identity/v3/auth/tokens
    keystone-->>glance-api: 201 CREATED /identity/v3/auth/tokens
    glance-api->>keystone: GET /identity/v3/auth/tokens
    keystone-->>glance-api: 200 OK /identity/v3/auth/tokens
    glance-api->>keystone: GET /identity
    keystone-->>glance-api: 300 MULTIPLE CHOICES /identity
    glance-api->>keystone: POST /identity/v3/auth/tokens
    keystone-->>glance-api: 201 CREATED /identity/v3/auth/tokens
    glance-api->>keystone: GET /identity/v3/limits/model
    keystone-->>glance-api: 200 OK /identity/v3/limits/model
    glance-api->>keystone: GET /identity/v3/endpoints/{image_endpoint_id}
    keystone-->>glance-api: 200 OK /identity/v3/endpoints/{image_endpoint_id}
    glance-api->>keystone: GET /identity/v3/limits
    keystone-->>glance-api: 200 OK /identity/v3/limits
    glance-api->>keystone: GET /identity/v3/registered_limits
    keystone-->>glance-api: 200 OK /identity/v3/registered_limits
  glance-api-->>openstack: 201 Created /image/v2/images
  openstack->>glance-api: PUT /image/v2/images/{image_id}/file
    glance-api->>keystone: GET /identity/v3/limits/model
    keystone-->>glance-api: 200 OK /identity/v3/limits/model
    glance-api->>keystone: GET /identity/v3/endpoints/{image_endpoint_id}
    keystone-->>glance-api: 200 OK /identity/v3/endpoints/{image_endpoint_id}
    glance-api->>keystone: GET /identity/v3/limits
    keystone-->>glance-api: 200 OK /identity/v3/limits
    glance-api->>keystone: GET /identity/v3/registered_limits
    keystone-->>glance-api: 200 OK /identity/v3/registered_limits
    glance-api->>keystone: GET /identity/v3/limits/model
    keystone-->>glance-api: 200 OK /identity/v3/limits/model
    glance-api->>keystone: GET /identity/v3/endpoints/{image_endpoint_id}
    keystone-->>glance-api: 200 OK /identity/v3/endpoints/{image_endpoint_id}
    glance-api->>keystone: GET /identity/v3/limits
    keystone-->>glance-api: 200 OK /identity/v3/limits
    glance-api->>keystone: GET /identity/v3/registered_limits
    keystone-->>glance-api: 200 OK /identity/v3/registered_limits
    glance-api->>keystone: POST /identity/v3/auth/tokens
    keystone-->>glance-api: 201 CREATED /identity/v3/auth/tokens
      glance-api->>swift-proxy-server: HEAD /v1/AUTH_{service_id}/glance
      swift-proxy-server-->>glance-api: 204 No Content /v1/AUTH_{service_id}/glance
      glance-api->>swift-proxy-server: PUT /v1/AUTH_{service_id}/glance/{image_id}
      swift-proxy-server-->>glance-api: 201 Created /v1/AUTH_{service_id}/glance/{image_id}
  glance-api-->>openstack: 204 No Content /image/v2/images/{image_id}/file

```

이미지 서비스(`glance`)에 `cirros-0.6.1-x86_64-disk.qcow2` 이미지 파일 생성 요청에 대한 시퀀스 다이어그램이다.  

시퀀스 다이어그램을 간략하게 요약하면 아래와 같다.  

* (1)-(4): `Identity` 서비스에 `credential`을 제공하여 사용자 `access token` 발급 및 서비스 카탈로그 수신(`Image` 서비스 EndPoint)  
* (5)-(6): `Image` 서비스 버전별 EndPoint 목록 요청  
* (7): `Image` 서비스에 이미지 생성 요청  
* (8)-(11): `Image` 서비스 `access token` 발급 (project-scope)  
* (12)-(13): `Image` 서비스가 사용자 `access token` 검증 (to `Identity` 서비스)  
* (14)-(17): `Image` 서비스 `access token` 발급 (system-scope)  
* (18)-(25): 요청에 대한 `quota` 체크  
* (26): (7)번 요청에 대해, 이미지 생성 성공 응답  
* (27): 이미지 파일 업로드  
* (28)-(43): `quota` 체크  
* (44)-(45): `object-store` 서비스(`swift`)에 대해 사용할 `accecss token` 발급  
* (46)-(49): 사용자가 업로드한 이미지 파일을 `object-store`에 등록  
* (50): 사용자에게 이미지 생성 결과 응답(27번 요청)  

!!! question
    `사용자` -> `이미지` 서비스로 이미지 파일 업로드 요청(27)에 대해 `access token` validation 과정이 없는 것은???  
    있는데 빠진 것인지, 캐싱된 토큰으로 처리하는 지 확인해 보자  
    (동일한 `access token` 요청에 대해 `expire at` 까지는 캐싱된 토큰으로 처리하는 게 아닐까 추측된다.)  

!!! note
    (46)-(49) 과정에서 `Image` 서비스의 요청에 대해 `access token` validation 과정이 있을 것을 짐작할 수 있다.  
    여기서 빠져 있는 이유는, `swift` 서비스는 `requests.send()`를 사용하지 않기 때문일 것으로 짐작된다.  


### (1) GET /identity
openstack --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity"
    User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    ```

=== "Body"
    ```
    None
    ```


### (2) 300 MULTIPLE CHOICES /identity
openstack <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:56 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 274
    Location: http://182.161.114.101/identity/v3/
    Vary: X-Auth-Token
    x-openstack-request-id: req-c5b51b6c-8282-449d-b55b-8548924f860f
    Connection: close
    ```

=== "Body"
    ```
    {
      "versions": {
        "values": [
          {
            "id": "v3.14",
            "status": "stable",
            "updated": "2020-04-07T00:00:00Z",
            "links": [
              {
                "rel": "self",
                "href": "http://182.161.114.101/identity/v3/"
              }
            ],
            "media-types": [
              {
                "base": "application/json",
                "type": "application/vnd.openstack.identity-v3+json"
              }
            ]
          }
        ]
      }
    }
    ```


### (3) POST /identity/v3/auth/tokens
openstack --> keystone

=== "Header"
    ``` http title="POST http://182.161.114.101/identity/v3/auth/tokens"
    User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    Content-Type: application/json
    Content-Length: 209
    ```

=== "Body"
    ```
    {
      "auth": {
        "identity": {
          "methods": [
            "password"
          ],
          "password": {
            "user": {
              "password": "asdf",
              "name": "admin",
              "domain": {
                "id": "default"
              }
            }
          }
        },
        "scope": {
          "project": {
            "name": "admin",
            "domain": {
              "id": "default"
            }
          }
        }
      }
    }
    ```


### (4) 201 CREATED /identity/v3/auth/tokens
openstack <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:56 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 3952
    X-Subject-Token: gAAAAABjoXL82HrnIHunE9nn8sJshiB_b-JPsVXlfe-62TL2UDohLQHPnQnGfNht8J8wDIqvKr_qJLX8Dwa1ODoLMYzM18P5Iantj4P_V_PBQmi87PR_NWHJ0aV454PNymS1FcmoJhiGlT-IM-GAJkPgpI_2o371xMPlYAlTaBlQMU9gBOAPKks
    Vary: X-Auth-Token
    x-openstack-request-id: req-ea05d523-fac2-48bd-8bf1-b540db479445
    Connection: close
    ```

=== "Body"
    ```
    {
      "token": {
        "methods": [
          "password"
        ],
        "user": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "8221f17fbf934d309ba96cf2218be3ff",
          "name": "admin",
          "password_expires_at": null
        },
        "audit_ids": [
          "mPoeaEJJSTOXZUw_2UyqLA"
        ],
        "expires_at": "2022-12-20T11:31:56.000000Z",
        "issued_at": "2022-12-20T08:31:56.000000Z",
        "project": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "a5362cbd04fd4783a038d5a342d58e87",
          "name": "admin"
        },
        "is_domain": false,
        "roles": [
          {
            "id": "9ec297ce89234ce6bf2813e5e0166e4d",
            "name": "member"
          },
          {
            "id": "dbf0266266eb4ea885528545e3eb59ec",
            "name": "reader"
          },
          {
            "id": "89efad03325b4fb9a021bd88e534bbad",
            "name": "admin"
          }
        ],
        "catalog": [
          {
            "endpoints": [
              {
                "id": "06b2a46d96e349d08631686ab53d7e83",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              }
            ],
            "id": "1ca87647ab754599b632a643e5b02c7c",
            "type": "compute_legacy",
            "name": "nova_legacy"
          },
          {
            "endpoints": [
              {
                "id": "254257a27e574069b9510cc4f218afa1",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/identity",
                "region": "RegionOne"
              }
            ],
            "id": "2241c852ef204ee5bd4f4092ae9b5c7d",
            "type": "identity",
            "name": "keystone"
          },
          {
            "endpoints": [
              {
                "id": "a49d989ec6dd4e8f9399cf8a5caf519b",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              }
            ],
            "id": "2baac711c03b4da1abaabe4e0f0f3575",
            "type": "volumev3",
            "name": "cinderv3"
          },
          {
            "endpoints": [
              {
                "id": "3195d06aa49541009838146ab9072997",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/image",
                "region": "RegionOne"
              }
            ],
            "id": "4134c089d54f40c4bff6629c9b3c8b17",
            "type": "image",
            "name": "glance"
          },
          {
            "endpoints": [
              {
                "id": "be657c3c268e49c08bb0d31aa7b79b01",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:9696/networking",
                "region": "RegionOne"
              }
            ],
            "id": "59d326ea519d4ad58dcc784330c372a4",
            "type": "network",
            "name": "neutron"
          },
          {
            "endpoints": [
              {
                "id": "64319e2274594aca8b7ff17be26f1306",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080",
                "region": "RegionOne"
              },
              {
                "id": "721a5750cf0448729c85f21749859ec0",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080/v1/AUTH_a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              }
            ],
            "id": "5b4f3dd567054612914e94f37e396d05",
            "type": "object-store",
            "name": "swift"
          },
          {
            "endpoints": [
              {
                "id": "dac7532e7f2547d59c42be1309388076",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/placement",
                "region": "RegionOne"
              }
            ],
            "id": "5f8648439aa0443588164675566362a4",
            "type": "placement",
            "name": "placement"
          },
          {
            "endpoints": [
              {
                "id": "0d3a0c91848845ddb1794791ceeed1db",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              },
              {
                "id": "d4e369995c8847d9aaeeab1824dc3a02",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              },
              {
                "id": "de625690480c40878f484c10f0cc21c3",
                "interface": "internal",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              }
            ],
            "id": "985cf9347e11431bbbc8640d3c73e064",
            "type": "database",
            "name": "trove"
          },
          {
            "endpoints": [
              {
                "id": "ce7d3f34c48d4c1a877222ca8403bcbc",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              }
            ],
            "id": "ec5120d847b7457485bbbafc555df0af",
            "type": "block-storage",
            "name": "cinder"
          },
          {
            "endpoints": [
              {
                "id": "82bfd276ae734c3788bc66159b0c6d39",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2.1",
                "region": "RegionOne"
              }
            ],
            "id": "fa5e4dfaffbe476c88be86bbc10f092e",
            "type": "compute",
            "name": "nova"
          }
        ]
      }
    }
    ```


### (5) GET /image
openstack --> glance-api

=== "Header"
    ``` http title="GET http://182.161.114.101/image"
    User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    ```

=== "Body"
    ```
    None
    ```


### (6) 300 Multiple Choices /image
openstack <-- glance-api

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:56 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 1347
    Connection: close
    ```

=== "Body"
    ```
    {
      "versions": [
        {
          "id": "v2.16",
          "status": "CURRENT",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.15",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.14",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.9",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.7",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.6",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.5",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.4",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.3",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.2",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.1",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        },
        {
          "id": "v2.0",
          "status": "SUPPORTED",
          "links": [
            {
              "rel": "self",
              "href": "http://182.161.114.101/image/v2/"
            }
          ]
        }
      ]
    }
    ```


### (7) POST /image/v2/images
openstack --> glance-api

=== "Header"
    ``` http title="POST http://182.161.114.101/image/v2/images"
    User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: */*
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL82HrnIHunE9nn8sJshiB_b-JPsVXlfe-62TL2UDohLQHPnQnGfNht8J8wDIqvKr_qJLX8Dwa1ODoLMYzM18P5Iantj4P_V_PBQmi87PR_NWHJ0aV454PNymS1FcmoJhiGlT-IM-GAJkPgpI_2o371xMPlYAlTaBlQMU9gBOAPKks
    Content-Type: application/json
    Content-Length: 260
    ```

=== "Body"
    ```
    {
      "container_format": "bare",
      "visibility": "public",
      "disk_format": "qcow2",
      "name": "cirros-0.6.1-x86_64-test",
      "owner_specified.openstack.md5": "",
      "owner_specified.openstack.sha256": "",
      "owner_specified.openstack.object": "images/cirros-0.6.1-x86_64-test"
    }
    ```


### (8) GET /identity
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity"
    User-Agent: glance/25.0.1.dev4 keystonemiddleware.auth_token/10.1.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    ```

=== "Body"
    ```
    None
    ```


### (9) 300 MULTIPLE CHOICES /identity
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:56 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 274
    Location: http://182.161.114.101/identity/v3/
    Vary: X-Auth-Token
    x-openstack-request-id: req-e6308e6e-7c58-4728-b189-de230c344a58
    Connection: close
    ```

=== "Body"
    ```
    {
      "versions": {
        "values": [
          {
            "id": "v3.14",
            "status": "stable",
            "updated": "2020-04-07T00:00:00Z",
            "links": [
              {
                "rel": "self",
                "href": "http://182.161.114.101/identity/v3/"
              }
            ],
            "media-types": [
              {
                "base": "application/json",
                "type": "application/vnd.openstack.identity-v3+json"
              }
            ]
          }
        ]
      }
    }
    ```


### (10) POST /identity/v3/auth/tokens
glance-api --> keystone

=== "Header"
    ``` http title="POST http://182.161.114.101/identity/v3/auth/tokens"
    User-Agent: glance/25.0.1.dev4 keystonemiddleware.auth_token/10.1.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    Content-Type: application/json
    Content-Length: 216
    ```

=== "Body"
    ```
    {
      "auth": {
        "identity": {
          "methods": [
            "password"
          ],
          "password": {
            "user": {
              "password": "asdf",
              "name": "glance",
              "domain": {
                "name": "Default"
              }
            }
          }
        },
        "scope": {
          "project": {
            "name": "service",
            "domain": {
              "name": "Default"
            }
          }
        }
      }
    }
    ```


### (11) 201 CREATED /identity/v3/auth/tokens
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:56 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 3833
    X-Subject-Token: gAAAAABjoXL8DX9Ehi2NJdU9AbuiUjdsMuinyhrULr_qtucgoMhNXONeXlUV1YwTJ-6scB-oT6dU9fBzz1pDK2QByk1DQ3hRIdgANcy3VDS5zpXj-Epd0Yd7JhdJvMQRRwCaeghrHc5yLadGFAMRkjNlLf_GRDYGn_YLYOSlM_8jnsfORezZOLs
    Vary: X-Auth-Token
    x-openstack-request-id: req-a15494a3-4a10-48b0-83aa-7ec80657f9c6
    Connection: close
    ```

=== "Body"
    ```
    {
      "token": {
        "methods": [
          "password"
        ],
        "user": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "e154ddd315034334b04ae767862bfac0",
          "name": "glance",
          "password_expires_at": null
        },
        "audit_ids": [
          "WoxTcjiaSdavdiFe6BurVQ"
        ],
        "expires_at": "2022-12-20T11:31:56.000000Z",
        "issued_at": "2022-12-20T08:31:56.000000Z",
        "project": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "3a16cadd069e4a70b95f71316ec6f3e8",
          "name": "service"
        },
        "is_domain": false,
        "roles": [
          {
            "id": "13c44153bc6a47f0b4fd116b3b4128fe",
            "name": "service"
          }
        ],
        "catalog": [
          {
            "endpoints": [
              {
                "id": "06b2a46d96e349d08631686ab53d7e83",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              }
            ],
            "id": "1ca87647ab754599b632a643e5b02c7c",
            "type": "compute_legacy",
            "name": "nova_legacy"
          },
          {
            "endpoints": [
              {
                "id": "254257a27e574069b9510cc4f218afa1",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/identity",
                "region": "RegionOne"
              }
            ],
            "id": "2241c852ef204ee5bd4f4092ae9b5c7d",
            "type": "identity",
            "name": "keystone"
          },
          {
            "endpoints": [
              {
                "id": "a49d989ec6dd4e8f9399cf8a5caf519b",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              }
            ],
            "id": "2baac711c03b4da1abaabe4e0f0f3575",
            "type": "volumev3",
            "name": "cinderv3"
          },
          {
            "endpoints": [
              {
                "id": "3195d06aa49541009838146ab9072997",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/image",
                "region": "RegionOne"
              }
            ],
            "id": "4134c089d54f40c4bff6629c9b3c8b17",
            "type": "image",
            "name": "glance"
          },
          {
            "endpoints": [
              {
                "id": "be657c3c268e49c08bb0d31aa7b79b01",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:9696/networking",
                "region": "RegionOne"
              }
            ],
            "id": "59d326ea519d4ad58dcc784330c372a4",
            "type": "network",
            "name": "neutron"
          },
          {
            "endpoints": [
              {
                "id": "64319e2274594aca8b7ff17be26f1306",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080",
                "region": "RegionOne"
              },
              {
                "id": "721a5750cf0448729c85f21749859ec0",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080/v1/AUTH_3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              }
            ],
            "id": "5b4f3dd567054612914e94f37e396d05",
            "type": "object-store",
            "name": "swift"
          },
          {
            "endpoints": [
              {
                "id": "dac7532e7f2547d59c42be1309388076",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/placement",
                "region": "RegionOne"
              }
            ],
            "id": "5f8648439aa0443588164675566362a4",
            "type": "placement",
            "name": "placement"
          },
          {
            "endpoints": [
              {
                "id": "0d3a0c91848845ddb1794791ceeed1db",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              },
              {
                "id": "d4e369995c8847d9aaeeab1824dc3a02",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              },
              {
                "id": "de625690480c40878f484c10f0cc21c3",
                "interface": "internal",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              }
            ],
            "id": "985cf9347e11431bbbc8640d3c73e064",
            "type": "database",
            "name": "trove"
          },
          {
            "endpoints": [
              {
                "id": "ce7d3f34c48d4c1a877222ca8403bcbc",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              }
            ],
            "id": "ec5120d847b7457485bbbafc555df0af",
            "type": "block-storage",
            "name": "cinder"
          },
          {
            "endpoints": [
              {
                "id": "82bfd276ae734c3788bc66159b0c6d39",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2.1",
                "region": "RegionOne"
              }
            ],
            "id": "fa5e4dfaffbe476c88be86bbc10f092e",
            "type": "compute",
            "name": "nova"
          }
        ]
      }
    }
    ```


### (12) GET /identity/v3/auth/tokens
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/auth/tokens"
    User-Agent: python-keystoneclient
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    X-Subject-Token: gAAAAABjoXL82HrnIHunE9nn8sJshiB_b-JPsVXlfe-62TL2UDohLQHPnQnGfNht8J8wDIqvKr_qJLX8Dwa1ODoLMYzM18P5Iantj4P_V_PBQmi87PR_NWHJ0aV454PNymS1FcmoJhiGlT-IM-GAJkPgpI_2o371xMPlYAlTaBlQMU9gBOAPKks
    OpenStack-Identity-Access-Rules: 1
    X-Auth-Token: gAAAAABjoXL8DX9Ehi2NJdU9AbuiUjdsMuinyhrULr_qtucgoMhNXONeXlUV1YwTJ-6scB-oT6dU9fBzz1pDK2QByk1DQ3hRIdgANcy3VDS5zpXj-Epd0Yd7JhdJvMQRRwCaeghrHc5yLadGFAMRkjNlLf_GRDYGn_YLYOSlM_8jnsfORezZOLs
    ```

=== "Body"
    ```
    None
    ```


### (13) 200 OK /identity/v3/auth/tokens
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:56 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 3952
    X-Subject-Token: gAAAAABjoXL82HrnIHunE9nn8sJshiB_b-JPsVXlfe-62TL2UDohLQHPnQnGfNht8J8wDIqvKr_qJLX8Dwa1ODoLMYzM18P5Iantj4P_V_PBQmi87PR_NWHJ0aV454PNymS1FcmoJhiGlT-IM-GAJkPgpI_2o371xMPlYAlTaBlQMU9gBOAPKks
    Vary: X-Auth-Token
    x-openstack-request-id: req-381bf899-aecf-4901-b05a-c8c88f6da541
    Connection: close
    ```

=== "Body"
    ```
    {
      "token": {
        "methods": [
          "password"
        ],
        "user": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "8221f17fbf934d309ba96cf2218be3ff",
          "name": "admin",
          "password_expires_at": null
        },
        "audit_ids": [
          "mPoeaEJJSTOXZUw_2UyqLA"
        ],
        "expires_at": "2022-12-20T11:31:56.000000Z",
        "issued_at": "2022-12-20T08:31:56.000000Z",
        "project": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "a5362cbd04fd4783a038d5a342d58e87",
          "name": "admin"
        },
        "is_domain": false,
        "roles": [
          {
            "id": "9ec297ce89234ce6bf2813e5e0166e4d",
            "name": "member"
          },
          {
            "id": "dbf0266266eb4ea885528545e3eb59ec",
            "name": "reader"
          },
          {
            "id": "89efad03325b4fb9a021bd88e534bbad",
            "name": "admin"
          }
        ],
        "catalog": [
          {
            "endpoints": [
              {
                "id": "06b2a46d96e349d08631686ab53d7e83",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              }
            ],
            "id": "1ca87647ab754599b632a643e5b02c7c",
            "type": "compute_legacy",
            "name": "nova_legacy"
          },
          {
            "endpoints": [
              {
                "id": "254257a27e574069b9510cc4f218afa1",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/identity",
                "region": "RegionOne"
              }
            ],
            "id": "2241c852ef204ee5bd4f4092ae9b5c7d",
            "type": "identity",
            "name": "keystone"
          },
          {
            "endpoints": [
              {
                "id": "a49d989ec6dd4e8f9399cf8a5caf519b",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              }
            ],
            "id": "2baac711c03b4da1abaabe4e0f0f3575",
            "type": "volumev3",
            "name": "cinderv3"
          },
          {
            "endpoints": [
              {
                "id": "3195d06aa49541009838146ab9072997",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/image",
                "region": "RegionOne"
              }
            ],
            "id": "4134c089d54f40c4bff6629c9b3c8b17",
            "type": "image",
            "name": "glance"
          },
          {
            "endpoints": [
              {
                "id": "be657c3c268e49c08bb0d31aa7b79b01",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:9696/networking",
                "region": "RegionOne"
              }
            ],
            "id": "59d326ea519d4ad58dcc784330c372a4",
            "type": "network",
            "name": "neutron"
          },
          {
            "endpoints": [
              {
                "id": "64319e2274594aca8b7ff17be26f1306",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080",
                "region": "RegionOne"
              },
              {
                "id": "721a5750cf0448729c85f21749859ec0",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080/v1/AUTH_a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              }
            ],
            "id": "5b4f3dd567054612914e94f37e396d05",
            "type": "object-store",
            "name": "swift"
          },
          {
            "endpoints": [
              {
                "id": "dac7532e7f2547d59c42be1309388076",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/placement",
                "region": "RegionOne"
              }
            ],
            "id": "5f8648439aa0443588164675566362a4",
            "type": "placement",
            "name": "placement"
          },
          {
            "endpoints": [
              {
                "id": "0d3a0c91848845ddb1794791ceeed1db",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              },
              {
                "id": "d4e369995c8847d9aaeeab1824dc3a02",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              },
              {
                "id": "de625690480c40878f484c10f0cc21c3",
                "interface": "internal",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              }
            ],
            "id": "985cf9347e11431bbbc8640d3c73e064",
            "type": "database",
            "name": "trove"
          },
          {
            "endpoints": [
              {
                "id": "ce7d3f34c48d4c1a877222ca8403bcbc",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/a5362cbd04fd4783a038d5a342d58e87",
                "region": "RegionOne"
              }
            ],
            "id": "ec5120d847b7457485bbbafc555df0af",
            "type": "block-storage",
            "name": "cinder"
          },
          {
            "endpoints": [
              {
                "id": "82bfd276ae734c3788bc66159b0c6d39",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2.1",
                "region": "RegionOne"
              }
            ],
            "id": "fa5e4dfaffbe476c88be86bbc10f092e",
            "type": "compute",
            "name": "nova"
          }
        ]
      }
    }
    ```


### (14) GET /identity
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    ```

=== "Body"
    ```
    None
    ```


### (15) 300 MULTIPLE CHOICES /identity
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:57 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 274
    Location: http://182.161.114.101/identity/v3/
    Vary: X-Auth-Token
    x-openstack-request-id: req-4568108e-3a1e-4608-81ff-ffd470015bfe
    Connection: close
    ```

=== "Body"
    ```
    {
      "versions": {
        "values": [
          {
            "id": "v3.14",
            "status": "stable",
            "updated": "2020-04-07T00:00:00Z",
            "links": [
              {
                "rel": "self",
                "href": "http://182.161.114.101/identity/v3/"
              }
            ],
            "media-types": [
              {
                "base": "application/json",
                "type": "application/vnd.openstack.identity-v3+json"
              }
            ]
          }
        ]
      }
    }
    ```


### (16) POST /identity/v3/auth/tokens
glance-api --> keystone

=== "Header"
    ``` http title="POST http://182.161.114.101/identity/v3/auth/tokens"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    Content-Type: application/json
    Content-Length: 178
    ```

=== "Body"
    ```
    {
      "auth": {
        "identity": {
          "methods": [
            "password"
          ],
          "password": {
            "user": {
              "password": "asdf",
              "name": "glance",
              "domain": {
                "name": "Default"
              }
            }
          }
        },
        "scope": {
          "system": {
            "all": true
          }
        }
      }
    }
    ```


### (17) 201 CREATED /identity/v3/auth/tokens
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:57 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 2374
    X-Subject-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    Vary: X-Auth-Token
    x-openstack-request-id: req-c955b42f-13bd-4836-abee-b3ce5229995c
    Connection: close
    ```

=== "Body"
    ```
    {
      "token": {
        "methods": [
          "password"
        ],
        "user": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "e154ddd315034334b04ae767862bfac0",
          "name": "glance",
          "password_expires_at": null
        },
        "audit_ids": [
          "ulMYpczaQE-toXOMGVNgQA"
        ],
        "expires_at": "2022-12-20T11:31:57.000000Z",
        "issued_at": "2022-12-20T08:31:57.000000Z",
        "roles": [
          {
            "id": "dbf0266266eb4ea885528545e3eb59ec",
            "name": "reader"
          }
        ],
        "system": {
          "all": true
        },
        "catalog": [
          {
            "endpoints": [],
            "id": "1ca87647ab754599b632a643e5b02c7c",
            "type": "compute_legacy",
            "name": "nova_legacy"
          },
          {
            "endpoints": [
              {
                "id": "254257a27e574069b9510cc4f218afa1",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/identity",
                "region": "RegionOne"
              }
            ],
            "id": "2241c852ef204ee5bd4f4092ae9b5c7d",
            "type": "identity",
            "name": "keystone"
          },
          {
            "endpoints": [],
            "id": "2baac711c03b4da1abaabe4e0f0f3575",
            "type": "volumev3",
            "name": "cinderv3"
          },
          {
            "endpoints": [
              {
                "id": "3195d06aa49541009838146ab9072997",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/image",
                "region": "RegionOne"
              }
            ],
            "id": "4134c089d54f40c4bff6629c9b3c8b17",
            "type": "image",
            "name": "glance"
          },
          {
            "endpoints": [
              {
                "id": "be657c3c268e49c08bb0d31aa7b79b01",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:9696/networking",
                "region": "RegionOne"
              }
            ],
            "id": "59d326ea519d4ad58dcc784330c372a4",
            "type": "network",
            "name": "neutron"
          },
          {
            "endpoints": [
              {
                "id": "64319e2274594aca8b7ff17be26f1306",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080",
                "region": "RegionOne"
              }
            ],
            "id": "5b4f3dd567054612914e94f37e396d05",
            "type": "object-store",
            "name": "swift"
          },
          {
            "endpoints": [
              {
                "id": "dac7532e7f2547d59c42be1309388076",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/placement",
                "region": "RegionOne"
              }
            ],
            "id": "5f8648439aa0443588164675566362a4",
            "type": "placement",
            "name": "placement"
          },
          {
            "endpoints": [],
            "id": "985cf9347e11431bbbc8640d3c73e064",
            "type": "database",
            "name": "trove"
          },
          {
            "endpoints": [],
            "id": "ec5120d847b7457485bbbafc555df0af",
            "type": "block-storage",
            "name": "cinder"
          },
          {
            "endpoints": [
              {
                "id": "82bfd276ae734c3788bc66159b0c6d39",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2.1",
                "region": "RegionOne"
              }
            ],
            "id": "fa5e4dfaffbe476c88be86bbc10f092e",
            "type": "compute",
            "name": "nova"
          }
        ]
      }
    }
    ```


### (18) GET /identity/v3/limits/model
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/limits/model"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: */*
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (19) 200 OK /identity/v3/limits/model
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:57 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 131
    Vary: X-Auth-Token
    x-openstack-request-id: req-4dcd0e0e-d554-4328-809b-58de8dc720ff
    Connection: close
    ```

=== "Body"
    ```
    {
      "model": {
        "name": "flat",
        "description": "Limit enforcement and validation does not take project hierarchy into consideration."
      }
    }
    ```


### (20) GET /identity/v3/endpoints/3195d06aa49541009838146ab9072997
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/endpoints/3195d06aa49541009838146ab9072997"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: */*
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (21) 200 OK /identity/v3/endpoints/3195d06aa49541009838146ab9072997
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:57 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 335
    Vary: X-Auth-Token
    x-openstack-request-id: req-6dcfa36b-b9d0-42fc-820a-701b1d020794
    Connection: close
    ```

=== "Body"
    ```
    {
      "endpoint": {
        "id": "3195d06aa49541009838146ab9072997",
        "interface": "public",
        "region_id": "RegionOne",
        "service_id": "4134c089d54f40c4bff6629c9b3c8b17",
        "url": "http://182.161.114.101/image",
        "enabled": true,
        "region": "RegionOne",
        "links": {
          "self": "http://182.161.114.101/identity/v3/endpoints/3195d06aa49541009838146ab9072997"
        }
      }
    }
    ```


### (22) GET /identity/v3/limits
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_count_total&project_id=a5362cbd04fd4783a038d5a342d58e87"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (23) 200 OK /identity/v3/limits
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:57 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 251
    Vary: X-Auth-Token
    x-openstack-request-id: req-70f0c115-8e5c-46f6-a34b-d0f312d3a3f7
    Connection: close
    ```

=== "Body"
    ```
    {
      "limits": [],
      "links": {
        "next": null,
        "self": "http://182.161.114.101/identity/v3/limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_count_total&project_id=a5362cbd04fd4783a038d5a342d58e87",
        "previous": null
      }
    }
    ```


### (24) GET /identity/v3/registered_limits
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/registered_limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_count_total"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (25) 200 OK /identity/v3/registered_limits
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:57 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 536
    Vary: X-Auth-Token
    x-openstack-request-id: req-7057ce68-6733-4d01-9e88-9d08577b80b4
    Connection: close
    ```

=== "Body"
    ```
    {
      "registered_limits": [
        {
          "id": "947b046d15744348860246b59fbe412e",
          "service_id": "4134c089d54f40c4bff6629c9b3c8b17",
          "region_id": "RegionOne",
          "resource_name": "image_count_total",
          "default_limit": 100,
          "description": null,
          "links": {
            "self": "http://182.161.114.101/identity/v3/registered_limits/947b046d15744348860246b59fbe412e"
          }
        }
      ],
      "links": {
        "next": null,
        "self": "http://182.161.114.101/identity/v3/registered_limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_count_total",
        "previous": null
      }
    }
    ```


### (26) 201 Created /image/v2/images
openstack <-- glance-api

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:58 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Length: 781
    Content-Type: application/json
    Location: http://127.0.0.1:60999/v2/images/5f0f89ee-9472-4df0-afdf-546157734351
    Openstack-Image-Import-Methods: glance-direct,web-download,copy-image
    X-Openstack-Request-Id: req-e8ca2b78-6151-408e-8d4f-91f092fe1b0f
    Connection: close
    ```

=== "Body"
    ```
    {
      "owner_specified.openstack.md5": "",
      "owner_specified.openstack.sha256": "",
      "owner_specified.openstack.object": "images/cirros-0.6.1-x86_64-test",
      "name": "cirros-0.6.1-x86_64-test",
      "disk_format": "qcow2",
      "container_format": "bare",
      "visibility": "public",
      "size": null,
      "virtual_size": null,
      "status": "queued",
      "checksum": null,
      "protected": false,
      "min_ram": 0,
      "min_disk": 0,
      "owner": "a5362cbd04fd4783a038d5a342d58e87",
      "os_hidden": false,
      "os_hash_algo": null,
      "os_hash_value": null,
      "id": "5f0f89ee-9472-4df0-afdf-546157734351",
      "created_at": "2022-12-20T08:31:59Z",
      "updated_at": "2022-12-20T08:31:59Z",
      "tags": [],
      "self": "/v2/images/5f0f89ee-9472-4df0-afdf-546157734351",
      "file": "/v2/images/5f0f89ee-9472-4df0-afdf-546157734351/file",
      "schema": "/v2/schemas/image"
    }
    ```


### (27) PUT /image/v2/images/5f0f89ee-9472-4df0-afdf-546157734351/file
openstack --> glance-api

=== "Header"
    ``` http title="PUT http://182.161.114.101/image/v2/images/5f0f89ee-9472-4df0-afdf-546157734351/file"
    User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: 
    Connection: keep-alive
    Content-Type: application/octet-stream
    X-Auth-Token: gAAAAABjoXL82HrnIHunE9nn8sJshiB_b-JPsVXlfe-62TL2UDohLQHPnQnGfNht8J8wDIqvKr_qJLX8Dwa1ODoLMYzM18P5Iantj4P_V_PBQmi87PR_NWHJ0aV454PNymS1FcmoJhiGlT-IM-GAJkPgpI_2o371xMPlYAlTaBlQMU9gBOAPKks
    Content-Length: 21233664
    ```

=== "Body"
    ```
    <_io.BufferedReader name='cirros-0.6.1-x86_64-disk.img'>
    ```


### (28) GET /identity/v3/limits/model
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/limits/model"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: */*
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (29) 200 OK /identity/v3/limits/model
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:58 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 131
    Vary: X-Auth-Token
    x-openstack-request-id: req-c4d5173d-956f-4003-afe9-57ef8b2cc45c
    Connection: close
    ```

=== "Body"
    ```
    {
      "model": {
        "name": "flat",
        "description": "Limit enforcement and validation does not take project hierarchy into consideration."
      }
    }
    ```


### (30) GET /identity/v3/endpoints/3195d06aa49541009838146ab9072997
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/endpoints/3195d06aa49541009838146ab9072997"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: */*
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (31) 200 OK /identity/v3/endpoints/3195d06aa49541009838146ab9072997
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:58 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 335
    Vary: X-Auth-Token
    x-openstack-request-id: req-31fe9cc7-8ce8-42fa-a03f-cdafae531c34
    Connection: close
    ```

=== "Body"
    ```
    {
      "endpoint": {
        "id": "3195d06aa49541009838146ab9072997",
        "interface": "public",
        "region_id": "RegionOne",
        "service_id": "4134c089d54f40c4bff6629c9b3c8b17",
        "url": "http://182.161.114.101/image",
        "enabled": true,
        "region": "RegionOne",
        "links": {
          "self": "http://182.161.114.101/identity/v3/endpoints/3195d06aa49541009838146ab9072997"
        }
      }
    }
    ```


### (32) GET /identity/v3/limits
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_size_total&project_id=a5362cbd04fd4783a038d5a342d58e87"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (33) 200 OK /identity/v3/limits
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:59 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 250
    Vary: X-Auth-Token
    x-openstack-request-id: req-8a67a6d9-09ae-4079-b71c-d61659bd011d
    Connection: close
    ```

=== "Body"
    ```
    {
      "limits": [],
      "links": {
        "next": null,
        "self": "http://182.161.114.101/identity/v3/limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_size_total&project_id=a5362cbd04fd4783a038d5a342d58e87",
        "previous": null
      }
    }
    ```


### (34) GET /identity/v3/registered_limits
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/registered_limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_size_total"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (35) 200 OK /identity/v3/registered_limits
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:59 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 535
    Vary: X-Auth-Token
    x-openstack-request-id: req-90043142-d69d-43b8-8cdd-955b37bc322f
    Connection: close
    ```

=== "Body"
    ```
    {
      "registered_limits": [
        {
          "id": "a05124bb153d4bccab75eaeef20b2564",
          "service_id": "4134c089d54f40c4bff6629c9b3c8b17",
          "region_id": "RegionOne",
          "resource_name": "image_size_total",
          "default_limit": 1000,
          "description": null,
          "links": {
            "self": "http://182.161.114.101/identity/v3/registered_limits/a05124bb153d4bccab75eaeef20b2564"
          }
        }
      ],
      "links": {
        "next": null,
        "self": "http://182.161.114.101/identity/v3/registered_limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_size_total",
        "previous": null
      }
    }
    ```


### (36) GET /identity/v3/limits/model
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/limits/model"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: */*
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (37) 200 OK /identity/v3/limits/model
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:59 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 131
    Vary: X-Auth-Token
    x-openstack-request-id: req-ade6eea5-bcea-446e-997d-6282b1a1a64f
    Connection: close
    ```

=== "Body"
    ```
    {
      "model": {
        "name": "flat",
        "description": "Limit enforcement and validation does not take project hierarchy into consideration."
      }
    }
    ```


### (38) GET /identity/v3/endpoints/3195d06aa49541009838146ab9072997
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/endpoints/3195d06aa49541009838146ab9072997"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: */*
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (39) 200 OK /identity/v3/endpoints/3195d06aa49541009838146ab9072997
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:59 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 335
    Vary: X-Auth-Token
    x-openstack-request-id: req-46ed5e88-0aa5-4337-b78a-6b59d085f783
    Connection: close
    ```

=== "Body"
    ```
    {
      "endpoint": {
        "id": "3195d06aa49541009838146ab9072997",
        "interface": "public",
        "region_id": "RegionOne",
        "service_id": "4134c089d54f40c4bff6629c9b3c8b17",
        "url": "http://182.161.114.101/image",
        "enabled": true,
        "region": "RegionOne",
        "links": {
          "self": "http://182.161.114.101/identity/v3/endpoints/3195d06aa49541009838146ab9072997"
        }
      }
    }
    ```


### (40) GET /identity/v3/limits
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_count_uploading&project_id=a5362cbd04fd4783a038d5a342d58e87"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (41) 200 OK /identity/v3/limits
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:31:59 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 255
    Vary: X-Auth-Token
    x-openstack-request-id: req-0564bf0a-a020-4fdf-82c2-52fd93aa7170
    Connection: close
    ```

=== "Body"
    ```
    {
      "limits": [],
      "links": {
        "next": null,
        "self": "http://182.161.114.101/identity/v3/limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_count_uploading&project_id=a5362cbd04fd4783a038d5a342d58e87",
        "previous": null
      }
    }
    ```


### (42) GET /identity/v3/registered_limits
glance-api --> keystone

=== "Header"
    ``` http title="GET http://182.161.114.101/identity/v3/registered_limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_count_uploading"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    X-Auth-Token: gAAAAABjoXL91KPiyX946fRpSRYTc-VtL5Pr7wxAp89sEGTQ6-xlObWs1E0_Rttq8Y5SeC3hg3jj5mLB69TrKLWSLzwMwEd16FXLMQ2xKuJFcn0RFM54nX95YHxyQVrh5617TC0V1dQAKQAeoSEZ9yoz5AtyKoCpJw
    ```

=== "Body"
    ```
    None
    ```


### (43) 200 OK /identity/v3/registered_limits
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:32:00 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 544
    Vary: X-Auth-Token
    x-openstack-request-id: req-546ffba2-50c4-4487-a2fe-cb408bc5a552
    Connection: close
    ```

=== "Body"
    ```
    {
      "registered_limits": [
        {
          "id": "00d03c3ab2e54a9dbecc16794cd301ee",
          "service_id": "4134c089d54f40c4bff6629c9b3c8b17",
          "region_id": "RegionOne",
          "resource_name": "image_count_uploading",
          "default_limit": 100,
          "description": null,
          "links": {
            "self": "http://182.161.114.101/identity/v3/registered_limits/00d03c3ab2e54a9dbecc16794cd301ee"
          }
        }
      ],
      "links": {
        "next": null,
        "self": "http://182.161.114.101/identity/v3/registered_limits?service_id=4134c089d54f40c4bff6629c9b3c8b17&region_id=RegionOne&resource_name=image_count_uploading",
        "previous": null
      }
    }
    ```


### (44) POST /identity/v3/auth/tokens
glance-api --> keystone

=== "Header"
    ``` http title="POST http://182.161.114.101/identity/v3/auth/tokens"
    User-Agent: glance-api keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    Content-Type: application/json
    Content-Length: 218
    ```

=== "Body"
    ```
    {
      "auth": {
        "identity": {
          "methods": [
            "password"
          ],
          "password": {
            "user": {
              "password": "asdf",
              "name": "glance-swift",
              "domain": {
                "id": "default"
              }
            }
          }
        },
        "scope": {
          "project": {
            "name": "service",
            "domain": {
              "id": "default"
            }
          }
        }
      }
    }
    ```


### (45) 201 CREATED /identity/v3/auth/tokens
glance-api <-- keystone

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:32:00 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: application/json
    Content-Length: 3908
    X-Subject-Token: gAAAAABjoXMAM3KIwS07erJzG3nnWmTkAvs82Jwzme5DdoL6nCgNYItVNS9zkPqTM41_qdwiqFqCEfq6y_pHi-XzVqNMo1p-J4ZoURy1Cr23n7Z81ArVkEazFC_Do40MbopN00Ne4aX2k8JxBzqqz4mLE9iFbIwULBu7oLiopDAZa2iRFHrIDUE
    Vary: X-Auth-Token
    x-openstack-request-id: req-521c4c10-60ec-4e64-9d0a-be1532ab7ccb
    Connection: close
    ```

=== "Body"
    ```
    {
      "token": {
        "methods": [
          "password"
        ],
        "user": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "ab4a76f145da47358af69726f9f18354",
          "name": "glance-swift",
          "password_expires_at": null
        },
        "audit_ids": [
          "480U00I-RtKFf2wJqpzOGw"
        ],
        "expires_at": "2022-12-20T11:32:00.000000Z",
        "issued_at": "2022-12-20T08:32:00.000000Z",
        "project": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "3a16cadd069e4a70b95f71316ec6f3e8",
          "name": "service"
        },
        "is_domain": false,
        "roles": [
          {
            "id": "9ca87e2f3c074bab876f4574ca5ea89a",
            "name": "ResellerAdmin"
          },
          {
            "id": "13c44153bc6a47f0b4fd116b3b4128fe",
            "name": "service"
          }
        ],
        "catalog": [
          {
            "endpoints": [
              {
                "id": "06b2a46d96e349d08631686ab53d7e83",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              }
            ],
            "id": "1ca87647ab754599b632a643e5b02c7c",
            "type": "compute_legacy",
            "name": "nova_legacy"
          },
          {
            "endpoints": [
              {
                "id": "254257a27e574069b9510cc4f218afa1",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/identity",
                "region": "RegionOne"
              }
            ],
            "id": "2241c852ef204ee5bd4f4092ae9b5c7d",
            "type": "identity",
            "name": "keystone"
          },
          {
            "endpoints": [
              {
                "id": "a49d989ec6dd4e8f9399cf8a5caf519b",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              }
            ],
            "id": "2baac711c03b4da1abaabe4e0f0f3575",
            "type": "volumev3",
            "name": "cinderv3"
          },
          {
            "endpoints": [
              {
                "id": "3195d06aa49541009838146ab9072997",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/image",
                "region": "RegionOne"
              }
            ],
            "id": "4134c089d54f40c4bff6629c9b3c8b17",
            "type": "image",
            "name": "glance"
          },
          {
            "endpoints": [
              {
                "id": "be657c3c268e49c08bb0d31aa7b79b01",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:9696/networking",
                "region": "RegionOne"
              }
            ],
            "id": "59d326ea519d4ad58dcc784330c372a4",
            "type": "network",
            "name": "neutron"
          },
          {
            "endpoints": [
              {
                "id": "64319e2274594aca8b7ff17be26f1306",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080",
                "region": "RegionOne"
              },
              {
                "id": "721a5750cf0448729c85f21749859ec0",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080/v1/AUTH_3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              }
            ],
            "id": "5b4f3dd567054612914e94f37e396d05",
            "type": "object-store",
            "name": "swift"
          },
          {
            "endpoints": [
              {
                "id": "dac7532e7f2547d59c42be1309388076",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/placement",
                "region": "RegionOne"
              }
            ],
            "id": "5f8648439aa0443588164675566362a4",
            "type": "placement",
            "name": "placement"
          },
          {
            "endpoints": [
              {
                "id": "0d3a0c91848845ddb1794791ceeed1db",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              },
              {
                "id": "d4e369995c8847d9aaeeab1824dc3a02",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              },
              {
                "id": "de625690480c40878f484c10f0cc21c3",
                "interface": "internal",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              }
            ],
            "id": "985cf9347e11431bbbc8640d3c73e064",
            "type": "database",
            "name": "trove"
          },
          {
            "endpoints": [
              {
                "id": "ce7d3f34c48d4c1a877222ca8403bcbc",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/3a16cadd069e4a70b95f71316ec6f3e8",
                "region": "RegionOne"
              }
            ],
            "id": "ec5120d847b7457485bbbafc555df0af",
            "type": "block-storage",
            "name": "cinder"
          },
          {
            "endpoints": [
              {
                "id": "82bfd276ae734c3788bc66159b0c6d39",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2.1",
                "region": "RegionOne"
              }
            ],
            "id": "fa5e4dfaffbe476c88be86bbc10f092e",
            "type": "compute",
            "name": "nova"
          }
        ]
      }
    }
    ```


### (46) HEAD /v1/AUTH_3a16cadd069e4a70b95f71316ec6f3e8/glance
glance-api --> swift-proxy-server

=== "Header"
    ``` http title="HEAD http://182.161.114.101:8080/v1/AUTH_3a16cadd069e4a70b95f71316ec6f3e8/glance"
    x-auth-token: b'gAAAAABjoXMAM3KIwS07erJzG3nnWmTkAvs82Jwzme5DdoL6nCgNYItVNS9zkPqTM41_qdwiqFqCEfq6y_pHi-XzVqNMo1p-J4ZoURy1Cr23n7Z81ArVkEazFC_Do40MbopN00Ne4aX2k8JxBzqqz4mLE9iFbIwULBu7oLiopDAZa2iRFHrIDUE'
    user-agent: python-swiftclient-4.1.0
    ```

=== "Body"
    ```
    None
    ```


### (47) 204 No Content /v1/AUTH_3a16cadd069e4a70b95f71316ec6f3e8/glance
glance-api <-- swift-proxy-server

=== "Header"
    ```
    Content-Type: text/plain; charset=utf-8
    X-Container-Object-Count: 3
    X-Container-Bytes-Used: 1070578176
    X-Timestamp: 1670302054.88139
    Last-Modified: Tue, 06 Dec 2022 04:47:35 GMT
    Accept-Ranges: bytes
    X-Storage-Policy: Policy-0
    X-Container-Sharding: False
    Vary: Accept
    Content-Length: 0
    X-Trans-Id: tx18c5ea2e55624522aaeb4-0063a17300
    X-Openstack-Request-Id: tx18c5ea2e55624522aaeb4-0063a17300
    Date: Tue, 20 Dec 2022 08:32:00 GMT
    ```

=== "Body"
    ```
    None
    ```


### (48) PUT /v1/AUTH_3a16cadd069e4a70b95f71316ec6f3e8/glance/5f0f89ee-9472-4df0-afdf-546157734351
glance-api --> swift-proxy-server

=== "Header"
    ``` http title="PUT http://182.161.114.101:8080/v1/AUTH_3a16cadd069e4a70b95f71316ec6f3e8/glance/5f0f89ee-9472-4df0-afdf-546157734351"
    x-auth-token: b'gAAAAABjoXMAM3KIwS07erJzG3nnWmTkAvs82Jwzme5DdoL6nCgNYItVNS9zkPqTM41_qdwiqFqCEfq6y_pHi-XzVqNMo1p-J4ZoURy1Cr23n7Z81ArVkEazFC_Do40MbopN00Ne4aX2k8JxBzqqz4mLE9iFbIwULBu7oLiopDAZa2iRFHrIDUE'
    Content-Length: 21233664
    user-agent: python-swiftclient-4.1.0
    ```

=== "Body"
    ```
    <swiftclient.utils.LengthWrapper object at 0x7f402c712fd0>
    ```


### (49) 201 Created /v1/AUTH_3a16cadd069e4a70b95f71316ec6f3e8/glance/5f0f89ee-9472-4df0-afdf-546157734351
glance-api <-- swift-proxy-server

=== "Header"
    ```
    Content-Type: text/html; charset=UTF-8
    Content-Length: 0
    Etag: 0c839612eb3f2469420f2ccae990827f
    Last-Modified: Tue, 20 Dec 2022 08:32:01 GMT
    X-Trans-Id: tx606a0770becf4bf886e21-0063a17300
    X-Openstack-Request-Id: tx606a0770becf4bf886e21-0063a17300
    Date: Tue, 20 Dec 2022 08:32:02 GMT
    ```

=== "Body"
    ```
    None
    ```


### (50) 204 No Content /image/v2/images/5f0f89ee-9472-4df0-afdf-546157734351/file
openstack <-- glance-api

=== "Header"
    ```
    Date: Tue, 20 Dec 2022 08:32:02 GMT
    Server: Apache/2.4.41 (Ubuntu)
    Content-Type: text/html; charset=UTF-8
    X-Openstack-Request-Id: req-2b608edf-df19-4820-9d6c-3c1c16f764d6
    Connection: close
    ```

=== "Body"
    ```
    None
    ```

??? quote "openstack image create logs"
    ``` log title="/var/log/requests-observer/requests.log" linenums="1"
    --8<-- "openstack/sequence/image/logs/openstack-image-create-requests.log"
    ```



## Output

``` json title="openstack image create --disk-format qcow2 --file cirros-0.6.1-x86_64-disk.img --public cirros-0.6.1-x86_64-disk --format json"
{
  "container_format": "bare",
  "created_at": "2022-12-20T08:31:59Z",
  "disk_format": "qcow2",
  "file": "/v2/images/5f0f89ee-9472-4df0-afdf-546157734351/file",
  "id": "5f0f89ee-9472-4df0-afdf-546157734351",
  "min_disk": 0,
  "min_ram": 0,
  "name": "cirros-0.6.1-x86_64-test",
  "owner": "a5362cbd04fd4783a038d5a342d58e87",
  "properties": {
    "os_hidden": false,
    "owner_specified.openstack.md5": "",
    "owner_specified.openstack.sha256": "",
    "owner_specified.openstack.object": "images/cirros-0.6.1-x86_64-test"
  },
  "protected": false,
  "schema": "/v2/schemas/image",
  "status": "queued",
  "tags": [],
  "updated_at": "2022-12-20T08:31:59Z",
  "visibility": "public"
}
```