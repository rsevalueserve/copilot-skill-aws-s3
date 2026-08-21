# AWS S3 Management Skill for GitHub Copilot

Global skill for managing AWS S3 operations with automatic Windows/WSL path translation.

## Features
- ✅ Upload/Download files to/from S3
- ✅ Automatic Windows (C:\) to Linux (/mnt/c/) path conversion
- ✅ List buckets and objects
- ✅ Generate presigned URLs
- ✅ Sync directories

## Installation

### Local (per-user)
```bash
mkdir -p ~/.config/github-copilot/skills/
cp -r aws-s3 ~/.config/github-copilot/skills/
```

### Project (per-repository)
```bash
mkdir -p .github/skills/
cp -r aws-s3 .github/skills/
```

## Usage

Simply ask Copilot:
- "Upload C:\file.pdf to s3://my-bucket/"
- "Download s3://bucket/file.zip"
- "List files in my S3 bucket"

## Requirements
- AWS CLI installed and configured
- Valid AWS credentials
