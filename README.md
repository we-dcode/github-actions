# Repository to manage public GitHub actions

## dcode-registry-login

Logs in to `registry.dcode.tech`, Dcode's Terraform module registry, and writes
the token to `~/.terraform.d/credentials.tfrc.json` for `terraform init`.

```yaml
- name: Log in to registry.dcode.tech
  uses: we-dcode/github-actions/dcode-registry-login@<full commit SHA> # <date>
  with:
    username: ${{ secrets.DCODE_REGISTRY_USER }}
    password: ${{ secrets.DCODE_REGISTRY_PASSWORD }}
```

- Pin it to a full commit SHA, not `@main`.
- The username and password come from Dcode. Store them as repository or
  organisation secrets; never write them into a workflow.
- Needs `bash`, `curl` and `jq` on the runner (GitHub-hosted runners have them).
  Nothing is downloaded or installed.
- The token is masked in the log. Other hosts already in the credentials file
  are kept. On a self-hosted runner that keeps its home directory, delete
  `~/.terraform.d/credentials.tfrc.json` when the job ends.
- On failure the step prints the HTTP status and the OAuth2 error code
  (for example `invalid_grant` for a wrong password), never the credentials.
