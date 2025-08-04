# Tickets Sync

A CI pipeline that automatically syncs open and active tickets from external platforms to GitHub repository issue trackers.

## Supported Platforms

- **Spack**: HPC package manager
- **Conda-forge**: Community-driven packaging for conda
- **OpenSUSE Bugzilla**: OpenSUSE bug tracking system
- **Ubuntu Launchpad**: Ubuntu project management and bug tracking

## How It Works

The pipeline uses Gemini Code CLI to intelligently search for and sync external tickets:

1. **Scheduled Execution**: Runs daily at 6 AM UTC via GitHub Actions
2. **Repository Processing**: Uses GitHub Actions matrix strategy for parallel processing
3. **Intelligent Sync**: Uses Gemini Code to:
   - Search external platforms for relevant tickets
   - Check for existing GitHub issues to avoid duplicates
   - Create new issues with proper labeling and formatting
   - Update existing external sync issues when needed

## Setup

### 1. Configure Repositories and Projects

Edit the workflow matrix in `.github/workflows/ticket-sync.yml` to include the repositories and projects you want to sync:

```yaml
strategy:
  matrix:
    include:
      - repo: "your-org/your-repo"
        project: "adios2"
        token_secret: "YOUR_REPO_TOKEN"
      - repo: "another-org/vtk-repo"
        project: "vtk"
        token_secret: "ANOTHER_REPO_TOKEN"
      - repo: "cmake-org/cmake-repo"
        project: "cmake"
        token_secret: "CMAKE_REPO_TOKEN"
```

### 2. Set up Secrets

In your GitHub repository settings, add these secrets:

- `GEMINI_API_KEY`: Your Anthropic Gemini API key
- Repository-specific tokens (e.g., `YOUR_REPO_TOKEN`, `ANOTHER_REPO_TOKEN`): GitHub Personal Access Tokens with appropriate permissions for each target repository

### 3. Manual Run

You can manually trigger the sync workflow from the GitHub Actions tab or run locally:

```bash
# Set environment variables
export GEMINI_API_KEY="your_api_key"
export GITHUB_TOKEN="your_github_token"
export TARGET_REPO="your-org/your-repo"
export PROJECT_NAME="adios2"  # or "vtk", "paraview", "cmake"

# Run the sync script
gemini --dangerously-skip-permissions ./prompt.txt
```

## Configuration Files

- `platforms.yml`: Platform-specific configuration and project search parameters
- `.github/workflows/ticket-sync.yml`: GitHub Actions workflow with repository matrix configuration

## Supported Projects

The system currently supports these software projects:

- **adios2**: ADIOS2 (Adaptable IO System for HPC)
- **vtk**: VTK (Visualization Toolkit)
- **paraview**: ParaView (Parallel Visualization Application)
- **cmake**: CMake (Cross-platform build system)

Each project has its own search terms and feedstock configurations defined in `platforms.yml`.

## Issue Labels

External tickets are automatically labeled with:
- `external-sync`: Identifies issues synced from external platforms
- Platform-specific labels: `spack`, `conda-forge`, `opensuse`, `ubuntu`

## Customization

### Adding New Projects

To add a new software project, edit `platforms.yml` and add it to the `projects` section:

```yaml
projects:
  your-project:
    name: "Your Project Name"
    description: "Brief description"
    search_terms:
      - "YourProject"
      - "your-project"
      - "libyour-project"
    feedstock_name: "your-project"
```

### Adding New Repositories

Add repositories to the matrix strategy in `.github/workflows/ticket-sync.yml` with their corresponding project and token secrets:

```yaml
- repo: "your-org/new-repo"
  project: "your-project"
  token_secret: "NEW_REPO_TOKEN"
```

### Modifying Platform Search Parameters

To modify platform search parameters or add new platforms, edit the `platforms` section in `platforms.yml`.