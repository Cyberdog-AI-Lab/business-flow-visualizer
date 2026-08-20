# business-flow-visualizer

業務フローを **テキスト（`.flow`）で書いて、役割×時系列のスイムレーンHTML図に自動変換**するための記法（**flow-notation**）とスキル。
図ツールでポチポチ描かず、**差分管理できるテキスト**でフローを保つことを狙う。Claude Code / Codex CLI 両対応。

```
「業務ヒアリング」 --(flow-interview)-->  business.flow  --(flow-visualize)-->  business.html
```

## できること

- **記法（DSL）** … 役割（行）× 時系列（列）のスイムレーン前提の業務フロー記述。業務行為・成果物・操作内容・手段・条件/例外分岐・課題・待ち・手戻り・並行(fork/join)・SLA・AS-IS/TO-BE などを表現。仕様は **[docs/SPEC.md](docs/SPEC.md)**。
- **`flow-visualize` スキル** … `.flow` → 自己完結HTML図（インラインSVG）。
- **`flow-interview` スキル** … 業務ヒアリング（対話）→ `.flow` を生成。

## クイックスタート

1. `examples/monthly-invoice.flow` を見て記法をつかむ。
2. エージェントに「この `.flow` を図にして」と頼む（`flow-visualize` が起動）。
3. ゼロから作るなら「この業務をヒアリングして `.flow` にして」（`flow-interview` が起動）。

サンプル：

- [examples/monthly-invoice.flow](examples/monthly-invoice.flow) — 月次請求フロー（AS-IS）
- [examples/loan-review.flow](examples/loan-review.flow) — 住宅ローン審査（v0.2 の並行/SLA/外部連携を一通り）

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
