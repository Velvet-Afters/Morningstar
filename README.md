# Morningstar
Hail Yourself 🤟🏻💗

## GitHub App: authenticate as an installation

To authenticate as a GitHub App installation, send a REST API `POST` request to
`/app/installations/INSTALLATION_ID/access_tokens`. This creates a short-lived installation
access token that you can use to call the GitHub REST API as that installation.

Your request must include a JSON web token (JWT) signed with your GitHub App's private key in the
`Authorization` header. The JWT identifies the app, and GitHub uses it to issue the installation
access token.

### Example request

For example, send this curl request. Replace `INSTALLATION_ID` with the ID of the installation and
`JWT` with your JSON web token:

```sh
curl --request POST \
  --url "https://api.github.com/app/installations/INSTALLATION_ID/access_tokens" \
  --header "Accept: application/vnd.github+json" \
  --header "Authorization: Bearer JWT" \
  --header "X-GitHub-Api-Version: 2026-03-10"
```

### Optional request body

Optionally, you can include a `permissions` body to select the access token's permissions, and a
`repositories` body to limit access to specific repositories. For example:

```json
{
  "permissions": {
    "issues": "write",
    "pull_requests": "read"
  },
  "repositories": ["repo-name-1", "repo-name-2"]
}
```

This keeps the installation token as narrowly scoped as needed.

### Generate a JWT

Before making the request, generate a JWT signed with your app's private key. Use the app's ID as
`iss` and a short expiration window, such as 10 minutes.

```js
const fs = require("node:fs");
const crypto = require("node:crypto");

const appId = "APP_ID";
const pem = fs.readFileSync("private-key.pem", "utf8");
const now = Math.floor(Date.now() / 1000);

const header = Buffer.from(
  JSON.stringify({ alg: "RS256", typ: "JWT" })
).toString("base64url");

const payload = Buffer.from(
  JSON.stringify({
    iat: now - 60,
    exp: now + 600,
    iss: appId
  })
).toString("base64url");

const signer = crypto.createSign("RSA-SHA256");
signer.update(`${header}.${payload}`);
const signature = signer.sign(pem, "base64url");
const jwt = `${header}.${payload}.${signature}`;

console.log(jwt);
```

> **Note:** Keep your JWT (and the private key used to sign it) secret. Never commit real tokens or
> keys to this repository—use the `JWT` and `INSTALLATION_ID` placeholders above.
