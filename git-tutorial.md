# git tutorial
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
$ git remote
origin

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
