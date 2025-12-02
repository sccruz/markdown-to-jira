# Add GitHub Pages Deployment and PR Testing

## Summary
This PR adds automated deployment to GitHub Pages and PR testing workflows, making it easy to deploy the markdown-to-jira converter and catch issues before merging.

## Changes

### 🚀 GitHub Pages Deployment
- **Added** `.github/workflows/deploy.yml` - Automated deployment workflow
  - Builds and deploys to GitHub Pages on push to `main`
  - Uses Bun for building the project
  - Creates `.nojekyll` file automatically to ensure all files are served correctly

### ✅ PR Testing Workflow
- **Added** `.github/workflows/pr-test.yml` - Automated testing on pull requests
  - Runs tests (`bun test`) on every PR
  - Builds the project to verify compilation
  - Uploads build artifacts for local testing
  - Comments on PRs with build status

### 📝 Documentation Updates
- **Updated** `README.md` with:
  - Prerequisites section clarifying Bun installation requirement
  - Bun installation instructions for macOS/Linux and Windows
  - Complete GitHub Pages deployment setup guide
  - Manual build instructions

### 🔧 Build Process Enhancement
- **Updated** `build.ts` to automatically create `.nojekyll` file
  - Ensures GitHub Pages serves all files correctly (including files starting with underscores)
  - No manual intervention needed

## Benefits

1. **Automated Deployment**: Site automatically deploys when code is merged to `main`
2. **Quality Assurance**: Tests run automatically on PRs, catching issues early
3. **Better Developer Experience**: Clear setup instructions and automated workflows
4. **Easy Forking**: Other developers can easily deploy their own fork to GitHub Pages

## Testing

- [x] Verified build process works correctly
- [x] Tested workflow syntax (no linter errors)
- [x] Confirmed `.nojekyll` file is created during build

## Setup Required

After merging, enable GitHub Pages:
1. Go to repository **Settings** → **Pages**
2. Under **Source**, select **GitHub Actions**
3. The site will deploy automatically on the next push to `main`

## Related

This addresses the need to:
- Deploy the forked repository to a custom GitHub Pages URL
- Test changes before merging
- Provide clear setup instructions for contributors

