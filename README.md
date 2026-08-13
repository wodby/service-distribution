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

Delegated token authentication is optional. The linked authentication service
owns the signing key and exposes only its public `registry-token` certificate
to Distribution.

Validate the manifest with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```
