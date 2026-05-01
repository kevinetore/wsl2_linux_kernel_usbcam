## WSL2 Linux kernel usb cam
Configuration file to build the kernel to access the usb camera connected to the host PC.

## Host

- Windows 11
- WSL2 Ubuntu 22.04

## WSL kernel

- 6.6.87.2

## Usage

### Compile the kernel

```
# compile all the modules according to the config file
sudo make KCONFIG_CONFIG=6.6.87.2/config-wsl modules -j$(nproc)
# install all the modules according to the config file
sudo make KCONFIG_CONFIG=6.6.87.2/config-wsl modules_install -j$(nproc)
```

Above commands will get you the WSL kernel `./vmlinux`.

### Replace the default kernel

Copy the kernel: `sudo cp ./vmlinux /mnt/c/WSL/kernel/`

Paste the following in your configuration located at `C:/Users/{your user name}/.wslconfig`:

```
[wsl2]
kernel=C:\\WSL\\kernel\\vmlinux
```

shutdown WSL: `wsl --shutdown`

enter WSL and check the version: `uname -a`
If you see the suffix (-usb-add), it means you have succeeded.

### Sharing the camera's

On the Windows host: `winget install usbipd`.

#### share your camera, suppose its BUSID is "3-1"
`usbipd bind --busid 3-1`

#### Attach to WSL

`usbipd attach --wsl --busid 3-1`

You should not be able to see the video devices within WSL: `ls /dev/video*`.


### Useful links

[wsl2 linux kernel enable conf](https://github.com/PINTO0309/wsl2_linux_kernel_usbcam_enable_conf)
[gelf.h no such file or dir](https://github.com/sslab-gatech/opensgx/issues/20)
[openssl/bio.h no such file or directory](https://stackoverflow.com/questions/59016911/fatal-error-openssl-bio-h-no-such-file-or-directory)
[BTF vmlinux.btf pahole is not available](https://stackoverflow.com/questions/61657707/btf-tmp-vmlinux-btf-pahole-pahole-is-not-available)

