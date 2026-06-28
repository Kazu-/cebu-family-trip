# 🌴 セブ島 家族旅行 2027

12名・福岡＋成田 2空港発／2027年夏（4泊5日）の家族旅行サイト。
GitHub Pages: <https://kazu-.github.io/cebu-family-trip/>

## 構成

| パス | 役割 |
|------|------|
| `index.html` | **ホーム（ハブ）**。プラン比較・詳細ガイド・WBS・旧プラン案への入口。`plans-data.js` からプランカードと比較表を自動生成 |
| `plans-data.js` | **プランの唯一の情報源**。プランA/B/C… を配列で定義 |
| `plan.html?id=A` | プラン詳細の汎用ビューア。`?id=` でA/B/Cを切替表示 |
| `guide.html` | 全プラン共通の詳細ガイド（航空券・ホテル比較・日程・アクティビティ・食事・持ち物・雨の日・予約） |
| `wbs.html` | 準備タスクのWBS。WEB上で入力・編集でき、進捗は **localStorage** に自動保存 |
| `styles.css` | 共通スタイル |
| `proposal-original/` | **以前作成したプランページ（保存版）**。上書きせず「たたき台」として保持 |

## 🔧 プランを追加・編集する（拡張性）

新しいプラン（例：プランD）を追加するには、`plans-data.js` の `CEBU.plans` 配列に
オブジェクトを **1件追記するだけ**。ホームのカード・比較表、`plan.html` の詳細が自動生成されます。

```js
{
  id: "D",
  name: "プランD",
  subtitle: "〇〇重視",
  status: "検討中",          // 採用 / 検討中 / アーカイブ
  accent: "ocean",          // coral / palm / sun / ocean / ink
  hotel: "ホテル名",
  hotelNote: "ホテルの特徴",
  flightsFuk: "福岡組の便",
  flightsNrt: "成田組の便",
  roomNote: "部屋割りメモ",
  budgetTotal: "≒ ○○万円",
  budgetAdult: "¥xxx,xxx〜",
  budgetChild: "¥xxx,xxx〜",
  highlights: ["強み1", "強み2"],
  reason: "このプランを選ぶ理由",
  cost: [ ["費目","大人1名","子供1名","12名合計"], ... ],
  costTotal: ["合計（目安）","...","...","≒ ¥..."]
}
```

採用プランを切り替えるには `CEBU.meta.confirmedId` を変更します（例：`"B"` → `"A"`）。

## 旧プラン案との関係

- 旧ページは `proposal-original/index.html` として**そのまま保存**（URL: `/proposal-original/`）。
- 新しいホームが `/`（サイトの入口）になり、全ページから旧プラン案へリンク済み。

## ローカル確認

各HTMLはサーバー無しでも動作しますが、`plans-data.js` を読む `index.html`/`plan.html` は
`file://` でも動きます。確実に確認するなら簡易サーバー：

```bash
python -m http.server 8000
# → http://localhost:8000/
```

## WBSデータの共有

WBSはブラウザごとの保存。端末間共有は WBSページの **「エクスポート」→ JSON →（別端末で）「インポート」** を利用。
