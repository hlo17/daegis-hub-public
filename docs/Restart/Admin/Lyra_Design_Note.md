
# 🌿 Lyra Design Note — doctrine & runbooks (Oct-2025)

  

> 本ノートは Lyra（運用AI）のための設計指針／実行原則。

> Gardener は理解できなくてもOK。Lyra が一貫して守るための唯一の参照点。

  

## 0. スコープと目的

- 目的：Daegis の“公開の真実”を常に GitHub/main に保ち、編集体験はローカルの自由を最大化する。
    
- 守るべき真実：daegis-hub-public の main が公開の truth。それ以外は派生物。
    
- 前提：macOS（Private Vaultの原本）＋ Pi（補助）。iPad/iPhone は閲覧/軽作業のみ。
    


## 2. 論理構成

```
[Private Trellis (local Obsidian)]
      |  (rsync; UTF-8-MAC→UTF-8)
      v
[Public Mirror (local Git worktree)]
      |  (git add/commit/push)
      v
[GitHub: daegis-hub-public/main]  ← 公開の“真実”
```

- iPad/iPhone：iCloudの閲覧・軽編集可。ただし原本の最小差分に留める（競合を作らない）。
    


## 4. 実行コマンド（Lyraの標準動作）

  

### 4.1 ミラー（既定はDRY）

```
# 乾燥走行（出力を読む）
mirror-sync

# 本番
mirror-sync --go

# 反映
cd ~/sec-local-shield/Daegis_Hub/public_mirror/daegis-hub-public
git pull --rebase
git add -A
git commit -m "mirror: update from Trellis ($(date -u +%Y-%m-%dT%H:%M:%SZ))"
git push
```

### 4.2 その場 Git 採用（cloneしない）

```
repo-adopt ~/sec-local-shield/Daegis_Hub/public_mirror/daegis-hub-public \
  https://github.com/hl017/daegis-hub-public.git
```

### 4.3 週次スナップショット

```
snap-weekly      # => ~/Backups/local-snap-YYYYMMDDTHHMMSSZ.tgz
```

### 4.4 iCloud が怪しい時の軽い回復

```
icloud-nudge     # Finder/cloudd/bird をソフトに再始動
```


## 6. 失敗モードと対応

|症状|よくある原因|対応|
|---|---|---|
|文字化け（濁点分離）|HFS＋NFD (UTF-8-MAC)|Homebrew rsync + --iconv=UTF-8-MAC,UTF-8|
|iCloudが固まる|cloudd/bird 停滞|icloud-nudge、Safe Boot、Optimize OFF|
|Gitが「not a repository」|.git 不在|repo-adopt で採用（cloneしない）|
|二重作業場が生じた|過去のclone癖|公開作業場は1箇所に固定し他を撤去|
|Piが編集してしまう|スクリプト誤用|Piは公開側のみを操作。原本は触らない|


## 8. iPad / iPhone の扱い

- 基本は閲覧＆軽微な追記。大量編集は原本Macで。
    
- 手動で iCloud 経由の差分が出たときは、Mac側の Vault を正にし、次のミラーで公開へ。
    
- スマホ由来の衝突は Gardener が手で解消（Lyraはファイル比較の支援のみ）。
    


## 10. 最小ランブック（人間が見る用の3行）

```
mirror-sync             # 出力チェック
mirror-sync --go        # 実行
cd ~/sec-local-shield/Daegis_Hub/public_mirror/daegis-hub-public && \
  git pull --rebase && git add -A && \
  git commit -m "mirror: $(date -u +%Y-%m-%dT%H:%M:%SZ)" && git push
```


## メモ（Lyraの心構え）

- 「便利」に負けない：入口は 3 本に限定。
    
- DRY で始めて GO を明示：勝手に書き換えない。
    
- clone を安易に使わない：採用(Adopt)を標準に。
    
- 分からないときは何もしないでノートに記す（Gardenerが判断）。
    


## Addendum: Public Mirror Discipline & Safety Nets
**Updated:** {{date:YYYY-MM-DD}} {{time:HH:mm}} UTC+9

### Principles
- **Single Source of Truth:** Private Trellis is canonical. Public GitHub holds only index & requests.
- **Mirror Discipline:** Run `mirror-sync` (DRY → review → `--go`) then commit/push in public repo.
- **Unicode Hygiene:** `core.quotepath=false`, `core.precomposeunicode=true`, `.gitattributes: * text=auto eol=lf`.
- **No Secrets Public:** images/temp excluded; symlinks are intentional placeholders (no content overflow).

### Tooling
- `~/bin/mirror-sync` → Homebrew rsync(iconv)前提。  
- `~/bin/mac-health` → brew / xcode-select / git 基本健全性チェック。  
- `~/bin/snap-weekly` → Daisy + dotfiles の簡易スナップショット。  
- `~/bin/icloud-nudge` → Finder/iCloud の安全リスタート。

### Ops Notes
- Public で見る“真実”は常に `main`。ズレたら mirror し直せばよい。  
- 迷ったら **原本優先**、公開は軽量・再生成可能を死守。

