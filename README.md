# Terraform Beginner Bootcamp 2023

## Semantic Versioning  :mage:

This is project is going to utilise semantic versioning for its tagging.
[semver.org](https://semver.org/)

The general format: 

**MAJOR.MINOR.PATCH**, eg. `1.0.1`

-  **MAJOR** version when you make incompatible API changes
-  **MINOR** version when you add functionality in a backward compatible manner
-  **PATCH** version when you make backward compatible bug fixes

## Install the Terraform CLI

### Consideration with Terraform CLI changes

The Terraform CLI installation instructions changed due to gpg keyring changes. So we needed to refer to the latest install CLI documentation via Terraform and change scripting for install.
[Install Terraform CLI]
(https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

### Consideration with Linux Distribution.

This project is build against Ubuntu. 
Please consider checking your Linux Distribution and change accordingly to distribution needs.
[How to check OS version in Linux](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)

Example of Checking OS version.
```sh

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
### Refactoring into Bash scripts.
While fixing Terraform CLI gpg depreciation issues. We notice that bash script were considerable amount of code changes. So we decided to create bash script to install Terraform CLI.

This bash script is located here [./bin/install_terraform_cli](./bin/install_terraform_Cli)

- This will keep Gitpod Task file Tidy ([.gitpod.yml](.gitpod.yml))
- This will allow us an easier to debug and execute manually Terraform CLI install.
- This will allow better portability for other projects that need to install Terraform CLI.

#### Shebang

A Shebang (pronounce Sha-bang) tells the bashscript what program that will interpret the script.

ChatGPT recommended this format for bash `#!/usr/bin/env bash` 

- for portability for different OS distribution.
- Will search for users PATH for bash executable.

https://en.wikipedia.org/wiki/Shebang_%2528Unix%2529

#### Execution considerations.

While  executing bash script we can use the `./` shorthand notiation for bash script.

eg. `./bin/install_terraform_cli`

If we are using script in .gitpod.yml we need to point a script to a program to interpret it.

eg. source `./bin/install_terraform_cli` 

#### Linux Permissions considerations.

In order to make our bash scripts executable we need to change linux permissions for fix to be executable at user mode.

```sh
chmod u+x ./bin/install_terraform_cli
```

Alternatively

```sh
chmod 744 ./bin/install_terraform_cli
```

https://en.wikipedia.org/wiki/Chmod

### Githu Lifecycle (Before, Init, Command) 

We need to be careful with Init command because it will not rerun if we restart an existing workspace.

https://www.gitpod.io/docs/configure/workspaces/tasks


### Working ENV Vars

#### env command

We can list out all Environment Variables (Env Vars) using the `env` command.

We can set specific Enviroment Variables using grep eg. `env | grep AWS_`

#### Setting and Unsetting Env Vars

In the terminal we can set using `export HELLO=world`

In the terrminal we unset using `unset HELLO`

We can set an env var temporarily when just running a command

```
HELLO='world' ./bin/print_message
```

Within a bash script we can set env without writing export eg.

```sh
#!/usr/bin/env bash

HELLO='world'

echo $HELLO
```

### Printing Vars

We can print an env var using echo eg. `echo $HELLO`

### Scoping of Env Vars

When you open up new bash terminals in VSCode it will not be aware of env vars that you have set in another window.

If you want to Env Vars to persist across all future bash terminals that are open you need to set env vars in your bash profile. eg. `.bash_profile`

### Persisting Env Vars in Gitpod

We can persist env vars into gitpod by storing them in Gitpod Secrets Storage.

```
gp env HELLO='world'
```

All future workspaces launched will set the env vars for all bash terminals opened in thoes workspaces.

You can also set en vars in the `.gitpod.yml` but this can only contain non-senstive env vars.