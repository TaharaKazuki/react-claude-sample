# CLAUDE.md

このファイルは、Claude Code がこのリポジトリで作業する際のガイダンスを提供する。

## プロジェクト概要

React 19 + TypeScript + Vite の最小構成スタータープロジェクト。ESモジュール使用。

## コマンド

- `npm run dev` — 開発サーバー起動（HMR付き）
- `npm run build` — 型チェック + ビルド
- `npm run lint` — ESLint実行
- `npm run preview` — 本番ビルドのプレビュー

テストフレームワークは未設定。

## アーキテクチャ

- **エントリーポイント:** `src/main.tsx` → `createRoot` + `<StrictMode>`
- **スタイリング:** プレーンCSS。グローバルは `index.css`、コンポーネント別は `App.css`。ライト/ダークモード対応。
- **状態管理:** Reactフックのみ（外部ライブラリなし）
- **ビルド:** Vite 7 + `@vitejs/plugin-react`

## TypeScript

Strictモード有効。`noUnusedLocals`, `noUnusedParameters`, `noFallthroughCasesInSwitch`, `noUncheckedSideEffectImports`, `erasableSyntaxOnly`。ターゲット ES2022、モジュール解決 bundler。

## リンティング

ESLint 9 フラットコンフィグ。`@eslint/js` recommended + `typescript-eslint` recommended + `react-hooks` + `react-refresh`。対象: `*.ts`, `*.tsx`。

## コーディング規約

### 全般

- コード変更後は必ず `npm run lint` と `npm run build` でエラーがないことを確認する
- 未使用の変数・import は残さない
- セミコロン省略
- シングルクォート使用（JSX属性含む）
- インデントはスペース2つ

### TypeScript

- `any` 禁止。`unknown` + 型ガードで絞り込む
- 型推論が効く箇所では型注釈を省略してよい
- Props は `interface` で定義。名前は `コンポーネント名Props`
- `type` は合併型・交差型など `interface` で表現しにくい場合のみ使用
- Enum 禁止（`erasableSyntaxOnly` のため）。`as const` またはユニオン型で代替

### React コンポーネント

- `function` 宣言で定義する（アロー関数不可）
- 1ファイル1コンポーネント。ファイル名 = コンポーネント名（PascalCase）
- `export default` で公開
- フラグメントは `<>...</>` を使用
- イベントハンドラは `handle` + イベント名（例: `handleClick`）
- Props コールバックは `on` + イベント名（例: `onClick`）

### React フック

- フックの呼び出しはコンポーネントのトップレベルに置く
- カスタムフックは `use` プレフィックス + `src/hooks/` に配置
- `useEffect` の依存配列省略は禁止
- `useState` の更新で前の値を参照する場合は関数形式を使う

### ファイル・ディレクトリ構成

- `src/components/` — コンポーネント（PascalCase）
- `src/hooks/` — カスタムフック（camelCase）
- `src/types/` — 型定義（camelCase）
- `src/utils/` — ユーティリティ関数（camelCase）
- `src/constants/` — 定数（camelCase）

### スタイリング

- プレーンCSS のみ使用（CSS-in-JS・ユーティリティCSS禁止）
- コンポーネント固有スタイルは `コンポーネント名.css` として同階層に配置
- CSSクラス名はケバブケース
- グローバルスタイルは `src/index.css`

### import の順序

グループ間に空行は入れない:

1. React 本体
2. サードパーティライブラリ
3. プロジェクト内コンポーネント
4. フック・ユーティリティ等
5. 型定義
6. アセット・CSS

### セッションの命名

- セッション開始時の最初のメッセージに `【カテゴリ】作業内容` 形式の表題を含める
- カテゴリ例: `機能追加`, `バグ修正`, `リファクタリング`, `設定変更`, `調査`
- 後から履歴でセッションを特定しやすくするため
