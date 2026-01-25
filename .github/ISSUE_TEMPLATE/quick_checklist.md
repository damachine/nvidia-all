# Quick pre-checks before opening an issue

- Ensure kernel headers matching your running kernel are installed (for example, check with `pacman -Qs linux-headers`).
- State which driver you are using: proprietary `nvidia`, `open-gpu-kernel-modules` (NVIDIA).
- If you are using DKMS: rebuild and reinstall DKMS modules for the current kernel and reboot. Example:

```bash
sudo dkms autoinstall
sudo reboot
```

- If you are NOT using DKMS (regular package): rebuild/reinstall the package so the kernel module is compiled against your running kernel's headers. Typical workflow when building locally:

```bash
# in the package directory
makepkg -si
```

- Try the alternative build method (DKMS vs non-DKMS) to compare behavior.
- Check udev rules and modprobe settings (`/etc/modprobe.d`, `system/60-nvidia.rules`).
- Run `nvidia-bug-report.sh` and attach its output if possible.
- Collect `dmesg` and `journalctl -b` output to paste into the issue.

For general guidance and troubleshooting steps see the Arch Wiki: https://wiki.archlinux.org/title/NVIDIA/Troubleshooting

If these steps do not solve the problem, open an issue using the appropriate template and attach the collected outputs.
