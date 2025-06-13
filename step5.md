# 第5章: CI・自動化との統合による発展的運用

---

## 📌 この章の目的

この章では、仮想環境やpip運用の安全性をさらに高めるために、CI（継続的インテグレーション）や自動化ツールと統合する方法について解説します。特に、複数人やチームでの開発・教育現場などで、意図しないライブラリのインストールや環境汚染を防ぐ仕組みを取り入れることを目指します。

---

## 🤖 CIでの仮想環境・パッケージチェック

CIツール（GitHub Actionsなど）を用いることで、コードをPushしたタイミングで次のようなチェックが自動化できます：

* `requirements.txt` に記載されていないライブラリのインストールを検出
* 仮想環境外のpip利用の禁止
* セキュリティスキャン（例：脆弱性のあるライブラリ検出）

### ✅ 例：GitHub Actions のセットアップ

```yaml
# .github/workflows/python-check.yml
name: Python Check

on: [push, pull_request]

jobs:
  lint-and-check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          python -m venv venv
          source venv/bin/activate
          pip install -r requirements.txt

      - name: Check unexpected packages
        run: |
          pip freeze > current.txt
          diff -u requirements.txt current.txt || echo '⚠️ requirements.txt に記載されていないパッケージがあります'
```

---

## 🧹 pre-commit によるローカルチェック

開発者がコミットする前に、仮想環境内でのpip使用や許可されたパッケージのみを確認させるには、`pre-commit` フックの活用が有効です。

### ✅ pre-commitの導入手順

```bash
pip install pre-commit
pre-commit install
```

### `.pre-commit-config.yaml` の例

```yaml
repos:
  - repo: local
    hooks:
      - id: check-venv-pip
        name: Ensure pip used inside virtualenv
        entry: bash scripts/check_venv.sh
        language: system
```

※ `scripts/check_venv.sh` にて `VIRTUAL_ENV` の環境変数が定義されているかを確認し、なければ警告を出すようにします。

---

## 🧠 ログ運用と組織的ポリシー

前章で解説した「pip.bat」の改良版を活用すれば、組織的に次のような管理が可能です：

* 誰がいつ、どんなコマンドで何をインストールしたか記録
* 非許可コマンドの実行ブロック
* ログ保存場所・期間のポリシー設定

こうした仕組みは、教育現場・社内チーム開発で特に有効です。事故やトラブル時のトレーサビリティ確保にも役立ちます。

---

## ✅ この章のまとめ

* CIにより `requirements.txt` の逸脱検出やセキュリティチェックが自動化できる
* `pre-commit` によって、ローカルでのpip誤使用を事前にブロック可能
* `pip.bat` のカスタム運用で、ログ管理とポリシー遵守を徹底できる

次章では、こうした運用全体をまとめて、教育・実務でどのように展開できるかを考察します。

👉 [次の章へ](step6.md)
