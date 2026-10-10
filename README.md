# Simple AI GitHub PR Reviewer

A minimal Node.js project that reviews a pull request with an OpenAI model and posts one summary review to GitHub. GitHub Actions triggers it automatically.

## Files

- `server.js` — fetches PR metadata and textual diffs, asks the model for a review, and publishes a summary.
- `.github/workflows/ai-pr-review.yml` — triggers on PR open/update/reopen.

> Despite its name, `server.js` does not start a web server. It is a Node.js script run by GitHub Actions.

## Setup

1. Create a GitHub repository and add these files on its default branch.
2. In GitHub, open **Settings → Secrets and variables → Actions → New repository secret**.
3. Add a secret named `GROQ_API_KEY` containing your GROQ API key.
4. Ensure Actions are enabled and the workflow token is allowed to write pull-request reviews.
5. Open a pull request from a branch in the same repository, or push another commit to an existing PR.
6. Open the repository's **Actions** tab to inspect the workflow run. The summary appears under the PR's **Conversation** tab.

## What it does

- Fetches PR metadata and textual patches through Octokit.
- Sends a size-limited patch bundle to the OpenAI Responses API.
- Asks for actionable findings in Markdown.
- Checks that the PR head SHA did not change during review.
- Posts one summary review and avoids duplicate summaries for the same commit.

## Limitations

- Same-repository PRs only; fork PRs are intentionally skipped.
- Summary review only, not inline line comments.
- Uses a basic character budget, not a tokenizer-aware context budget.
- Does not run or install dependencies from the PR's code.
- The model can be wrong. Verify findings before acting.
- The workflow uses `pull_request`, so repository secrets are not exposed to fork PRs. Do not switch to `pull_request_target` without redesigning the security model.

## Model configuration

The default is `gpt-4.1-mini`. Change `OPENAI_MODEL` in the workflow to another model supported by your OpenAI API account.

## Local smoke test

You can run `node server.js` locally after setting the environment variables `GITHUB_TOKEN`, `GROQ_API_KEY`, `GITHUB_REPOSITORY`, and `PR_NUMBER`. It will publish a review to the selected PR, so use a test PR.
# pr-review-by-ai
# pr-review-by-ai
