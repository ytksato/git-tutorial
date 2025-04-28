# Git理解度確認テスト - 模範解答

## 基礎知識編（各2点、計20点）

1. Gitの3つの作業エリアの順番：
```
1. 作業ディレクトリ（Working Directory）
2. ステージングエリア（Staging Area）
3. リポジトリ（Repository）
```

2. `git add`と`git commit`の違い：
- `git add`：変更をステージングエリアに追加する操作
- `git commit`：ステージングエリアの変更をリポジトリに永続的に記録する操作

3. `.gitignore`の役割と含めるべきファイル：
- 役割：バージョン管理対象外とするファイルを指定する
- 一般的に含めるファイル：
  1. ログファイル（*.log）
  2. 環境設定ファイル（.env）
  3. ビルド生成物（/dist, /build）

4. `git status`と`git log`の違い：
- `git status`：現在の作業ディレクトリとステージングエリアの状態を表示
- `git log`：コミット履歴を表示

## コマンド実践編（各3点、計30点）

5. 正しい操作手順：
```bash
b. git init
d. echo "# My Project" > README.md
c. git add .
a. git commit -m "first commit"
```

6. コミットメッセージ修正コマンド：
```bash
git commit --amend
```

7. ステージング取り消しコマンド：
```bash
git restore --staged <filename>
```

8. リモートリポジトリ情報確認コマンド：
```bash
git remote -v
```

9. ファイル変更履歴確認コマンド：
```bash
git log -p <filename>
```

## ブランチ操作編（各5点、計25点）

10. 新しいブランチ作成と切り替え：
```bash
git switch -c feature/new-branch
```

11. マージとリベースの違い：
- マージ：ブランチの履歴を保持したまま統合。共有ブランチへの統合に適している。
- リベース：履歴を直線的に整理。個人の作業ブランチの更新に適している。

12. コンフリクト対処手順：
1. コンフリクトファイルを確認（git status）
2. コンフリクト箇所を修正（<<<<<<, =======, >>>>>>>のマーカーを整理）
3. 修正したファイルをステージング（git add）
4. コミットを作成（git commit）

13. git stashの説明：
- 用途：作業中の変更を一時的に保存する機能
- 基本的な使用方法：
```bash
git stash save "作業内容の説明"  # 変更を保存
git stash list                   # 保存した変更の一覧表示
git stash pop                    # 保存した変更を復元
```

14. プルリクエストの基本的な流れ：
1. 機能ブランチを作成
2. 変更を実装しコミット
3. リモートにプッシュ
4. GitHubでプルリクエストを作成
5. レビューとフィードバック対応
6. マージ実行

## 実践問題（25点）

15. 新機能開発の手順：
```bash
# mainブランチから新機能ブランチを作成
git switch main
git pull origin main
git switch -c feature/new-function

# 機能実装と変更確認
# （ファイル編集後）
git status
git diff

# 変更をコミット
git add .
git commit -m "新機能の実装"

# mainの最新変更を取り込む
git fetch origin main
git merge origin/main

# リモートにプッシュ
git push -u origin feature/new-function

# プルリクエスト作成後、マージ完了後のクリーンアップ
git switch main
git pull origin main
git branch -d feature/new-function
git push origin --delete feature/new-function
```

採点のポイント：
- コマンドの正確性
- 手順の論理的な流れ
- 安全性への配慮
- ベストプラクティスの採用
