# Security and credential handling

The workflow files in this repository are sanitized public exports.

Before publishing, instance-specific and sensitive values were removed or replaced, including API keys, credential bindings, personal email addresses, webhook identifiers, local user paths, and private infrastructure identifiers.

## Before importing

- Recreate or reconnect credentials inside your own n8n instance.
- Replace all `YOUR_*` placeholders with your own configuration.
- Store secrets in n8n credentials or environment variables where possible.
- Never commit a workflow export containing live API keys.
- Review webhook URLs and public storage domains before enabling the workflow.

## If a secret is accidentally committed

Removing it from the latest commit is not enough. Revoke or rotate the credential at the provider immediately, then remove it from Git history if necessary.
