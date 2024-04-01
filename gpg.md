# GPG (GNU Privacy Guard) Usage Guide

GPG (GNU Privacy Guard) is a powerful tool for secure communication and data encryption. This guide will walk you through the basic usage of GPG for managing keys, encrypting and decrypting files, and more.

## Installation

```sh
sudo pacman -S gnupg
```

## Basic Usage

### Key Generation

Generate a new GPG key pair:

```sh
gpg --gen-key
```

Follow the prompts to enter your information, including your name, email address, and a passphrase.

### Listing Keys

List GPG keys:

```sh
gpg --list-keys
```

```sh
gpg --list-secret-keys
```

Result:

![gpg1](https://github.com/loctvl842/development_wiki/assets/80513079/c5827609-d190-4876-bf71-1370672f45e2)

### Key Management

gpg --edit-key <key_id>

# pass

Password management should be simple and follow Unix philosophy. With pass, each password lives inside of a gpg encrypted file whose filename is the title of the website or resource that requires the password. These encrypted files may be organized into meaningful folder hierarchies, copied from computer to computer, and, in general, manipulated using standard command line file management utilities.

`pass` is a simple password manager for the command line. pass is a shell script that makes use of existing tools like [GnuPG](https://wiki.archlinux.org/title/GnuPG), [tree](https://archlinux.org/packages/?name=tree) and [Git](https://wiki.archlinux.org/title/Git).

## Installation

```sh
sudo pacman -S pass
```

## Basic Usage

### Initialization and Git Integration

Initialize `pass` with your GPG key and set up **git**:

```sh
pass init <gpg_key_id>
pass git init
```

### Storing Passwords

Store a password for a specific service (e.g., GitHub):

### Listing and Searching Passwords

List all stored passwords:

```sh
pass
```

Search for a password:

```sh
pass find <search_term>
```

### Viewing and Editing Passwords

Show a password:

```sh
pass show github
```

Edit a password (opens in the default text editor):

```sh
pass edit github
```

### Copying Passwords

Copy a password to the clipboard without displaying it:

```sh
pass show -c github
```

The password will be copied to the clipboard and cleared after a short time.

### Restore Removed Passwords

```sh
pass git revert <commit_id>
```

**NOTE:** When utilizing `Git` commands within `pass`, make sure to use `pass git` instead of `git`. This ensures adherence to the conventional commit style of `pass`.

### Usage In Remote Systems

**From `Original System`:**

```sh
mkdir ~/guardian-keys
cd ~/guardian-keys
```

```sh
gpg --output public.gpg --armor --export <email>
```

```sh
gpg --output private.gpg --armor --export-secret-key <email>
```

**Copy to remote system:**

```sh
scp -r /path/to/guardian-keys username@remote_server:/path/to/destination
```

**From remote system, import keys:**

```sh
gpg --import public.gpg
gpg --import private.gpg
```

**Test it out:**

- gpg

```sh
gpg --list-keys
gpg --list-secret-keys
```

- pass

```sh
pass show <filename>
```
