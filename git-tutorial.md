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
$ git checkout remotes/origin/main
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