# openstack image show

`openstack image show` 커맨드를 이용하여 이미지 상세 정보를 조회하여 보고, 그 과정을 API 시퀀스 다이어그램으로 도출하고 `Request`, `Response`를 분석해 본다.  

* `id` 또는 `name`으로 이미지 정보를 조회(여기서는 `id`로 조회)

## Openstack CLI Command & Output

!!! reference "CLI 참조"
    [openstack image show](https://docs.openstack.org/python-openstackclient/zed/cli/command-objects/image-v2.html#image-show)

??? example "openstack image show a42bfade-78ec-4c95-b7b4-272ba265072c --format json"
    ``` json title="Console Output" linenums="1"
    {
      "checksum": "0c839612eb3f2469420f2ccae990827f",
      "container_format": "bare",
      "created_at": "2023-01-02T06:29:17Z",
      "disk_format": "qcow2",
      "file": "/v2/images/a42bfade-78ec-4c95-b7b4-272ba265072c/file",
      "id": "a42bfade-78ec-4c95-b7b4-272ba265072c",
      "min_disk": 0,
      "min_ram": 0,
      "name": "cirros-0.6.1-x86_64-disk",
      "owner": "de5af600557d44d0996e667499376dbb",
      "properties": {
        "os_hidden": false,
        "os_hash_algo": "sha512",
        "os_hash_value": "df88bac2791254f68941229792539621516bd480aa3d6fe4c0ca16057393d024a4944d644959f323dc01a25e3417c0b581776ab3c8db8da542039f2a67230579",
        "owner_specified.openstack.md5": "",
        "owner_specified.openstack.object": "images/cirros-0.6.1-x86_64-disk",
        "owner_specified.openstack.sha256": ""
      },
      "protected": false,
      "schema": "/v2/schemas/image",
      "size": 21233664,
      "status": "active",
      "tags": [],
      "updated_at": "2023-01-02T06:29:33Z",
      "virtual_size": 117440512,
      "visibility": "public"
    }
    ```

## Sequence Diagram

`openstack image show ...` 커맨드를 이용한 이미지 상세 정보 조회 과정은 아래의 시퀀스 다이어그램으로 표현할 수 있다.  

``` mermaid
sequenceDiagram
    autonumber
    --8<-- "openstack/image/show/diagram.md"
```

각 과정에 대한 간략한 설명은 다음과 같다.   

- `openstack-client`가 `keystone` 서비스에 인증 정보(`credential:password`)를 보내 인증 토큰 발급 및 서비스 카탈로그를 수신한다. `(1-4)`
- `a42bfade-78ec-4c95-b7b4-272ba265072c` `id`로 이미지 상세 정보를 요청한다. `(5)`
- `(5)` 요청에 대해 사용자 인증 토큰을 체크한다. `(6-7)`
- `a42bfade-78ec-4c95-b7b4-272ba265072c` 이미지의 상세 정보를 응답한다. `(8)`

!!! note "이미지 이름으로 조회를 요청하면..."
    만약 이미지의 `id`가 아닌 `name`으로 요청하게 되면(가령, `cirros-0.6.1-x86_64-disk`) `(5)` 과정에서 `GET /v2/images/cirros-0.6.1-x86_64-disk` 요청은 `404 Not Found` 에러메세지와 함께 실패하고, `GET /v2/images?name=cirros-0.6.1-x86_64-disk`로 다시 요청하여 이미지 상세 정보를 수신하게 된다.  
    즉, `openstack-client`는 입력 받은 이미지를 `id`로 우선 인식하여 요청하고, 실패하면 `name`으로 인식하여 요청한다.  

## Request / Response

??? warning "X-Requestshook-Request-* 헤더"
    `Header` 에 포함된 `X-Requestshook-Request-Id`, `X-Requestshook-Request-From` 항목은 API Sequence 추적을 위해 `requestshook`에서 추가한 항목이며, 오픈스택에서 제공하는 정보가 아니라는 점을 주의한다.    

--8<-- "openstack/image/show/body.md"

## Full Logs

??? quote "/var/log/requestshook/requestshook.log"
    ``` text title="" linenums="1"
    --8<-- "openstack/image/show/log.md"
    ```
