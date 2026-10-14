# AWS CLI

## Installation

```bash
❯ curl -fsSL https://awscli.amazonaws.com/v2/install.sh | bash
Resolving user-local install paths:
  HOME=/home/anhao
  XDG_DATA_HOME=<unset, using default>
  XDG_BIN_HOME=<unset, using default>
Installing AWS CLI 2.38.0
Downloading AWS CLI installer from https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.38.0.zip?src=script-exe
######################################################################## 100.0%
Downloading AWS CLI signature from https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.38.0.zip.sig?src=script-exe
######################################################################## 100.0%
GPG signature verified.
You can now run: /home/anhao/.local/bin/aws --version
Tip: run 'aws configure agent-toolkit' to set up AWS skills and the AWS MCP server for your AI coding agent(s).
AWS CLI 2.38.0 installed to /home/anhao/.local/share/aws-cli
Ensure /home/anhao/.local/bin is on your PATH. When multiple installs exist, PATH order determines which 'aws' runs.
```

---

config: `aws congifure`

Note: the values you provide for the AWS Access Key ID and the AWS Secret Access Key will be written to the shared credentials file (`~/.aws/credentials`).

## General Commands

```bash
aws s3 ls

# upload file to s3
aws s3 cp test.txt s3://adrepo-etl/etl-harbest-webapi/
```

## S3 Commands

k
