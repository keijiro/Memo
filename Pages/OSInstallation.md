# OS Installation

各種 OS の初期セットアップの手順について記す。

## Windows 11

- 不要なアプリを削除する
- Edge の検索設定を変更。Google アカウントでサインインする
- [WSL2 を有効化し Ubuntu をインストールする](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide)
  - `wsl --install`
- Terminal アプリを開き[プロファイル](https://gist.github.com/keijiro/b7f3ec31f3862abb9bfc7af91059139a)をコピーする
- `sudo apt update && sudo apt upgrade`
- GitHub にサインインし [SSH キーを生成・登録する](https://github.com/settings/keys)
  - `ssh-keygen -t rsa -b 4096 -C "keijiro@gmail.com"`
- [dotfiles](https://github.com/keijiro/dotfiles) をクローンし `setup` を実行する
- [Unity Hub](https://public-cdn.cloud.unity3d.com/hub/prod/UnityHubSetup.exe) をインストールする
