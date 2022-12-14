
### (1) GET /identity/
`openstack` --> `keystone`

=== "Header"
    ``` http title="GET /identity/" linenums="1"
    Host: 182.161.114.101
    User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    X-Requestshook-Request-Id: 05c5eca7b10f4e0f845af842e6af7513
    X-Requestshook-Request-From: openstack
    ```

=== "Body"
    ``` json title="GET /identity/" linenums="1"
    none
    ```


### (2) 300 MULTIPLE CHOICES /identity/
`openstack` <-- `keystone`

=== "Header"
    ``` http title="300 MULTIPLE CHOICES /identity/" linenums="1"
    Content-Type: application/json
    Content-Length: 274
    Location: http://182.161.114.101/identity/v3/
    Vary: X-Auth-Token
    X-Requestshook-Request-Id: 05c5eca7b10f4e0f845af842e6af7513
    X-Requestshook-Request-From: openstack
    ```

=== "Body"
    ``` json title="300 MULTIPLE CHOICES /identity/" linenums="1"
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
`openstack` --> `keystone`

!!! reference "API 참조 - Password authentication with scoped authorization"
    [POST /v3/auth/tokens](https://docs.openstack.org/api-ref/identity/v3/index.html?expanded=password-authentication-with-scoped-authorization-detail#password-authentication-with-scoped-authorization)

=== "Header"
    ``` http title="POST /identity/v3/auth/tokens" linenums="1"
    Host: 182.161.114.101
    User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    Content-Type: application/json
    Content-Length: 209
    X-Requestshook-Request-Id: e979001d70504f179b506f86cb99a80c
    X-Requestshook-Request-From: openstack
    ```

=== "Body"
    ``` json title="POST /identity/v3/auth/tokens" linenums="1"
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
`openstack` <-- `keystone`

=== "Header"
    ``` http title="201 CREATED /identity/v3/auth/tokens" linenums="1" hl_lines="3"
    Content-Type: application/json
    Content-Length: 3952
    X-Subject-Token: gAAAAABjso1tRVlid2ys_vShS8yICW15ZVZgacI39RPWc_2q52Q_mnJbnf6N3zVmOw7naEEf6y637rJ-6fFz8TFkjMh8dd0tbh2o_Un7vajESqEiTqPrybmrF2YmF7zbxTrJNrP0KWyLY94ac11pnZRHkt9G0mLScpCq7VXzryAjsKAylxEzRZM
    Vary: X-Auth-Token
    X-Requestshook-Request-Id: e979001d70504f179b506f86cb99a80c
    X-Requestshook-Request-From: openstack
    ```

=== "Body"
    ``` json title="201 CREATED /identity/v3/auth/tokens" linenums="1"
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
          "id": "f7a842b4104247debd7589051c5321fc",
          "name": "admin",
          "password_expires_at": null
        },
        "audit_ids": [
          "YEHgNS8eQNOmdfiKvGsuHA"
        ],
        "expires_at": "2023-01-02T10:53:17.000000Z",
        "issued_at": "2023-01-02T07:53:17.000000Z",
        "project": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "de5af600557d44d0996e667499376dbb",
          "name": "admin"
        },
        "is_domain": false,
        "roles": [
          {
            "id": "28dea0246b6f47df9d99b5b33b423c8b",
            "name": "reader"
          },
          {
            "id": "5f0594b46c8346d2996759ed1f5014f4",
            "name": "member"
          },
          {
            "id": "3e1adda493764db8921e97d68a5c8bc7",
            "name": "admin"
          }
        ],
        "catalog": [
          {
            "endpoints": [
              {
                "id": "6fbbbec7587e4989a4de6e7a43c7205f",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/placement",
                "region": "RegionOne"
              }
            ],
            "id": "10304edc7ec942d282567b8c4f5610d6",
            "type": "placement",
            "name": "placement"
          },
          {
            "endpoints": [
              {
                "id": "366c8616c77243d38971272e0eaf4a9f",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2.1",
                "region": "RegionOne"
              }
            ],
            "id": "117766093c004578b95a185ecf2a5bbf",
            "type": "compute",
            "name": "nova"
          },
          {
            "endpoints": [
              {
                "id": "0c0d6e47d833442f8da165ada8ad43a3",
                "interface": "internal",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              },
              {
                "id": "9440e2831cc84d4da9f7d20c7b42638e",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              },
              {
                "id": "c812ae1a5a3e40449f3c61edae4a6bcb",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              }
            ],
            "id": "22844f546aa941798f06e4978be9ab94",
            "type": "database",
            "name": "trove"
          },
          {
            "endpoints": [
              {
                "id": "fad6bb86523b4f3aba4a6a6fb25cda0e",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/image",
                "region": "RegionOne"
              }
            ],
            "id": "3b7aead421ce4611a82e684ab8c2ad2f",
            "type": "image",
            "name": "glance"
          },
          {
            "endpoints": [
              {
                "id": "82c559324ad244beacb2fd0406f0298b",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080",
                "region": "RegionOne"
              },
              {
                "id": "d1029ab4d5bf4cd6843a5b819966c2ea",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080/v1/AUTH_de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              }
            ],
            "id": "628d3cea8d3b4615a4b047bae6112d0e",
            "type": "object-store",
            "name": "swift"
          },
          {
            "endpoints": [
              {
                "id": "a7eecf5314434aca81e5feaa8bf7521e",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/identity",
                "region": "RegionOne"
              }
            ],
            "id": "7055978143b74523b826bae1a9c0aabf",
            "type": "identity",
            "name": "keystone"
          },
          {
            "endpoints": [
              {
                "id": "30dd85ebed584c54ba849ca0e2a4c825",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:9696/networking",
                "region": "RegionOne"
              }
            ],
            "id": "b3b850a4ec574d16a0cbece3c172a701",
            "type": "network",
            "name": "neutron"
          },
          {
            "endpoints": [
              {
                "id": "871e0e3393974f9b9071342b59f62090",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              }
            ],
            "id": "cb1921a9c779492484907683bb447e84",
            "type": "compute_legacy",
            "name": "nova_legacy"
          },
          {
            "endpoints": [
              {
                "id": "b6a779acd3e04ed480847d4cb99ca710",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              }
            ],
            "id": "d5345b92210c49aa975a873d04921780",
            "type": "block-storage",
            "name": "cinder"
          },
          {
            "endpoints": [
              {
                "id": "8c624f7ed1e44096b8f0413866a11b1b",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              }
            ],
            "id": "e559802e75824163a070bd92dd143476",
            "type": "volumev3",
            "name": "cinderv3"
          }
        ]
      }
    }
    ```


### (5) GET /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c
`openstack` --> `glance-api`

!!! reference "API 참조 - Show Image"
    [GET /v2/images/{image_id}](https://docs.openstack.org/api-ref/image/v2/index.html?expanded=show-image-detail#show-image) 

=== "Header"
    ``` http title="GET /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c" linenums="1" hl_lines="5"
    Host: 127.0.0.1:60999
    User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: */*
    X-Auth-Token: gAAAAABjso1tRVlid2ys_vShS8yICW15ZVZgacI39RPWc_2q52Q_mnJbnf6N3zVmOw7naEEf6y637rJ-6fFz8TFkjMh8dd0tbh2o_Un7vajESqEiTqPrybmrF2YmF7zbxTrJNrP0KWyLY94ac11pnZRHkt9G0mLScpCq7VXzryAjsKAylxEzRZM
    X-Requestshook-Request-Id: 94b0932943f249989abf482444545486
    X-Requestshook-Request-From: openstack
    X-Forwarded-For: 182.161.114.101
    X-Forwarded-Host: 182.161.114.101
    X-Forwarded-Server: 127.0.0.1
    Connection: Keep-Alive
    ```

=== "Body"
    ``` json title="GET /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c" linenums="1"
    none
    ```


### (6) GET /identity/v3/auth/tokens
`glance-api` --> `keystone`

!!! reference "API 참조 - Validate and show information for token"
    [GET /v3/auth/tokens](https://docs.openstack.org/api-ref/identity/v3/index.html?expanded=validate-and-show-information-for-token-detail#validate-and-show-information-for-token)

`X-Auth-Token`에 사용된 인증 토큰은 `glance-api`가 이미 보유하고 있는 인증 토큰이다.  
만약, 만료된 토큰이라면 이 요청은 실패를 응답하고, 다시 인증 토큰을 발급 받는 과정을 거쳐야 한다.  

=== "Header"
    ``` http title="GET /identity/v3/auth/tokens" linenums="1" hl_lines="6 8"
    Host: 182.161.114.101
    User-Agent: python-keystoneclient
    Accept-Encoding: gzip, deflate
    Accept: application/json
    Connection: keep-alive
    X-Subject-Token: gAAAAABjso1tRVlid2ys_vShS8yICW15ZVZgacI39RPWc_2q52Q_mnJbnf6N3zVmOw7naEEf6y637rJ-6fFz8TFkjMh8dd0tbh2o_Un7vajESqEiTqPrybmrF2YmF7zbxTrJNrP0KWyLY94ac11pnZRHkt9G0mLScpCq7VXzryAjsKAylxEzRZM
    Openstack-Identity-Access-Rules: 1
    X-Auth-Token: gAAAAABjsngzWyVGDHbiSj5O8qRegJFlrF6uhymf73wRY30-fdJy6Z5__WEEU_v9HN2B1fKW4tjnkh673cmzR2781Rz8taI2YJh8vI_0zgDRvwUxWzGGIhUCh7554NDMLnvCrcvVGvIdBVONzHpcKBc2cEwaU0KQIAunm5Mk4kOXkl8pN32scro
    X-Requestshook-Request-Id: 2b68b2de403d4be7b8bc7680612af7dc
    X-Requestshook-Request-From: glance-api
    ```

=== "Body"
    ``` json title="GET /identity/v3/auth/tokens" linenums="1"
    none
    ```


### (7) 200 OK /identity/v3/auth/tokens
`glance-api` <-- `keystone`

=== "Header"
    ``` http title="200 OK /identity/v3/auth/tokens" linenums="1"
    Content-Type: application/json
    Content-Length: 3952
    X-Subject-Token: gAAAAABjso1tRVlid2ys_vShS8yICW15ZVZgacI39RPWc_2q52Q_mnJbnf6N3zVmOw7naEEf6y637rJ-6fFz8TFkjMh8dd0tbh2o_Un7vajESqEiTqPrybmrF2YmF7zbxTrJNrP0KWyLY94ac11pnZRHkt9G0mLScpCq7VXzryAjsKAylxEzRZM
    Vary: X-Auth-Token
    X-Requestshook-Request-Id: 2b68b2de403d4be7b8bc7680612af7dc
    X-Requestshook-Request-From: glance-api
    ```

=== "Body"
    ``` json title="200 OK /identity/v3/auth/tokens" linenums="1"
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
          "id": "f7a842b4104247debd7589051c5321fc",
          "name": "admin",
          "password_expires_at": null
        },
        "audit_ids": [
          "YEHgNS8eQNOmdfiKvGsuHA"
        ],
        "expires_at": "2023-01-02T10:53:17.000000Z",
        "issued_at": "2023-01-02T07:53:17.000000Z",
        "project": {
          "domain": {
            "id": "default",
            "name": "Default"
          },
          "id": "de5af600557d44d0996e667499376dbb",
          "name": "admin"
        },
        "is_domain": false,
        "roles": [
          {
            "id": "28dea0246b6f47df9d99b5b33b423c8b",
            "name": "reader"
          },
          {
            "id": "5f0594b46c8346d2996759ed1f5014f4",
            "name": "member"
          },
          {
            "id": "3e1adda493764db8921e97d68a5c8bc7",
            "name": "admin"
          }
        ],
        "catalog": [
          {
            "endpoints": [
              {
                "id": "6fbbbec7587e4989a4de6e7a43c7205f",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/placement",
                "region": "RegionOne"
              }
            ],
            "id": "10304edc7ec942d282567b8c4f5610d6",
            "type": "placement",
            "name": "placement"
          },
          {
            "endpoints": [
              {
                "id": "366c8616c77243d38971272e0eaf4a9f",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2.1",
                "region": "RegionOne"
              }
            ],
            "id": "117766093c004578b95a185ecf2a5bbf",
            "type": "compute",
            "name": "nova"
          },
          {
            "endpoints": [
              {
                "id": "0c0d6e47d833442f8da165ada8ad43a3",
                "interface": "internal",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              },
              {
                "id": "9440e2831cc84d4da9f7d20c7b42638e",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              },
              {
                "id": "c812ae1a5a3e40449f3c61edae4a6bcb",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8779/v1.0/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              }
            ],
            "id": "22844f546aa941798f06e4978be9ab94",
            "type": "database",
            "name": "trove"
          },
          {
            "endpoints": [
              {
                "id": "fad6bb86523b4f3aba4a6a6fb25cda0e",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/image",
                "region": "RegionOne"
              }
            ],
            "id": "3b7aead421ce4611a82e684ab8c2ad2f",
            "type": "image",
            "name": "glance"
          },
          {
            "endpoints": [
              {
                "id": "82c559324ad244beacb2fd0406f0298b",
                "interface": "admin",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080",
                "region": "RegionOne"
              },
              {
                "id": "d1029ab4d5bf4cd6843a5b819966c2ea",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:8080/v1/AUTH_de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              }
            ],
            "id": "628d3cea8d3b4615a4b047bae6112d0e",
            "type": "object-store",
            "name": "swift"
          },
          {
            "endpoints": [
              {
                "id": "a7eecf5314434aca81e5feaa8bf7521e",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/identity",
                "region": "RegionOne"
              }
            ],
            "id": "7055978143b74523b826bae1a9c0aabf",
            "type": "identity",
            "name": "keystone"
          },
          {
            "endpoints": [
              {
                "id": "30dd85ebed584c54ba849ca0e2a4c825",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101:9696/networking",
                "region": "RegionOne"
              }
            ],
            "id": "b3b850a4ec574d16a0cbece3c172a701",
            "type": "network",
            "name": "neutron"
          },
          {
            "endpoints": [
              {
                "id": "871e0e3393974f9b9071342b59f62090",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/compute/v2/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              }
            ],
            "id": "cb1921a9c779492484907683bb447e84",
            "type": "compute_legacy",
            "name": "nova_legacy"
          },
          {
            "endpoints": [
              {
                "id": "b6a779acd3e04ed480847d4cb99ca710",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              }
            ],
            "id": "d5345b92210c49aa975a873d04921780",
            "type": "block-storage",
            "name": "cinder"
          },
          {
            "endpoints": [
              {
                "id": "8c624f7ed1e44096b8f0413866a11b1b",
                "interface": "public",
                "region_id": "RegionOne",
                "url": "http://182.161.114.101/volume/v3/de5af600557d44d0996e667499376dbb",
                "region": "RegionOne"
              }
            ],
            "id": "e559802e75824163a070bd92dd143476",
            "type": "volumev3",
            "name": "cinderv3"
          }
        ]
      }
    }
    ```


### (8) 200 OK /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c
`openstack` <-- `glance-api`

=== "Header"
    ``` http title="200 OK /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c" linenums="1"
    Content-Length: 950
    Content-Type: application/json
    x-openstack-request-id: req-cecd5177-f553-4520-84b4-1a69c9178080
    X-Requestshook-Request-Id: 94b0932943f249989abf482444545486
    X-Requestshook-Request-From: openstack
    ```

=== "Body"
    ``` json title="200 OK /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c" linenums="1"
    {
      "owner_specified.openstack.md5": "",
      "owner_specified.openstack.object": "images/cirros-0.6.1-x86_64-disk",
      "owner_specified.openstack.sha256": "",
      "name": "cirros-0.6.1-x86_64-disk",
      "disk_format": "qcow2",
      "container_format": "bare",
      "visibility": "shared",
      "size": 21233664,
      "virtual_size": 117440512,
      "status": "active",
      "checksum": "0c839612eb3f2469420f2ccae990827f",
      "protected": false,
      "min_ram": 0,
      "min_disk": 0,
      "owner": "de5af600557d44d0996e667499376dbb",
      "os_hidden": false,
      "os_hash_algo": "sha512",
      "os_hash_value": "df88bac2791254f68941229792539621516bd480aa3d6fe4c0ca16057393d024a4944d644959f323dc01a25e3417c0b581776ab3c8db8da542039f2a67230579",
      "id": "a42bfade-78ec-4c95-b7b4-272ba265072c",
      "created_at": "2023-01-02T06:29:17Z",
      "updated_at": "2023-01-02T07:46:34Z",
      "tags": [],
      "self": "/v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c",
      "file": "/v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c/file",
      "schema": "/v2/schemas/image"
    }
    ```


### (9) GET /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c/members
`openstack` --> `glance-api`

!!! reference "API 참조 - List image members"
    [GET /v2/images/{image_id}/members](https://docs.openstack.org/api-ref/image/v2/index.html?expanded=list-image-members-detail#list-image-members)

`List image members` API를 이용하여 `a42bfade-78ec-4c95-b7b4-272ba265072c` 이미지의 멤버 목록을 조회한다.  
`X-Auth-Token` 인증 토큰은 `(5)` 요청에서 보낸 토큰과 동일하고 `(6-7)` 과정을 통해 이미 검증하였기 때문에, 토큰 캐싱으로 처리하고 다시 키스톤으로 검증 요청하지 않는다.  


=== "Header"
    ``` http title="GET /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c/members" linenums="1" hl_lines="5"
    Host: 127.0.0.1:60999
    User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
    Accept-Encoding: gzip, deflate
    Accept: application/json
    X-Auth-Token: gAAAAABjso1tRVlid2ys_vShS8yICW15ZVZgacI39RPWc_2q52Q_mnJbnf6N3zVmOw7naEEf6y637rJ-6fFz8TFkjMh8dd0tbh2o_Un7vajESqEiTqPrybmrF2YmF7zbxTrJNrP0KWyLY94ac11pnZRHkt9G0mLScpCq7VXzryAjsKAylxEzRZM
    X-Requestshook-Request-Id: 4036b910a45a4378a4cbdf137ae5292a
    X-Requestshook-Request-From: openstack
    X-Forwarded-For: 182.161.114.101
    X-Forwarded-Host: 182.161.114.101
    X-Forwarded-Server: 127.0.0.1
    Connection: Keep-Alive
    ```

=== "Body"
    ``` json title="GET /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c/members" linenums="1"
    none
    ```


### (10) 200 OK /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c/members
`openstack` <-- `glance-api`

`Body`에서 결과 출력 화면의 `image_id`, `member_id`, `status` 항목을 확인할 수 있다.  

=== "Header"
    ``` http title="200 OK /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c/members" linenums="1"
    Content-Length: 278
    Content-Type: application/json
    x-openstack-request-id: req-20d32f7f-cf92-4575-89f0-4d3e4773e3ad
    X-Requestshook-Request-Id: 4036b910a45a4378a4cbdf137ae5292a
    X-Requestshook-Request-From: openstack
    ```

=== "Body"
    ``` json title="200 OK /v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c/members" linenums="1" hl_lines="4 5 6"
    {
      "members": [
        {
          "member_id": "cdb477d9329c450e996cae2d02a2c44f",
          "image_id": "a42bfade-78ec-4c95-b7b4-272ba265072c",
          "status": "pending",
          "created_at": "2023-01-02T07:49:03Z",
          "updated_at": "2023-01-02T07:49:03Z",
          "schema": "/v2/schemas/member"
        }
      ],
      "schema": "/v2/schemas/members"
    }
    ```

