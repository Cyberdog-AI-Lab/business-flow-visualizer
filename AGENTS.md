# business-flow-visualizer

業務フローをテキスト（`.flow`）で記述し、**役割×時系列のスイムレーンHTML図**へ変換する記法（**flow-notation**）とスキル。

## これは何か

- **記法（DSL）本体の仕様 → [docs/SPEC.md](docs/SPEC.md)**（唯一の正）
- **スキル2種：**
  - `flow-interview` … 業務ヒアリング（対話）→ `.flow` テキストを生成する
  - `flow-visualize` … `.flow` テキスト → 自己完結HTML図（インラインSVG）を生成する
  - `flow-verify` … 3つの成果物（ヒアリング記録・`.flow`・HTML）の食い違いを逆流チェックする

## エージェントへの指示

- 記法の語彙・書式・配置ルールは **必ず `docs/SPEC.md` を参照**する（推測で書式を足さない）。
- 図を描くとき（`.flow` → HTML）は `flow-visualize` スキルの手順に従う。
- 業務を聞き取って記法に落とすときは `flow-interview` スキルの手順に従う。
- **聞いていないことを書かない。** 所要時間・課題・改善案・システム名を推測で埋めない（`flow-interview` の鉄則）。
- `flow-interview` / `flow-visualize` を実行したら、**必ず `flow-verify` を通す**。
- サンプルは `examples/` に**シナリオ単位のフォルダ**で置いている（`examples/README.md` に一覧）。

## 設計原則（両対応）

- **記載は1つ、置き場所を橋渡しする。** Claude Code は `.claude/skills/` を、Codex CLI は `.codex/skills/`（symlink）を読む。
- スキル本文に「Claudeなら〜／Codexなら〜」の分岐を書かない。ツール固有メタは `agents/openai.yaml` 等の sidecar に隔離する。
- この `AGENTS.md` は簡潔に保つ（Codex は 32KiB で打ち切る）。長い手順は SPEC / スキルへ。

## リポジトリ構成

```
business-flow-visualizer/
├── AGENTS.md                  指示の正本（このファイル）
├── CLAUDE.md                  @AGENTS.md 取り込み
├── docs/SPEC.md               記法仕様（v0.2）★中核
├── examples/{番号}-{名前}/     シナリオ集（概要・ヒアリング記録・.flow・.html）
├── .claude/skills/
│   ├── flow-interview/SKILL.md
│   ├── flow-visualize/SKILL.md
│   └── flow-verify/SKILL.md
├── .codex/skills/             → ../../.claude/skills/* への symlink
├── LICENSE / NOTICE           Apache-2.0（公開時に配置）
└── README.md                  人間向け・両対応インストール手順
```

## ライセンス

Apache License 2.0（1リポジトリ1ライセンス）。`LICENSE`＋`NOTICE` を参照。
Copyright 2026 Cyberdog AI Lab（株式会社サイバードッグ）。
