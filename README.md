# nvidia-all

### This fork tracks [upstream](https://github.com/Frogging-Family/nvidia-all) closely and adds some spice on top.

> [!NOTE]
> **New to nvidia-all?** Start with the [upstream project README](https://github.com/Frogging-Family/nvidia-all/blob/master/README.md) to get familiar with the general concept, the available options and the build workflow. Come back here once you know the basics.
> 
> Please do not report bugs to the upstream repository when using this fork.

---

<br />

### Extra knobs in `customization.cfg`

All on top of what upstream already offers - knobs live in [`customization.cfg`](https://github.com/damachine/nvidia-all/blob/staging/customization.cfg#L28-L192).

- [nvidia-all](#nvidia-all)
    - [This fork tracks upstream closely and adds some spice on top.](#this-fork-tracks-upstream-closely-and-adds-some-spice-on-top)
    - [Extra knobs in `customization.cfg`](#extra-knobs-in-customizationcfg)
      - [`_build_utils_package_only` - build userspace/utility packages only (skip kernel module package)](#_build_utils_package_only---build-userspaceutility-packages-only-skip-kernel-module-package)
      - [`_driver_version_tag` - direct known-version selector (skip interactive version prompt)](#_driver_version_tag---direct-known-version-selector-skip-interactive-version-prompt)
      - [`_target_kernel` - target a specific installed kernel pkgbase for non-DKMS builds](#_target_kernel---target-a-specific-installed-kernel-pkgbase-for-non-dkms-builds)
      - [`_module_signing` - install post-transaction NVIDIA module signing hook (experimental)](#_module_signing---install-post-transaction-nvidia-module-signing-hook-experimental)
    - [Install procedure](#install-procedure)

<br />

#### `_build_utils_package_only` - build userspace/utility packages only (skip kernel module package)

| Value | Description |
|---|---|
| `"false"` | Default behavior (build kernel module package and userspace packages) |
| `"true"` | Skip kernel module package and only build userspace/utility packages |

Userspace/utility package set:

- `opencl-nvidia-tkg`
- `nvidia-utils-tkg`
- `nvidia-settings-tkg`
- `lib32-opencl-nvidia-tkg` (when `_lib32="true"`)
- `lib32-nvidia-utils-tkg` (when `_lib32="true"`)

Example:

```properties
_build_utils_package_only="true"
```

<br />

#### `_driver_version_tag` - direct known-version selector (skip interactive version prompt)

| Value | Description |
|---|---|
| `""` | Default behavior (normal interactive driver selection) |
| `"595.44.03"` | Vulkan developer branch preset |
| `"595.58.03"` | 595 regular branch preset |
| `"580.142"` | 580 regular branch preset |
| any other string | Falls back to interactive prompt |

Example:

```properties
_driver_version_tag="595.45.04"
```

<br />

#### `_target_kernel` - target a specific installed kernel pkgbase for non-DKMS builds

| Value | Description |
|---|---|
| `""` | Default autodetection behavior |
| kernel pkgbase name (for example `"linux-tkg"`) | Resolve that installed kernel and build/install modules for it |

Only relevant when `_dkms="false"` or `_dkms="full"`.

Examples:

```properties
_target_kernel="linux-tkg"
```

```shell
grep -r "" /usr/lib/modules/*/pkgbase
```

<br />
<br />

> These options below were added for personal testing and are left in for anyone who might find it useful.

#### `_module_signing` - install post-transaction NVIDIA module signing hook (experimental)

```properties
_module_signing="false"
```

When set to `"true"`, nvidia-all installs a root-run pacman hook that signs installed NVIDIA modules after package installation/upgrade.

Requirements:

- Kernel build tree must provide `scripts/sign-file`
- Key path from `CONFIG_MODULE_SIG_KEY` must exist
- Certificate `certs/signing_key.x509` must exist

Notes:

- No effect when `_disable_libalpm_hook="true"`
- For linux-tkg kernels, this typically requires building headers with `_install_signing_keys="true"`

> [!WARNING]
> The private key is stored on disk (root-readable). Anyone with root or physical access can extract it and sign arbitrary modules. If security is a concern, use full-disk encryption (for example LUKS).

---

<br />

### Install procedure

> [!TIP]
> **Recommended:** Use [tkginstaller](https://github.com/damachine/tkginstaller) for an interactive guided build experience with fzf menus, automatic dependency handling, and config management.
> ```shell
> # install tkginstaller (AUR)
> yay -S tkginstaller-git
> 
> # Use fzf-finder TUI mode, simply run
> tkginstaller
> # Use direct command with package name, for example
> tkginstaller nvidia         # or shortcut n
> tkginstaller linux-nvidia   # or shortcut ln
> ```

(Arch & derivatives)

```shell
git clone https://github.com/damachine/nvidia-all.git
cd nvidia-all
makepkg -si
```

---
