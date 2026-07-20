---
publish: true
---
2026-07-09

ChromeOS Flex の Linux 開発環境（Crostini）をダークモードにするには、「Linux 開発環境における GTK のテーマ設定」がある。

## ウィンドウの枠（タイトルバー）も黒くするダークモード

システム全体（Linux 開発環境）でダークモードに同期させるには、Linux のターミナルで以下のコマンドを実行する。

**1.ターミナルを起動する:** ChromeOS アプリ。

ランチャーから **「ターミナル (Terminal)」** アプリ（ペンギンのアイコン）を起動し、`penguin`（標準のコンテナ）を選択する。

**2.Linux のテーマをダークに設定する:** コマンドの実行。

Linux アプリ全体に「ダークモードを優先する」という命令を出すため、以下のコマンドをコピペ実行する。

```
gsettings set org.gnome.desktop.interface color-scheme 'prefer-dark'
```

_※ 元に戻すには以下のコマンドを実行する。_
```
gsettings reset org.gnome.desktop.interface color-scheme
```

**3.再起動する:** 設定の反映確認。

再度起動する。これでウィンドウの枠（タイトルバー）もダークモードに変化する。
```
sudo reboot
```
