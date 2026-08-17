# Git training

V1.0（作成）

#1. ファイルの管理

Aさんは、マイクラを楽しんでいる。
↓
その夜、いい建造物のアイデアが思いついたと思い、ワールドに大幅な建造物を作る
↓
しかし翌朝起きてみたら、やっぱ深夜テンションもあったからこの建造物はないほうが良いと思い、元に戻そうと思った
↓
でも、バックアップデータもなく、復元することができなかった。
↓
詰み

※バックアップ（今更）：元のデータを予め複製しておくこと

バージョンの命名規則

V 1.2.3 の場合、

V1(Major version).2(Minor version).3(Bug fix)

#2. バージョン管理システム（Version Control System）

Version Control System：
ファイルのバックアップ、バージョンと変更履歴の管理、ユーザー権限管理を行う
→ 一般的に単一のファイルではなく指定のディレクトリ内のファイル群を一括で管理

できること
・ファイルのバックアップ作成
・前のバージョンに戻す
・バージョンと変更履歴を共有する
・ファイルを共有する

#3. バージョン管理システム：分散型について（Git）

Git が分散型に該当

・サーバー上のリモートリポジトリ / クライアントPCのローカルリポジトリ
・クライアントPC上にRepositories を複製して、ファイルを編集
・Local repositories に対して commit を行い、後でまとめて server 上に反映する

Advantages
→ クライアントPC内でバージョン管理機能が動作するため、
サーバーに接続していない環境でも commit が可能
書きかけの文章で commit してもほかの人に影響が出ない

Disadvantages
→ 学習むずい
→ Repositories の情報が誤って public になってしまうケース…

#4. Making Git Repositories

（略）

#5. Git の操作（individual）

## 3 つの領域

Git Repositories の中には３つの領域が存在

1. 作業ツリー：ファイルの変更作業をする場所
2. インデックス：コミット前にコミット対象ファイルを指定する場所
3. リポジトリ：コミットされたファイルと変更履歴を保管・管理する場所

現在の作業場所の状態を調べる

git status を使って確認しよう

```bash
git status
```

Repositories に何も変更がない

```bash
working tree clean → 作業ツリーには何も変更がない
```

Repositories にファイルの変更がある状態

modified: aaa.md → aaa.md にファイルの変更がある

ステージング

変更したファイルを作業ツリーからインデックスに登録する

コミット対象のファイルと非対象のものを仕分け（すべてステージングしたかったらすればいいだけ）

ステージングの操作

```bash
git add <ステージングしたいファイルのパス>
```

作業ツリーの変更をすべてステージングする場合は、ファイルパスに . を指定

```bash
git add .
```

といわれども、VSCode上では、ソース管理から＋マーク押したりしてわざわざこんなことしなくても
ステージングできるよね

ステージング後に git status で現状の状況を確認すると、

Changes not staged for commit
↓
Changes to be committed

ステージングされているファイル

```bash
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   ファイル名
        new file:   ファイル名
        deleted:    ファイル名
```

ステージングされていないファイル（作業ツリーで変更されているファイル）

```bash
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   ファイル名
        deleted:    ファイル名
```

new file：新規追加したファイル、インデックスにステージングされて初めてこの状態に
→ ステージングされてないファイルは untracked の状態

modified：前回のコミットから内容が変更されたファイル

deleted：前回のコミットから削除されたファイル

renamed：前回のコミットから名前が変更されたファイル
untracked：Git の管理対象外、作業ツリーで新規追加したファイルはこの状態に

commit

リポジトリ内のインデックス（ステージングエリア）にあるファイルをリポジトリに保存する操作のこと

変更者はコミットメッセージを記述可能
コミット時にられる ID は連番にならない（ID として、SHA-1 と呼ばれるハッシュ値が自動生成）

git commit コマンドでコミットを行う

```bash
git commit -m "コミットメッセージ"
```

ただこれしなくても VS のソース管理でできるくない？

Git 操作のやり直し

ファイルに変更を加えたが、最初からやり直したいとき（前回のコミットの状態に戻したいとき）

```bash
git restore コマンドで変更を取り消す
```
```bash
git restore ファイルパス
```

すべてのファイルの変更を取り消す場合にはファイルパスに . を指定
```bash
git restore .
```
Untracked のファイルを削除する場合は git clean コマンド
```bash
git clean -f
```
ステージングのやり直し

commit しないはずのファイルをステージングしたとき

```bash
git reset HEAD ファイルパス
```
すべてのファイルのステージングを取り消す場合
```bash
git reset HEAD
```

commit のやり直し

reset / revert

reset：commit の「取り消し」
commit 自体をなかったことに！
soft / hard / mixed ―― 3種類

Ex：

commit A，B，C ←削除

revert：commit の「打ち消し」
特定の commit を打ち消すための差分を作成し、変更としてコミットする
コミットは削除されないため他の作業者への影響が発生しづらい

Ex：

commit A，B，C を打ち消すための → D

reset の操作

soft reset：コミットのみリセットし、変更をインデックスに残す

```bash
git reset --soft 戻したい version の SHA-1
```

mixed reset：コミットをリセットし、変更を作業ツリーに残す

```bash
git reset --mixed 戻したい version の SHA-1
```
```bash
hard reset：作業ツリーの状態とインデックスの状態を含めすべてリセットする
```
```bash
git reset --hard 戻したい version の SHA-1
```
revert の操作
特定のコミットの変更を打ち消す
```bash
git revert 打ち消したい version の SHA-1
```

直近のコミットの変更を打ち消す
```bash
git revert HEAD
```
特定の範囲のコミットの変更を打ち消す
```bash
git revert 打ち消したいversion の SHA-1...打ち消したいversion の SHA-1 の終点
```
リモートリポジトリにプッシュ後にコミットをやり直す

プッシュ後にコミットを取り消す場合は revert を使う

バージョン毎の比較

作業ツリー上のファイルを前回コミットと比較する
```bash
git diff ファイル名
```
インデックス上のファイルの前回コミットと比較する
```bash
git diff --cached ファイル名
```
コミット同士を比較する
```bash
git diff コミットの参照...コミットの参照 ファイル名
```
プッシュ
ローカルリポジトリの内容を、リモートリポジトリに反映する操作

リモートリポジトリに反映することで、他の人に変更部分を共有することができる

main ブランチをリモートリポジトリにプッシュする
```bash
git push origin main
```
他のブランチをリモートリポジトリにプッシュする
```bash
git push origin ブランチ名
```