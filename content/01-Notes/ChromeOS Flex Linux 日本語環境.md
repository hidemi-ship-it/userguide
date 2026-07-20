---
publish: true
---
2026-07-08

ChromeOS Flex の Linux 開発環境（Crostini）で日本語入力（IME）を使うためのセットアップ手順である。

ChromeOS 本体が日本語設定になっていても、Linux 開発環境内はデフォルトで英語環境になっているため、コマンドを使って日本語の設定を行う。

- **確認したバージョン:** ChromeOS バージョン 149.0.7827.226（公式ビルド） （64 ビット）

まず先に、[[ChromeOS Flex Linux 開発環境のインストール]] を済ませておく。

- コマンドのコピペ実行についてはこれを読め。[[ChromeOS Flex Linux コピペ実行]]

## Linux 開発環境に日本語環境をインストールする

![[ChromeOS Flex Linux 日本語環境を設定.webm]]

### ステップ 1: パッケージの更新

ターミナル（Linux）を起動し、システムを最新の状態に更新する。

```
sudo apt update && sudo apt upgrade -y
```

### ステップ 2: 日本語ロケールの追加

Linux システムに日本語環境を追加する。

1. **設定画面を開く**
    
    ```
    sudo dpkg-reconfigure locales
    ```
    
1. **`ja_JP.UTF-8` を探す** 矢印キーで下へスクロールし、`ja_JP.UTF-8 UTF-8` を見つけたら **スペースキー** でチェックを入れる（`[*] ja_JP.UTF-8 UTF-8` になる）。その後、`Tab` キーで `OK` を選択し、Enterキーを押す。
    
2. **デフォルトロケールの選択** 次の画面でシステムのデフォルトロケールを聞かれるので、`ja_JP.UTF-8` を選択し、Enterキーを押す。
    

### ステップ 3: 日本語フォントのインストール

文字化け（豆腐現象）を防ぐために、日本語フォントをインストールする。

```
sudo apt install -y fonts-noto-cjk fonts-noto-cjk-extra
```

### ステップ 4: 日本語入力（Fcitx5-Mozc）のインストール

以下のコマンドを実行して Fcitx5 一式をインストールする。

```
sudo apt install -y fcitx5 fcitx5-mozc zenity
```

_※ `zenity` はFcitx5の設定画面を正常に表示させるために必要なツールだ。_

### ステップ 5: 環境変数の設定

Linux アプリが Fcitx5 を認識できるように設定を書き換える。ターミナルにて、以下、3つのコマンドを１つづつ、順番に、コピペ実行する。
1. 
```
mkdir .config/environment.d/
```
2. 
```
cat <<'EOF' >> .config/environment.d/fcitx5.conf
XMODIFIERS=@im=fcitx
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
DEFAULT_IM_MODULE=fcitx
QT_QPA_PLATFORM=xcb
GDK_BACKEND=x11
EOF
```
3. 
```
cat << 'EOF' >> ~/.sommelierrc
export DISABLE_CROS_IM=1
fcitx5 -d > /dev/null 2>&1
EOF
```

_※ 環境変数の指定は `fcitx5` ではなく `fcitx` のままで正常に動作する。_
_※ QT_QPA_PLATFORM と GDK_BACKEND は x11動作するために必要である。_
_※ DISABLE_CROS_IM は cros im(ChromeOS 標準の IM) が fcitx5 と干渉するので必要である。_

### ステップ 6: Linux 開発環境の再起動

設定を反映させるため、一度 Linux 開発環境を再起動する。以下のコマンドをターミナルでコピペ実行する。

```
sudo reboot
```

### ステップ 7: 初期設定（Mozcの追加）

再起動後、Fcitx5 の設定画面を開いて「キーボードの配列」と「Mozc（日本語入力）」を紐付ける。

1. 以下のコマンドをターミナルでコピペ実行する。
    ```
    fcitx5-configtool
    ```
    
2. 設定画面が開いたら、右側の「Available Input Method（利用可能な入力メソッド）」から **Mozc** を探す。
    
3. **Mozc** を選択し、真ん中の **`<-`（追加ボタン）** を押して左側のリスト（Current Input Method）へ移動させる。
    
4. 右下の **Apply（適用）** または **OK** を押して閉じる。
    

## 切り替え方法

Linux アプリを開き、以下のショートカットで日本語入力を切り替える。

- **`半角/全角`**
- **`Ctrl` + `Space`**
    
Linux アプリの mousepad で確認できる。
[[ChromeOS Flex Linux mousepadをインストール]]

## 最後に

2026-06 現在、ChromeOS の Linux 開発環境における Wayland 対応が**未完成**のため、うまく動作しない。そこで、アプリケーションの方で x11 対応になるように動作させる必要がある。x11 対応は環境変数のところで一部対応しているので、mousepad など、このままでうまく行く。Electron ライブラリを使用しているアプリケーション（Obsidian や VSCode など ）は、`--ozone-platform=x11` を引数に指定するとうまく動く。

ChromeOS の Linux 開発環境において、 Wayland 対応が**完成**したあとには、この日本語環境のやり方が随分と変わるだろう。もっと簡単になるはずだ。
