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
      echo "export default { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
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