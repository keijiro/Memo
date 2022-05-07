# Ubuntu Linux System

## インストール手順

- Install Ubuntu using [rufus](https://rufus.ie/) and a USB stick.
- Install the NVIDIA dirver from "Software & Updates" - "Additional Drivers".
- Install Mozc input method from "Region & Language" - "Input Sources".
  - [Modify the keymap](https://qiita.com/nabenabe0928/items/09affae67df9c150ad50) to switch IME with ctrl + space.
- Install [Unity Hub](https://public-cdn.cloud.unity3d.com/hub/prod/UnityHub.AppImage).
- Install Git, Vim and Vulkan.
  - `sudo apt install git vim vulkan-utils`
- Sign in to GitHub and [add a new SSH key](https://github.com/settings/keys).
  - `ssh-keygen -t rsa -b 4096 -C "keijiro@gmail.com"`
- Clone [dotfiles](https://github.com/keijiro/dotfiles) and run `setup`.
- (Optional): [Install the latest NVIDIA driver](https://linuxconfig.org/how-to-install-the-nvidia-drivers-on-ubuntu-18-04-bionic-beaver-linux)
- (Optional): [Disable power management on Wi-Fi](https://unix.stackexchange.com/questions/269661/how-to-turn-off-wireless-power-management-permanently)
- (Optional): [Disable power management on audio driver](https://ubuntuforums.org/showthread.php?t=1897012&p=11546745#post11546745)
- (Optional): [Disable USB audio level control](https://askubuntu.com/questions/65565/headphone-output-is-far-too-loud)

## Unity 適合バージョン

2022 年 5 月時点では Ubuntu 22.04 LTS への対応は行われていない。 Unity Hub と Unity Editor の両方で動作に問題が発生する。 Ubuntu 21.04 LTS を使用するのが無難。
