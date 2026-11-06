# MerValue

**家の中のモノ、いまいくら？**

自宅で保有するガジェット・家電・家具・楽器などを登録し、  
**減価償却の目安**と**参考中古相場**から資産価値を見える化する Web アプリケーションです。

---

## できること

| 機能 | 内容 |
| --- | --- |
| 資産 CRUD | 製品名・カテゴリ・購入価格／日・状態・画像などを登録・編集・削除 |
| カテゴリ自動判定 | 製品名からカテゴリを推定（API） |
| 参考相場の推定 | カテゴリ・状態・購入価格から中古相場を推定（スクレイピングなし） |
| メ〇カリ検索導線 | 製品名付きで外部検索へ遷移し、実相場の確認を支援 |
| 減価償却の目安 | カテゴリ別耐用年数・定額法（残存 10%）で簿価を算出 |
| 対簿価シグナル | 推定相場と簿価の差分をプラス／マイナスで表示 |
| ダッシュボード | 相場合計、推移グラフ、カテゴリ構成比、最近の登録 |
| 一覧 UX | サムネイル付きリスト、ページネーション、上部へ戻る、削除確認 |

---

## 技術スタック

| 領域 | 技術 |
| --- | --- | 
| フレームワーク | [Astro](https://astro.build/) SSR + TypeScript | 
| UI | Tailwind CSS v4 / React Islands |
| データ | [Supabase](https://supabase.com/)（PostgreSQL） | 

**Node.js** `>= 22.12` を想定しています。

---

## 画面構成

| パス | 役割 |
| --- | --- |
| `/` | TOP（ブランド・キャッチ・導線） |
| `/dashboard` | 資産サマリー（金額 / 推移 / カテゴリ構成 / 最近の登録） |
| `/assets` | 資産一覧（フィルタ・ページネーション・メ〇カリ検索） |
| `/assets/add` | 新規登録 |
| `/assets/[id]` | 詳細・編集・削除確認 |
| `/api/auto-category` | カテゴリ自動判定 API |
| `/api/fetch-price` | 参考相場推定 API |

---

## セットアップ

### 1. 依存関係のインストール

```bash
npm install
cp .env.example .env
```

### 2. デモモード（おすすめの初回起動）

`.env` の `PUBLIC_DEMO_MODE=true` のまま起動できます。

```bash
npm run dev
```

`http://localhost:4321` でシードデータを使った UI 確認が可能です。  
※ プロセス再起動でデモデータは初期化されます。

### 3. Supabase 接続（永続化）

1. Supabase プロジェクトを作成  
2. SQL Editor で `supabase/migrations/0000_init_assets.sql` を実行  
3. `.env` を設定

```env
PUBLIC_SUPABASE_URL=https://xxxx.supabase.co
PUBLIC_SUPABASE_ANON_KEY=your-anon-key
PUBLIC_DEMO_MODE=false
```

> 初期マイグレーションには個人利用向けの `anon` 全操作ポリシーが含まれます。  
> Auth を本格運用する場合は、当該ポリシーを見直してください。

## ディレクトリ構成

```text
src/
├── components/     # UI（Astro / React Islands）
├── layouts/        # 共通レイアウト
├── lib/            # Supabase クライアント / デモストア
├── pages/          # ルーティング & API エンドポイント
├── services/       # ビジネスロジック（減価償却・相場・集計など）
├── types/          # 型定義
└── utils/          # 共通ユーティリティ（表示整形・Mercari URL など）
tests/
├── api/            # API 結合テスト
├── helpers/        # テストヘルパー
└── setup.ts
.github/workflows/  # CI
supabase/migrations/
```

---

## 運用上の注意

- **推定相場は参考値です。** 税務計算や投資判断の根拠には使えません。  

---

## ライセンス / 作者

リポジトリ: [yuki-snowy/MerValue](https://github.com/yuki-snowy/MerValue)
