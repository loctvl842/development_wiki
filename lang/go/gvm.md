# GVM (Go Version Manager)

## Installation

Follow the instructions at [moovweb/gvm](https://github.com/moovweb/gvm)

```bash
bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
```

## Usage

**1. Install Go version**

```bash
gvm install go1.21.3
```

**2. Use Go version**

```bash
gvm use go1.21.3
```

Check the version

```bash
go version
```

**3. List installed Go versions**

```bash
gvm list
```

**4. Uninstall Go version**

```bash
gvm uninstall go1.21.3
```

**5. List available Go versions**

```bash
gvm list all
```

## Packages

Each go version can have multiple packages installed, the default package is `global`. Multiple projects can use the same go version, so I think it makes sense to have multiple packages, one for each project. To install a package, use the following command:

```bash
gvm pkgset create myproject --local --activate
```

This to manage repositories under local path. All libraiies will be installed under `~/.gvm/pkgsets/go1.21.3/myproject`. To list all packages, use the following command:

```bash
gvm pkgset list
```

## Lazy load gvm

To speed up the shell startup time, instead config `zshrc` or `bashrc` like below:

```bash
export GVM_ROOT=$HOME/.gvm
[[ -s "$HOME/.gvm/scripts/gvm" ]] && source "$HOME/.gvm/scripts/gvm"
```

I lazy load `gvm` by below script:

```bash
export GVM_ROOT=$HOME/.gvm
# Lazy load gvm
gvm_lazy_load() {
  echo 'gvm is loading...'
  if [ -f "$GVM_ROOT/scripts/gvm" ]; then
    # Source gvm script if it exists
    source "$GVM_ROOT/scripts/gvm"
  else
    echo "gvm script not found. Make sure gvm is installed and configured correctly."
  fi
}

# Override the gvm command to trigger lazy loading
gvm() {
  gvm_lazy_load
  # Forward the gvm command to the actual gvm executable
  command gvm "$@"
}
```
