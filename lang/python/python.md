## Python

**Install `python3`:**

```sh
sudo pacman -S python3
```

**Install `pip`:**

```sh
sudo pacman -S python-pip
```

**Create Virtual Environment:**

```sh
python3 -m venv .venv
```

**Activate the virtual environment:**

```sh
source .venv/bin/activate
```

**Create a `requirements.txt`:**

```sh
pip freeze > requirements.txt
```
