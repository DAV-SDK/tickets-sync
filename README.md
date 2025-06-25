# Tickets Sync

A CI pipeline that automatically syncs open and active tickets from external platforms to GitHub repository issue trackers.

## Supported Platforms

- **Spack**: HPC package manager
- **Conda-forge**: Community-driven packaging for conda
- **OpenSUSE Bugzilla**: OpenSUSE bug tracking system
- **Ubuntu Launchpad**: Ubuntu project management and bug tracking

## How It Works

The pipeline uses Claude Code CLI to intelligently search for and sync external tickets:

1. **Scheduled Execution**: Runs daily at 6 AM UTC via GitHub Actions
2. **Repository Processing**: Uses GitHub Actions matrix strategy for parallel processing
3. **Intelligent Sync**: Uses Claude Code to:
   - Search external platforms for relevant tickets
   - Check for existing GitHub issues to avoid duplicates
   - Create new issues with proper labeling and formatting
   - Update existing external sync issues when needed

## Setup

### 1. Configure Repositories

Edit the workflow matrix in `.github/workflows/ticket-sync.yml` to include the repositories you want to sync:

```yaml
strategy:
  matrix:
    include:
      - repo: "your-org/your-repo"
        token_secret: "YOUR_REPO_TOKEN"
      - repo: "another-org/another-repo"
        token_secret: "ANOTHER_REPO_TOKEN"
```

### 2. Set up Secrets

In your GitHub repository settings, add these secrets:

- `CLAUDE_API_KEY`: Your Anthropic Claude API key
- Repository-specific tokens (e.g., `YOUR_REPO_TOKEN`, `ANOTHER_REPO_TOKEN`): GitHub Personal Access Tokens with appropriate permissions for each target repository

### 3. Manual Run

You can manually trigger the sync workflow from the GitHub Actions tab or run locally:

```bash
# Set environment variables
export CLAUDE_API_KEY="your_api_key"
export GITHUB_TOKEN="your_github_token"
export TARGET_REPO="your-org/your-repo"

# Run the sync script
claude --dangerously-skip-permissions ./prompt.txt
```

## Configuration Files

- `platforms.yml`: Platform-specific configuration and search parameters
- `.github/workflows/ticket-sync.yml`: GitHub Actions workflow with repository matrix configuration

## Issue Labels

External tickets are automatically labeled with:
- `external-sync`: Identifies issues synced from external platforms
- Platform-specific labels: `spack`, `conda-forge`, `opensuse`, `ubuntu`

## Customization

To add more repositories, add them to the matrix strategy in `.github/workflows/ticket-sync.yml` with their corresponding token secrets. To modify platform search parameters or add new platforms, edit `platforms.yml`.