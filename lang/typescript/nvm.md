# NVM (Node Version Manager)

## Installation

## Usage

## Lazy Loading NVM

To speed up the shell startup time, instead config `zshrc` or `bashrc` like below:

```bash
# zsh
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```

For `nvm` especially, I recommend to lazy load it becauase above source script will slow down the shell startup time. To lazy load `nvm` by below script:

```bash
export NVM_DIR="$HOME/.nvm"
nvm_lazy_load() {
  echo 'nvm is loading...'
  if command -v nvm &> /dev/null; then
    [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
    [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
  else
    echo "nvm command not found. Make sure nvm is installed and in your PATH."
  fi
}

# Use a command that triggers nvm_lazy_load when needed
nvm() {
  nvm_lazy_load
  command nvm "$@"
}
```
