# business-flow-visualizer

業務フローを **テキスト（`.flow`）で書いて、役割×時系列のスイムレーンHTML図に自動変換**するための記法（**flow-notation**）とスキル。
図ツールでポチポチ描かず、**差分管理できるテキスト**でフローを保つことを狙う。Claude Code / Codex CLI 両対応。

```
「業務ヒアリング」 --(flow-interview)-->  business.flow  --(flow-visualize)-->  business.html
```

**デモサイト：** [`index.html`](index.html) — 実例ごとに「①ヒアリング → ②`.flow` → ③業務フロー図」を
切り替えて確認できます（GitHub Pages で公開する場合はリポジトリのルートを配信対象にしてください）。

## できること

- **記法（DSL）** … 役割（行）× 時系列（列）のスイムレーン前提の業務フロー記述。業務行為・成果物・操作内容・手段・条件/例外分岐・課題・待ち・手戻り・並行と合流・SLA・AS-IS/TO-BE などを表現。仕様は **[docs/SPEC.md](docs/SPEC.md)**。
- **用語補足** … 専門用語を `glossary:` に定義し、本文で `[[用語]]` と書くと、図の上でマウスオーバー説明が出る。業務を知らない人がその場で意味を確認できる。
- **`flow-interview` スキル** … 業務ヒアリング（対話）→ `.flow` を生成。**聞いていないことは書かない**（所要時間や課題を推測で埋めない）。
- **`flow-visualize` スキル** … `.flow` → 自己完結HTML図（インラインSVG）。
- **`flow-verify` スキル** … ヒアリング記録・`.flow`・HTML の食い違いを逆流チェックする。AI が業務の詳細を勝手に補っていないかを検出する。

## クイックスタート

1. [`examples/01-auto-parts-factory/`](examples/01-auto-parts-factory/) を見て記法をつかむ。
2. エージェントに「この `.flow` を図にして」と頼む（`flow-visualize` が起動）。
3. ゼロから作るなら「この業務をヒアリングして `.flow` にして」（`flow-interview` が起動）。
4. 仕上げに「逆流チェックして」（`flow-verify` が起動）。

サンプル：

1シナリオ = 1フォルダ。各フォルダに「シナリオ概要」「（あれば）ヒアリング時の会話」「`.flow`」「`.html`」が入っている。

| シナリオ | 内容 | 出典 |
|---------|------|------|
| [01-auto-parts-factory](examples/01-auto-parts-factory/) | 自動車部品工場の受注→出荷。8役割×7列の実寸大 | ヒアリング |

一覧は [examples/README.md](examples/README.md)。

## インストール（両対応）

スキル本体は `.claude/skills/` にあり、Codex 用は `.codex/skills/` から symlink で共用しています。

- **Claude Code** … `.claude/skills/` を自動で読みます。このリポジトリを配置すれば `flow-visualize` / `flow-interview` が使えます。
- **Codex CLI** … `.codex/skills/` を読みます（同じ実体を指す symlink）。

> symlink が効かない環境（Windows 等）では、手動コピーしてください：
> ```bash
> cp -r .claude/skills/flow-visualize .codex/skills/flow-visualize
> cp -r .claude/skills/flow-interview .codex/skills/flow-interview
> ```
> ※ Codex の実スキルディレクトリは版により `.codex/skills/` / `.agents/skills/` の差異があります。実機で確認したパスに合わせてください。

## 記法のさわり

```yaml
roles:      [営業部, 経理部, 部長, 顧客#社外]
timeline:   [月初, 月末, +1営業日, +2営業日, +4営業日, +5営業日]
milestones: { 請求確定: +3営業日 }

steps:
  - start: 月次締め
    at: [営業部, 月初]
  - id: a1
    at: [営業部, 月末]
    act: 案件実績を集計する
    by: 人手/Excel手入力
    artifact: 実績Excel
    op: 行追加
    issue: 二重入力でミス
```

`at: [役割, 時点]` が図のセル位置を決め、矢印は `from:` から自動で引かれます。詳細は [docs/SPEC.md](docs/SPEC.md)。

## ライセンス

Apache License 2.0 — [LICENSE](./LICENSE) を参照。帰属表示は [NOTICE](./NOTICE) を参照。
Copyright 2026 Cyberdog AI Lab（株式会社サイバードッグ）。

本プロジェクトは "AS IS"（現状有姿）で提供され、いかなる種類の保証もありません。
