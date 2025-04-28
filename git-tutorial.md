# git tutorial

## gitの初期設定
```bash
# ユーザー名を設定
git config --global user.name "Your Name"

# emailを設定
git config --global user.email "your.email@example.com"

# エディタを設定
git config --global core.editor "vim"

# それぞれを確認
git config user.name
git config user.email
git config core.editor

# 設定を確認
git config --list
cat ~/.gitconfig
```

## gitの基本的な仕組み
### 1. Gitの基本概念

1. **スナップショット方式**
- 各コミットは、ファイルシステム全体のスナップショット
- 変更があったファイルのみ新しく保存
- 変更のないファイルは前のスナップショットへの参照

2. **3つの作業エリア**
```bash
# 作業ディレクトリ（Working Directory）
# ↓ git add
# ステージングエリア（Staging Area）
# ↓ git commit
# リポジトリ（Repository）
```

### 2. オブジェクトの種類

1. **blobオブジェクト**
- ファイルの内容を保存
- ファイル名は保存しない
- SHA-1ハッシュで識別

2. **treeオブジェクト**
- ディレクトリ構造を表現
- ファイル名とblobへの参照を保持
- ディレクトリの階層構造を管理

3. **commitオブジェクト**
- コミット情報を保持
- 親コミットへの参照
- ルートtreeオブジェクトへの参照
- 作者、コミッター、日時、メッセージ

4. **tagオブジェクト**
- 特定のコミットに対する参照
- バージョン番号などの追加情報

### 3. データの流れ

```bash
# 1. ファイル変更
vim example.txt

# 2. ステージングエリアに追加
git add example.txt

# 3. リポジトリにコミット
git commit -m "Add example.txt"
```

### 4. 内部動作の例

1. **ファイル保存時**
```bash
# ファイルの内容からblobオブジェクト作成
# → .git/objects/に保存
```

2. **コミット時**
```bash
# 1. ステージング情報からtreeオブジェクト作成
# 2. コミット情報からcommitオブジェクト作成
# 3. HEADとブランチ参照を更新
```

### 5. 重要な概念

1. **SHA-1ハッシュ**
- 全てのオブジェクトを一意に識別
- 内容に基づいて生成
- 40文字の16進数

2. **参照（References）**
```bash
# ブランチ（.git/refs/heads/）
# タグ（.git/refs/tags/）
# HEAD（現在のブランチ位置）
```

3. **コミットグラフ**
- コミットは有向非循環グラフ（DAG）を形成
- 各コミットは親コミットを参照
- マージコミットは複数の親を持つ

### 6. 基本的なファイル構造

```bash
.git/
  ├── objects/     # Gitオブジェクトの保存
  ├── refs/        # 参照情報
  ├── HEAD         # 現在のブランチ
  ├── config       # リポジトリの設定
  └── index        # ステージング情報
```

### 7. 重要なコマンドと内部動作

1. **git init**
```bash
# .gitディレクトリを作成
# 基本的なGitファイル構造を初期化
```

2. **git add**
```bash
# 1. ファイルの内容からblobオブジェクト作成
# 2. インデックス（ステージング）を更新
```

3. **git commit**
```bash
# 1. ステージング状態からtreeオブジェクト作成
# 2. コミットオブジェクト作成
# 3. ブランチ参照を更新
```

### 8. 分散型システムの特徴

1. **完全なリポジトリコピー**
- 各クローンは完全な履歴を持つ
- オフライン作業が可能
- 分散型のバックアップ

2. **リモート操作**
```bash
# リモートからデータ取得
git fetch origin

# リモートに変更を送信
git push origin main
```

### 9. 効率性の仕組み

1. **圧縮とパッキング**
- オブジェクトは圧縮して保存
- 定期的にパック化して最適化
- 重複データの効率的な管理

2. **差分アルゴリズム**
- 効率的な差分計算
- 変更箇所のみを保存
- ネットワーク転送の最適化

### まとめ

Gitの特徴：
- コンテンツアドレス可能なファイルシステム
- 分散型バージョン管理
- データの整合性保証
- 効率的なストレージ管理
- 柔軟なワークフロー

これらの仕組みにより：
- 高速な操作
- 信頼性の高い履歴管理
- 効率的なチーム開発
が実現されています。


## ローカルリポジトリの作成
```bash
$ git init
```

## GitHub上にあるプロジェクトから始める
```bash
$ git clone https://github.com/ytksato/git-tutorial.git
```
## 変更をステージに追加する
```bash
# ファイルを指定して追加
$ git add filename

# 全てのファイルを追加
$ git add .
```

## 変更を記録する（コミット）
```bash
# コミットメッセージを指定しない
$ git commit

# コミットメッセージを指定
$ git commit -m "commit message"

# 変更内容の差分を表示しながらコミット
$ git commit -v
```

## gitの基本コマンド
- git status
- git diff

## 新）ステージングした変更を取り消す
- git restore --staged filename 
### ワークツリーのファイルを元に戻す
- git restore filename

### 旧）ステージングした変更を取り消す
- git reset HEAD filename
### ワークツリーのファイルを元に戻す
- git checkout -- filename

## 直前のコミットをやり直す
- git commit --amend
### git commit --amendでできること
- 直前のコミットメッセージを修正できる
- 直前のコミットに変更を追加できる
  - 追加の変更をステージングしてから--amendを実行
  - 新しいコミットは作られず、直前のコミットが上書きされる

### 注意点
- プッシュ済みのコミットを--amendで修正すると、履歴が変わってしまう
- 複数人で作業している場合は、プッシュ済みのコミットの--amendは避ける
  - 他の開発者の作業に影響を与える可能性がある
  - 必要な場合は、チームメンバーに事前に相談する

# GitHubとやりとり
## リモートの情報を確認
```bash
$ git remote origin

$ git remote -v
origin  https://github.com/ytksato/git-tutorial.git (fetch)
origin  https://github.com/ytksato/git-tutorial.git (push)
```

## fetchとpushのURLが異なるケース：
```bash
origin  git://github.com/ytksato/git-tutorial.git (fetch)  # 読み取り専用
origin  https://github.com/ytksato/git-tutorial.git (push) # 書き込み可能

# 例えば以下のようなシチュエーション:
# - セキュリティ上の理由でfetchは読み取り専用URLを使用
# - プロキシ経由でのアクセスが必要な環境
# - ミラーリングされたリポジトリからfetchする場合
# - 異なるプロトコル(git://, https://, ssh等)を使い分ける場合
```

## リモートリポジトリを追加
```bash
$ git remote add bak https://github.com/ytksato/git-tutorial-bak.git
```

## リモートリポジトリを確認
```bash
$ git remote   
bak
origin

$ git remote -v
bak     https://github.com/ytksato/git-tutorial-bak.git (fetch)
bak     https://github.com/ytksato/git-tutorial-bak.git (push)
origin  https://github.com/ytksato/git-tutorial.git (fetch)
origin  https://github.com/ytksato/git-tutorial.git (push)
```

## 作成したバックアップ用リモートリポジトリにプッシュ
```bash
$ git push -u bak main
Enumerating objects: 42, done.
Counting objects: 100% (42/42), done.
Delta compression using up to 14 threads
Compressing objects: 100% (38/38), done.
Writing objects: 100% (42/42), 7.80 KiB | 7.80 MiB/s, done.
Total 42 (delta 14), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (14/14), done.
To https://github.com/ytksato/git-tutorial-bak.git
 * [new branch]      main -> main
branch 'main' set up to track 'bak/main'.
```

## リモートから取得しよう（fetch）
```bash
$ git fetch origin
```
### 例として、リモートリポジトリに直接「fetch-test.md」と言うファイルを作成してみる（通常やってはいけない）
```bash
$ git fetch origin
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Unpacking objects: 100% (3/3), 1001 bytes | 333.00 KiB/s, done.
From https://github.com/ytksato/git-tutorial
   2ad294d..b3c9774  main       -> origin/main

# fetchの内容は remotes/origin/main に反映される
# これはワークツリーにファイルの内容を落としている訳ではなく、リモートリポジトリのブランチに反映しているだけである。
$ git branch -a
* main
  remotes/bak/main
  remotes/origin/main
```

### ローカルのブランチをリモートのブランチに反映
```bash
$ git switch remotes/origin/main
M       git-tutorial.md
Note: switching to 'remotes/origin/main'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at b3c9774 Create fetch-test.md

# fetch-test.mdと言うリモートリポジトリにしかないはずのファイルが確認できる
$ ls -al
total 32
drwxr-xr-x   8 ytk  staff   256  4 26 17:51 .
drwxr-xr-x@ 40 ytk  staff  1280  4 25 13:31 ..
-rw-r--r--   1 ytk  staff   109  4 25 13:41 .cursorindexingignore
drwxr-xr-x  13 ytk  staff   416  4 26 17:51 .git
-rw-r--r--   1 ytk  staff    11  4 25 15:22 .gitignore
drwxr-xr-x   4 ytk  staff   128  4 25 13:41 .specstory
-rw-r--r--   1 ytk  staff     5  4 26 17:51 fetch-test.md
-rw-r--r--   1 ytk  staff  3742  4 26 17:51 git-tutorial.md

# リモートリポジトリの変更をローカルに反映
$ git merge origin/main
Merge made by the 'ort' strategy.
 fetch-test.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 fetch-test.md
```

## リモートから取得しよう（pull）
```bash
# fetchとmergeを同時に行うのと同義ではあるが注意が必要
$ git pull origin main

# リモートリポジトリで作成したpull-test.mdがローカルに反映された
$ ls -al
total 48
drwxr-xr-x   9 ytk  staff   288  4 26 18:09 .
drwxr-xr-x@ 40 ytk  staff  1280  4 25 13:31 ..
-rw-r--r--   1 ytk  staff   109  4 25 13:41 .cursorindexingignore
drwxr-xr-x  14 ytk  staff   448  4 26 18:09 .git
-rw-r--r--   1 ytk  staff    11  4 25 15:22 .gitignore
drwxr-xr-x   4 ytk  staff   128  4 25 13:41 .specstory
-rw-r--r--   1 ytk  staff     5  4 26 17:57 fetch-test.md
-rw-r--r--   1 ytk  staff  5321  4 26 17:58 git-tutorial.md
-rw-r--r--   1 ytk  staff    10  4 26 18:09 pull-test.md
```

## fetchとpullを使い分けよう

1. **`git pull` の内部動作**:
- `git pull` は実際には `git fetch` + `git merge` の組み合わせです
- つまり：
  ```bash
  git pull origin hoge
  ```
  は以下と同じ動作をします：
  ```bash
  git fetch origin hoge  # リモートの情報を取得
  git merge origin/hoge  # 現在のブランチにマージ
  ```

2. **誤解が生じやすい理由**:
- `git pull hoge` というコマンドを打つとき、多くの人は「`hoge`ブランチを最新にする」と考えます
- しかし実際の動作は「現在いるブランチに`hoge`の内容をマージする」です
- この認識の違いが事故（意図しないマージ）を引き起こす原因になります

3. **`git fetch` + `git merge` の利点**:
- 2つのステップに分けることで、各操作の意図が明確になります
- `git fetch` で安全にリモートの情報を取得
- `git merge` で明示的にマージ操作を行う
- マージする前に変更内容を確認できる（`git diff HEAD origin/hoge`など）

4. **安全な運用のために**:
- 特に Git に慣れていない段階では、`git pull` の代わりに `git fetch` と `git merge` を個別に実行する方が安全です
- これにより意図しないマージを防ぎ、各操作の意味をより理解しやすくなります

## リモートの情報を詳しく知ろう
```bash
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/ytksato/git-tutorial.git
  Push  URL: https://github.com/ytksato/git-tutorial.git
  HEAD branch: main
  Remote branch:
    main tracked
  Local ref configured for 'git push':
    main pushes to main (fast-forwardable)
```

## リモートを変更・削除しよう
```bash
# リモートを確認
$ git remote
bak
origin

# リモートの名前を変更
$ git remote rename bak backup
Renaming remote references: 100% (1/1), done.

# 変更を確認
$ git remote                  
backup
origin

# リモートを削除
$ git remote rm backup

# 削除されていることを確認
$ git remote          
origin
```

## 新しいブランチを作成しよう
```bash
$ git branch feature

# 新しいブランチを確認
$ git branch
* main
  feature

# 全てのブランチを確認（リモートのブランチも含む）
$ git branch -a
* main
  remotes/origin/main
```

```bash
# ブランチの履歴を確認
$ git log --oneline --decorate
3abb0f1 (HEAD -> main, feature) Merge branch 'main' of https://github.com/ytksato/git-tutorial
e842553 (origin/main) Create pull-test.md
4380951 リモートから取得しよう（fetch）を追加
ee9e85c Merge remote-tracking branch 'origin/main'
b3c9774 Create fetch-test.md
2ad294d ファイル名を変更
9963c18 git commit --amendの注意点を追記
6993162 git commit --amendを追記
142a253 index.htmlにgit restoreとgit resetコマンドの詳細な説明を追加し、ワークツリーのファイルを元に戻す手順を明確化しました。
e01a4b3 index.htmlにgit restoreとgit resetコマンドの説明を追加
146fe33 .specstoryディレクトリを.gitignoreに追加し、不要な履歴ファイルを削除しました。
1fd59f3 git mvコマンドの意義についての詳細を追加し、リネーム操作の利点や履歴追跡の容易さを説明しました。
bd955eb index.htmlの削除を取り消し、復元手順を記録した内容を追加
eb5ea9a index.htmlにgit diffコマンドを追記
579828a git statusコマンドを追記
eca8e39 index.htmlファイルを新規作成し、"git tutorial"という見出しを追加しました。
e2167cc first commit
```

## ブランチを切り替えよう
```bash
$ git switch feature

# ブランチを新規作成して切り替える
$ git switch -c feature
```

# コンフリクトを起こしてみよう
```bash
$ git merge feature
Auto-merging feature-test.md
CONFLICT (content): Merge conflict in feature-test.md
Automatic merge failed; fix conflicts and then commit the result.

$ git status
On branch main
Your branch is ahead of 'origin/main' by 6 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   feature-test.md

no changes added to commit (use "git add" and/or "git commit -a")
```

### コンフリクトを起こしたファイルを確認して中身をきれいにする
```
# feature-branch
## git merge test
<<<<<<< HEAD
## コンフリクトテスト
=======
## feature-conflict-test
>>>>>>> feature
```

### コンフリクトを防ぐための運用ルール

1. **プッシュ前の最新化を徹底**
```bash
# 作業開始前に必ず実行
git fetch  # リモートの最新情報を取得
git merge origin/main  # メインブランチの最新内容を取り込む
```

2. **ブランチ運用のベストプラクティス**
- 機能単位で作業ブランチを作成
```bash
# 新しい機能の開発を始める時
git switch -c feature/new-function main  # mainブランチから新規ブランチを作成
```
- 作業ブランチは短期間で完結させる（長期化を避ける）
- 1ブランチ1機能の原則を守る

3. **こまめなコミットと同期**
```bash
# 小さな単位で変更をコミット
git commit -m "具体的な変更内容を記述"

# 定期的にメインブランチの変更を取り込む
git fetch
git merge origin/main
```

4. **同じファイルの同時編集を避ける**
- チーム内で作業対象ファイルを事前に共有
- 作業範囲が重複する場合は、事前に調整

5. **コミュニケーションの徹底**
- 作業開始前にチームメンバーに共有
- 大きな変更がある場合は事前告知
- プルリクエストの内容を明確に記述

6. **git statusの習慣化**
```bash
# 作業前、コミット前、プッシュ前に必ず確認
git status
```

7. **コードフォーマットの統一**
- チーム共通のコードフォーマッターを使用
- コミット前にフォーマットを適用

8. **マージ前のレビュー必須化**
- プルリクエストを必須にする
- コードレビューを必ず実施
- CI/CDでの自動テストを通過させる

## ブランチを変更・削除する
### ブランチ名を変更する
```bash
$ git branch -m new-feature
```

### ブランチを削除する
```bash
$ git branch -d new-feature
Deleted branch new-feature (was 511f7f1).
```
`git switch`の使用をお勧めします。理由は以下の通りです：

1. **`git switch`の利点**:
- より直感的で目的が明確
- ブランチの切り替えに特化したコマンド
- Git 2.23以降で導入された新しいコマンド
- より安全な設計（ブランチ操作に限定）

基本的な使い方：
```bash
# ブランチの切り替え
git switch main

# 新しいブランチを作成して切り替え
git switch -c new-feature
```

2. **`git checkout`の特徴**:
- 多機能すぎて混乱しやすい
- ブランチの切り替え以外にも様々な機能がある：
  - ファイルの復元
  - 特定のコミットへの移動
  - ブランチの作成
- 古いコマンドで、歴史的な理由で多機能になっている

3. **なぜ`switch`が推奨されるのか**:
- 機能が明確で分かりやすい
- ミスを防ぎやすい
- モダンなGitの使用方法に沿っている
- コマンドの意図が明確（"切り替える"という動作）

結論：
- 新しいプロジェクトでは`git switch`を使うことをお勧めします
- `checkout`は必要な場合（ファイルの復元など）に限定して使用するのがベストプラクティスです

## プルリクエストの流れ
プルリクエスト（PR）の基本的な流れを説明します：

### 1. 作業用ブランチの作成
```bash
# mainブランチに移動
git switch main

# リモートの最新情報を取得
git fetch

# リモートの変更を取り込む
git merge origin/main
# または
git pull  # fetch + mergeを一度に実行

# mainブランチから新しいブランチを作成
git switch -c feature/new-function main

# 作業内容をコミット
git add .
git commit -m "新機能の実装"
```

### 2. リモートにプッシュ
```bash
# 初回プッシュ時
git push -u origin feature/new-function

# 2回目以降
git push
```

### 3. GitHubでプルリクエストを作成
- GitHubのリポジトリページで「Pull requests」タブを開く
- 「New pull request」ボタンをクリック
- 以下を設定：
  - base branch: `main`（マージ先）
  - compare branch: `feature/new-function`（作業ブランチ）
- タイトルと説明を記入
  - 何を変更したか
  - なぜ変更したか
  - テスト結果
  - 関連する課題番号

### 4. レビュー依頼とフィードバック
- レビュワーを指定
- レビューコメントへの対応
- 必要に応じて追加の修正をコミット
```bash
# レビュー指摘への対応
git commit -m "レビュー指摘の修正"
git push
```

### 5. マージ
- 全てのレビューが承認されたら
- CIテストがパスしていることを確認
- 「Merge pull request」をクリック

### 6. クリーンアップ
```bash
# mainブランチに戻る
git switch main

# リモートの変更を取り込む
git pull

# 作業ブランチを削除（ローカル）
git branch -d feature/new-function

# 作業ブランチを削除（リモート）
git push origin --delete feature/new-function
```

### プルリクエストの良い習慣
1. **適切なサイズ**
   - レビューしやすい大きさに保つ
   - 大きな変更は複数のPRに分割

2. **明確な説明**
   - 変更内容が分かりやすいタイトル
   - 詳細な説明
   - スクリーンショットなどの視覚的な情報

3. **レビューの質**
   - コードレビューは丁寧に
   - 建設的なフィードバック
   - 迅速なレスポンス

4. **テスト**
   - 必要なテストを追加
   - 既存のテストが通ることを確認
   - 手動テストも実施

これらの手順を守ることで、効率的で質の高い開発フローを維持できます。

## GitHub Flow

GitHub Flowの基本的なライフサイクルを説明します：

### 1. mainブランチの最新化
```bash
git switch main
git fetch
git merge origin/main
```

### 2. 作業用ブランチの作成
```bash
# 命名規則例：
# feature/機能名
# fix/バグ修正
# hotfix/緊急修正
# docs/ドキュメント
git switch -c feature/new-function
```

### 3. 開発作業
```bash
# 作業中は定期的に変更をコミット
git add .
git commit -m "変更内容の説明"

# 長期の作業の場合は定期的にmainの変更を取り込む
git fetch
git merge origin/main

# リモートにプッシュ（変更のバックアップと共有）
git push -u origin feature/new-function
```

### 4. プルリクエスト（PR）の作成
- GitHubで作成
- 以下を明確に記述：
  - 変更の目的
  - 変更の概要
  - テスト結果
  - レビュー時の注意点

### 5. コードレビュー
- レビュワーからのフィードバック対応
```bash
# レビュー指摘を修正
git commit -m "レビュー指摘の修正"
git push
```

### 6. CI/CDとテスト
- 自動テストの実行
- コードの品質チェック
- ビルドの確認
- 必要に応じてステージング環境でのテスト

### 7. mainへのマージ
- レビュー承認後
- CIテスト通過確認後
- GitHubの「Merge pull request」ボタンでマージ

### 8. デプロイ
- mainブランチへのマージが即デプロイのトリガーに
- 継続的デプロイメント（CD）による自動デプロイ
- デプロイ後の動作確認

### 9. クリーンアップ
```bash
# ローカルのmainを最新化
git switch main
git pull

# 作業ブランチの削除
git branch -d feature/new-function
git push origin --delete feature/new-function
```

### GitHub Flowの特徴と利点
1. **シンプルさ**
   - mainブランチが常にデプロイ可能
   - ブランチモデルがシンプル
   - 理解しやすく導入が容易

2. **継続的デリバリー**
   - 小さな変更を頻繁にリリース
   - フィードバックの早期取得
   - 問題の早期発見

3. **品質管理**
   - PRでのコードレビュー必須
   - 自動テストによる品質保証
   - デプロイ前の確認プロセス

4. **透明性**
   - 全ての変更がPRを通過
   - 変更履歴が明確
   - チーム全体での知識共有

### ベストプラクティス
1. **ブランチ運用**
   - 機能単位で作成
   - 寿命は短く保つ
   - 定期的にmainと同期

2. **コミット**
   - 意味のある単位でコミット
   - 明確なコミットメッセージ
   - 頻繁なプッシュ

3. **レビュー**
   - 建設的なフィードバック
   - 迅速なレスポンス
   - 知識共有の機会として活用

4. **デプロイ**
   - 自動化の推進
   - 監視の徹底
   - 素早いロールバック体制

## `main`ブランチでは`git fetch`の使用を推奨します。

### 推奨される方法（より安全）
```bash
# 1. リモートの変更を確認
git fetch origin main

# 2. 変更内容を確認できる
git diff main origin/main

# 3. 問題なければマージ
git merge origin/main
```

### `git pull`を避ける理由：

1. **予期せぬマージの防止**
   - `pull`は`fetch`と`merge`を自動で行う
   - 変更内容を確認する機会がない
   - 意図しないマージが発生する可能性

2. **コンフリクト対応**
   - `pull`でコンフリクトが発生すると、準備なしに対応が必要
   - `fetch`なら事前に変更を確認でき、計画的に対応可能

3. **変更の可視性**
   - `fetch`では変更内容を事前確認できる
   - 特に`main`のような重要なブランチでは変更確認が重要

4. **操作の明確さ**
   - 各ステップ（取得→確認→マージ）が明確
   - チーム内での操作の透明性が高まる

### まとめ：
`main`ブランチでは`git fetch`を使用し、変更を確認してから`merge`する方法が推奨されます。これにより、より安全で制御された開発フローを維持できます。

## マージとリベースの違い
マージとリベースの違いについて説明します：

### マージ（Merge）
```bash
# mainの変更を作業ブランチに取り込む場合
git switch feature/new-function
git merge main
```

**特徴：**
1. **履歴の保持**
   - マージコミットが作成される
   - ブランチの分岐と統合が履歴に残る
   - 全ての変更履歴が保存される

2. **安全性**
   - 非破壊的な操作
   - 元の履歴が維持される
   - コンフリクト解決が比較的簡単

3. **デメリット**
   - 履歴が複雑になりやすい
   - マージコミットが増える
   - 履歴が枝分かれして見づらくなる可能性

### リベース（Rebase）
```bash
# mainの変更を作業ブランチに取り込む場合
git switch feature/new-function
git rebase main
```

**特徴：**
1. **履歴の書き換え**
   - コミット履歴が直線的になる
   - ブランチの開始点を移動する
   - きれいな履歴が作成される

2. **メリット**
   - 履歴がクリーンになる
   - 直線的な履歴で読みやすい
   - マージコミットが不要

3. **デメリット**
   - 履歴が書き換えられる（危険）
   - コンフリクト解決が複雑になる可能性
   - 共有ブランチでの使用は要注意

### 使い分けの指針

**マージを使う場合：**
- 共有ブランチ（main等）への統合時
- チーム開発での作業統合時
- 履歴の正確な保持が必要な場合
- 安全性を重視する場合

**リベースを使う場合：**
- 個人の作業ブランチの更新時
- フィーチャーブランチの整理
- きれいな履歴を維持したい場合
- まだプッシュしていない変更の整理

### 重要な注意点

1. **ゴールデンルール**
   - プッシュ済みのコミットはリベースしない
   - 共有ブランチではマージを使用
   - 個人ブランチではリベース可

2. **リベースの危険性**
   ```bash
   # これは危険（共有ブランチ）
   git switch main
   git rebase feature/branch
   ```
   - 共有履歴の書き換えは混乱の元
   - チームメンバーの作業に影響

3. **推奨フロー**
   ```bash
   # 作業ブランチの更新（OK）
   git switch feature/branch
   git rebase main
   
   # mainへの統合（推奨）
   git switch main
   git merge feature/branch
   ```

### まとめ

1. **使い分けの基本**
   - 共有・公開ブランチ → マージ
   - 個人作業ブランチ → リベース

2. **選択の基準**
   - 履歴の重要性
   - チーム開発の有無
   - 変更の公開状況
   - 運用ルール

3. **ベストプラクティス**
   - チームでルールを統一
   - 履歴の重要性を考慮
   - 安全性を優先
   - 状況に応じて使い分け

## タグについて
Gitのタグについて説明します：

### タグの種類

1. **軽量タグ（Lightweight）**
```bash
# 作成
git tag v1.0.0

# 特定のコミットにタグ付け
git tag v1.0.1 <commit-hash>
```
- 単純なポインター
- コミットへの参照のみ
- 追加情報なし

2. **注釈付きタグ（Annotated）**
```bash
# 作成（推奨）
git tag -a v1.0.0 -m "リリースバージョン1.0.0"

# 特定のコミットにタグ付け
git tag -a v1.0.1 -m "バグ修正リリース" <commit-hash>
```
- タグ付けした人の情報
- 日時情報
- メッセージ付き
- GPG署名可能（-s オプション）

### タグの管理

1. **タグの一覧表示**
```bash
# 全てのタグを表示
git tag

# パターンでフィルタ
git tag -l "v1.0.*"

# タグの詳細情報を表示
git show v1.0.0
```

2. **タグのプッシュ**
```bash
# 特定のタグをプッシュ
git push origin v1.0.0

# 全てのタグをプッシュ
git push origin --tags
```

3. **タグの削除**
```bash
# ローカルタグの削除
git tag -d v1.0.0

# リモートタグの削除
git push origin --delete v1.0.0
```

### タグの使用例

1. **リリースバージョン管理**
```bash
# メジャーリリース
git tag -a v1.0.0 -m "First stable release"

# マイナーリリース
git tag -a v1.1.0 -m "New features added"

# パッチリリース
git tag -a v1.1.1 -m "Bug fixes"
```

2. **特定バージョンのチェックアウト**
```bash
# タグの状態にチェックアウト
git checkout v1.0.0

# タグからブランチを作成
git checkout -b hotfix/bug v1.0.0
```

### タグの命名規則

1. **セマンティックバージョニング（推奨）**
- `v1.0.0`（メジャー.マイナー.パッチ）
- `v2.1.3`
- `v0.9.0-beta`

2. **リリースタイプ**
- `v1.0.0-alpha`
- `v1.0.0-beta`
- `v1.0.0-rc.1`（リリース候補）

### ベストプラクティス

1. **タグ付けのタイミング**
- リリース時
- 重要なマイルストーン
- 安定版の公開時

2. **注釈付きタグの使用**
- 重要なリリースには注釈付きタグを使用
- 詳細な説明を含める
- 変更履歴への参照を含める

3. **命名規則の統一**
- プロジェクト内で一貫した命名規則
- セマンティックバージョニングの採用
- プレフィックス（v）の一貫した使用

4. **ドキュメント化**
- タグの意味を文書化
- リリースノートとの連携
- 重要な変更点の記録

### 活用シーン

1. **リリース管理**
- 製品リリースの記録
- バージョン管理
- リリースの追跡

2. **デプロイメント**
- 特定バージョンのデプロイ
- ロールバック用のポイント
- 環境別のデプロイ管理

3. **バグ修正**
- 問題発生時の状態記録
- 修正前の状態参照
- 検証環境の再現

## stashについて
Gitのstashについて説明します：

### 基本的な使い方

1. **変更を一時保存**
```bash
# 基本的なstash
git stash

# メッセージ付きでstash
git stash save "作業中の機能A"

# 未追跡（untracked）ファイルも含める
git stash -u

# 全てのファイル（.gitignoreされたものも）を含める
git stash -a
```

2. **stashの確認**
```bash
# stashリストの表示
git stash list

# 特定のstashの詳細表示
git stash show stash@{0}

# 特定のstashの変更内容を詳細表示
git stash show -p stash@{0}
```

3. **stashの復元**
```bash
# 最新のstashを適用（stashは保持）
git stash apply

# 特定のstashを適用
git stash apply stash@{1}

# 最新のstashを適用して削除
git stash pop

# 特定のstashを適用して削除
git stash pop stash@{1}
```

4. **stashの削除**
```bash
# 特定のstashを削除
git stash drop stash@{0}

# 全てのstashを削除
git stash clear
```

### 活用シーン

1. **急な作業の切り替え**
```bash
# 現在の作業を保存
git stash save "機能A作業中"

# 別のブランチに切り替え
git switch hotfix/urgent-bug

# 緊急作業後、元の作業を復元
git switch feature/function-a
git stash pop
```

2. **ブランチの切り替え時**
```bash
# コミットしたくない変更を保存
git stash

# ブランチを切り替え
git switch other-branch

# 作業後、元のブランチに戻って復元
git switch original-branch
git stash pop
```

3. **コンフリクト解決時**
```bash
# 現在の変更を保存
git stash

# mainの変更を取り込む
git pull origin main

# 保存した変更を戻してコンフリクト解決
git stash pop
```

### 高度な使い方

1. **部分的なstash**
```bash
# 特定のファイルだけをstash
git stash push -m "特定ファイルの変更" path/to/file

# インタラクティブにstashする変更を選択
git stash -p
```

2. **stashからブランチを作成**
```bash
# stashを新しいブランチとして展開
git stash branch new-feature stash@{0}
```

3. **stashの変更確認**
```bash
# stashとの差分を表示
git diff stash@{0}
```

### ベストプラクティス

1. **命名規則**
- 分かりやすい説明を付ける
- 作業内容を明確に
- 日付や機能名を含める
```bash
git stash save "2024-04-25 ログイン機能 バリデーション追加中"
```

2. **管理のコツ**
- stashは短期的な使用を推奨
- 長期保存は避ける
- 定期的にクリーンアップ
- 重要な変更は適切にコミット

3. **安全な使用**
- 重要な変更は必ずコミット
- stash前に状態を確認
- 複数のstashは混乱の元
- 定期的にリストを確認

### 注意点

1. **stashの制限**
- ブランチをまたいで使用可能
- マージコンフリクトの可能性
- 長期保存は非推奨

2. **一般的な問題を避ける**
- 多すぎるstashを作らない
- 説明なしのstashを避ける
- 長期間の保存を避ける
- 重要な変更はコミット

3. **トラブルシューティング**
```bash
# stashの復元に失敗した場合
git stash show -p stash@{0} | git apply -R

# コンフリクト発生時
git status  # 状態確認
git diff    # 差分確認
```

### まとめ

stashは：
- 一時的な変更の保存に最適
- ブランチ切り替え時の作業保存
- 緊急作業対応時の退避
- 短期的な変更管理に使用

ただし：
- 長期保存は避ける
- 適切な説明を付ける
- 定期的な整理が必要
- コミットすべき変更との使い分けを意識
