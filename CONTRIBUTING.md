# Contributing Guide

## Development Workflow

### Branch Naming
- `feature/description` - New features
- `fix/description` - Bug fixes  
- `docs/description` - Documentation updates
- `ci/description` - CI/CD pipeline changes
- `container/description` - Container/Dockerfile changes

### Pull Request Labels
Labels control release note categorization:

| Label | Release Category | Usage |
|-------|------------------|-------|
| `enhancement` | New Features 🎉 | New functionality |
| `bug` | Bug Fixes 🐛 | Bug fixes |
| `documentation` | Documentation Updates 📚 | Docs changes |
| `container` | Container Updates 🐳 | Dockerfile/container config |
| `ci` | CI/CD Updates 🚀 | Workflow/pipeline changes |
| `dependencies` | Dependency Updates ⬆️ | Package updates |
| `backwards-incompatible` | Breaking Changes 🛠 | Breaking changes |

### Excluded from Changelog
- `skip-changelog` - Internal changes
- `duplicate`, `invalid`, `wont-fix` - Non-changes

## Quick Commands
```bash
# Create feature branch
git checkout -b feature/new-capability

# Create fix branch  
git checkout -b fix/security-issue

# Create container update branch
git checkout -b container/update-base-image
```
