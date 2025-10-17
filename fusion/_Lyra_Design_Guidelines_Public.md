---
title: Lyra Design Guidelines (Public Summary)
visibility: public
version: v0
tags: [daegis, lyra, design, summary]
---

# Lyra Design Guidelines — Public Summary

## We Share
- Conceptual architecture, policies, evaluation metrics (e.g., pass@k, idempotency)

## We Do Not Share
- Secrets, internal URLs, hostnames, runbooks with privileged commands

## Highlights
- …

## Changelog
- …
## Access & Mirrors (Public summary)
- **Canonical (Private / Trellis)**  
  `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/Daisy/2_Areas/50_Daegis/Trellis/fusion/Lyra_Design_Guidelines.md`
  - Private 正本。秘密/内部URL/ホスト固有値は **ここだけ**。
- **Public Mirror (要約 / GitHub 公開OK)**  
  （このファイル）`fusion/_Lyra_Design_Guidelines_Public.md`  
  - 公開可能な概念・方針・運用原則のみ。**秘密は載せない**。
- **Truth の所在**  
  「公開可能な事実」に関しては **public mirror の main** を唯一の truth とする。  
  Private は詳細と履歴の保存、Public は状況共有のための要約。
- **運用**  
  - Private → Public は **要約して同期**（rsync ではなく “抽出・編集”）。  
  - コミットメッセ: `docs(public): … (YYYY-MM)`  
  - 安全ゲート: 秘密・内部URL・具体的なコマンド/ホスト名は除去。
- **端末ポリシー（参考）**  
  - macOS: Private Vault 原本の編集＋Public への要約反映。  
  - Pi / iOS: 閲覧・軽作業（編集はしない）。


### What we share / hide
- **Share**: 概念、運用原則、ガードレール、評価指標（例: pass@k 運用方針、冪等性の考え方）
- **Hide**: シークレット、内部URL、具体的ホスト名、トークン値、手元専用の実行コマンド

## Glossary (public)

