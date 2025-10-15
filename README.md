# Puppet Server Container

[![CI](https://github.com/rorymcmahon/container-puppetserver/actions/workflows/ci.yaml/badge.svg)](https://github.com/rorymcmahon/container-puppetserver/actions/workflows/ci.yaml)
[![License](https://img.shields.io/github/license/rorymcmahon/container-puppetserver.svg)](https://github.com/rorymcmahon/container-puppetserver/blob/main/LICENSE)
[![Docker Pulls](https://img.shields.io/docker/pulls/rorymcmahon/puppetserver.svg)](https://hub.docker.com/r/rorymcmahon/puppetserver)

---

- [Puppet Server Container](#puppet-server-container)
  - [Quick Start](#quick-start)
  - [Configuration](#configuration)
  - [Initialization Scripts](#initialization-scripts)
  - [Persistence](#persistence)
  - [Version Schema](#version-schema)
  - [Contributing](#contributing)
  - [Attribution](#attribution)

---

This project provides a containerized Puppet Server with comprehensive configuration options and production-ready features.

## Quick Start

Run Puppet Server with default settings:

```bash
docker run --name puppet --hostname puppet ghcr.io/rorymcmahon/puppetserver:latest
```

Mount your Puppet code:

```bash
docker run --name puppet --hostname puppet -v ./code:/etc/puppetlabs/code ghcr.io/rorymcmahon/puppetserver:latest
```

## Configuration

| Environment Variable | Description | Default |
|---------------------|-------------|---------|
| `PUPPETSERVER_HOSTNAME` | DNS name for SSL certificate | unset |
| `CERTNAME` | Certificate name | unset |
| `DNS_ALT_NAMES` | Additional DNS names for SSL | unset |
| `PUPPETSERVER_PORT` | Puppet server port | `8140` |
| `AUTOSIGN` | Enable autosigning (`true`/`false`/path) | `true` |
| `CA_ENABLED` | Enable Certificate Authority | `true` |
| `CA_TTL` | CA expiration time | `157680000` |
| `PUPPETSERVER_JAVA_ARGS` | JVM arguments | `-Xms1024m -Xmx1024m` |
| `USE_PUPPETDB` | Connect to PuppetDB | `true` |
| `PUPPETDB_SERVER_URLS` | PuppetDB server URLs | `https://puppetdb:8081` |
| `PUPPETSERVER_ENVIRONMENT_TIMEOUT` | Environment cache timeout | `unlimited` |

### Environment Caching

⚠️ Environment caching is enabled by default. Clear cache after deployments:

```bash
curl -i --cert $(puppet config print hostcert) \
--key $(puppet config print hostprivkey) \
--cacert $(puppet config print cacert) \
-X DELETE \
https://$(puppet config print server):8140/puppet-admin-api/v1/environment-cache?environment=production
```

Or disable caching: `PUPPETSERVER_ENVIRONMENT_TIMEOUT=0`

## Initialization Scripts

Add custom initialization scripts to `/docker-custom-entrypoint.d/`:

- `/docker-custom-entrypoint.d/pre-default/` - Run before default scripts
- `/docker-custom-entrypoint.d/` - Run after default scripts, before service start
- `/docker-custom-entrypoint.d/post-startup/` - Run after service starts
- `/docker-custom-entrypoint.d/sigterm-handler/` - Run on SIGTERM
- `/docker-custom-entrypoint.d/post-execution/` - Run after service stops

## Persistence

Persist CA data to maintain trust relationships:

```bash
docker run -v $PWD/ca-ssl:/etc/puppetlabs/puppetserver/ca ghcr.io/rorymcmahon/puppetserver:latest
```

## Version Schema

Format: `<puppet.major>.<puppet.minor>.<puppet.patch>-v<container.major>.<container.minor>.<container.patch>`

Example: `8.6.1-v1.7.0`

- `puppet.*` - Puppet version
- `container.*` - Container version (features, fixes, base image changes)

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for development workflow, branch naming conventions, and PR labeling guidelines.

## Attribution

This project is a fork of the original Voxpupuli Puppet Server container, which was originally created by Puppet Inc. and later maintained by the Voxpupuli community.

**Original Authors:**
- [Puppet Inc.](https://github.com/puppetlabs) - Original implementation
- [Voxpupuli Community](https://github.com/voxpupuli) - Community maintenance and improvements

**Current Maintainer:**
- [Rory McMahon](https://github.com/rorymcmahon) - Fork maintenance and new features

This fork continues development of the container with additional features and customizations while maintaining compatibility with the original design.

---

**License:** Apache 2.0 (inherited from original project)
