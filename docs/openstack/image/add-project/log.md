
### (openstack) REQ >>> (keystone) : GET http://182.161.114.101/identity/
Host: 182.161.114.101
User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
Accept-Encoding: gzip, deflate
Accept: application/json
Connection: keep-alive
X-Requestshook-Request-Id: 273dcb9bd3404794ad1203a5a49b692f
X-Requestshook-Request-From: openstack

none


### (openstack) RESP <<< (keystone) : 300 MULTIPLE CHOICES GET http://182.161.114.101/identity/
Content-Type: application/json
Content-Length: 274
Location: http://182.161.114.101/identity/v3/
Vary: X-Auth-Token
X-Requestshook-Request-Id: 273dcb9bd3404794ad1203a5a49b692f
X-Requestshook-Request-From: openstack

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


### (openstack) REQ >>> (keystone) : POST http://182.161.114.101/identity/v3/auth/tokens
Host: 182.161.114.101
User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
Accept-Encoding: gzip, deflate
Accept: application/json
Connection: keep-alive
Content-Type: application/json
Content-Length: 209
X-Requestshook-Request-Id: 579b4f8cf4024e959e30cc77b9958b45
X-Requestshook-Request-From: openstack

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


### (openstack) RESP <<< (keystone) : 201 CREATED POST http://182.161.114.101/identity/v3/auth/tokens
Content-Type: application/json
Content-Length: 3952
X-Subject-Token: gAAAAABjspLyEYEtrXJ0vvq7TYXVKZXuhGppzLDCMGpebh2v7iFXcVXjNMUAXgfPBc2lS4CNTbI8Is3p4oo_sU5MwQVSs7mgL_2p21Uxf6wZmU31ljeZAzH7wRqXYRLtPpabodLkT-D0LfSqPsUVrLVa1sChubIyqXig0d3SjGPb7hh1tEzSvTU
Vary: X-Auth-Token
X-Requestshook-Request-Id: 579b4f8cf4024e959e30cc77b9958b45
X-Requestshook-Request-From: openstack

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
      "ZFK9Vfr6Rz6p8hU3Pei1Dw"
    ],
    "expires_at": "2023-01-02T11:16:50.000000Z",
    "issued_at": "2023-01-02T08:16:50.000000Z",
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


### (openstack) REQ >>> (keystone) : GET http://182.161.114.101/identity/v3/projects/demo
Host: 182.161.114.101
User-Agent: python-keystoneclient
Accept-Encoding: gzip, deflate
Accept: application/json
Connection: keep-alive
X-Auth-Token: gAAAAABjspLyEYEtrXJ0vvq7TYXVKZXuhGppzLDCMGpebh2v7iFXcVXjNMUAXgfPBc2lS4CNTbI8Is3p4oo_sU5MwQVSs7mgL_2p21Uxf6wZmU31ljeZAzH7wRqXYRLtPpabodLkT-D0LfSqPsUVrLVa1sChubIyqXig0d3SjGPb7hh1tEzSvTU
X-Requestshook-Request-Id: dcd541aed14e4012b6b1fa006113d420
X-Requestshook-Request-From: openstack

none


### (openstack) RESP <<< (keystone) : 404 NOT FOUND GET http://182.161.114.101/identity/v3/projects/demo
Content-Type: application/json
Content-Length: 85
Vary: X-Auth-Token
X-Requestshook-Request-Id: dcd541aed14e4012b6b1fa006113d420
X-Requestshook-Request-From: openstack

{
  "error": {
    "code": 404,
    "message": "Could not find project: demo.",
    "title": "Not Found"
  }
}


### (openstack) REQ >>> (keystone) : GET http://182.161.114.101/identity/v3/projects?name=demo
Host: 182.161.114.101
User-Agent: python-keystoneclient
Accept-Encoding: gzip, deflate
Accept: application/json
Connection: keep-alive
X-Auth-Token: gAAAAABjspLyEYEtrXJ0vvq7TYXVKZXuhGppzLDCMGpebh2v7iFXcVXjNMUAXgfPBc2lS4CNTbI8Is3p4oo_sU5MwQVSs7mgL_2p21Uxf6wZmU31ljeZAzH7wRqXYRLtPpabodLkT-D0LfSqPsUVrLVa1sChubIyqXig0d3SjGPb7hh1tEzSvTU
X-Requestshook-Request-Id: 312706e887c44370b2565a0a7dbb3400
X-Requestshook-Request-From: openstack

none


### (openstack) RESP <<< (keystone) : 200 OK GET http://182.161.114.101/identity/v3/projects?name=demo
Content-Type: application/json
Content-Length: 413
Vary: X-Auth-Token
X-Requestshook-Request-Id: 312706e887c44370b2565a0a7dbb3400
X-Requestshook-Request-From: openstack

{
  "projects": [
    {
      "id": "cdb477d9329c450e996cae2d02a2c44f",
      "name": "demo",
      "domain_id": "default",
      "description": "",
      "enabled": true,
      "parent_id": "default",
      "is_domain": false,
      "tags": [],
      "options": {},
      "links": {
        "self": "http://182.161.114.101/identity/v3/projects/cdb477d9329c450e996cae2d02a2c44f"
      }
    }
  ],
  "links": {
    "next": null,
    "self": "http://182.161.114.101/identity/v3/projects?name=demo",
    "previous": null
  }
}


### (openstack) REQ >>> (glance-api) : GET http://127.0.0.1:60999/v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c
Host: 127.0.0.1:60999
User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
Accept-Encoding: gzip, deflate
Accept: */*
X-Auth-Token: gAAAAABjspLyEYEtrXJ0vvq7TYXVKZXuhGppzLDCMGpebh2v7iFXcVXjNMUAXgfPBc2lS4CNTbI8Is3p4oo_sU5MwQVSs7mgL_2p21Uxf6wZmU31ljeZAzH7wRqXYRLtPpabodLkT-D0LfSqPsUVrLVa1sChubIyqXig0d3SjGPb7hh1tEzSvTU
X-Requestshook-Request-Id: bbdab06fd70d47e6b15986642bfd7f14
X-Requestshook-Request-From: openstack
X-Forwarded-For: 182.161.114.101
X-Forwarded-Host: 182.161.114.101
X-Forwarded-Server: 127.0.0.1
Connection: Keep-Alive

none


### (glance-api) REQ >>> (keystone) : GET http://182.161.114.101/identity/v3/auth/tokens
Host: 182.161.114.101
User-Agent: python-keystoneclient
Accept-Encoding: gzip, deflate
Accept: application/json
Connection: keep-alive
X-Subject-Token: gAAAAABjspLyEYEtrXJ0vvq7TYXVKZXuhGppzLDCMGpebh2v7iFXcVXjNMUAXgfPBc2lS4CNTbI8Is3p4oo_sU5MwQVSs7mgL_2p21Uxf6wZmU31ljeZAzH7wRqXYRLtPpabodLkT-D0LfSqPsUVrLVa1sChubIyqXig0d3SjGPb7hh1tEzSvTU
Openstack-Identity-Access-Rules: 1
X-Auth-Token: gAAAAABjsngzWyVGDHbiSj5O8qRegJFlrF6uhymf73wRY30-fdJy6Z5__WEEU_v9HN2B1fKW4tjnkh673cmzR2781Rz8taI2YJh8vI_0zgDRvwUxWzGGIhUCh7554NDMLnvCrcvVGvIdBVONzHpcKBc2cEwaU0KQIAunm5Mk4kOXkl8pN32scro
X-Requestshook-Request-Id: 5b407005f908457ebb56bc2111f52808
X-Requestshook-Request-From: glance-api

none


### (glance-api) RESP <<< (keystone) : 200 OK GET http://182.161.114.101/identity/v3/auth/tokens
Content-Type: application/json
Content-Length: 3952
X-Subject-Token: gAAAAABjspLyEYEtrXJ0vvq7TYXVKZXuhGppzLDCMGpebh2v7iFXcVXjNMUAXgfPBc2lS4CNTbI8Is3p4oo_sU5MwQVSs7mgL_2p21Uxf6wZmU31ljeZAzH7wRqXYRLtPpabodLkT-D0LfSqPsUVrLVa1sChubIyqXig0d3SjGPb7hh1tEzSvTU
Vary: X-Auth-Token
X-Requestshook-Request-Id: 5b407005f908457ebb56bc2111f52808
X-Requestshook-Request-From: glance-api

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
      "ZFK9Vfr6Rz6p8hU3Pei1Dw"
    ],
    "expires_at": "2023-01-02T11:16:50.000000Z",
    "issued_at": "2023-01-02T08:16:50.000000Z",
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


### (openstack) RESP <<< (glance-api) : 200 OK GET http://127.0.0.1:60999/v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c
Content-Length: 950
Content-Type: application/json
x-openstack-request-id: req-4e78efbd-e56d-400b-884f-8620dd30e075
X-Requestshook-Request-Id: bbdab06fd70d47e6b15986642bfd7f14
X-Requestshook-Request-From: openstack

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


### (openstack) REQ >>> (glance-api) : POST http://127.0.0.1:60999/v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c/members
Host: 127.0.0.1:60999
User-Agent: openstacksdk/0.101.0 keystoneauth1/5.0.0 python-requests/2.28.1 CPython/3.8.10
Accept-Encoding: gzip, deflate
Accept: */*
X-Auth-Token: gAAAAABjspLyEYEtrXJ0vvq7TYXVKZXuhGppzLDCMGpebh2v7iFXcVXjNMUAXgfPBc2lS4CNTbI8Is3p4oo_sU5MwQVSs7mgL_2p21Uxf6wZmU31ljeZAzH7wRqXYRLtPpabodLkT-D0LfSqPsUVrLVa1sChubIyqXig0d3SjGPb7hh1tEzSvTU
Content-Type: application/json
X-Requestshook-Request-Id: af14f4e99bf54d1a9f0c188bb64b228e
X-Requestshook-Request-From: openstack
X-Forwarded-For: 182.161.114.101
X-Forwarded-Host: 182.161.114.101
X-Forwarded-Server: 127.0.0.1
Content-Length: 46
Connection: Keep-Alive

{
  "member": "cdb477d9329c450e996cae2d02a2c44f"
}


### (openstack) RESP <<< (glance-api) : 200 OK POST http://127.0.0.1:60999/v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c/members
Content-Length: 230
Content-Type: application/json
x-openstack-request-id: req-3dacfd5a-18be-4906-9dbd-7d0a344db952
X-Requestshook-Request-Id: af14f4e99bf54d1a9f0c188bb64b228e
X-Requestshook-Request-From: openstack

{
  "member_id": "cdb477d9329c450e996cae2d02a2c44f",
  "image_id": "a42bfade-78ec-4c95-b7b4-272ba265072c",
  "status": "pending",
  "created_at": "2023-01-02T08:16:50Z",
  "updated_at": "2023-01-02T08:16:50Z",
  "schema": "/v2/schemas/member"
}

