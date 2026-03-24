https://roadmap.sh/projects/github-actions-deployment-workflow/solutions?u=69c2f16f33a0ad7a57af32cc

https://roadmap.sh/projects/github-actions-deployment-workflow

# GitHub Pages Deployment Workflow

This project demonstrates a simple but powerful GitHub Actions workflow that automatically deploys a static website to GitHub Pages. The key feature of this workflow is that it only triggers a deployment when the `index.html` file is modified, saving unnecessary runs.

## How It Works

The magic happens in the `.github/workflows/deploy.yml` file.

1.  **Trigger**: The workflow is triggered by a `push` to the `main` branch. However, it includes a `paths` filter:
    ```yaml
    paths:
      - 'index.html'
    ```
    This ensures the workflow only runs if the `push` includes changes to the `index.html` file. Pushing changes to `README.md` or other files will not trigger a deployment.

2.  **Build Job**:
    *   It checks out your repository code.
    *   It uses the official `actions/upload-pages-artifact` action to package the content of your repository into an artifact. For this simple project, it just packages the root directory.

3.  **Deploy Job**:
    *   This job depends on the `build` job (`needs: build`).
    *   It uses the official `actions/deploy-pages` action to take the artifact from the build job and publish it to GitHub Pages.
    *   It runs in a special `github-pages` environment with the necessary permissions.

## Setup Instructions

1.  **Create the Repository**:
    *   Create a new public repository on GitHub named `gh-deployment-workflow`.
    *   **Important**: Do **NOT** initialize it with a README, license, or .gitignore, as we will add these files manually.

2.  **Enable GitHub Pages**:
    *   In your new repository, go to **Settings**.
    *   In the left menu, click on **Pages**.
    *   Under "Build and deployment", set the **Source** to **GitHub Actions**. This tells GitHub to expect deployments from a workflow.

3.  **Add the Project Files**:
    *   Create the three files as described above: `index.html`, `README.md`, and `.github/workflows/deploy.yml`.
    *   You can do this locally using `git` or directly on GitHub's web interface.
    *   Commit and push these files to your `main` branch.

4.  **Check the Deployment**:
    *   Go to the **Actions** tab in your repository. You should see a workflow running.
    *   Once it's complete, your website will be live at `https://<your-username>.github.io/gh-deployment-workflow/`.

## Testing the Trigger

To see the conditional trigger in action:

1.  Modify the `index.html` file (e.g., change the "Hello" text).
2.  Commit and push the change. You will see a new workflow run and the website will update.
3.  Now, modify only the `README.md` file.
4.  Commit and push the change. **No workflow will be triggered**, and the website will remain unchanged. This proves the filter is working correctly.

## Stretch Goal: Using a Static Site Generator

This workflow is a perfect foundation for more complex projects built with static site generators like **Hugo**, **Jekyll**, or **Astro**.

To adapt it for a generator like Hugo, you would modify the `build` job:

1.  **Add a setup step**: Install the generator (e.g., `peaceiris/actions-hugo@v2` for Hugo).
2.  **Add a build step**: Run the build command (e.g., `hugo`).
3.  **Change the artifact path**: Instead of uploading the whole repository (`path: '.'`), you would upload the generated static site directory (`path: 'public'` for Hugo).

The `deploy` job would remain exactly the same, as its job is simply to deploy whatever artifact the `build` job creates.

## License

This project is open-source and available under the [MIT License](LICENSE).
