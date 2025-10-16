---
tags:
  - Trellis
aliases:
created: 2025-10-15T09:57:30Z
updated: 2025-10-15T09:59:17Z
Mind: 誰でも見れるようになるから厳選すること
---

```dataview
TABLE 誰でも見れるやつ
FROM #Public_Mirror or #
SORT file.mtime DESC
```

すごくいいタイミングでの質問だね。
ここで一度「Public Mirror」の本質と、いまの仕組みを整理しておくのが Phase 3 のゴールにぴったりです。
👇以下で「①Public Mirrorとは」「②Lyraがどう参照しているか」「③VSCとObsidianの連携」「④今やるべきコマンド」「⑤Phase 3の進捗」をまとめます。

---

## **① Public Mirror の意味とリスク**

### **✅ 定義**

> **Public Mirror（公開ミラー）とは、あなたのローカル知識空間（Obsidian Vault）のうち、**
> **「外部AI（Lyraなど）が参照してよい部分だけを自動・機械可読**に同期したもの」です。

つまり：
- **内部（Private）:** Obsidian Vault → あなた専用。実務・法務・試験ノート・秘匿情報あり。
- **外部（Public Mirror）:** GitHub の public リポジトリ → Lyra など外部AI・自動化プロセスが読みに行く。

この Mirror は **GitHub 上に公開されている**ので、URL を知っていれば「誰でも」アクセスできるのがポイント（Lyraだけじゃない）。

たとえば：

```
https://raw.githubusercontent.com/hlo17/daegis-hub-public/main/bridges/obsidian/mirror/2_Areas/50_Daegis/Trellis/Trellis.md
```

このリンクは、ブラウザで誰でも開けます。

👉 **だから “Public Mirror” は「安全な切り出し」に徹する必要がある。**
---

## **② Lyra が参照できる仕組み（今の状態）**

Lyra は
1. 公開ミラー（daegis-hub-public）の **raw.githubusercontent.com** を監視していて
2. Markdown のメタデータ（frontmatter）や本文を自動解析して
3. 最新ノートを照合・要約・推論に使う

という構造になっています。

つまり、あなたが git push した時点で
→ GitHub の Public Mirror が更新され
→ Lyra が **自動的に新しいノートを取り込める** 状態になっています。

（Pi 上でやった curl テストの「200 OK & head -n5」が、Lyra 側からも成功していることを意味します）
---

## **③ VSC と Obsidian の関係（重要）**

|**操作場所**|**動作内容**|**公開への反映**|
|---|---|---|
|**Obsidian (Daisy Vault)**|ローカルに保存される|❌ コマンド実行しない限りGitHubに反映されない|
|**VSC (daegis-hub-public)**|すでにGitHubクローン。VSCで編集するとリポの中身を直接変更|✅ git commit && git push で即GitHub→Lyraへ反映される|
|**Piのcron/script**|rsync + git pushを自動で動かせるようにできる|✅ 将来的には“自動鏡像”|

---

## **④ 本適用してOK？ → はい、して大丈夫です。**

あなたの rsync ドライラン（-avn）結果を見る限り、危険ファイル（Slack webhookなど）はありません。
本適用（-av）すれば、Trellis配下の .md と .txt が全て **GitHubへ反映**され、Lyraが参照可能になります。

👇次のコマンドをそのまま実行してOKです。

```
# --- Phase 3 本適用 ---
hubpub
VAULT="$HOME/Library/Mobile Documents/iCloud~md~obsidian/Documents/Daisy"
SRC="$VAULT/2_Areas/50_Daegis"
DST="bridges/obsidian/mirror/2_Areas/50_Daegis"
rsync -av --delete --prune-empty-dirs \
  --include='Trellis/' --include='Trellis/**/' \
  --include='Trellis/*.md' --include='Trellis/*.txt' \
  --exclude='Trellis/*RISKY*' --exclude='Trellis/“触っちゃダメ”*' \
  --exclude='**/.DS_Store' \
  --exclude='**/*.png'  --exclude='**/*.jpg'  --exclude='**/*.jpeg' \
  --exclude='**/*.gif'  --exclude='**/*.heic' \
  --exclude='**/*.pdf'  --exclude='**/*.zip'  --exclude='**/*.tar' \
  --exclude='**/*.gz'   --exclude='**/*.rar'  --exclude='**/*.7z' \
  --exclude='**/*.xlsx' --exclude='**/*.csv' \
  --exclude='*' \
  "$SRC/" "$DST/"
# シークレットチェック（再確認）
grep -RIn -E '(hooks\.slack\.com|api(key|_key)|Bearer[[:space:]]+[A-Za-z0-9._-]{20,})' "$DST/Trellis" || true
# コミット＆プッシュ
git add -A && git commit -m "publish: Phase3 sync Trellis (safe subset)" || true
git push
```

---

## **⑤ Phase 3 の進捗まとめ**

|**フェーズ**|**内容**|**状況**|
|---|---|---|
|Phase 1|Public Mirror 概念設計・リポ作成|✅ 完了|
|Phase 2|手動 push フロー（rsync + commit + push）確立|✅ 完了|
|**Phase 3**|Lyra が自動参照できる安全同期ルール構築（今ここ）|⏳ → 本適用で完了予定|
|Phase 4（次）|自動化・監視（Pi で cron or systemd timer）＋ Slack 通知|🔜 準備段階|

---

## **🚦まとめ（超要約）**

- Public Mirror = “安全に外へ出すコピー” → 誰でも見れるが、中身は制限されている。
- Lyra は **raw.githubusercontent.com** 経由で自動で参照中。
- Obsidian はローカルだけ、pushしない限り反映されない。
- VSC で編集 → git push で即反映（Lyraも更新）。
- 今の rsync -av コマンドは安全。実行してOK。
- Phase 3 本適用で、Daegis_Hub の公開鏡像構造が完成。

---
次のステップ（Phase 4）では、✅ Pi 側で rsync + git push を 1コマンド化 →
✅ Slack に「Mirror更新通知」送信

まで自動化しよう。
---
もし今すぐに「Public Mirror 全体を一枚の概念図」にして整理したい？
Obsidian上で俯瞰できるMarkdown構成図（SSOT→Mirror→Lyra参照の流れ）も出せるよ。
