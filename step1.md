# 第1章: Python仮想環境とpipの基礎知識

---

## 📌 章の目的

この章では、Pythonの仮想環境（venv）とpipの基本的な使い方について学びます。ライブラリの管理におけるグローバル環境の問題点と、仮想環境の導入によって得られるメリットを理解し、安全な開発環境の第一歩を踏み出すことが目標です。

---

## 🔍 背景・問題提起

Pythonでは `pip` を使ってライブラリをインストールしますが、グローバル環境に対して直接インストールすると以下のような問題が生じます：

* 複数プロジェクトでライブラリのバージョンが衝突する
* 他人の環境で再現できない
* 誤って重要なシステムライブラリを上書きしてしまう可能性がある

これらを防ぐためには、プロジェクトごとに独立した**仮想環境**を用意することが重要です。

---

## 🛠 実践手順・コード例

### ● pipとは？

`pip` はPythonの公式パッケージ管理ツールです。以下のような操作が可能です：

* `pip install requests`：ライブラリのインストール
* `pip uninstall requests`：ライブラリの削除
* `pip list`：インストール済み一覧表示
* `pip show ライブラリ名`：ライブラリの詳細表示
* `pip freeze > requirements.txt`：依存関係の固定化

### ● 仮想環境とは？

仮想環境とは、**Pythonの実行環境をプロジェクトごとに独立して分離できる仕組み**です。

* 他のプロジェクトとライブラリのバージョンが干渉しない
* プロジェクト固有の依存関係を安全に保持できる
* グローバル環境を汚さずにライブラリのインストール・アンインストールができる

### ● 仮想環境の作成と有効化・無効化

```bash
# プロジェクトディレクトリ作成
mkdir my_project
cd my_project

# 仮想環境の作成（Windows）
python -m venv .venv

# 仮想環境の有効化（コマンドプロンプト）
.venv\Scripts\activate.bat

# 仮想環境の有効化（PowerShell）
.venv\Scripts\Activate.ps1
```

仮想環境が有効化されると、コマンドプロンプトの先頭に `(venv)` のような表示が付きます。

```bash
# 仮想環境の無効化（deactivate）
deactivate
```

無効化することで、仮想環境外の（グローバルな）Python実行環境に戻ることができます。

---

## 📎 補足・Tips

### ● なぜ python -m pip を使うのか？

`pip install` と `python -m pip install` は同じ結果になるように見えますが、実際には以下のような違いがあります：

| コマンド                             | 説明                                          |
| -------------------------------- | ------------------------------------------- |
| `pip install requests`           | 実行されるpipがシステム依存になるため、別のPythonに対して動作する可能性がある |
| `python -m pip install requests` | 現在使っている `python` に対応した `pip` が確実に使用される      |

特に複数のPythonがインストールされている場合や、仮想環境を使っている場合に `python -m pip` を使うことで意図した環境にだけ影響を与えることができます。

また、スクリプトやCI環境での一貫性を保つためにも、この形式が推奨されます。

---

## 🖥️ VSCode・PowerShell 操作補足

### 🔧 PowerShellでの仮想環境操作

初回実行時にPowerShellでスクリプトがブロックされることがあります。その場合は一時的に以下の設定で回避できます：

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

### 💻 VSCodeで仮想環境を使う

#### 1. インタプリタの選択

1. `Ctrl + Shift + P` でコマンドパレットを開く
2. `Python: Select Interpreter` を選択
3. `.venv\Scripts\python.exe` を選択

#### 2. `.vscode/settings.json` の設定例

```json
{
  "python.defaultInterpreterPath": ".venv\\Scripts\\python.exe"
}
```

この設定により、新しいターミナルでも仮想環境がデフォルトで使われます。

#### 3. 文字化け対策（PowerShell / VSCode）

* コマンドプロンプトで文字化けが起きる場合：

```cmd
chcp 65001
```

* VSCode のターミナルで UTF-8 をデフォルトにするには、`settings.json` に以下を追加：

```json
"terminal.integrated.defaultEncoding": "utf8"
```

---

## ✅ この章のまとめ

* `pip` はPythonのパッケージ管理を担う基本ツール
* 仮想環境 `venv` によりプロジェクトごとに独立した環境を構築できる
* 仮想環境は `activate` で有効化し、`deactivate` で解除する
* `python -m pip` を使用することで確実に現在のPython環境に対してpip操作を行える
* VSCodeやPowerShellでも確実に仮想環境を扱う設定が可能
* 実際の運用でありがちなミスも予防できる

👉 [次の章へ](step2.md)
