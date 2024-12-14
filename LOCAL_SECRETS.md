# Local Secrets

Before starting this step, you need to make sure that you have a way to pass handle secrets.

With [tooling](./TOOLING.md), you already have a Password Manager to store secrets but you need a way
for these secrets to be used in your scripts.

We have this strict rule to

- **NEVER, EVER, EVER** store secrets in files.
- I repeat: **NEVER STORE SECRETS IN FILES**

There is no reason to do this and once we start doing it for "not so important secrets", it becomes
a habit.

Better to learn the proper way.

## Use a vault

Here we are using Ansible Vault.

### Installation

0. Install ansible

```sh
$ # Mac Users:
$ brew install ansible
```

```sh
$ # WSL / Linux users:
$ sudo apt update
$ sudo apt install ansible
```

```sh
$ # All users
$ ansible-vault --version
```

```sh
$ ansible-vault --version
```

- [✔] Ansible installed: I see the version !

### Setup

1. Create a `.zsh_vault` file in your home directory (create a strong password you can remember and store it in the password manager):

```sh
$ ansible-vault create ~/.zsh_vault
```

2. Add the following code to your `~/.zshrc` file:

```zsh
export EDITOR=vim
ZSH_VAULT=${HOME}/.zsh_vault

function vault.load() {
    echo "Loading zsh vault..."
    if [ -f "${ZSH_VAULT}" ]; then
        ansible-vault view --ask-vault-pass "${ZSH_VAULT}" | source /dev/stdin
    else
        echo "Error: ${ZSH_VAULT} not found."
        return 1
    fi
}

function vault.edit() {
    echo "Editing zsh vault..."
    if [ -f "${ZSH_VAULT}" ]; then
        ansible-vault edit --ask-vault-pass "${ZSH_VAULT}"
        echo "${ZSH_VAULT} edited. Use vault.load if you want your changes in the environment."
    else
        echo "Error: ${ZSH_VAULT} not found."
        return 1
    fi
}

```

3. Source your `.zshrc` file to apply the changes:

```sh
$ source ~/.zshrc
```

- [✔] Vault ready 🦄

### Storing Secrets

1. **Edit** your secrets (and enter your password):

```sh
$ vault.edit
```

Write this in the vault (note this is done with vim):

```sh
export TF_VAR_hetzner_api_token="your api token here"
```

- [✔]  API key saved in vault 🔑

### Using Secrets

You can now use `vault.load` when you start a session and your environment variable will be available in your terminal session.

```sh
$ vault.load
```

- [✔]  It works 🎉 !
