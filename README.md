# MFA Git Access CLI

Cross-platform Python CLI for ephemeral MFA-based Git repository authentication using AWS STS. No long-lived credentials.

## Tech Stack

- **Language**: Python
- **AWS**: STS (AssumeRole with MFA), IAM
- **Libraries**: Boto3, qrcode
- **Platforms**: macOS, Windows, Linux

## How It Works

1. Developer runs the CLI and enters an MFA code from their authenticator app
2. CLI calls AWS STS AssumeRole with the MFA code to obtain temporary credentials
3. Git credential helper is configured with short-lived access keys
4. First-time users get a QR code to scan with their authenticator app
5. All Git operations work transparently — credentials are injected automatically
6. Credentials expire after the configured duration; developer re-authenticates

## Features

- Ephemeral credentials via AWS STS with MFA
- Cross-platform (macOS, Windows, Linux)
- Automatic Git credential helper configuration
- QR code generation for authenticator app setup
- Self-service for 60+ developers
- No long-lived credentials stored anywhere

## Architecture

See [diagram.puml](./diagram.puml) for authentication flow.

## References

- [PRD](./prd.md)
- Blog post: [MFA for Git Repositories](https://yokharian.dev/posts/mfa-for-git-repositories)