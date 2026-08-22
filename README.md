# Distribution Registry service for Kubernetes on Wodby

Run CNCF Distribution Registry v3 with the Docker Official Image on Wodby.

- [Distribution documentation](https://distribution.github.io/distribution/)
- [Docker Official Image](https://hub.docker.com/_/registry)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)

## Service overview

The service uses `registry:3.1.1`, exposes the Registry HTTP API on port 5000,
and enables bcrypt-backed basic authentication by default. It supports local
persistent storage, AWS S3, Google Cloud Storage, and generic S3-compatible
object storage. A Redis-compatible metadata-cache link accepts both Valkey and
Redis services.

The Docker Official Image configuration remains authoritative for the default
filesystem driver and its `/var/lib/registry` root. Google Cloud Storage and
Amazon S3 use separate optional variable integrations. Each integration exports
the complete Distribution environment contract for its driver, including the
bucket and credentials. Attach at most one of them; Distribution rejects
configurations containing more than one storage driver.

The GCP integration stores the service-account JSON in
`DISTRIBUTION_GCS_KEYFILE` and sets `REGISTRY_STORAGE_GCS_KEYFILE` to its mounted
path, normally `/mnt/config/env/DISTRIBUTION_GCS_KEYFILE`.

Delegated token authentication is optional. The linked authentication service
owns the signing key and exposes only its public `registry-token` certificate
to Distribution. New auth services should export `DISTRIBUTION_AUTH_REALM`,
`DISTRIBUTION_AUTH_SERVICE`, and `DISTRIBUTION_AUTH_ISSUER` and use the
`auth-service` link. The original `auth` link remains available for services
that export the legacy `registry-realm`, `registry-service`, and
`registry-issuer` service tokens.

The generated basic-auth password is stored under the internal
`DISTRIBUTION_HTPASSWD_PASSWORD` secret key. Google service-account JSON is
stored as `DISTRIBUTION_GCS_KEYFILE` and mounted into the container; the native
`REGISTRY_STORAGE_GCS_KEYFILE` variable points to that file. These helper names
are not Registry configuration fields and do not use Wodby's reserved
environment namespace.

Validate the manifest with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```
