# DPy action for GitHub

This action analyzes your Python code using [DPy](https://www.designite-tools.com/products-dpy), detects a comprehensive set of design and implementation smells, computes a large set of object-oriented code quality metrics, and uploads the results as an artifact. 
Additionally, the action enables logging code quality analysis reports directly to [DCode](https://dcodehub.com/), our analytics platform. DCode automatically tracks and visualizes report trends, monitors code health, and supports data-driven engineering decisions.

## How to configure it?

Configuration for CI pipeline is a one-time process.

### Step 1. Obtain personal access token and add it to GitHub secrets

- Create a new personal access token for your GitHub repository by navigating to your account’s user menu (top right) → Settings → Developer settings → Personal access tokens. Create a new token, and if using a fine-grained token, ensure read access to Actions, Code, and Metadata.
- Add the token to your repository’s secrets by going to the repository’s Settings → Secrets and variables → Actions. Create a new secret, paste the token into the Value field, and assign a meaningful name (e.g., PAT).

### Step 2 (Optional): Add your Designite license key to secrets

If you have a Designite **Professional** or **Academic** license key, add it to your repository’s GitHub secrets as `D_KEY`. This key enables the analysis of large projects.

### Step 3 (Optional): Add your DCode project credentials to secrets

Create a new project in DCode, if not exists yet, and retrieve the *project-id* and *api-key* from the **Settings** menu (accessible via the three dots on the project card). Add these values to your repository’s GitHub secrets as `PROJECT_ID` and `API_KEY`. This setup allows your code quality analysis reports to be automatically logged to your DCode project, where you can monitor trends and track code health.

### Step 4: Add a GitHub Actions workflow file

Finally, create a `.github` folder in the root of your project, then add a `workflows` folder inside it. In the `workflows` folder, create a workflow file (e.g., `actions.yml`). The content of this file will depend on your project’s language and the tasks you want to automate.

A sample action file is provided below.

```yaml
name: DPy Code Quality

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - name: DPy Action
        uses: DesigniteTools/DPyAction@v1.2.0
        with:
          PAT: ${{ secrets.PAT }}
          PROJECT_ID: ${{ secrets.PROJECT_ID }}
          API_KEY: ${{ secrets.API_KEY }}
```

Change the version number of action with the latest available action.

## What You’ll Get

Once configured, your Python repository will be analyzed on each event defined in your workflow file (in the example above, the analysis runs on every push or pull request to the `main` branch). The resulting report will be stored as a GitHub Actions artifact, which you can download later to review detected code quality smells and metrics for deeper analysis.

If integrated with DCode, you can automatically track code quality over time. DCode provides interactive visualizations to help you understand and monitor code health, enabling data-driven engineering decisions.

