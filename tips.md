# vim

## Tabs and Spaces

Add the below lines in ~/.vimrc
This wil make Vim to make use of spaces when tab is pressed
Very useful for Yaml file editing.

```
set tabstop=2 shiftwidth=2 expandtab # for editing
set nu # numbers
set ai # auto indent
```

## Multiline select

In visual mode we can use arrow keys or j(down) for selecting the lines
Press 'v' to activate visual mode

## Indentation

In visual mode we can press > to indent
In command mode we can press >> to indent

## yank and paste

### visual mode

- y for yank
- p for paste

### command mode

- yy for yank
- p for paste

# export and alias

```
# useful to generate the yaml definition from imperative commands
export do="--dry-run=client --o yaml"

# Delete the pods forcefully without any waiting time, saves 30s
export now="--force --grace-period=0"

alias kn="kubectl config set-context --current --namespace"
alias cc='kubectl config view --minify -o jsonpath="{..current-context} - {..namespace}"'

kc() {
  k config use-context $1;
  kn default;
}

```

# kubectl create

- clusterrole
- clusterrolebinding
- role
- rolebinding
- configmap
- secret
- deployment
- namespace
- service
- ingress
- serviceaccount
- token (service account token)
