---
tags:
  - Trellis
  - Gardener
aliases:
created: 2025-10-15 14:43
modified: 2025-10-15 14:43
Base: "[[Trellis]]"
Derivative: "[[RISKY レポート]]"
---
# “触っちゃダメ”最終確認（Hubへ入れない）


## “触っちゃダメ”最終確認（Hubへ入れない）

- 仮想環境/ビルド: **/.venv*/, venv*/, **/__pycache__/, node_modules/
- 実データ: ops/grafana/grafana-data/**（DB/プラグイン含む一式）
- 認可系: snapshots/**/mosquitto/acl
- シークレット: secrets/**, *.key, id_*（秘密鍵系）, .env
- DB/巨大: *.db, **20MB超**のファイル（バイナリ/アーカイブ等）

→ 迷ったら [[RISKY レポート]]に出たものは入れない、でOK。