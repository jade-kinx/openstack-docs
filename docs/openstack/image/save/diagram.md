openstack->>keystone: GET /identity/
keystone-->>openstack: 300 MULTIPLE CHOICES /identity/
openstack->>keystone: POST /identity/v3/auth/tokens
keystone-->>openstack: 201 CREATED /identity/v3/auth/tokens
openstack->>glance-api: GET /v2/images/{image_id}
glance-api->>keystone: GET /identity/v3/auth/tokens
keystone-->>glance-api: 200 OK /identity/v3/auth/tokens
glance-api-->>openstack: 200 OK /v2/images/{image_id}
openstack->>glance-api: GET /v2/images/{image_id}/file
opt glance-api 인증 토큰 발급
  glance-api->>keystone: POST /identity/v3/auth/tokens
  keystone-->>glance-api: 201 CREATED /identity/v3/auth/tokens
end
glance-api->>swift-proxy-server: GET /v1/AUTH_{account}/glance/{image_id}
swift-proxy-server->>keystone: GET /identity/v3/auth/tokens
keystone-->>swift-proxy-server: 200 OK /identity/v3/auth/tokens
swift-proxy-server-->>glance-api: 200 OK /v1/AUTH_{account}/glance/{image_id}
glance-api-->>openstack: 200 OK /v2/images/{image_id}/file
