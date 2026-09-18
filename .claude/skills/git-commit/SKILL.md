---
name: git-commit
description: ptcg-abc で変更をコミットするときの手順。main にいれば作業ブランチを切り、Conventional Commits 形式のメッセージでコミットする。コミットやブランチの作成を頼まれたとき、作業のまとまりをコミットするときに使う。
---

# git-commit

## 1. ブランチを確認する

- `main` に直接コミットしない。
- `git branch --show-current` が `main` なら、`main` を最新にしてから作業ブランチを切る。

```bash
git checkout main
git pull
git checkout -b <type>/<短い説明>
```

- ブランチ名の `<type>` は、下の Conventional Commits の type と同じものを使う（例: `feat/fetch-competition-data`、`docs/agents-md`）。
- `<短い説明>` は英小文字と `-` で書く。
- 関係するイシューがあれば、そのイシューの作業だけをそのブランチで行う。

## 2. ステージする内容を確認する

- `git status` と `git diff` で、コミットに含める変更を確認する。
- `git diff --cached --name-status` で、ステージ済みの内容を確認する。`git rm` や `git mv` で先にステージした変更が、意図しないコミットに混ざることがある。
- 関係のない変更を同じコミットに入れない。
- 他人のコミットしていない作業を上書きしない。
- 認証情報（`.env`、`~/.kaggle/` のトークンなど）をステージしない。

## 3. Conventional Commits 形式でメッセージを書く

```
<type>[(<scope>)][!]: <説明>

<本文>

<フッター>
```

- `<type>` は次のいずれか。

| type | 使いどころ |
|---|---|
| `feat` | 機能の追加 |
| `fix` | 不具合の修正 |
| `docs` | ドキュメントだけの変更 |
| `refactor` | 振る舞いを変えないコードの整理 |
| `test` | テストの追加・修正 |
| `perf` | 性能の改善 |
| `build` | ビルドや依存関係（`pyproject.toml`、`uv.lock`）の変更 |
| `ci` | CI の設定の変更 |
| `chore` | 上のどれにも当てはまらない雑務（ファイルの移動、`.gitignore` など） |

- `<scope>` は任意で、変更の範囲を英小文字で書く（例: `festival_lead`、`cg`、`setup`）。
- 互換性を壊す変更は、type の後ろに `!` を付け、フッターに `BREAKING CHANGE: <内容>` を書く。
- `<説明>` は日本語で、何をしたかを簡潔に書く。末尾に句点を付けない。
- `<本文>` は任意で、変更の理由や背景を日本語で1文1行で書く。
- `<フッター>` には、関係するイシューを `Refs #<番号>` で書く。
  - コミットには `Closes #<番号>` を書かない。main に入った時点でイシューがクローズされるため、クローズは PR 本文で指定する（`create-pr` を参照）。

例:

```
feat(fetch): コンペデータから cg・エンジンのソース・カードデータを取得するスクリプトを追加

一次情報を無加工で保存し、パス中の半角スペースだけを _ に置き換える。

Refs #<番号>
```

## 4. コミットする

- メッセージは HEREDOC で渡す。

```bash
git commit -F - <<'EOF'
<メッセージ>
EOF
```

- コミット後に `git log --oneline -3` と `git status --short` で結果を確認する。
- push はユーザーの指示があったときだけ行う。
