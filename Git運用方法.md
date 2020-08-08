# Git運用方法
[Gitの運用方法（初心者向け） - Qiita](https://qiita.com/kussea/items/f402cb18871390274f92)
#git運用方法
## 個人開発編
『手順の流れ』 ※vagrant 接続状態だとうまくいかない可能あり
::**〜場面〜ローカルにリポジトリを作成し、リモートリポジトリにプッシュする**::
* Step①：現在いるディレクトリをgitで管理するための設定ファイルを生成（Gitの初期化）
`$ git init  `
         
* Step②：[file]の部分にファイル名を指定し、ステージングエリアに追加（基本は -A, —all など使用する）（全部あげたい時は 「. 」や「*」を使用 ）
`$ git add [file] `

* Step③：メッセージをつけて変更履歴を保存
`$ git commit -m “コメント"`
　　　　　　  　  
* Step④：originという名前でリモートリポジトリのURLを登録
`$ git remote add origin [リモートリポジトリのurl] `

* Step⑤：ローカルリポジトリの変更をリモートリポジトリにアップロードする。[branchname]にアップロードしたいブランチの名前を指定
`$ git push origin [branch名]`（pushの後 -u を入れると今後、[git push] だけでGitHubにあげられる）


::**〜場面〜ローカルリポジトリの変更をリモートにプッシュする**::
*  Step①：[file]の部分にファイル名を指定し、ステージングエリアに追加（基本は -A, —all など使用する）（全部あげたい時は 「. 」や「*」を使用 ）
` $ git add [file]`                           

* Step②：メッセージをつけて変更履歴を保存する。
`$ git commit -m “コメント"`　　　　　　　　　　　  　
  
* Step③：ローカルリポジトリの変更をリモートリポジトリにアップロードする。[branchname]にアップロードしたいブランチの名前を指定
`$ git push origin [branch名]`               





## チーム開発編
### TeamOrganizationの機能でソース管理
メンバーの設定はAdmin、Writeでも問題は起こらない
ただマスターブランチのマージはAdminでないとできない（未確認だがAdminでないとできない機能はいくつかあるので、全員Adminの方が楽）
### issueの活用
基本的に作業メッセージをここに記載しておく（コミットメッセージだけでは管理ができないため）
::Gitの流れ（個人的なやり方、下記の作業繰り返す!!!!!）::
* Step①：マスターにいる状態で最新版をダウンロード
`$ git fetch`

* Step②：ローカルのマスターファイルを最新にする
`$git merge origin/master`

* Step③：最新のマスターファイルからブランチ作成&移動
`$ git checkout -b [branch名]`

〜〜〜作業〜〜〜

* Step④：作業完了後、ブランチファイルをローカルにあげる（以下作業フローは同じ）
`$ git add -A`
`$ git commit -m “コメント"`
`$ git push origin [branch名]  `

* Step⑤：GitHub上で以下の作業を実施
[Compare&pull request]  にプルリクを記載 
[Merge pull request] でマージを確定させる
       

::**〜場面〜他の人のアプリをもらうとき**::
* Step①：リモートリポジトリをローカルリポジトリに複製する(ダウンロード)　（urlは複製したいリモート先のurl）
`$ git clone [url] `


::**〜場面〜ログを確認するとき**::
* コミットの履歴を確認する
`$ git log`

* コマンド$ git reset [commit] ・HEAD^ で一つ前・コミットの識別番号で指定も可能
`$ git revert`

* ファイルの追加_変更_削除の確認
`$ git status`


::**〜場面〜ブランチ操作**::
* ブランチの一覧を表示する
`$ git branch`

* ブランチを作成する [branchname]の部分に、作成するブランチの名前を指定します
`$ git branch [branch名]`

* ブランチ名を指定して切り替える
`$ git checkout [branch名]`

* ブランチの作成と切り替えをまとめて実行できるコマンド
`$ git checkout -b [branch名]`

* ブランチの削除
`$ git branch -d [branch名]`（マージ済みブランチの削除）
`$ git branch -D [branch名]`（強制的なブランチの削除）


::**〜場面〜新しい作業を始めるとき**::
* Step①：まずリモートの最新の情報をローカルに取得する
`$ git fetch`

* Step②：どのブランチから切るか確認する（ローカルとリモートのブランチを確認)
`$ git branch -a`

* Step③：②で確認したブランチから、新しくブランチを切る
`$ git checkout -b [新たなブランチ名] [切る元のブランチ]`

* Step④：念のためにブランチができたか確認
`$ git branch`


::**〜場面〜任された作業が終わってリモートにあげる時**::
* Step①：push前までの状態にする
`$ git status`（変更のあるファイルを確認する）
`$ git add -A`（自分が更新した分だけステージングエリアにあげる）
`$ git commit -m “コミットメッセージ”`

* Step②：pushする前に差分の確認→いざpush
`$ git push origin [あげる先のブランチ名]`（現在いるローカルのブランチ名を入れる。初回だと新しくリモートにそのブランチを作成）

* Step③：プルリクエスト編集(New pull request)
[Compare&pull request]  にプルリクを記載 

* Step④：リモート先にあげたブランチをマージする
[Merge pull request] でマージを確定させる


::**〜場面〜他の人が更新した内容をローカルに取り込む時**::
* Step①：ローカルのリモートの情報を更新する
`$ git fetch`（この段階では自分のローカルリポジトリにある状態）
`$ git merge origin/master`（ここで自分のローカルファイルにリポジトリにあるファイルをマージする）
あまり使わないがいちおう覚えておく：（$ git fetch）＋（$ git merge origin/master）＝　（$ git pull origin master）


::**〜場面〜他の人が更新した内容をローカルに取り込んでコンフリクトした時**::
* $ git status でコンフリクトしたファイルを確認（いくつか方法をメンバーと相談して決める）方法は下記に記載
* 対策案①：「一番早くて安全（これがおすすめ）」VScodeで自分のコンフリクトしたファイルを戻す
* 対策案②：「多少不安要素があるがほぼ問題なし（責任は取れない）」自分でpush してGithubでマージ後に（$ git fetch）＋（$ git merge origin/master）する
* 対策案③：「できればやりたくない（ブランチ作りから始まる）」ファイル内にある（.git）ファイルを消すために（rm -rf .git）コマンド実行後リポジトリを再構築
* 対策案④：「最終手段（自分の書いたコードを失う）」ファイル内にある（.git）ファイルを消すために（rm -rf .git）コマンド実行後クローンを実行


::参考リンク::
* [git初学者の初めてのチーム開発で気をつける事の備忘録 - Qiita](https://qiita.com/shh-nkmr/items/fde133cbdfa5f0092be1)
* [【GitHub】Pull Requestの手順 - Qiita](https://qiita.com/aipacommander/items/d61d21988a36a4d0e58b)
* [【Git】git add: ステージする(索引に追加)コマンドとオプション | 雪の天秤](http://yukiten.com/blog/info-tec/git-git-add/#content_1_11)
* [【永久保存版】Gitのあらゆるトラブルが解決する神ノウハウ集を翻訳した - LABOT 機械学習ブログ](https://blog.labot.jp/entry/2019/07/01/183204)
* [新しいMacでGitHubのSSH接続をするまでの環境構築手順 - Qiita](https://qiita.com/unsoluble_sugar/items/14bea376d8e6fce82eb3)
* [macOSアップデート後の『xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools)…』の対処法 - Qiita](https://qiita.com/nishina555/items/e23d73067a5cac182a63)


* 補足
`$ cd`
`$ ls -ld .ssh`
→[ls -l]コマンドの1文字目にはファイルの種類をが表示されるよ
 [ls -ld .ssh]の場合はディレクトリを示すよ
`$ ls -la .ssh`
→[ls -la .ssh]は-aオプションを使うと、ファイル名の先頭にピリオドがある(要するに隠しファイル)ファイルも表示するよ
→id_rsa (秘密鍵=クライアントPCに配置)、id_rsa.pub(公開鍵=接続対象サーバに登録)が表示されるよ

::**Permission deniedの場合**::
`$ mkdir .ssh`(まずディレクトリがないなら作る)
`$ chmod 700 .ssh` →意味：所有者は全てOK、グループと他人は何も権限なし
   →chmod(change mode)は、ファイルやディレクトリの権限を変更するコマンド。700は、変更する権限を数値にしたもの
   →chmod[オプション][アクセス権][対象のファイルやディレクトリ]
`$ ssh-keygen`(認証や暗号化に使う鍵を生成、管理するコマンド)
　→ssh-keygen[オプション]
　  　
※ファイルのアクセス権などを決定するのが、[パーミッション(許可属性)]になります
   パーミッションには「読み出し許可(r=4)」「書き込み許可(w=2)」「実行許可(x=1)」の3種類があります (-)は実行許可なし
　読み出し許可のないファイルの中身を見ようとした場合は、「Permission denied（許可されていない）」というエラーが表示されるよ 
　従って、コマンドも許可が与えられて、初めて実行可能になる
 ※参考例　
   “3組”が意味するのは「所有者」「グループ」「他人（所有者でも所有グループでもない人）」で、パーミッションが「rwxr-x—」のとき
　所有者は読み書き実行が全て許可（rwx）、グループは読み出しと実行が許可（r-x）、それ以外の人は全ての操作が一切不可（—）をさす


* デバック：`$ ssh -vT git@github.com`
  →keygenrateでパスフレーズをイジってはいけないんだよ(結局はココが原因)
[【 ssh 】コマンド――リモートマシンにログインしてコマンドを実行する：Linux基本コマンドTips（80） - ＠IT](https://www.atmarkit.co.jp/ait/articles/1701/26/news015.html)
　　  　　　　　　　　　　　　　　　　　
　　  　　　　　　　　　　　　　　　　　　　
* `$ git init` を入力→.git 隠しファイルが生成される→正確にはリポジトリの初期化
`$ls -la`で確認する必要があり
もしgitが邪魔していたらその隠しファイルを消す

::**Mac OS  CatalinaインストールでGitが消滅したの場合**::
* Homebrew経由でインストールする
`$ brew install git`
エラーが発生「xcrun: error: invalid active developer path〜」
`$ xcode-select --install`
* gitのバージョン番号が表示されればOK
`$ git —version`
* Githubに接続するための.sshのディレクトリ
`$ ls -ld .ssh`
`$ ls -la .ssh`
* 秘密キーをGithubにコピーする
`$ pbcopy < ~/.ssh/id_rsa.pub`
* Githubに接続する
`$ ssh git@github.com`




