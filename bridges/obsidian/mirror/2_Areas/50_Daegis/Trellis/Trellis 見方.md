---
tags:
  - Trellis
aliases:
created: 2025-10-14 01:25
modified: 2025-10-14 01:25
---
# Trellis 見方


完璧。

この状態はまさに「🌳＝いま集中ゾーン」になってます。

---

### **✅ いまどういう状態か**

```
last_update: 0m ago | DND=OFF | ✅ ok
why-now: Obsidian denylistの反映を本番に適用（30mで確認まで）
value-shift: free
gate: auto
```

- ✅ ok → 最新の @current が更新されたばかり。
    
- DND=OFF → 通知ON（nudge も走る）。
    
- free → 探索・発想モード（“やりながら考える” に最適）。
    
- auto → gate を最小限にした軽量モード。

つまり、**Trellis的には「今これに集中していい」状態**。

これを更新し続けるだけで、Lyra や Gardener はあなたの「どこに注意を置いているか」を把握できます。

---

### **🌿 これからやること（このフェーズ）**

1. 実際に「denylist反映」作業をする

    　→ 終わったら **次の @current に切り替えるだけ**。

    　例：

```
trellis
```

1. → 1) Set Focus → why-nowを新しい作業に書き換え。
    
2. 作業中に「状態を残す」場合

    　→ どこまで進んだかを

```
trellis status
```

2. で確認しておけばOK。
    
3. もしブレイクしたら

    　→ trellis ping を1回打つ。

    　これは「記録用のゆるいマーカー」。あとでChronicleが拾います。

---

### **🌬 ちなみに**

- 8時間経つと自動で「🌬 soon」に変化（更新リマインド）
    
- 12時間で「🌬 due」＝“要リセット”
    
- 24時間放置で「🌬 due+」＝“意識の焦点がズレてるよ”状態に

---

今のうちに

```
cat ~/daegis/garden/hub/@current
```

を開くと、メモ構造としても再利用できるようになっています。

これが **“現行フォーカスカード”**（Gardenの心臓部）。

---

次ステップは：

> denylist反映が終わったら trellis status の更新結果を貼る

> それをもとに、Lyra側の “feedback→Chronicle” 自動反映を組み込みましょう。