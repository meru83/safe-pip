# 🔪 Pythonパッケージ運用の安全設計ガイド

## 📖 概要

この技術書は、**Pythonの仮想環境とpipのパッケージ管理を安全に、再現性を保って運用するためのノウハウ**を体系的にまとめたものです。

## 🛠 特徴と目的

* pipの安全な使用方法 (`python -m pip` の推奨)
* 仮想環境を前提とした開発の5ステップ
* pip.bat のカスタマイズとログ機能の追加
* ホワイトリストによる許可コマンドの管理
* VSCodeやPowerShellの環境上での操作の最適化
* CIやpre-commitフックへの統合アイデアまでケア

## 📚 章構成

| 章   | タイトル                     | ファイル名                |
| --- | ------------------------ | -------------------- |
| 第1章 | 仮想環境の構築と基本操作             | [step1.md](step1.md) |
| 第2章 | Pythonのグローバル環境をクリーンアップする | [step2.md](step2.md) |
| 第3章 | カスタムpip.batの作成と設置        | [step3.md](step3.md) |
| 第4章 | 安全なpip運用のための拡張           | [step4.md](step4.md) |
| 第5章 | CI・自動化との統合               | [step5.md](step5.md) |

> 各章は単体でも読めるようにしつつ、通読すると系統的に理解が深まる構成になっています。

## ⚡ 使い方 (概要)

1. 本リポジトリをクローン
2. `step3.md`の手順に従って`pip.bat`をSystem32へ設置
3. 仮想環境でのみpipを実行
4. `C:\pip_logs\pip_allow.txt`を編集して許可コマンドを管理
5. `step5.md`を参照してCI/フックへの統合を試してもOK

## 📂 ディレクトリ構成例

```
project/
├── step1.md
├── step2.md
├── step3.md
├── step4.md
├── step5.md
├── pip_allow.txt            # pip許可リスト
└── pip.bat                  # カスタム pip 実行ファイル
```

## 📁 想定読者

* Python開発経験のある中級〜上級者
* TA/教員/研修担当者
* pip や venv の運用ミスを防ぐ手段を探している開発者

## 🙌 貢献・フィードバック

バグ報告や改善提案はIssuesまたはPull Requestで歓迎します！
