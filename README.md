# oidc-deploy-demo

A minimal CI workflow that deploys without a stored deploy credential for its own identity:

1. Build a tiny static site.
2. Request the job's GitHub Actions OIDC token.
3. Exchange it ([RFC 8693](https://www.rfc-editor.org/rfc/rfc8693)) for a short-lived access token.
4. Call an internal console's API with that token to record the deployment.

Every endpoint and audience is a repository secret, so none of them appear in public logs.

## Secrets

| Secret | Meaning |
|---|---|
| `DEPLOY_AUTH_URL` | Base URL of the token endpoint host |
| `DEPLOY_APP_URL` | Base URL of the console |
| `DEPLOY_OIDC_AUDIENCE` | Audience requested on the GitHub OIDC token |
| `DEPLOY_TARGET_AUDIENCE` | Audience of the access token (the console) |
| `EDGE_CLIENT_ID` / `EDGE_CLIENT_SECRET` | Service credential for the edge in front of both hosts |

Without them, the workflow builds and skips the deploy.
