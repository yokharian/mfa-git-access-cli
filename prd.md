# 🧠 PRD: MFA Git Access CLI

## tl;dr

Cross-platform Python CLI that enables ephemeral MFA-based authentication for Git repositories. Uses AWS STS to obtain temporary credentials, configures Git credential helpers, and generates QR codes for easy mobile setup — serving 60+ developers across Mac, Windows, and Linux.

---

## 🎯 Goals

- **Ephemeral Credentials**: Generate short-lived AWS STS credentials for Git repository access, eliminating long-lived credential leaks
- **Cross-Platform Support**: Works seamlessly on macOS, Windows, and Linux
- **Git Credential Integration**: Configures Git credential helpers so developers never enter passwords manually
- **QR Code Setup**: Generates QR codes for quick MFA configuration on mobile devices
- **Self-Service**: Developers can obtain repository access without ops team intervention

## 👤 User Stories

- As a **developer**, I want to authenticate to Git repos with MFA so my credentials are temporary and secure
- As a **developer**, I want a single CLI command to set up access so I can start working without manual configuration
- As a **developer on any OS**, I want the tool to work on my platform (Mac/Windows/Linux) without modification
- As a **security team member**, I want ephemeral credentials so compromised tokens expire automatically
- As an **ops team member**, I want self-service access so I don't need to manually provision credentials for 60+ developers

## 🔄 Authentication Flow

```text
1. Developer runs CLI command: mfa-git-auth
   ↓
2. CLI prompts for MFA code (from authenticator app)
   ↓
3. CLI calls AWS STS AssumeRole with MFA code
   ↓
4. AWS returns temporary credentials (access key, secret key, session token)
   ↓
5. CLI configures Git credential helper with temporary credentials
   ↓
6. CLI generates QR code for MFA app setup (first-time use)
   ↓
7. Developer uses Git commands normally — credentials are injected automatically
   ↓
8. Credentials expire after configured duration — developer re-runs CLI
```

## 🧱 Core Components

### CLI Interface

- **Auth Command**: Primary command that initiates MFA authentication flow
- **Status Command**: Shows current credential expiration and validity
- **Configure Command**: Sets up AWS profile and Git configuration
- **QR Code Generator**: Displays QR code in terminal for authenticator app enrollment

### AWS Integration

- **STS Client**: Boto3-based STS AssumeRole with MFA token
- **Cross-Account Access**: Assumes role in target AWS account that hosts Git repositories
- **Credential Store**: Temporary credentials stored in AWS credential file with expiration metadata

### Git Configuration

- **Credential Helper**: Configures `git-credential-store` or platform-native helper
- **Multi-Repo Support**: Single authentication covers all repositories under the configured account

### Cross-Platform Support

- **macOS**: Keychain integration for credential storage
- **Windows**: Windows Credential Manager integration
- **Linux**: libsecret/gpg integration for credential storage

## 📚 References

- [Architecture Diagram](./diagram.puml)
- Blog post: [MFA Security for Git Repositories](https://yokharian.dev/posts/mfa-for-git-repositories)