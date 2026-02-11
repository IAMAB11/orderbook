# Deployment Guide

This repository is configured to automatically build and publish the `order-book` package to PyPI using GitHub Actions.

## Automatic Deployment

The package is automatically deployed to PyPI when a new release is published on GitHub.

### Prerequisites

1. **PyPI Trusted Publishing** (Recommended): Configure PyPI to trust this GitHub repository
   - Go to PyPI project settings
   - Add GitHub Actions as a trusted publisher
   - Repository: `IAMAB11/orderbook`
   - Workflow: `publish.yml`
   - Environment: (leave empty)

   OR

2. **PyPI API Token**: Store a PyPI API token as a GitHub repository secret named `PYPI_API_TOKEN`

### Creating a Release

1. Update the version number in `setup.py`
2. Update `CHANGES.md` with the new version changes
3. Commit and push the changes
4. Create a new release on GitHub:
   ```bash
   git tag v0.6.2
   git push origin v0.6.2
   ```
5. Go to GitHub Releases and create a new release from the tag
6. The workflow will automatically:
   - Build wheels for Python 3.11, 3.12, and 3.13
   - Build wheels for Linux, macOS, and Windows
   - Build a source distribution
   - Upload all packages to PyPI

### Manual Deployment

You can also manually trigger the deployment workflow:

1. Go to the Actions tab in the GitHub repository
2. Select "Build and Publish to PyPI" workflow
3. Click "Run workflow"
4. Select the branch and click "Run workflow"

## Workflow Details

The deployment workflow consists of three jobs:

1. **build_wheels**: Builds binary wheels for multiple Python versions and platforms
2. **build_sdist**: Builds the source distribution
3. **publish**: Publishes all built packages to PyPI

The workflow uses PyPI's trusted publishing feature for secure, tokenless authentication.

## Local Testing

To test the build process locally:

```bash
# Install build tools
pip install build

# Build wheel
python -m build --wheel

# Build source distribution
python -m build --sdist

# Check dist/ directory
ls -la dist/
```

## Troubleshooting

### Build Failures

If the build fails, check the Actions logs for specific error messages. Common issues:

- C compiler not available (ensure build tools are installed)
- Missing dependencies (add to `setup.py` if needed)
- Version conflicts

### Publishing Failures

If publishing fails:

- Verify PyPI trusted publishing is configured correctly
- Check that the version number is unique (not already published)
- Ensure the workflow has the necessary permissions (`id-token: write`)
