# AWS CloudShell

## What is AWS CloudShell?
• **Browser-based shell environment** accessible từ AWS Management Console
• **Pre-configured** với AWS CLI, AWS SDKs, và common development tools
• **No additional charges** - chỉ pay cho AWS resources bạn tạo
• **Persistent storage** 1 GB per region cho home directory

## Pre-installed Tools
• **AWS CLI v2**: Latest version với all services
• **AWS SDKs**: Python (boto3), Node.js, .NET
• **Development Tools**: 
  - Git, Docker, kubectl
  - Python 3, Node.js, PowerShell
  - vim, nano, emacs editors
• **Utilities**: curl, wget, jq, zip/unzip

## Key Features
• **Persistent Storage**: Files trong $HOME persist across sessions
• **Multiple Tabs**: Mở multiple shell sessions
• **File Upload/Download**: Transfer files between local và CloudShell
• **Safe Environment**: Isolated environment per user
• **Regional**: Available trong most AWS regions

## Getting Started
1. **Access**: Click CloudShell icon trong AWS Console
2. **First Launch**: Takes ~30 seconds to initialize
3. **Credentials**: Automatically configured với current IAM permissions
4. **Home Directory**: 1 GB persistent storage

## Common Use Cases
• **Quick AWS CLI commands** without local setup
• **Script development và testing**
• **Learning AWS services** trong safe environment
• **Temporary development work**
• **Running AWS tools** như eksctl, sam cli

## Best Practices
• **Organize files**: Use directories để organize scripts
• **Version control**: Use git cho important scripts
• **Security**: Don't store sensitive data trong CloudShell
• **Resource cleanup**: Delete unused files để save space
• **Backup important work**: Download critical files locally

## Limitations
• **1 GB storage limit** per region
• **Session timeout**: Inactive sessions close after ~20 minutes
• **No root access**: Cannot install system packages
• **Network restrictions**: Some outbound connections may be blocked
