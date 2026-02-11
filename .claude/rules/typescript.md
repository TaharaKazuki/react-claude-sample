---
paths:
  - "src/**/*.{ts,tsx}"
---

# TypeScript ルール

## コンパイラ設定

Strictモード有効。`noUnusedLocals`, `noUnusedParameters`, `noFallthroughCasesInSwitch`, `noUncheckedSideEffectImports`, `erasableSyntaxOnly`。ターゲット ES2022、モジュール解決 bundler。

## コーディング規約

- `any` 禁止。`unknown` + 型ガードで絞り込む
- 型推論が効く箇所では型注釈を省略してよい
- Props は `interface` で定義。名前は `コンポーネント名Props`
- `type` は合併型・交差型など `interface` で表現しにくい場合のみ使用
- Enum 禁止（`erasableSyntaxOnly` のため）。`as const` またはユニオン型で代替
