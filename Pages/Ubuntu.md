# Ubuntu Linux System

## Windows/Linux マルチブート

Windows とのマルチブートを実現するには物理的にドライブを２つ用意するのが無難。単一ドライブでのマルチブートは OS の再インストール時にトラブルを生じやすい。十分に気をつければ対処も可能だが、それよりもドライブ増設の手間を受け入れた方が手っ取り早い。

## インストール手順

- Ubuntu を USB メモリを使ってインストールする。インストール設定は minimal installation を使う。
- "Software & Updates" - "Additional Drivers" から NVIDIA ドライバをインストールする。
  - Secure Boot の設定が必要とされる場合は適切に行うこと（ドライバが正しくインストールされない）。
- [Unity Hub Debian repository](https://docs.unity3d.com/hub/manual/InstallHub.html) を使って Unity Hub をインストール。
- Git, Vim, Vulkan をインストール。
  - `sudo apt install git vim vulkan-tools`
- GitHub にサインインし [SSH key を登録する](https://github.com/settings/keys)。
  - `ssh-keygen -t rsa -b 4096 -C "keijiro@gmail.com"`
- [dotfiles](https://github.com/keijiro/dotfiles) リポジトリを clone し `setup` スクリプトを実行する。

## Unity 適合バージョン

2022 年 5 月時点では Ubuntu 22.04 LTS への対応は行われていない。 Unity Hub と Unity Editor の両方で動作に問題が発生する。 Ubuntu 21.04 LTS を使用するのが無難。
