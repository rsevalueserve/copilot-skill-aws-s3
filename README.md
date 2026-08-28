# AWS S3 Skills for GitHub Copilot

GitHub Copilot skill for managing AWS S3 with automatic Windows/Linux path translation.

## Skills Included

### `aws-s3`
Upload, download, list, and manage files in AWS S3 using AWS CLI. Features intelligent path translation between Windows and Linux/WSL environments.

**Features:**
- 📤 Upload/Download files to/from S3
- 🔄 Automatic Windows (C:\) to Linux (/mnt/c/) path conversion
- 📋 List buckets and objects
- 🔗 Generate presigned URLs
- 🔁 Sync directories
- 🗑️ Delete objects
- 🔐 Support for encryption and storage classes

## Installation

Using GitHub CLI:

```bash
# Install for GitHub Copilot
gh skill install rsevalueserve/copilot-skill-aws-s3 aws-s3

# Install for other agents (e.g., Claude Code)
gh skill install rsevalueserve/copilot-skill-aws-s3 aws-s3 --agent claude-code

# Install at user scope (available everywhere)
gh skill install rsevalueserve/copilot-skill-aws-s3 aws-s3 --scope user
```

## Prerequisites

1. **AWS CLI:**
   ```bash
   # Install AWS CLI
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   unzip awscliv2.zip
   sudo ./aws/install
   ```

2. **AWS Configuration:**
   Configure your AWS credentials:
   ```bash
   aws configure
   ```

3. **IAM Permissions:**
   Ensure your AWS user/role has appropriate S3 permissions:
   - `s3:PutObject` - Upload files
   - `s3:GetObject` - Download files
   - `s3:ListBucket` - List bucket contents
   - `s3:DeleteObject` - Delete files (optional)

## Usage

Once installed, GitHub Copilot can use the skill automatically. Example requests:

- "Upload C:\report.pdf to s3://my-bucket/docs/"
- "Download s3://backups/database.sql to ~/Downloads"
- "List all files in my S3 bucket"
- "Create a 7-day presigned URL for s3://bucket/file.pdf"
- "Sync this folder to S3"

## Path Translation

The skill automatically handles path translation when running on Linux/WSL with Windows-style paths:

```
Windows:  C:\Users\John\file.pdf
WSL:      /mnt/c/Users/John/file.pdf

Windows:  D:\Projects\data.csv
WSL:      /mnt/d/Projects/data.csv
```

## Security

✅ **No hardcoded credentials** - Uses AWS CLI configuration  
✅ **Secure authentication** - Supports IAM roles, profiles, and environment variables  
✅ **Safe for version control** - No secrets stored in code  
✅ **Path validation** - Verifies file existence before operations

## Updating

```bash
gh skill update aws-s3
```

## License

MIT License - see [LICENSE](LICENSE) for details.

## Contributing

Issues and pull requests are welcome!
