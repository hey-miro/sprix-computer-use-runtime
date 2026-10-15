# Sprix Windows release signing

The release workflow signs `computer-use-napi.win32-x64.node`, verifies its
Authenticode chain, loads it with Node.js 22, and only then publishes the fixed
GitHub release.

## Preferred: Microsoft Artifact Signing

Create a Public Trust certificate profile in Azure Artifact Signing and grant
the GitHub OIDC application the `Artifact Signing Certificate Profile Signer`
role. Configure these repository settings:

Secrets:

- `AZURE_CLIENT_ID`
- `AZURE_TENANT_ID`
- `AZURE_SUBSCRIPTION_ID`

Variables:

- `ARTIFACT_SIGNING_ENDPOINT`
- `ARTIFACT_SIGNING_ACCOUNT_NAME`
- `ARTIFACT_SIGNING_CERTIFICATE_PROFILE_NAME`

The Entra application must have a federated credential for repository
`hey-miro/sprix-computer-use-runtime`, branch `codex/windows-authenticode`.
No Azure client secret or exportable signing key is used.

## Existing PFX fallback

Only use this path when an existing publicly trusted code-signing certificate
is already available as a PFX. Configure:

- `WINDOWS_CODE_SIGNING_PFX_BASE64`
- `WINDOWS_CODE_SIGNING_PASSWORD`

Do not commit a PFX or its password. Run the workflow manually and choose the
matching signing provider. Self-signed certificates are not release-eligible.
