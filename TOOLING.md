# TOOLING

IMPORTANT: On Windows, all setup and tooling have to be done **in WSL**.

Every steop in this tooling takes time and is important to do right. Trying to
do any of this fast will take double the time to debug and fix. Keep it slow.

## Setup Prompt

The prompt is very important to avoid confusion like executing commands in the
wrong place or wrong server.

<img width="527" alt="image" src="https://github.com/user-attachments/assets/8b824b1c-aba4-4e2e-b257-dad0306a13ef">


You can personalise the [colors](https://www.hackitu.de/termcolor256/) where you
see codes that look like `%F{66}` but keep the information shown.

### zsh

You need to use **zsh** as default terminal command (instead of bash).

Add this to `.zshrc`:

```sh
MACHINE=%m # you can replace this with any name that you like to identify "local"
# Function to get Git branch and changes
git_info() {
    local branch=$(git symbolic-ref --short HEAD 2>/dev/null)
    if [[ -n "$branch" ]]; then
        local changes=$(git status --porcelain | awk 'END{print NR}')
        echo -n " %F{66}[${branch}%F{80} ${changes} %F{66}]"
    fi
}

# Set the prompt
setopt PROMPT_SUBST
PROMPT='%F{39}%n%F{115}@%F{208}$MACHINE%F{15} %F{76}%~%F{15}$(git_info)
%F{202}➜ %f'

alias gst='git status'
# add more here as you need...
```

And this to `.bashrc`

```sh
MACHINE=\h # you can replace this with any name that you like to identify "local"
# Function to get Git branch and changes
git_info() {
    local branch=$(git symbolic-ref --short HEAD 2>/dev/null)
    if [ -n "$branch" ]; then
        local changes=$(git status --porcelain | awk 'END{print NR}')
        echo -e " \033[38;5;66m[$branch\033[38;5;80m ${changes} \033[38;5;66m]"
    fi
}

# Set the prompt
PS1='\[\033[38;5;39m\]\u\[\033[38;5;115m\]@\[\033[38;5;208m\]$MACHINE\[\033[38;5;15m\] \[\033[38;5;76m\]\w\[\033[38;5;15m\]$(git_info)\n\[\033[38;5;202m\]➜ \[\033[0m\]'
alias gst='git status'
# add more here as you need...
```

- [✔] Prompt set. I see colors.

## Install Proton Password Manager (or Google Password Manager)

You will be handling secrets and need long and impossible to remember passwords.

Here is some information to [create a safe master
password](https://proton.me/blog/create-remember-strong-passwords) for your
Password Manager.

- [✔] Password Manager installed.
- [✔] Strong password set for the password manager (using trick above).
- [✔] Authenticator (Google Authenticator or Proton Pass) installed (for 2 factor auth).

Change your Github password now for a random string saved in the password manager.
Enable two factor auth.

- [✔] Github password changed.
- [✔] 2FA enabled on Github.

## Create a new SSH key

This key will be used to authentify you on github and other services. The private
part of this key (without the `.pub` extension) should be kept as safe as possible.
If your computer gets stolen, you should remove the public key from the list of
places where you put it.

[Follow this guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) to create an new ssh key and add it to ssh-agent.

Choose a strong random password for this key and save it inside the password manager.

- [✔] New key created.
- [✔] Strong password saved in password manager.
- [✔] Password saved in the ssh agent.

Add the SSH key to your Github account with [this manual](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

- [✔] New SSH key added to Github account.

## Install GPG

You will need this to sign commits. This is important to prove that a certain
commit is done by you and is required in many projects.

WSL: [gpg for WSL](https://blog.jmorbegoso.com/post/configure-github-gpg-key-in-windows-and-wsl/)
LINUX:

```sh
$ sudo apt-get install gnupg
```

- [✔] GPG installed. `gpg --version` working.

Create a new signing key:

- [✔] GPG key created.

Copy the key id:

```sh
$ gpg --list-secret-keys --keyid-format=long
```

On the line that says `rsa4096/[YOUR_GPG_KEY_ID]` for the key above your email, copy this key id.

## Setup Git

Install [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

- [✔] Git installed (`git --version` working)

Setup git configuration:

```sh
$ git config --global user.name "Your Name"
$ git config --global user.email "your.email@example.com"
$ git config --global commit.gpgsign true
$ git config --global user.signingkey YOUR_GPG_KEY_ID
$ git config --global init.defaultBranch main
# Very your configuration (stored in ~/.gitconfig)
$ git config --list
```

- [✔] Git configuration done.

## Clone repository

Create the folder structure for your projects and clone the repo.

```sh
$ cd
$ mkdir git
$ cd git
$ git clone [git+ssh URL of this repository]
```

- [✔] Repository cloned.

Make sure you install the recommended VS Code extensions for this workspace.

Render the preview of this file in your VS Code (in the commands, search for Markdown Preview).

Click on the check box below inside the preview:

- [✔] Check this in the markdown preview.

In the terminal (you can use the one in VS Code: command "Terminal: Focus Terminal")

```sh
$ git add .
$ git commit -m "Test commit"
$ git push
```

If everything is setup correctly, you should have a signed commit pushed to github. Congratulations !

Here I`m porposing some changes [✔]