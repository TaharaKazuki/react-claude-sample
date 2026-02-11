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

## リンティング

ESLint 9 フラットコンフィグ。`@eslint/js` recommended + `typescript-eslint` recommended + `react-hooks` + `react-refresh`。対象: `*.ts`, `*.tsx`。

## コーディング規約

### 全般

- コード変更後は必ず `npm run lint` と `npm run build` でエラーがないことを確認する
- 未使用の変数・import は残さない
- セミコロン省略
- シングルクォート使用（JSX属性含む）
- インデントはスペース2つ

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

## 開発ワークフロー

### ブランチ運用

- `main` は常にデプロイ可能な状態を保つ
- 作業は `feature/xxx`, `fix/xxx`, `refactor/xxx` 等のブランチで行う
- `main` への直接コミットは禁止。PR経由でマージする

### コミット規約

Conventional Commits 形式を使用:

- `feat:` — 新機能
- `fix:` — バグ修正
- `refactor:` — リファクタリング（機能変更なし）
- `style:` — コードスタイルの変更（フォーマット等）
- `docs:` — ドキュメントのみの変更
- `chore:` — ビルド・設定の変更
- `test:` — テストの追加・修正

メッセージは日本語で簡潔に書く（例: `feat: ログインフォームを追加`）
コミットメッセージに `Co-Authored-By` は付与しない

### コミット前の確認事項

以下をすべてパスしてからコミットする:

1. `npm run lint` — lint エラーなし
2. `npm run build` — 型チェック + ビルド成功
3. テストがある場合はテストもパス

### テスト方針

- テストフレームワーク導入時は Vitest を使用する（Viteとの統合が容易なため）
- テストファイルは対象ファイルと同階層に `*.test.ts` / `*.test.tsx` で配置
- コンポーネントテストには React Testing Library を使用
- 新機能追加・バグ修正時はテストも合わせて書く
- テスト対象の優先度: ユーティリティ関数 > カスタムフック > コンポーネント

### PR ルール

- PRタイトルはコミット規約と同じ形式
- 変更内容の概要と動作確認手順をPR本文に記載する
- `npm run lint` と `npm run build` がパスしていること
