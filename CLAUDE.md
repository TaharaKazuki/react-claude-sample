# CLAUDE.md

このファイルは、Claude Code (claude.ai/code) がこのリポジトリで作業する際のガイダンスを提供します。

## プロジェクト概要

React 19 + TypeScript + Vite の最小構成スタータープロジェクト。ESモジュール (`"type": "module"`) を使用。

## コマンド

- `npm run dev` — Vite開発サーバーをHMR付きで起動
- `npm run build` — `tsc -b` で型チェック後、Viteでバンドル
- `npm run lint` — プロジェクト全体にESLintを実行
- `npm run preview` — 本番ビルドをローカルでプレビュー

テストフレームワークは未設定。

## アーキテクチャ

- **エントリーポイント:** `src/main.tsx` が React 19 の `createRoot` を使い、`<StrictMode>` 内で `<App />` をレンダリング
- **スタイリング:** プレーンCSSファイル（グローバルスタイルは `index.css`、コンポーネントスタイルは `App.css`）。CSSメディアクエリによるライト/ダークモード対応。
- **状態管理:** Reactフックによるローカルコンポーネントステートのみ（外部状態管理ライブラリなし）
- **ビルド:** Vite 7 + `@vitejs/plugin-react`（Babelベースの Fast Refresh）

## TypeScript

Strictモード有効。追加チェック: `noUnusedLocals`, `noUnusedParameters`, `noFallthroughCasesInSwitch`, `noUncheckedSideEffectImports`, `erasableSyntaxOnly`。ターゲットは ES2022、モジュール解決は bundler。

## リンティング

ESLint 9 フラットコンフィグ (`eslint.config.ts`)。`@eslint/js` recommended、`typescript-eslint` recommended、`react-hooks`、`react-refresh` プラグインを使用。対象は `*.ts` および `*.tsx` ファイル。

## コーディング規約

### 全般

- コードの変更後は必ず `npm run lint` と `npm run build` を実行してエラーがないことを確認する
- 未使用の変数・import は残さない（`noUnusedLocals`, `noUnusedParameters` が有効）
- セミコロンは省略する
- 文字列はシングルクォートを使用する（JSX 属性もシングルクォート）
- インデントはスペース2つ

### TypeScript

- `any` 型の使用を禁止する。`unknown` を使い、型ガードで絞り込む
- 型推論が効く箇所では明示的な型注釈を省略してよい（例: `useState(0)` に `number` の注釈は不要）
- コンポーネントの Props は `interface` で定義し、`type` は合併型・交差型など `interface` で表現しにくい場合に使う
- Props の `interface` 名は `コンポーネント名Props` とする（例: `interface ButtonProps`）
- Enum は使用禁止（`erasableSyntaxOnly` 有効のため）。`as const` オブジェクトまたはユニオン型で代替する

### React コンポーネント

- コンポーネントは `function` 宣言で定義する（アロー関数ではなく `function ComponentName() {}` を使用）
- コンポーネントは1ファイルにつき1つとし、ファイル名はコンポーネント名と一致させる（PascalCase: `Button.tsx`）
- コンポーネントは `export default` で公開する
- フラグメントは短縮構文 `<>...</>` を使用する
- イベントハンドラの命名は `handle` + イベント名とする（例: `handleClick`, `handleSubmit`）
- Props で渡すコールバックの命名は `on` + イベント名とする（例: `onClick`, `onSubmit`）

### React フック

- フックの呼び出しはコンポーネントのトップレベルに置く
- カスタムフックは `use` プレフィックスをつけ、`src/hooks/` ディレクトリに配置する（例: `useAuth.ts`）
- `useEffect` には必ず依存配列を指定する。依存配列の省略は禁止
- `useState` の更新関数で前の値を参照する場合は関数形式を使う（例: `setCount((prev) => prev + 1)`）

### ファイル・ディレクトリ構成

- コンポーネントファイル: `src/components/` に PascalCase で配置（例: `src/components/Button.tsx`）
- カスタムフック: `src/hooks/` に camelCase で配置（例: `src/hooks/useAuth.ts`）
- 型定義ファイル: `src/types/` に camelCase で配置（例: `src/types/user.ts`）
- ユーティリティ関数: `src/utils/` に camelCase で配置（例: `src/utils/format.ts`）
- 定数定義: `src/constants/` に camelCase で配置（例: `src/constants/routes.ts`）

### スタイリング

- プレーンCSS を使用する（CSS-in-JS やユーティリティCSSフレームワークは導入しない）
- コンポーネント固有のスタイルは `コンポーネント名.css` として同階層に配置する
- CSS クラス名はケバブケースを使用する（例: `read-the-docs`, `card-title`）
- グローバルスタイルは `src/index.css` にまとめる

### import の順序

import は以下の順序でグループ化し、グループ間に空行は入れない:

1. React 本体（`react`, `react-dom`）
2. サードパーティライブラリ
3. プロジェクト内のコンポーネント（`./components/...`）
4. プロジェクト内のフック・ユーティリティ等（`./hooks/...`, `./utils/...`）
5. 型定義（`./types/...`）
6. アセット・CSS ファイル

### セッションの命名

- セッション開始時の最初のメッセージには、作業内容がひと目でわかる表題を含める
- 形式: `【カテゴリ】具体的な作業内容` とする
- カテゴリ例: `機能追加`, `バグ修正`, `リファクタリング`, `設定変更`, `調査`
- 例: `【機能追加】ユーザー認証のログイン画面を実装したい`
- 例: `【バグ修正】カウンターが二重にカウントされる問題を直したい`
- 例: `【リファクタリング】App.tsxからヘッダーコンポーネントを分離したい`
- 後から履歴を見返した際にどのセッションか特定しやすくすることが目的
