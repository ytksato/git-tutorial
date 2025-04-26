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