# Conda

## Installation

**Quick command line install `Conda`**

```sh
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
```

> **Source:** https://docs.conda.io/projects/miniconda/en/latest/

If you do not like the environment's name shown in shell, do this:

```sh
conda config --set changeps1 false
```

Or edit file `.condarc` with this content:

```.condarc
changeps1: false
```

### Conda Usage:

**1. Update Conda:**

```sh
conda update conda
```

**2. Create a Conda Environment:**

```sh
conda create --name myenv python=3.x
```

**3. Activate the environment:**

```sh
conda activate myenv
```

**4. Install packages:**

```sh
conda install <package-name>
```

**5. Deactivate the Environment:**

```sh
conda deactivate
```

**6. List Environment:**

```sh
conda env list
```

**7. Remove Environment:**

```sh
conda env remove -n myenv
```

**8. Export the Environment:**

You can export the environment configuration to a YAML file:

```sh
conda env export > env.yaml
```

To recreate the environment elsewhere, you can use:

```sh
conda env create -f environment.yml
```

Generate the requirements.txt file for the specific environment:

```sh
conda list --export -n myenv > requirements.txt
```

### Lazy Load Conda

To speed up the shell startup time, instead config `zshrc` or `bashrc` like below:

```sh
__conda_setup="$(\"${CONDA_DIR}/bin/conda\" 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
  eval "$__conda_setup"
else
  if [ -f "${CONDA_DIR}/etc/profile.d/conda.sh" ]; then
    . "${CONDA_DIR}/etc/profile.d/conda.sh"
  else
    export PATH="${CONDA_DIR}/bin:$PATH"
  fi
fi
unset __conda_setup
```

I lazily load the `Conda` environment by adding the following line to the `.zshrc` or `.bashrc` file:

```sh
# Lazy load Conda
conda() {
  __conda_setup="$('/home/loc/miniconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
  if [ $? -eq 0 ]; then
    eval "$__conda_setup"
  else
    if [ -f "/home/loc/miniconda3/etc/profile.d/conda.sh" ]; then
      . "/home/loc/miniconda3/etc/profile.d/conda.sh"
    else
      export PATH="/home/loc/miniconda3/bin:$PATH"
    fi
  fi
  unset __conda_setup
    if command -v conda &> /dev/null; then
        echo "Conda is already loaded."
    else
        __conda_setup="$('/home/loc/miniconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
        if [ $? -eq 0 ]; then
            eval "$__conda_setup"
        else
            if [ -f "/home/loc/miniconda3/etc/profile.d/conda.sh" ]; then
                . "/home/loc/miniconda3/etc/profile.d/conda.sh"
            else
                export PATH="/home/loc/miniconda3/bin:$PATH"
            fi
        fi
        unset __conda_setup
    fi
}
```
