---
description: ペルソナ準拠の記事をヒアリングから執筆・品質チェック・HTML確認まで一貫して生成するコンテンツワークフロー
---

# ryu コンテンツ生成ワークフロー

## 前提

- スキル: `.claude/skills/writter/SKILL.md`
- ペルソナ: `.claude/skills/writter/character/persona.md`
- トリガー: 「記事を書いて」「記事作成」「ライティング」「ブログを書いて」「コンテンツ作成」「write article」「/write」
- **1回のトリガーで MD記事 + 確認用HTML を生成する**

## Step 1: スキル読み込み

`.claude/skills/writter/SKILL.md` を読み込む。
記事タイプ、利用フロー、ファイル名規則、記事テンプレート（frontmatter含む）を把握する。

## Step 2: ペルソナ読み込み

`.claude/skills/writter/character/persona.md` を読み込む。
以下を把握した上で、全ての執筆に反映する：

- 文章のトーン（ゆるく、やわらかく）
- 言葉選びのルール（専門用語の言い換え）
- 共感ファーストの姿勢
- 記事タイプに応じたライティングテンプレート（AIDMA / PREP / SDS）

## Step 3: テーマ確認（ヒアリング）

SKILL.md の Step 1 に従い、ユーザーにテーマと要件を確認する。

| 確認項目 | 必須 | 備考 |
|---------|------|------|
| テーマ・タイトル | 必須 | |
| 記事タイプ | 必須 | blog / seo / column / howto / news |
| ターゲット読者 | 必須 | |
| 文字数目安 | 任意 | 未指定の場合は記事タイプに応じて判断 |
| トーン | 任意 | 未指定の場合は persona.md の基本スタンスに従う |
| キーワード | 任意 | SEO記事の場合は必ず確認する |
| 参考情報・URL | 任意 | |

## Step 4: 構成案の作成・承認

SKILL.md の Step 2 に従い、アウトラインを作成してユーザーに提示する。
記事タイプに応じて persona.md のテンプレートを適用する：

| 記事タイプ | 使用テンプレート |
|-----------|----------------|
| seo | AIDMA |
| blog / howto | PREP |
| column / news | SDS |

**ユーザー承認を待つ** - 構成案がOKになるまで次に進まない。

## Step 5: 記事執筆

SKILL.md の Step 3 に従い、マークダウン形式で本文を執筆する。

執筆時の必須チェック：
- persona.md の「共感ファースト」を全パートに織り込む
- 専門用語は persona.md の言い換え表に従う
- 読者の不安を先に言葉にする、自分の経験を交える
- 一文は60文字以内を目安にする

## Step 6: 品質チェック

SKILL.md の Step 4 に従い、セルフチェックを実施する。

加えて、persona.md 準拠の追加チェック：

| チェック項目 | 基準 |
|-------------|------|
| 共感要素 | 導入部に読者への共感が含まれていること |
| トーン | ゆるく・やわらかい文体になっていること |
| 専門用語 | かみ砕いた説明が添えられていること |
| 読者目線 | 「仲間」として語りかける姿勢であること |

チェック結果をユーザーに報告する。

## Step 7: MD ファイル保存

SKILL.md のファイル名規則・出力先ディレクトリに従い保存する。

- **保存先:** `output/articles/{type}/YYYY-MM-DD-slug.md`
- **下書き:** 途中で中断する場合は `output/articles/drafts/` に保存

例：
```
output/articles/blog/2026-02-21-ai-agent-introduction.md
output/articles/seo/2026-02-21-gas-automation-guide.md
```

## Step 8: 確認用 HTML 生成・保存

記事の内容を確認用HTMLに変換して保存する。
HTML はあくまで内容の見た目確認用であり、簡易的なものでよい。

- **保存先:** `html/articles/{type}/YYYY-MM-DD-slug.html`

例：
```
html/articles/blog/2026-02-21-ai-agent-introduction.html
html/articles/seo/2026-02-21-gas-automation-guide.html
```

HTML の要件：
- シンプルで読みやすいデザイン
- レスポンシブ対応（スマホでも確認できること）
- 見出し・段落・リスト・表が正しく表示されること
- frontmatter の情報（タイトル、日付、タイプ、タグ等）をヘッダー部分に表示

## Step 9: ブラウザで開く

生成したHTMLをブラウザで表示し、ユーザーが内容を目視確認できるようにする。

## Step 10: ユーザーレビュー

生成したコンテンツの概要をユーザーに提示する：

```
---
生成完了:
- MD: output/articles/{type}/YYYY-MM-DD-slug.md
- HTML: html/articles/{type}/YYYY-MM-DD-slug.html
---
```

フィードバックがあれば修正し、MD・HTML の両方を更新する。

## ディレクトリ構成（全体）

```
project-fils/
├── .agent/
│   └── workflow/
│       └── ryu-content.md            # このワークフロー定義
├── .claude/
│   └── skills/
│       └── writter/
│           ├── SKILL.md              # スキル定義
│           └── character/
│               └── persona.md        # ペルソナ設定
├── output/
│   └── articles/
│       ├── blog/                     # ブログ記事 MD
│       ├── seo/                      # SEO記事 MD
│       ├── column/                   # コラム MD
│       ├── howto/                    # ハウツー MD
│       ├── news/                     # ニュース MD
│       └── drafts/                   # 下書き MD
└── html/
    └── articles/
        ├── blog/                     # ブログ確認用 HTML
        ├── seo/                      # SEO確認用 HTML
        ├── column/                   # コラム確認用 HTML
        ├── howto/                    # ハウツー確認用 HTML
        └── news/                     # ニュース確認用 HTML
```
