# Automated Markdown Search

This repository contains a GitHub Actions workflow that searches for specific keywords in markdown files (`.md`). The workflow is automated to run every time a new commit is pushed to the repository, and it can also be manually triggered. The search results will be displayed directly in the workflow logs, with bold formatting for file paths and headings to make the output easy to read.

## How It Works

1. **Triggering the Workflow**:
   - The search can be triggered in two ways:
     1. **Automatically**: The workflow runs whenever there is a new push to the repository.
     2. **Manually**: You can manually trigger the workflow from the **Actions** tab in GitHub.

2. **Search Keywords**:
   - The workflow is currently set to search for the following keywords:
     - `"ChatGPT"`
     - `"Generative AI"`
     - `"black box"`

3. **Search Scope**:
   - The workflow only searches within `.md` (markdown) files. You can modify the file types or the directory in which the search occurs if needed.

## How to Use

1. **Set Up**:
   - This workflow is already configured in the `.github/workflows/search.yml` file.
   - To use it, simply push changes to the repository or manually trigger the workflow from the **Actions** tab.

2. **Viewing Results**:
   - After the workflow runs, click on the workflow name (e.g., "Automated Markdown Search") under the **Actions** tab.
   - Check the logs for the search results, where file paths and line numbers will be bolded for better readability.

## How to Modify the Search

To change the search keywords or file types, follow these steps:

1. **Change Keywords**:
   - In the `.github/workflows/search.yml` file, find the line with the `grep` command:
     ```bash
     grep -rni -e "ChatGPT" -e "Generative AI" -e "black box" --include \*.md .
     ```
   - Replace `"ChatGPT"`, `"Generative AI"`, or `"black box"` with the keywords you want to search for.

2. **Change File Types**:
   - The workflow is currently set to search within `.md` files. To search other file types, modify the `--include` part of the command:
     ```bash
     --include \*.md
     ```
   - For example, to search `.txt` files as well, change it to:
     ```bash
     --include \*.md --include \*.txt
     ```

3. **Change Search Directory**:
   - The workflow searches the entire repository (`.`). To limit the search to a specific folder (e.g., `./notes`), modify the directory path in the command:
     ```bash
     grep -rni -e "ChatGPT" -e "Generative AI" -e "black box" --include \*.md ./notes
     ```

## Example Workflow

Here is an example of the `.github/workflows/search.yml` file:

```yaml
name: Automated Markdown Search

on: 
  workflow_dispatch:   # Allows you to run this manually from GitHub Actions tab
  push:                # Runs automatically every time you push new code
    branches:
      - main           # Only runs when changes are made to the main branch

jobs:
  search:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout repository content
      uses: actions/checkout@v3    # Updated to v3 to avoid Node.js deprecation warnings

    - name: Search for multiple keywords in markdown files
      run: |
        echo "Searching for the keywords 'ChatGPT', 'Generative AI', and 'black box' in markdown files..."
        grep -rni -e "ChatGPT" -e "Generative AI" -e "black box" --include \*.md . || echo "No matches found"

