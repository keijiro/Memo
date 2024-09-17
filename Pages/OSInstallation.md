# OS Installation

My clean installation steps for several operating systems.

## macOS

- Change the host name.
- Install brew.
  - `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
- Sign into my GitHub account and [generate/register my SSH key](https://github.com/settings/keys).
  - `ssh-keygen -t rsa -b 4096 -C "keijiro@gmail.com"`
- Install [Lilex Nerd Font](https://www.nerdfonts.com/font-downloads).
- Clone [dotfiles](https://github.com/keijiro/dotfiles) and run `setup`.
- Install [Unity Hub](https://public-cdn.cloud.unity3d.com/hub/prod/UnityHubSetup.exe).

## Windows 11

- Remove unused stock apps like OneDrive.
- Change the search engine settings in Edge. Sign in to my Google account.
- [Enable WSL2 and install Ubuntu](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).
  - `wsl --install` (should run as administrator)
- Open Terminal and copy-paste [my custom profile](https://gist.github.com/keijiro/b7f3ec31f3862abb9bfc7af91059139a).
- `sudo apt update && sudo apt upgrade`
- Install [Lilex Nerd Font](https://www.nerdfonts.com/font-downloads).
- Sign into my GitHub account and [generate/register my SSH key](https://github.com/settings/keys).
  - `ssh-keygen -t rsa -b 4096 -C "keijiro@gmail.com"`
- Clone [dotfiles](https://github.com/keijiro/dotfiles) and run `setup`.
- Install [Unity Hub](https://public-cdn.cloud.unity3d.com/hub/prod/UnityHubSetup.exe).
