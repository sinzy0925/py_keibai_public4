# py_keibai_public4

公開リポジトリ（**CI 起動専用**）。Python コード・transcript JSON は載せません。

| 項目 | リポジトリ |
|------|------------|
| このリポ（public） | `sinzy0925/py_keibai_public4` … workflow のみ |
| 本体（private） | `sinzy0925/py_keibai` … コード・`data/**/transcripts/` |

## 動き

1. ここ（public）の Actions で workflow を実行（public の無料枠）
2. `PRIVATE_REPO_PAT` で private を checkout（`app/`）
3. 文字起こし後、**private のみ**に commit / push
4. ローカルは `py_keibai` で `git pull`

public 側には JSON を commit しません。

## 初回セットアップ

### 1. GitHub に public リポを作成

- 名前: `py_keibai_public4`
- Visibility: **Public**
- 中身: このフォルダ（`py_keibai_public4/`）の内容をルートに push

```powershell
cd c:\Users\sinzy\py_keibai\py_keibai_public4
git init
git add .
git commit -m "Initial: transcribe workflows for private py_keibai"
git branch -M main
git remote add origin https://github.com/sinzy0925/py_keibai_public4.git
git push -u origin main
```

### 2. private リポを非公開にする

`sinzy0925/py_keibai` → Settings → Danger zone → **Change visibility** → Private

### 3. Secrets（`py_keibai_public4` の Settings → Actions）

| Name | 内容 |
|------|------|
| `PRIVATE_REPO_PAT` | private `py_keibai` へ **Contents: Read and write** の PAT |
| `_env` | ローカル `.env` 全文（[docs/github-actions-secrets.md](../docs/github-actions-secrets.md) と同じ） |
| `client_secret_yoshinagataroai` | OAuth クライアント JSON 全文 |
| `google_drive_token` | `secrets/google_drive_token.json` 全文 |

### 4. 動作確認

1. **Actions** → **BIT pdf transcribe gemini** → Run workflow
2. 成功後、**private** の `data/blocks/06_kinki/transcripts/` にコミットがあること
3. ローカル `py_keibai` で `git pull`

## ワークフロー

| ファイル | Actions 名 |
|----------|------------|
| `pdf_transcribe_gemini.yml` | BIT pdf transcribe gemini |

全ブロック（`01_hokkaido` 〜 `09_kyushu`）を 1 本の workflow で選択実行します。

詳細: [docs/github-actions-secrets.md](../docs/github-actions-secrets.md)
