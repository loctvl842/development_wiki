# Flutter Version Manager (FVM)

## Installation

```sh
yay -S fvm
```

## Usage

1. Install a version of Flutter

```sh
fvm install 3.7.12
```

2. Use a version of Flutter
```sh
fvm use 3.7.12
```

3. List installed versions of Flutter

```sh
fvm ls
```

4. List available versions of Flutter
```sh
fvm releases
```

5. Install a version of Flutter from the stable channel
```sh
fvm install stable
```

## Usage

### Install packages

Add the target package to `pubspec.yaml` under `dependencies`.

Read more [here](https://docs.flutter.dev/packages-and-plugins/using-packages)

Then run:

```sh
fvm flutter pub get # If you are using FVM
flutter pub get
```
