# Callstack.ai Action
This action performs an automated review on your pull requests.

[Callstack.ai](https://app.callstack.ai) account is required for this action to work.

# Usage
1. Create account at [https://app.callstack.ai](https://app.callstack.ai).
2. Install Callstack.ai GitHub app to your repository (follow setup guide in Callstack.ai platform).
3. Create `.github/workflows/callstack-reviewer.ym` workflow file to the main branch.

```yml filename=".github/workflows/callstack-reviewer.ym"
name: Callstackai PR Review

on:
  workflow_dispatch:
    inputs:
      config:
        type: string
        description: "config for reviewer"
        required: true
      head:
        type: string
        description: "head commit sha"
        required: true
      base:
        type: string
        description: "base commit sha"
        required: false

jobs:
  callstack_pr_review_job:
    runs-on: ubuntu-latest
    steps:
      - name: Review PR
        uses: callstackai/action@main
        with:
          config: ${{ inputs.config }}
          head: ${{ inputs.head }}

          # Optional (by default Callstack.ai OpenAI API key will be used)
          openai_key: ${{ secrets.OPENAI_KEY }}
```

# Configuring Review
You can configure reviewer by adding `.callstack.yml` config file to the main branch.

```yml filename=".callstack.yml"
pr_review:
  modules:
    # Automatically create a description summarizing the changes in pull request. 
    description:
      enabled: true
      diagram: false

    # Find potential bugs in pull request changes or related files.
    bug_hunter:
      enabled: true
      # Include fixes to possible bugs.
      suggestions: true

    # Suggest improvements to added code.
    code_suggestions:
      enabled: true

    # Suggest changes to follow defined code conventions.
    code_conventions:
      enabled: false
      # Describe your code conventions in plain text.
      conventions: |
        E.g. Exported variables, functions, classes and methods should be defined before private.
        

    # Point out any typos or grammatical errors in variable names, texts, comments.
    grammar:
      enabled: false

    # Suggest performance improvements to added code.
    performance:
      enabled: true

    # Find potential security issues in added code.
    security:
      enabled: true
```
