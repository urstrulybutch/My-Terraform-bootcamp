# Terraform Beginner Bootcamp 2023

## Semantic Versioning

This project is going to utilize semantic versioning for its tagging
[semver.org](https://semver.org)

The general format:

Given a version number **MAJOR.MINOR.PATCH**, `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes
Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.
``

[install Terraform CLI]
##  Consideration with the Terraform CLI changes
The process of installing the Terraform CLI underwent alterations because of the modifications in the GPG keyring. Consequently, I found it necessary to consult the most up-to-date installation instructions provided in the Terraform documentation and subsequently adapt the scripting for the installation.
[install Terraform CLI](https:developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Linux consideration

This project is built against ubuntu.
I checked the linux distribution and changed accordignly.

## Refactoring into bash scripts
While addressing the deprecation issues related to the Terraform CLI's GPG signatures, I observed that the steps in the Bash scripts were quite extensive. Consequently, I made the decision to create a Bash script specifically for installing the Terraform CLI.

Here is where the bash script is located:[./bin/install_terraform_cli](./bin/install_terrdorm_cli)
- This keeps the Gitpod task file clean and tidy ([.gitpod.yml](.gitpod.yml)
- This allows me to easily debug and execute manually the terraform CLI
- This willl allow better portability for other 

(https://en.wikipedia.org/wiki/File-system_permissions)

### How to check os version on Linux
https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/
Here is an example:
```
$ cat /etc/os-release
PRETTY_NAME="Ubuntu 22.04.3 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.3 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```


 ## Shebang referencing

https://en.wikipedia.org/wiki/shebang_(unix)

A shebang tells the bash ccript what program that will interpret the scripts eg `#!/bin/ bash`


#!/usr/bin/env bash
#!/bin/bash
This is mainly used for portability and for different OS Distribution
Also,this will help search user's PATH for bash execution 
``


### Execution consideration

When executing the bash script we can us the  `./` notation to excute the bash script

eg `./bin/install_terraform_cli`

For the purpose of using a script in .gitpod.yml i needed to point the script to program to interpret it

eg `source ./bin/install_terraform_cli`

### Linux permission consideration

For the bsah scripts to be executable,I needed o change the linux permission for the fix to be executable at the use mode

```sh
chmod u+x /bin/install_terraform_cli
```

alternatively:

```sh
chmod 744 ./bin/install_terraform_cli

https://en.wikipedia.org/wiki/Chmod
```

###  Github lifecycle(before ,Init,command)

I needed to be careful when uisng the init because,it will not rerun if i restart the existing workspace

https://gitpod.io/doc/configure/workspaces/tasks

### Working Env Variables

#### env commands 

I listed out all environment variables (Env Vars) using the `env` command

I can filter specific env vars using grep eg. `env | grep AWS_`

#### setting and unsetting Env Vars

In the terminal i can sret using `export HELLO=`WORLD`

in the terminal i can unset `unset HELLO`

i can set an env var temporarily when just running a command

```
HELLO=`world` ./bin/print_message
```
Within a bash script i can set an env without export 

```sh
#!/usr/bin/env bash

HELLO=`world`

echo $HELLO
```

#### Printing variables

I can print an env var using echo eg. `echo $HELLO`

#### Scoping of Env Vars

opening new bash terminals in VsCode it will bot be aware of env vars that you have set in another window.

if i want env Vars to persist across all future bash terminals that are open i need to set env vars in your bash profile. eg. `.bash_profile`

####  persisting Env Vars in Gitpod

Persisting env Vars into gitpod by storing them in Gitpod secrets storage.

```
gp env HELL=`world`
```

All future launched will set the env vars for all bash terminals oepened the workspaces.

I can also set en vars in `.gitpod.yml` but this can only contain non sensitive env vars.

#### AWS CLI installation

AWS CLI is installed for the project via the bash script ['./bin/install_aws_cli'](./bin/install_aws_cli)

[Getting started this is the reference ](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

If i want to check if my AWS credentials is configured correctly ,I will need to run the following AWS CLI command 
```sh
aws sts get-caller-identity
```
You get a successful json payload return that looks like this.

```json
{
    "UserId": "AIDAQMAWHMSRISCrtFWO5",
    "Account": "02346367353eehh",
    "Arn": "arn:aws:iam::234455566677566:user/Clive-terraform-bootcamp"
}
```

I needed to generate AWS CLI credits from IAM user in order for the user AWS CLI