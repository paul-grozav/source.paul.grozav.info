---
layout: page
ptitle: Amazon Web Services (AWS)
---

### 1. CLI
Since this is not a binary that you just download, unzip and run, I decided to
use a preinstalled version in a container. See the container page here:
https://gallery.ecr.aws/aws-cli/aws-cli .

I've created an alias for `aws` to run it inside the container. Add this to your
`~/.bashrc`:
```sh
AWS_PATH=/data/aws &&
mkdir -p ${AWS_PATH}{,/sso} &&
alias aws='podman run --rm -it \
  -v ${AWS_PATH}/config.ini:/root/.aws/config.ini:ro \
  -v ${AWS_PATH}/sso:/root/.aws/sso:rw \
  -e AWS_CONFIG_FILE=/root/.aws/config.ini \
  public.ecr.aws/aws-cli/aws-cli:2.15.34'
```

Otherwise, use the installer:
```sh
$ cat /data/programs/awscli/readme.txt
# ============================================================================ #
# Authors:
# - Tancredi-Paul Grozav <paul@grozav.info>
# ============================================================================ #
current_ts="$(date +"%Y%m%d%H%M%S")" &&
mkdir ${current_ts} &&
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" \
  -o "${current_ts}/awscli.zip" &&
(
  cd ${current_ts} &&
  unzip awscli.zip &&
  mkdir install_path &&
  $(pwd)/aws/install \
    --install-dir $(pwd)/install_path \
    --bin-dir ${HOME}/.local/bin \
    &&
  true
) &&
true
# ============================================================================ #
```

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

Then I login to get the token: `sso login --no-browser`.

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
