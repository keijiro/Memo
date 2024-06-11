# Unity on Android

相変わらずまともな環境構築をせずに Unity Hub からのインストールだけでなんとか済まそうとしている。

### 簡易コンソールフィルタ

```
/Applications/Unity/Hub/Editor/xxxx.x.xxf1/PlaybackEngines/AndroidPlayer/SDK/platform-tools/adb logcat | grep --line-buffered "Unity   :"
```

Console ウィンドウをリモート接続する手もあるが、ネットワークパーミッションの取得状態など変化してしまうため、このような簡易的な方法が望ましい場合もある。
