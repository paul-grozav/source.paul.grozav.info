---
layout: page
ptitle: Amazon Web Services (AWS)
---

### 1. CLI
Since this is not a binary that you just download, unzip and run, I decided to
use a preinstalled version in a container. See the container page here:
https://gallery.ecr.aws/aws-cli/aws-cli .

First, we'll have to configure SSO (
https://awscli.amazonaws.com/v2/documentation/api/latest/reference/configure/sso.html
), to gain access to the account.

I have created a `.ini` syntax config file:
```ini
# ============================================================================ #
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# ============================================================================ #
# Defines the default session, which is described below
[default]
sso_session = my-sso
sso_account_id = 111122223333
sso_role_name = readOnly
region = us-west-2
output = text

# Can be pointed to by using parameter --profile or env var: AWS_DEFAULT_PROFILE
[profile user1]
sso_session = my-sso
sso_account_id = 444455556666
sso_role_name = readOnly
region = us-east-1
output = json

# sso_session definition, that is used by default
[sso-session my-sso]
sso_region = us-east-1
sso_start_url = https://my-sso-portal.awsapps.com/start
sso_registration_scopes = sso:account:access
# ============================================================================ #
```

Then I login to get the token.

```sh
mkdir -p /data/aws{,/sso} &&
podman run --rm -it \
  -v /data/aws/config.ini:/root/.aws/config.ini:ro \
  -v /data/aws/sso:/root/.aws/sso:rw \
  -e AWS_CONFIG_FILE=/root/.aws/config.ini \
  public.ecr.aws/aws-cli/aws-cli:2.15.34 \
  sso login --no-browser
```
Open the link with the (prefilled) code at the end, and click "Confirm and
continue" and then "Allow access". At this point the aws CLI client will create
two `.json` files in `/data/aws/sso/cache` which contain the access tokens.

These tokens are required for future commands to work.

You can remove the tokens by running `aws sso logout` and
`rm -f /data/aws/sso/cache/*` .

With these tokens in place, you can run commands like:
- `aws ec2 describe-instances`
- `aws s3 ls`
- and others ...
