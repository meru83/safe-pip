# 第3章: pip誤使用防止のための安全対策

---

## 📌 章の目的

この章では、誤ってグローバル環境に `pip install` を実行してしまうことを防ぐための仕組みを構築します。Pythonの仮想環境を運用していても、うっかりグローバルにパッケージをインストールしてしまうミスはよくあるトラブルです。

この章では、

* `pip` コマンドの誤使用を防ぐ
* ログを記録する
* 仮想環境外での `pip` 使用を制限する

という目的を、バッチスクリプト（`pip.bat`）を活用して実現します。

---

## 🔍 背景・問題提起

* 仮想環境を有効にせず `pip install` を実行すると、意図せずグローバルにインストールされてしまう。
* 開発者の習慣や環境によっては、この誤操作に気づきにくい。
* 環境が汚染されると、後から再現や管理が困難になる。

**→ これを防ぐには、グローバルの `pip` 実行自体に制限をかける必要があります。**

---

## 🛠 対策手順：pip.bat の設置

### ● スクリプトの目的

この章で作成する `pip.bat` スクリプトは、以下の制御を行います：

* 仮想環境外で `pip` が実行されると警告を出す
* 危険な操作には確認を求める
* 安全なコマンド（例：`pip list`, `pip show` など）だけは実行可能
* 実行ログをファイルに保存

### ● 配置構成と理由

| ファイル            | パス                                        |
| --------------- | ----------------------------------------- |
| `pip.bat`       | `C:\Windows\System32\pip.bat`             |
| `pip_allow.txt` | `C:\pip_logs\pip_allow.txt`               |
| ログ保存先           | `C:\pip_logs\logs\pip_log_YYYY-MM-DD.txt` |

> `pip_allow.txt` を `System32` から分離している理由：
>
> * 管理者権限が必要な `System32` は編集が困難
> * コマンドの許可リストを頻繁に変更できるよう、編集しやすい場所に分離しておくのが望ましい

---

## ✍ pip.bat の中身

```bat
@echo off
setlocal EnableDelayedExpansion

:: ==============================
:: 設定（ファイルパスなど）
:: ==============================

set "ALLOW_FILE=C:\pip_logs\pip_allow.txt"
set "LOGDIR=C:\pip_logs\logs"

:: 日付（YYYY-MM-DD）取得
for /f %%a in ('powershell -Command "Get-Date -Format yyyy-MM-dd"') do set "TODAY=%%a"
set "LOG_FILE=%LOGDIR%\pip_!TODAY!.log"

:: ログディレクトリがなければ作成
if not exist "%LOGDIR%" mkdir "%LOGDIR%"

:: コマンド全体をログに出力
echo [%date% %time%] pip %* >> "%LOG_FILE%"

:: 仮想環境かどうか判定
if not defined VIRTUAL_ENV (
    echo.
    echo 仮想環境が有効ではありません。
    echo.
)

:: 安全なコマンドだけを許可
set "CMDLINE=%*"
for /f "usebackq delims=" %%A in ("%ALLOW_FILE%") do (
    if /i "!CMDLINE!" == "%%A" goto :SAFE
)

:: 許可されていない場合は確認
set /p CONT=この操作はグローバル環境で実行される可能性があります。続行しますか？ [Y/N]：
if /i "!CONT!" NEQ "Y" exit /b

:SAFE
python -m pip %*
```

---

## 📄 pip\_allow\.txt の内容（例）

ファイルパス： `C:\pip_logs\pip_allow.txt`

```text
list
freeze
show
help
--version
check
```

> 上記に含まれる `pip` サブコマンドは無条件で実行を許可されます。

---

## 🧠 補足：文字コードに関する注意点

### 日本語が文字化けする問題について

`pip.bat` は Shift\_JIS で保存されていないと、コマンドプロンプト上で日本語部分が文字化けする可能性があります。

### 対応方法

#### 1. メモ帳で保存する場合

* 「名前を付けて保存」時にエンコーディングを「ANSI（Shift\_JIS）」にする

#### 2. Visual Studio Code の場合

* 右下のエンコーディング表示（例：UTF-8）をクリック
* 「エンコーディング付きで保存」→「Shift\_JIS」を選択

この設定を忘れると、スクリプト内の日本語が「縺・∪縺帙ｓ縲」などと文字化けしてしまいます。

---

## ✅ この章のまとめ

* 仮想環境を使っていても、`pip` の誤使用で環境が汚染されるリスクがある
* 自作の `pip.bat` を用いることで、誤操作を未然に防ぐ安全な仕組みを作れる
* 許可されたコマンドのみ即時実行し、ログに記録
* 学習環境・チーム環境でも有効な保守手段となる
* `pip_allow.txt` を外部ファイルとして分離することで、柔軟な管理が可能
* 文字コードは Shift\_JIS に設定し、日本語が正しく表示されるように注意

👉 [次の章へ](step4.md)
