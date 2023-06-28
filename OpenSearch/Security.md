To use your own certificates in your configuration, add all of the necessary certificates to the volumes section of the compose file:

```yaml
volumes:
  - ./root-ca.pem:/usr/share/opensearch/config/root-ca.pem
  - ./admin.pem:/usr/share/opensearch/config/admin.pem
  - ./admin-key.pem:/usr/share/opensearch/config/admin-key.pem
  - ./node1.pem:/usr/share/opensearch/config/node1.pem
  - ./node1-key.pem:/usr/share/opensearch/config/node1-key.pem
```

When you add TLS certificates to your OpenSearch nodes with Docker Compose volumes, you should also include a custom `opensearch.yml` file that defines those certificates. For example:

```yaml
volumes:
  - ./root-ca.pem:/usr/share/opensearch/config/root-ca.pem
  - ./admin.pem:/usr/share/opensearch/config/admin.pem
  - ./admin-key.pem:/usr/share/opensearch/config/admin-key.pem
  - ./node1.pem:/usr/share/opensearch/config/node1.pem
  - ./node1-key.pem:/usr/share/opensearch/config/node1-key.pem
  - ./custom-opensearch.yml:/usr/share/opensearch/config/opensearch.yml
```

Remember that the certificates you specify in your compose file must be the same as the certificates defined in your custom `opensearch.yml` file. You should replace the root, admin, and node certificates with your own. For more information see [Configure TLS certificates](https://opensearch.org/docs/latest/security/configuration/tls).

```yaml
plugins.security.ssl.transport.pemcert_filepath: node1.pem
plugins.security.ssl.transport.pemkey_filepath: node1-key.pem
plugins.security.ssl.transport.pemtrustedcas_filepath: root-ca.pem
plugins.security.ssl.http.pemcert_filepath: node1.pem
plugins.security.ssl.http.pemkey_filepath: node1-key.pem
plugins.security.ssl.http.pemtrustedcas_filepath: root-ca.pem
plugins.security.authcz.admin_dn:
  - CN=admin,OU=SSL,O=Test,L=Test,C=DE
```

After configuring security settings, your custom `opensearch.yml` file might look something like the following example, which adds TLS certificates and the distinguished name (DN) of the admin certificate, defines a few permissions, and enables verbose audit logging:

```yaml
plugins.security.ssl.transport.pemcert_filepath: node1.pem
plugins.security.ssl.transport.pemkey_filepath: node1-key.pem
plugins.security.ssl.transport.pemtrustedcas_filepath: root-ca.pem
plugins.security.ssl.transport.enforce_hostname_verification: false
plugins.security.ssl.http.enabled: true
plugins.security.ssl.http.pemcert_filepath: node1.pem
plugins.security.ssl.http.pemkey_filepath: node1-key.pem
plugins.security.ssl.http.pemtrustedcas_filepath: root-ca.pem
plugins.security.allow_default_init_securityindex: true
plugins.security.authcz.admin_dn:
  - CN=A,OU=UNIT,O=ORG,L=TORONTO,ST=ONTARIO,C=CA
plugins.security.nodes_dn:
  - 'CN=N,OU=UNIT,O=ORG,L=TORONTO,ST=ONTARIO,C=CA'
plugins.security.audit.type: internal_opensearch
plugins.security.enable_snapshot_restore_privilege: true
plugins.security.check_snapshot_restore_write_privileges: true
plugins.security.restapi.roles_enabled: ["all_access", "security_rest_api_access"]
cluster.routing.allocation.disk.threshold_enabled: false
opendistro_security.audit.config.disabled_rest_categories: NONE
opendistro_security.audit.config.disabled_transport_categories: NONE
```

For a full list of settings, see [Security](https://opensearch.org/docs/latest/security/configuration/index/).

Use the same process to specify a [Backend configuration](https://opensearch.org/docs/latest/security/configuration/configuration/) in `/usr/share/opensearch/config/opensearch-security/config.yml` as well as new internal users, roles, mappings, action groups, and tenants in their respective [YAML files](https://opensearch.org/docs/latest/security/configuration/yaml/).

After replacing the certificates and creating your own internal users, roles, mappings, action groups, and tenants, use Docker Compose to start the cluster:

```bash
docker-compose up -d
```