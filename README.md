# k8s-deployment-action

Action to manage Kubernetes Deployments

# Usage

## Trigger

```yml
name: "Manual deploy"
on:
  workflow_dispatch:

jobs:
  deploy:
    name: Deploy
    runs-on: self-hosted
    container: ghcr.io/sitkoru/actions-container
    steps:
      - name: Prepare
        id: prep
        shell: bash
        run: |
          VERSION=${GITHUB_REF#refs/tags/}
          echo ::set-output name=version::${VERSION}
          
      - uses: sitkoru/k8s-deployment-action@v1
        name: Create GitHub deployment
        with:
          version: ${{ github.event.deployment.description }}
          config: ${{ secrets.KUBE_CONFIG }}
          common_env: ${{ secrets.COMMON_ENV }}
          app_values_path: .helm/values.yml
          app: ${{ secrets.APP_NAME }}

```