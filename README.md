# GCS-GitHub-Sync

A simple tool to automatically sync files from Google Cloud Storage to your GitHub repository.

## What it does

This workflow automatically copies files from your GCS bucket to your GitHub repository on a schedule or when manually triggered. It's useful for:

- Keeping datasets in sync with your code
- Storing model outputs in your repository
- Maintaining a version history of your ML artifacts

## Setup Instructions

### 1. Create Google Cloud Service Account

1. Create a service account in Google Cloud with access to your bucket
2. Generate and download a JSON key for this service account

### 2. Add GitHub Secrets

In your repository, go to Settings → Secrets and Variables → Actions, and add:

- `GCP_SA_KEY`: Paste the entire content of your service account JSON key
- `GCP_PROJECT_ID`: Your Google Cloud project ID

### 3. Configure the Workflow

1. Copy the workflow file to `.github/workflows/sync-gcs.yml` in your repository
2. Edit the file to set:
   - Your GCS bucket path: `gs://your-bucket-name/your-folder/*`
   - Your target directory: `ml_artifacts` (or any name you prefer)
   - Schedule frequency (default is daily)

### 4. Run the Workflow

The workflow will run automatically according to the schedule, or you can run it manually:
1. Go to the Actions tab in your repository
2. Select "Sync GCS Files to GitHub"
3. Click "Run workflow"

## Customization

### Change Sync Frequency

Edit the `cron` value in the workflow file. For example, to run every 6 hours:
```yaml
- cron: '0 */6 * * *'
```

### Use Efficient Sync (for large datasets)

Replace the `gsutil cp` command with `gsutil rsync` to only transfer changed files:
```yaml
gsutil -m rsync -d -r gs://your-bucket-name/your-folder/ ml_artifacts/
```

## Troubleshooting

- **Authentication Issues**: Check that your GCP_SA_KEY is correct
- **No Files Synced**: Verify your bucket path is correct
- **Workflow Failures**: Check the Actions logs for error messages

---

*Created by Jay Dasani 
