# Morningstar
Hail Yourself 🤟🏻💗

## GitHub App: authenticate as an installation

To authenticate as a GitHub App installation, send a REST API `POST` request to
`/app/installations/INSTALLATION_ID/access_tokens`. Include your JSON web token (JWT) in the
`Authorization` header of your request. Replace `INSTALLATION_ID` with the ID of the installation
that you want to authenticate as.

For example, send this curl request. Replace `INSTALLATION_ID` with the ID of the installation and
`JWT` with your JSON web token:

```sh
curl --request POST \
  --url "https://api.github.com/app/installations/INSTALLATION_ID/access_tokens" \
  --header "Accept: application/vnd.github+json" \
  --header "Authorization: Bearer JWT" \
  --header "X-GitHub-Api-Version: 2026-03-10"
```

Optionally, you can include a `permissions` body to select the access token's permissions, and a
`repositories` body to limit access to specific repositories. See the GitHub REST API documentation
for creating an installation access token for an app for the full request schema and response.

> **Note:** Keep your JWT (and the private key used to sign it) secret. Never commit real tokens or
> keys to this repository—use the `JWT` and `INSTALLATION_ID` placeholders above.
