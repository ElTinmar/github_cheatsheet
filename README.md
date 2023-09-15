# github_cheatsheet

## Setting up git on a new computer 

### Generating a new SSH key

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

copy the public key to your github account

### Config name and user

```
git config user.name "Your Name"
git config user.email "youremail@yourdomain.com"
```

### Automatically launch ssh-agent on login via SSH

You can run ssh-agent automatically when you open bash. Copy the following lines and paste them into your ~/.profile or ~/.bashrc:

```
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add /path/to/your/github_key
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add /path/to/your/github_key
fi

unset env
```

