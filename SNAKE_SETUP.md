# 🐍 GitHub Contribution Snake Setup Guide

This guide will help you set up the animated contribution snake for your GitHub profile.

## Prerequisites

- GitHub account
- GitHub profile repository (repository named same as your username)

## Setup Steps

### 1. Create GitHub Actions Workflow

Create a new file in your repository at `.github/workflows/snake.yml`:

```yaml
name: Generate Snake

on:
  # Run automatically every 12 hours
  schedule:
    - cron: "0 */12 * * *"
  
  # Allows manual workflow run
  workflow_dispatch:
  
  # Run on every push to main
  push:
    branches:
      - main

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    
    steps:
      # Checkout the repository
      - name: Checkout
        uses: actions/checkout@v3
      
      # Generate the snake
      - name: Generate github-contribution-grid-snake.svg
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
      
      # Push the generated files to the output branch
      - name: Push github-contribution-grid-snake.svg to output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### 2. Enable GitHub Actions

1. Go to your repository settings
2. Navigate to **Actions** → **General**
3. Under **Workflow permissions**, select **Read and write permissions**
4. Click **Save**

### 3. Run the Workflow

1. Go to the **Actions** tab in your repository
2. Click on **Generate Snake** workflow
3. Click **Run workflow** → **Run workflow**
4. Wait for the workflow to complete

### 4. Verify the Output

After the workflow completes successfully:

1. Go to your repository
2. Switch to the `output` branch
3. You should see two files:
   - `github-contribution-grid-snake.svg`
   - `github-contribution-grid-snake-dark.svg`

### 5. Update README (Already Done!)

The snake animation is already embedded in your README.md. It will automatically update every 12 hours!

## Troubleshooting

### Workflow fails with permission error

- Make sure you've enabled **Read and write permissions** in repository settings
- Check that GitHub Actions is enabled for your repository

### Snake not showing in README

- Verify the `output` branch exists
- Check that the SVG files are in the `output` branch
- Make sure your username in the README matches your GitHub username

### Want to customize the snake?

You can customize colors, animation speed, and more by modifying the workflow file. Check out the [snk documentation](https://github.com/Platane/snk) for more options.

## Manual Update

To manually update the snake animation:

1. Go to **Actions** tab
2. Select **Generate Snake** workflow
3. Click **Run workflow**
4. Select the `main` branch
5. Click **Run workflow**

---

**Note:** The snake animation will be generated based on your actual GitHub contributions, so it will look different for everyone!
