# GitとGitHubの設定とかについて
## git configの設定
- ユーザーネームとメールアドレスの設定のために以下を実行（これをしないとMacOSのユーザーネーム・コンピュータ名がGitHub上で見えてしまう）
    - `$ git config --global user.name githubアカウント名`
    - `$ git config --global user.email 数字列+GitHubアカウント名@users.noreply.github.com`
        - メールアドレス`数字列+GitHubアカウント名@users.noreply.github.com`はGitHubプロフィールページ>Settings>Emailsから見れるのでコピペ（`Keep my email addresses private`にチェックがついている必要がある）
        - `Block command line pushes that expose my email`にチェックしておくとメールアドレスがバレるのを防いでくれる
## sshキーの設定
- [sshに関する公式資料](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/about-ssh)を読んで順にやっていけばOK
    - ハマったところ
        - パスフレーズは自由に決めてOK
## gpg（コミット署名）の設定
- [gpgに関する公式資料](https://docs.github.com/ja/authentication/managing-commit-signature-verification/about-commit-signature-verification)を読んで順にやっていけばOK
    - ハマったところ
        - HomeBrewでgpgをインストール
            - その後[GPG Suite](https://gpgtools.org/)も先にインストールしておくと良い（後ほど入れさせられるので）
        - キーの種類はよくわからないのでとりあえずRSA and RSAにしたらいけた
        - gpgキー貼り付けの際には`-----BEGIN PGP PUBLIC KEY BLOCK-----`と`-----END PGP PUBLIC KEY BLOCK-----`も含めてコピペ
        - `adduid`するときに`Comment: GitHub key`を忘れずに
        - 公式の手順通りにやっていくとgpgキーは2回アップロードするがそれで正解
    - なんかいけないとき
        - `$ gpgconf --kill all`でgpgを再起動するといけたりする
    - Q.uidが二種類（CommentがGitHub keyのuidと空のuid）あるけどいるの？Commentが空のuidはいらないのでは？
        - A.両方要る
        - 試しにCommentが空のuidを消してみたら署名付きコミット時にエラー（以下の手順で回復）
            - `$ gpg --edit-key 3AA5C34371567BD2`
            - `$ gpg> adduid`
            - `Comment`は何も入力せずEnter
            - `O`を入力して`$ gpg> save`
## コミットメッセージを書くエディタを変更
- vimに設定`$ git config --global core.editor 'vim -c "set fenc=utf-8"'`
    - `-c "set fenc=utf-8"`は文字化け防止
- vscodeに設定`$ git config --global core.editor 'code --wait'`
## pushを取り消して完全になかったことにする方法
- ローカルで`$ git git reset --hard HEAD~`を実行しコミットを1つ前に戻す
- `$ git push -f origin main`で強制的にpush
