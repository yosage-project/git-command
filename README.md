# git-command

Gitでよく使うコマンドの解説。


## Gitで管理するエリアについて

コマンド類を知る前にまずはGitで管理されるエリアについて、確認していきましょう。

1. ワーキングディレクトリ(ワーキングツリー)
2. ステージングエリア
3. ローカルリポジトリ
4. リモートリポジトリ(GitHubなど)


### 1. ワーキングディレクトリ

読んでそのまま実際に作業を行うための場所です。  
VisualStudioCodeなどのエディタを使って、コード修正を行う際に開くディレクトリです。

ワーキングディレクトリ内のファイルがGitでの管理対象となります。


### 2. ステージングエリア

コミットしてローカルリポジトリにいれるまでの中間領域です。  
ワーキングディレクトリで編集したファイルを`git add`コマンドを使って、この領域に追加します。


### 3. ローカルリポジトリ

コミットと呼ばれる作業ログを記録し、差分を管理するための管理台帳です。  
ステージングエリアに追加されているファイル群を`git commit`コマンドを使って追加します。


### 4. リモートリポジトリ

ローカルで完結している場合は、この領域を使うことはありません。  
しかし、多くの開発現場においてリポジトリをGitHubなどのサービスに保存します。

ローカルリポジトリにコミットされた作業ログを`git push`コマンドによってアップロードします。


## よく使うコマンド類

### git init

Gitの初期化を行い、ローカルリポジトリを作成するコマンドです。  
実行したディレクトリに隠しディレクトリ`.git`を作成します。

Gitの作業ログなどはこの`.git`内のファイルに書き込まれていきます。

逆を言えばGitの管理をやめたければ、`.git`を削除すれば良いです。

リモートリポジトリがまだ作成されておらず、ローカルで先行してリポジトリを作成して作業する時などに利用します。

だいたいリモートリポジトリが先行して存在することの多そうなので、あまり日の目を見ないかも？


### git clone

GitHubなどで管理されているリモートリポジトリをクローンし、ローカルリポジトリを作成するコマンドです。

#### コマンド

```
git clone リポジトリURL
```


### git branch

ローカルリポジトリ内にブランチと呼ばれる、作業領域を作り出すコマンドです。

#### コマンド

```
git branch ブランチ名
```

ブランチは現在作業中のブランチを起点に新規で作成されます。  
またブランチを作成するコマンドであるので、ブランチを切り替えるには後述の`git checkout`または`git switch`の実行が必要です。


### git checkout, git switch

ワーキングディレクトリを作成済みのブランチに切り替えるコマンドです。  
`git checkout`と`git switch`の2通りの方法がありますが、ブランチを切り替えるという点においては同じです。

#### コマンド

```
git checkout ブランチ名
```
```
git switch ブランチ名
```

また、存在しないブランチを作成しつつブランチを切り替えるには、以下のコマンドが利用してください。

```
git checkout -b ブランチ名
```
```
git switch -c ブランチ名
```


### git add

ワーキングディレクトリにて作業したファイルをステージングエリアに追加します。

後述の`git commit`はこのステージングエリアに追加されているファイルが対象となるので、以下に気をつける必要があります。

- 必要なファイルを含めておくこと
- 不必要なファイルを含めないこと

#### コマンド

ファイル単体を追加する場合
```
git add ファイルパス
```

ディレクトリを追加する場合
```
git add ディレクトリパス
```

誤ってステージングに追加してしまった場合を取り消ししたい場合は、`git reset`を利用します。  
こちらは後述。


### git commit

ステージングエリアに追加されているファイル群を作業ログとして、ローカルリポジトリに記録します。  
コミットにはコミットメッセージが必須となります。

#### コマンド

```
git commit -m "コミットメッセージ"
```

ちなみに`git commit`のみを実行すると、コミットメッセージ入力用のエディタが開きます。

コミットを取り消したい場合は、`git reset`または`git revert`を利用します。  
こちらは後述。


### git push

ローカルリポジトリにコミットされている作業ログを、リモートリポジトリにアップロードします。

アップロードはブランチ単位となるため、現在のブランチ(カレントブランチ)がアップロード対象となります。

#### コマンド

カレントブランチをアップロードする場合
```
git push
```

はじめて`git push`するブランチをアップロードする場合
```
git push -u origin ブランチ名
```
※`origin`はリモートブランチを指す。


### git pull

リモートリポジトリからローカルリポジトリに作業ログをダウンロードし、カレントリポジトリの内容を最新に更新します。

#### コマンド

```
git pull
```


### git fetch

リモートリポジトリからローカルリポジトリに作業ログをダウンロードします。  
`git pull`と異なり作業ログのダウンロードを行うのみで、カレントディレクトリの更新は行いません。

#### コマンド

```
git fetch
```

ベースブランチに変更がないか(あればマージが必要になる可能性がある)を確認するなどの用途に使ったりできます。


### git merge

ブランチは隔絶された作業領域なので、ブランチ間の作業内容はマージを行って取り込むことになります。

ベースブランチから派生させた作業ブランチにて作業を行い、その作業ブランチでの作業ログをベースブランチにも取り込む際に利用します。

#### コマンド

`main`ブランチから派生させた`task`ブランチの内容を取り込む場合(以下コマンドは`main`ブランチで実行)
```
git merge task
```

`main`ブランチへの取り込みなどはプルリクエストなどを使って行われることも多いです。


### git reset

Gitを利用した作業中に発生する以下のような事象を取り消します。

- ステージングエリアへの追加の取り消し
- コミットの取り消し

用途に合わせて、

- `git reset --soft`(コミットの取り消し)
- `git reset --mixed`(コミット及びステージングエリアへの追加の取り消し)
- `git reset --hard`(コミット、ステージングエリアへの追加、ワーキングディレクトリの変更の取り消し)

があります。  

#### コマンド

直前のコミットを取り消す場合
```
git reset --soft HEAD^
```
※`HEAD^`は直前のコミットを指す。

直前のコミットとステージングエリアへの追加を取り消す場合
```
git reset --mixed HEAD^
```
※`git reset`のデフォルトが`--mixed`なので、省略可。

直前のコミットとステージングエリアへの追加、ワーキングディレクトリへの変更を取り消す場合
```
git reset --hard HEAD^
```
※ログも何も残らないので不可逆なので注意。


### git revert

`git reset --hard`と同じく、コミットの取り消し～ワーキングディレクトリの変更の取り消しを行います。  
取り消し用の逆コミットを作成して相殺を行うので、コミット履歴に残ります。

#### コマンド

直前のコミットをリバートする場合
```
git revert
```

コミット番号を指定してリバートする場合
```
git revert コミット番号
```
※古いコミットをリバートしようとした場合、後続のコミットログとコンフリクトを起こす場合があるので注意。


### git status

以下を確認することができます。

- ワーキングディレクトリでの追加・変更したファイル
- ステージングエリアに追加されているファイル

コミット前に必要なファイルがステージングエリアに追加されているかを確認するなどに利用します。


### git stash

ステージングエリアに追加されている変更を一時的に退避します。  
退避した変更内容は`git stash apply`または`git stash pop`で戻すことができます。

変更前の内容を確認したい場合や、ベースブランチの変更内容を取り込む際にステージングエリアを一時的に空にしたい場合などに利用します。


### git cherry-pick

カレントブランチとは異なるブランチにコミットされている内容を、カレントブランチに取り込みます。

まだベースブランチにマージされていないが、実装において必要なコミットが他ブランチに存在している場合など。  
部分的に実装内容を取り込むことができます。

#### コマンド

```
git cherry-pick コミット番号
```


### git rebase

- 他ブランチのコミットをカレントブランチに付け加える
- 複数のコミットを1つのコミットとしてまとめる

の2通りで利用します。

前者の場合はマージと同等の目的。後者の場合はコミット履歴の掃除が目的となることが多いです。

#### コマンド

他ブランチのコミットをカレントブランチに取り込む場合
```
git rebase ブランチ名
```
※カレントブランチのコミット履歴に`task1`ブランチのコミットが追加される。

複数のコミットを1つコミットとしてまとめる場合
```
git rebase -i 直前コミットより前のコミット番号
```
※実行後エディタで直前のコミットから指定したコミットまでの範囲で、まとめるコミットを選択します。