# Camera Drivers for MIC-AI Series

This repository is the download center for Advantech MIC-AI camera drivers. It contains:

- **Auto-install script** (`install_camera.sh` + `online_mode.sh`): select your camera from a menu, and the script downloads and installs the correct driver for your system.
- **Driver list** (`internet_list.sh`): the list of driver packages the script can download. The script always reads the latest version of this file from GitHub.
- **Driver packages**: stored as [GitHub Releases](https://github.com/advantech-edge-ai/MIC-AI-camera-driver/releases).

## Requirements

- An Advantech MIC-AI system running the **Advantech BSP image** (the script reads the board model and JetPack or IGX version from `/opt/version`)
- Supported boards:
  - Orin Nano / Orin NX: MIC-711, MIC-712AR, MIC-713
  - AGX Orin: MIC-732, MIC-733AO
  - AGX Thor: MIC-735AR (IGX 2.0), MIC-741, MIC-742, MIC-743, MIC-743RB
- Internet access to `github.com`
- `sudo` permission

> A camera driver is available only for specific board and JetPack combinations. Check the [Releases](https://github.com/advantech-edge-ai/MIC-AI-camera-driver/releases) page or `internet_list.sh` to confirm your combination is supported.

## Quick Start

**1. Download the script**

```bash
git clone https://github.com/advantech-edge-ai/MIC-AI-camera-driver.git
cd MIC-AI-camera-driver
```

**2. Run the script**

```bash
sudo bash install_camera.sh
```

**3. Follow the menu**

1. Select the camera **Vendor** (for example: `otobrite`)
2. Select the camera **Sensor** (for example: `imx728`)
3. Select your **system model** and JetPack version (for example: `MIC-733AO  AGX Orin  JP6.2`). This menu is shown even when only one model is available, so you can confirm the driver is for your system
4. The script checks that the package matches your system (see [Device Check](#device-check))
5. Confirm with `y` to download and install

Enter `0` in any menu to go back or exit.

**4. Reboot and start the camera**

Most driver installers **reboot the system automatically** when installation finishes. After the reboot, go to the install folder shown at the end of the installation and run:

```bash
cd vendors/<vendor>/<sensor>/<platform>/<board>/<jetpack>
sudo bash init.sh     # load the camera driver (run again after every reboot)
bash cam.sh 0         # show the image from /dev/video0
```

`cam.sh` opens a preview window, so run it from the system's desktop (not over SSH). The number is the video device index (`/dev/video0`, `/dev/video1`, ...).

## Device Check

After you select a package, the script compares it with your system before anything is installed:

| Result | What happens |
|---|---|
| Package matches your board, JetPack, and SoC | Continues to installation |
| Package is for a different SoC (for example, a Thor package on an Orin system) | **Refused.** The script returns to the menu |
| Package is for a different board or JetPack version | **Warning** `Install this driver anyway? [y/N]` |
| Board model or JetPack version cannot be detected | **Warning** `Install this driver anyway? [y/N]` |

**When you see a warning, answer `N`** (or press Enter). Installing a driver built for a different board or JetPack can leave the camera not working or the system unable to boot.

## Notes

- **Install one camera driver per system.** Installing a second driver, or the same driver again, may conflict with the previous one. To change to a different camera, reflash the Advantech BSP image first.
- Messages such as `Internal server (gitlab) not reachable` and `No bundled vendors/ tree` are normal. The script uses GitHub as the download source.

## Manual Download

The auto-install script is the recommended way to install a driver. The driver packages are also available on the [Releases](https://github.com/advantech-edge-ai/MIC-AI-camera-driver/releases) page.

## Questions

If you have questions about a driver package or the installation process, please open an [Issue](https://github.com/advantech-edge-ai/MIC-AI-camera-driver/issues) in this repository.

## Next Step


