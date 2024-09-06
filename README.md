# RELEASE-PLEASE DEMO

## Repository

1. Create new repository on [GitHub](https://github.com)
1. [Scaffold](https://vitejs.dev/guide/#scaffolding-your-first-vite-project) your vite project

    ```shell
    npm create vite@latest <NAME OF APPLICATION> -- --template <TEMPLATE NAME>
    ```

1. Initialize GitHub in your project

    ```shell
    git init
    git branch -M main
    git remote add origin <URL TO YOUR REPOSITORY>
    ```

## Husky and commitlint

1. Add [husky](https://typicode.github.io/husky/) and [commitlint](https://commitlint.js.org/guides/getting-started.html) to project and configure it

    - Initialize it

      ```shell
      npm install -D husky @commitlint/{cli,config-conventional} &&
      npx husky init
      ```

    - Edit `.husky/pre-commit` file with this line

      ```shell
      npm run lint
      ```

    - Add new file `.husky/commit-msg` with this content

      ```shell
      npx --no -- commitlint --edit $1
      ```

    - Configure [commitlint](https://commitlint.js.org/guides/getting-started.html)

      ```shell
      echo "export default {\n\textends: ['@commitlint/config-conventional']\n};" > commitlint.config.js
      ```

1. Enable `husky`

    ```shell
    npx husky
    ```


1. Commit changes

    ```shell
    git status
    git add .
    git commit -m "<COMMIT MESSAGE>"
    git push --set-upstream origin main
    ```

## [Release-please](https://github.com/googleapis/release-please)

Add release-please configuration files

where `0.0.0` application version from `./package.json`

  ```shell
  echo "{\n\t\".\": \"0.0.0\"\n}" > .release-please-manifest.json

  echo "{\n\t\"\$schema\": \"https://raw.githubusercontent.com/googleapis/release-please/main/schemas/config.json\",\n\t\"release-type\": \"node\",\n\t\"pull-request-title-pattern\": \"chore(main): release\${component} \${version}\",\n\t\"include-component-in-tag\": false,\n\t\"packages\": {\n\t\t\".\": {}\n\t}\n}" > release-please-config.json
  ```

## [GitHub Actions](https://docs.github.com/en/actions)

Set up GitHub actions by adding bellow content to `.github\workflows\main.yml` file

```yaml
name: Create release PR

on:
  push:
    branches:
      - main

jobs:

  # Pre-jobs add here... for example deploy to test env

  # Make release PR
  release-please:
    runs-on: ubuntu-latest
    outputs:
      release_created: ${{ steps.release-please.outputs.releases_created }}
      version: ${{ steps.release-please.outputs.major }}.${{ steps.release-please.outputs.minor }}.${{ steps.release-please.outputs.patch }}
    steps:
      - uses: googleapis/release-please-action@v4
        id: release-please
        with:
          target-branch: ${{ github.ref_name }}

  # Here we can do anything with merged release PR... for example deploy to stage or production and etc.
  post-release-branch-merge-job:
    needs: [release-please]
    # Check if we merged our release PR
    if: needs.release-please.outputs.release_created == 'true'
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to prod environment
        run: echo "Merged to release branch and deployed to production!"
```
