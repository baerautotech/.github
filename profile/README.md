# BaerAutoTech

Welcome to BaerAutoTech's GitHub organization.

## 🚀 For Developers

### Building and Deploying Your Application

All BaerAutoTech repositories can use our automated build and deploy system powered by Kubernetes and Kaniko.

**Quick Start**:

1. Add this workflow to your repo (`.github/workflows/build-and-deploy.yml`):

```yaml
name: Build and Deploy
on:
  push:
    branches: [main, develop]

jobs:
  build:
    runs-on: [self-hosted]
    steps:
      - uses: actions/checkout@v4
      
      - uses: baerautotech/cerebral-deployment/.github/actions/build-with-kaniko@main
        id: build
        with:
          image-name: 'cerebral/${{ github.event.repository.name }}'
      
      - run: |
          kubectl set image deployment/${{ github.event.repository.name }} \
            app=${{ steps.build.outputs.image }} \
            -n cerebral-platform
```

2. Push your code - builds happen automatically!

**Full documentation**: [cerebral-deployment/DEVELOPER_QUICK_START.md](https://github.com/baerautotech/cerebral-deployment/blob/main/DEVELOPER_QUICK_START.md)

### Resources

- **📖 Developer Guide**: [DEVELOPER_QUICK_START.md](https://github.com/baerautotech/cerebral-deployment/blob/main/DEVELOPER_QUICK_START.md)
- **🏗️ Build System Architecture**: [ENTERPRISE_BUILD_SYSTEM_REDESIGN.md](https://github.com/baerautotech/cerebral-deployment/blob/main/ENTERPRISE_BUILD_SYSTEM_REDESIGN.md)
- **🔧 Kaniko Build Details**: [k8s/jobs/README.md](https://github.com/baerautotech/cerebral-deployment/blob/main/k8s/jobs/README.md)
- **📦 Registry Access**: `registry.dev.cerebral.baerautotech.com`

### Getting Help

- **Issues**: File in the relevant repository
- **Questions**: Check documentation links above
- **Platform Issues**: [cerebral-deployment/issues](https://github.com/baerautotech/cerebral-deployment/issues)

---

## Organization Variables

These are available to all workflows:

| Variable | Value | Usage |
|----------|-------|-------|
| `INTERNAL_REGISTRY_URL` | `internal-registry.registry.svc.cluster.local:5000` | For builds running in cluster |
| `EXTERNAL_REGISTRY_URL` | `registry.dev.cerebral.baerautotech.com` | For external access |
| `BUILD_NAMESPACE` | `build-system` | Kaniko jobs namespace |
| `KUBE_NAMESPACE` | `cerebral-platform` | Default deployment namespace |

Access in workflows: `${{ vars.INTERNAL_REGISTRY_URL }}`

---

**Platform maintained by**: BaerAutoTech Platform Engineering
