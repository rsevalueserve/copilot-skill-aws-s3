---
name: aws-s3
description: Upload, download, list, and manage files in AWS S3 with automatic Windows/Linux path translation
license: MIT
---

# AWS S3 Management Skill

This skill helps you upload, download, list, and manage files in AWS S3 using the AWS CLI configured on the machine.

## Capabilities

- **Upload files** to S3 buckets (supports large files with multipart upload)
- **Download files** from S3 buckets  
- **List buckets** and objects
- **Delete objects** from S3
- **Sync directories** to/from S3
- **Generate presigned URLs** for temporary access
- **Path translation**: Automatically converts Windows paths (C:\...) to WSL paths (/mnt/c/...) when running in Linux

## Prerequisites

- AWS CLI must be installed and configured
- Valid AWS credentials (via `aws configure`, environment variables, or IAM role)
- Appropriate S3 permissions (s3:PutObject, s3:GetObject, s3:ListBucket, etc.)

## When to Use This Skill

Invoke this skill when the user wants to:
- Upload files to S3: "Upload C:\file.pdf to s3://bucket/"
- Download from S3: "Download s3://bucket/file.zip"
- List S3 contents: "Show files in my S3 bucket"
- Manage S3 objects: "Delete old backups from S3"
- Sync directories: "Sync this folder to S3"
- Share files: "Create a temporary link for this S3 file"

## Instructions

### 1. Path Translation (Critical for Cross-Platform)

**Always** check the operating system and translate Windows paths to Linux/WSL format:

```bash
# Detect OS
uname -a

# If running on Linux/WSL and user provides Windows path like:
# C:\Users\Name\file.txt
# 
# Translate to:
# /mnt/c/Users/Name/file.txt

# Pattern:
# C:\ → /mnt/c/
# D:\ → /mnt/d/
# Backslashes → Forward slashes
```

**Before any upload**, verify the file exists:
```bash
if [ -f "/mnt/c/Users/Name/file.txt" ]; then
  echo "✓ File found"
else
  echo "✗ File not found"
fi
```

### 2. Upload Files to S3

```bash
# Basic upload
aws s3 cp <local-path> s3://<bucket>/<key>

# Large files (automatic multipart)
aws s3 cp <local-path> s3://<bucket>/<key>

# With storage class
aws s3 cp <local-path> s3://<bucket>/<key> --storage-class INTELLIGENT_TIERING
```

### 3. Download Files from S3

```bash
# Basic download
aws s3 cp s3://<bucket>/<key> <local-path>
```

### 4. List S3 Contents

```bash
# List all buckets
aws s3 ls

# List bucket contents
aws s3 ls s3://bucket-name/ --human-readable --summarize
```

### 5. Generate Presigned URLs

```bash
# Generate URL valid for 7 days (604800 seconds)
aws s3 presign s3://bucket/file.pdf --expires-in 604800
```

## Best Practices

1. **Always validate paths** before operations
2. **Show progress** for large uploads/downloads
3. **Use appropriate storage classes** (STANDARD, INTELLIGENT_TIERING, GLACIER)
4. **Enable encryption** for sensitive data (--sse)
5. **Check AWS credentials** are configured before operations

## Output Format

When performing operations:

1. **Before operation**: Show what will be done
   ```
   📤 Uploading file to S3...
   Source: /mnt/c/Users/Name/file.pdf
   Destination: s3://bucket/folder/
   Size: 5.2 MB
   ```

2. **After operation**: Confirm success with details
   ```
   ✅ Upload completed successfully!
   URL: s3://bucket/folder/file.pdf
   ```

## Examples

### Example 1: Upload Windows file from Linux
```
User: "Upload C:\Users\John\report.pdf to s3://reports-bucket/2026/"

Response:
1. Detect OS: Linux (WSL)
2. Translate path: C:\Users\John\report.pdf → /mnt/c/Users/John/report.pdf
3. Verify file exists
4. Execute: aws s3 cp "/mnt/c/Users/John/report.pdf" s3://reports-bucket/2026/
5. Confirm upload with URL
```

### Example 2: Download file
```
User: "Download s3://backups/database.sql to my Downloads folder"

Response:
1. Determine Downloads path
2. Execute: aws s3 cp s3://backups/database.sql ~/Downloads/
3. Verify download successful
```
