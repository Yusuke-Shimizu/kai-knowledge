# kai-knowledge — 案件 KAI の正本

案件の **decision / knowledge / meeting** を Markdown で持つ正本リポジトリ。
Slack 常駐エージェント（[project-agent](https://github.com/Yusuke-Shimizu/project-agent)）が
ここを索引して根拠付きで答え、**書き戻しはこのリポジトリへの PR として来る**。

**private。** 実案件のデータが入るので public にしない。

| | どこ |
|---|---|
| 正本（このリポジトリ） | `knowledge_base/` の3ディレクトリ |
| エージェントの実装・CDK・検証コード | [project-agent](https://github.com/Yusuke-Shimizu/project-agent)（public） |
| front matter の仕様の正典 | project-agent の `knowledge_base/README.md`。**ここでは繰り返さない**（二重管理になる） |
| 設計方針 | `~/Documents/output/2026/08/tech_design_study/` の `architecture_v1.md` / `writeback_pr_tool.md` |

## 置くもの / 置かないもの

```
knowledge_base/
├── decisions/   DEC-*.md   正式な意思決定。1 ファイル = 1 決定
├── knowledge/   KNW-*.md   案件固有の制約・暗黙知
└── meetings/    MTG-*.md   議事録。正式 decision に昇格していない検討メモを含む
```

**置かない**：顧客提供の Excel / PPT / PDF の原本（markdown 化したものだけを入れる）、
Slack の会話ログ、エージェントの出力そのもの、認証情報。

## 書き方

1. `docs/templates/` からコピーする
2. `doc_id` は既存の最大 + 1。**版を置き換えるなら接尾辞**（`DEC-003a` → `DEC-003b`）で、
   旧版には `status: superseded` と `superseded_by` を**同じ PR で**入れる
3. `topic` は `docs/topics.yml` にある値から選ぶ。増やしたいときは `topics.yml` も同じ PR で更新する
4. PR を出す。CI が front matter を検証する
5. **マージが昇格**。`status: active` で出してよく、マージされた時点で正本になる

## エージェントからの起案

label `proposed-by-agent` が付く。**エージェントが書けるのは提案まで。正本を変えるのはマージだけ。**

レビューの観点：

- **`based_on` に挙がっている doc を実際に開いて**、書かれている内容と合っているか
- 既存 doc と重複していないか（重複していれば新規ではなく追記に寄せる）
- decision の内容を knowledge として書いていないか（**decision は人が起こす**）
- front matter の値が妥当か（形式は CI が見るが、日付や `owner` の妥当性は人しか見られない）

**追記 PR は「末尾追記のみ・front matter 不変」。** 既存行の変更や削除が含まれていたら差し戻す
（エージェントにだけ課している縛りなので、破れていたらツール側の不具合）。

## CI

`.github/workflows/validate.yml` が front matter を検証する。検証コードは project-agent 側に
あるものを `actions/checkout` で取ってきて使う（**同じ規則を2か所に置かない**）。

マージ後の S3 同期と Knowledge Base への取り込みは別途配線する（未実装）。
