# GitFlowを用いた小規模チーム運用ルール

## 基本ブランチ構成
GitFlowの小規模チーム向け簡略版では、次の主要ブランチを活用します：

- `main`：本番環境用のブランチ。常に安定した状態を維持
- `develop`：開発の主軸となるブランチ
- `feature`：新機能開発用のブランチ
- `hotfix`：緊急バグ修正用のブランチ

## 小規模チーム向けGitFlow運用ルール

### 1. ブランチの命名規則
- `feature` ブランチ：`feature/機能名` または `feature/#Issue番号_機能名`
- `hotfix` ブランチ：`hotfix/バグ内容` または `hotfix/#Issue番号_バグ内容`

### 2. 開発の流れ
1. まず必ずissueを作成し、開発内容を明確にする（`github-issue-template.md`のテンプレートを使用）
2. `develop`ブランチからissueに対応する`feature`ブランチを作成
3. `feature`ブランチで開発作業を行う
4. 開発完了後、`develop`ブランチにマージする（プルリクエスト推奨、`github-pr-template.md`のテンプレートを使用）
5. 一定量の機能が溜まったら、`develop`から`main`へマージ
6. `main`ブランチにバージョンタグを付与

### 3. バグ修正の流れ
1. `main`ブランチから`hotfix`ブランチを作成
2. 修正後、`main`と`develop`の両方にマージ

### 4. コミットメッセージのルール
- Prefixを使用：`add:`、`fix:`、`update:`、`refactor:` など
- Issue番号を含める：`fix: ログイン画面のバリデーションエラー #123`
- 変更内容を明確に記述する

### 5. プルリクエストの活用
- `feature`ブランチから`develop`へのマージ時に必ずプルリクエストを使用
- レビューを義務付け（最低1人以上）
- テンプレートを用意し統一フォーマットで記述（`github-pr-template.md`を参照）

### 6. CI/CDとの連携
- プルリクエスト時に自動テストを実行
- `main`へのマージ時に自動デプロイ（または半自動）

### 7. 小規模チーム向けの簡略化ポイント
- 開発者が少ない場合はレビュー担当者を固定せず全員でレビュー
- コードオーナー制度の導入（特定の領域に詳しい人を指定）

### 8. チームコミュニケーション
- ブランチ作成時にチャットで通知
- マージ前に簡単な説明やデモを行う
- 週1回程度の進捗共有ミーティング


### 9. デプロイ管理
- `main`ブランチへのマージ＝リリース可能な状態
- リリースごとにタグ付け（例：`v1.0.1`）
- リリースノートの作成（変更内容を記録）

### 10. 標準テンプレートの活用
- issue作成時は`github-issue-template.md`を使用して情報を整理
- PRの作成時は`github-pr-template.md`を使用して変更内容を明確に記述

> **Note:** 小規模チームでは、GitFlowを状況に合わせて簡略化することが効果的です。
重要なのは、必ずissueを作成してから開発を始め、プルリクエストを通じてコードレビューを行うワークフローを維持することです。