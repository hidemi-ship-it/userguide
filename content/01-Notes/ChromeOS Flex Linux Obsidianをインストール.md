---
publish: true
---
2026-07-08

ChromeOS Flex Linux 開発環境に、Obsidian をインストールして使えるようにする手順を説明する。

まず先に、[[ChromeOS Flex Linux 日本語環境]] と [[ChromeOS Flex Linux gnome-text-editorをインストール]] を済ませておく。

## Obsidian のインストールプログラムを入手する

![[ChromeOS Flex Linux Obsidianを入手.webm]]

1. Obsidianの [公式サイト](https://obsidian.md/) に、アクセスする。

2. **Get Obsidian for Linux（AppImage）** の右にある **More platforms** をクリックする。

3. Linux のインストールプログラムは複数の形式がある。ここから **Deb** をクリックしてダウンロードする。

4. ダウンロードした**インストールプログラム**は、マイファイルの「ダウンロード」にある。このインストールプログラム (obsidian_1.12.7_...) の上で右クリックをして、コピーを選択する。

5. マイファイルの 「Linux ファイル」を開き、ここに、貼り付ける。すると、インストールプログラムが複製されて「Linux ファイル」に入る。「Linux ファイル」の中にあるものは、ターミナルからアクセスできる。

## Obsidian をインストールする

![[ChromeOS Flex Linux Obsidianをインストール.webm]]

1. Linux 開発環境を起動し、ターミナルにてコマンドを入力して実行する。ダウンロードしたファイル名が違うため、コピペ実行は禁止。「sudo apt install ./obsidian」まで入力したら、「Tab」(タブキー)を押すと、正しいファイル名に補完される。「.deb」まで入力できたら、「Enter」(エンターキー)を押すと実行される。

```
sudo apt install ./obsidian_1.12.7_amd64.deb
```

2. 「Continue? \[Y/n]」が表示され止まったら、再度、「Enter」(エンターキー)を押すと続行される。

## Obsidian の設定ファイルを変更する

![[ChromeOS Flex Linux Obsidianの設定ファイル-2.webm]]

Linux 開発環境の Wayland 対応が不完全なため、このままでは Obsidian が動かない。X11(えっくすいれぶん)モードで実行すると動作するので、起動にかかわる設定ファイルを gnome-text-editor（テキストエディター） で変更する。

順番に、ターミナルで、コピペ実行する。
1. 
```
mkdir ~/.local/share/applications/
```
2. 
```
cp /usr/share/applications/obsidian.desktop ~/.local/share/applications/
```
3. 
```
gnome-text-editor ~/.local/share/applications/obsidian.desktop
```

ここで、gnome-text-editor（テキストエディター）が開くので、下記を参考に変更し保存する。

変更前：
```
Exec=/opt/Obsidian/obsidian %U
```

変更後：
```
Exec=/opt/Obsidian/obsidian --ozone-platform=x11 %U
```

## Obsidian を起動する

![[ChromeOS Flex Linux Obsidianを起動.webm]]

1. 画面下部の左側にある「G」をクリックする
2. 表示されたポップアップの上側に、「Linux」があるので、これをクリックする。
3. 開いたLinuxのところに「Obsidian」があるので、これをクリックすると、Obsidian が起動する。
4. 動画では、「ホーム」の中に「Obsidian」フォルダを作成し、その「Obsidian」フォルダの中に、最初の保管庫である「MyVault」を作成している。
