Skip to content 
# Using alternative package indexes
While uv uses the official Python Package Index (PyPI) by default, it also supports alternative package indexes. Most alternative indexes require various forms of authentication, which requires some initial setup.
Important
Please read the documentation on using multiple indexes in uv — the default behavior is different from pip to prevent dependency confusion attacks, but this means that uv may not find the versions of a package as you'd expect.
## Azure Artifacts
uv can install packages from Azure DevOps Artifacts. Authenticate to a feed using a Personal Access Token (PAT) or interactively using the `keyring` package.
### Using a PAT
If there is a PAT available (eg `$(System.AccessToken)` in an Azure pipeline), credentials can be provided via the "Basic" HTTP authentication scheme. Include the PAT in the password field of the URL. A username must be included as well, but can be any string.
For example, with the token stored in the `$ADO_PAT` environment variable, set the index URL with:
```
$ exportUV_INDEX=https://dummy:$ADO_PAT@pkgs.dev.azure.com/{organisation}/{project}/_packaging/{feedName}/pypi/simple/

```

### Using `keyring`
If there is not a PAT available, authenticate to Artifacts using the `keyring` package with the `artifacts-keyring` plugin. Because these two packages are required to authenticate to Azure Artifacts, they must be pre-installed from a source other than Artifacts.
The `artifacts-keyring` plugin wraps the Azure Artifacts Credential Provider tool. The credential provider supports a few different authentication modes including interactive login — see the tool's documentation for information on configuration.
uv only supports using the `keyring` package in subprocess mode. The `keyring` executable must be in the `PATH`, i.e., installed globally or in the active environment. The `keyring` CLI requires a username in the URL, so the index URL must include the default username `VssSessionToken`.
```
$ # Pre-install keyring and the Artifacts plugin from the public PyPI
$ uvtoolinstallkeyring--withartifacts-keyring

$ # Enable keyring authentication
$ exportUV_KEYRING_PROVIDER=subprocess

$ # Configure the index URL with the username
$ exportUV_INDEX=https://VssSessionToken@pkgs.dev.azure.com/{organisation}/{project}/_packaging/{feedName}/pypi/simple/

```

## Google Artifact Registry
uv can install packages from Google Artifact Registry. Authenticate to a repository using password authentication or using `keyring` package.
Note
This guide assumes `gcloud` CLI has previously been installed and setup.
### Password authentication
Credentials can be provided via "Basic" HTTP authentication scheme. Include access token in the password field of the URL. Username must be `oauth2accesstoken`, otherwise authentication will fail.
For example, with the token stored in the `$ARTIFACT_REGISTRY_TOKEN` environment variable, set the index URL with:
```
exportARTIFACT_REGISTRY_TOKEN=$(gcloudauthapplication-defaultprint-access-token)
exportUV_INDEX=https://oauth2accesstoken:$ARTIFACT_REGISTRY_TOKEN@{region}-python.pkg.dev/{projectId}/{repositoryName}/simple

```

### Using `keyring`
You can also authenticate to Artifact Registry using `keyring` package with `keyrings.google-artifactregistry-auth` plugin. Because these two packages are required to authenticate to Artifact Registry, they must be pre-installed from a source other than Artifact Registry.
The `artifacts-keyring` plugin wraps gcloud CLI to generate short-lived access tokens, securely store them in system keyring and refresh them when they are expired.
uv only supports using the `keyring` package in subprocess mode. The `keyring` executable must be in the `PATH`, i.e., installed globally or in the active environment. The `keyring` CLI requires a username in the URL and it must be `oauth2accesstoken`.
```
# Pre-install keyring and Artifact Registry plugin from the public PyPI
uvtoolinstallkeyring--withkeyrings.google-artifactregistry-auth

# Enable keyring authentication
exportUV_KEYRING_PROVIDER=subprocess

# Configure the index URL with the username
exportUV_INDEX=https://oauth2accesstoken@{region}-python.pkg.dev/{projectId}/{repositoryName}/simple

```

## AWS CodeArtifact
uv can install packages from AWS CodeArtifact.
The authorization token can be retrieved using the `awscli` tool.
Note
This guide assumes the AWS CLI has previously been authenticated.
First, declare some constants for your CodeArtifact repository:
```
exportAWS_DOMAIN="<your-domain>"
exportAWS_ACCOUNT_ID="<your-account-id>"
exportAWS_REGION="<your-region>"
exportAWS_CODEARTIFACT_REPOSITORY="<your-repository>"

```

Then, retrieve a token from the `awscli`:
```
exportAWS_CODEARTIFACT_TOKEN="$(
awscodeartifactget-authorization-token\
--domain$AWS_DOMAIN\
--domain-owner$AWS_ACCOUNT_ID\
--queryauthorizationToken\
--outputtext
)"

```

And configure the index URL:
```
exportUV_INDEX="https://aws:${AWS_CODEARTIFACT_TOKEN}@${AWS_DOMAIN}-${AWS_ACCOUNT_ID}.d.codeartifact.${AWS_REGION}.amazonaws.com/pypi/${AWS_CODEARTIFACT_REPOSITORY}/simple/"

```

### Publishing packages
If you also want to publish your own packages to AWS CodeArtifact, you can use `uv publish` as described in the publishing guide. You will need to set `UV_PUBLISH_URL` separately from the credentials:
```
# Configure uv to use AWS CodeArtifact
exportUV_PUBLISH_URL="https://${AWS_DOMAIN}-${AWS_ACCOUNT_ID}.d.codeartifact.${AWS_REGION}.amazonaws.com/pypi/${AWS_CODEARTIFACT_REPOSITORY}/"
exportUV_PUBLISH_USERNAME=aws
exportUV_PUBLISH_PASSWORD="$AWS_CODEARTIFACT_TOKEN"

# Publish the package
uvpublish

```

## Other indexes
uv is also known to work with JFrog's Artifactory.
Back to top 
![](https://cdn.usefathom.com/?h=https%3A%2F%2Fdocs.astral.sh&p=%2Fuv%2Fguides%2Fintegration%2Falternative-indexes%2F&r=&sid=ESKBRHGN&qs=%7B%7D&cid=93630993)
