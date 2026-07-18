# Build Zig 0.15.2 (Termux aarch64 deb) via GitHub Actions

This fork branch packages **Zig 0.15.2** for Termux (Bionic), for hosts that need
Ghostty / libghostty-vt (`requireZig` locks **0.15.x**).

## Base commit

```
6bd499e7135a0a06a524087ade4dc58a607a2cfc  bump(main/zig): 0.15.2
```

## Why not Docker for zig?

`zig` is listed in `scripts/big-pkgs.list`. Official CI therefore sets
`docker-build=false` and runs `./build-package.sh` on the **ubuntu runner host**
(with zram + free-space + setup-ubuntu/sdk), not `./scripts/run-docker.sh`.

See `.github/workflows/packages.yml` and this workflow:
`.github/workflows/build-zig-0.15.2.yml`.

## Trigger

```bash
# from a machine with gh auth
gh workflow run "Build zig 0.15.2 (aarch64)" \
  --repo Haleclipse/termux-packages \
  --ref build/zig-0.15.2

# watch
gh run watch --repo Haleclipse/termux-packages
```

Or: GitHub → Actions → **Build zig 0.15.2 (aarch64)** → Run workflow.

## Install on phone (Termux)

```bash
# download artifact zig-0.15.2-aarch64-deb, unzip
pkg uninstall zig   # drop 0.16 if installed
pkg install ./zig_0.15.2_aarch64.deb
# or: dpkg -i ./zig_0.15.2_aarch64.deb && pkg install -f

zig version   # expect 0.15.2
```

Note: the Termux package installs `zig` as a small wrapper that **prefers proot**
when available (upstream Termux workaround). The real binary lives under
`$PREFIX/lib/zig/zig` and can still be invoked directly.

## After install (ghostty-android)

```bash
source ~/WorkSpace/ghostty-android/scripts/env.termux.sh
cd ~/WorkSpace/ghostty-android
git submodule update --init libghostty-vt
make build-native ANDROID_ABIS=arm64-v8a
```
