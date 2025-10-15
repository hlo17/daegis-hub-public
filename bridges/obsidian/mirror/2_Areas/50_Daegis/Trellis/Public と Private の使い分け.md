---
tags:
  - Lyra
  - Gardener
aliases:
created: 2025-10-15T07:46:01Z
updated: 2025-10-15T07:50:49Z
---

## Public/Privateの使い分け（簡潔な指針）

- **Private（sec-local-shield）**：運用本体・生ログ・秘密鍵周辺・アーカイブ全量
- **Public（daegis-hub-public）**：
    - ドキュメント/Runbook/Chronicleの**安全サブセット**
    - Obsidianブリッジ（Trellis等）の**公開して良いもののみ**
    - 監視メトリクス定義・ダッシュボード雛形・APIスキーマ
    
- 追加の保険：public_mirror/config/denylist.txt（既にある）に危険パターンを増やしていく
