---
layout: post
title: "OctopressとGitHub Pagesでブログを始めてみた"
date: 2013-08-05 16:42
comments: true
categories: miscellaneous
---

これまではてなダイアリーを使ってきましたが、  
octopressを知って面白そうだったので導入してみました。  
「A blogging framework for hackers.」  
とか言われたら使うしかないですよね。  
導入も簡単だし、markdownで記事書けるし楽しそうなので、  
しばらく使ってみます。  

***

# 参考にさせていただいたサイト
[OctopressとGitHub Pagesを使用したブログの構築手順](http://onigra.github.io/blog/2013/04/28/introduction-of-octopress/)

[Octopress で GitHub Pages を置き換える](http://randd.kwappa.net/2013/04/16/521)

***

# 環境
* Ubuntu 13.04
* Ruby 1.9.3

***

# Octpressの設定

## Octopressをfork
Octopressをダウンロードする前に、自分のリポジトリにforkしておくのをお勧めします。  
いきなりcloneしてもいいのですが、forkしておけば変更後そのままpushすることで、  
GitHub上でソースを管理することができます。  
[こちら](https://github.com/imathis/octopress)からforkしましょう。  

##Octopressをダウンロード
cloneするだけです。  
```bash Octopressのダウンロード
$ git clone git@github.com:sudix/octopress.git
$ cd octopress
$ echo "1.9.3-debian" | cat > .rbenv-version #rewrite for ubuntu's ruby version
$ bundle install --path vendor/bundle
$ bundle exec rake install
```
  
## 初期設定
rubyのバージョン設定と、必要なgemのインストールを実行します。  
rubyのバージョン設定は自分の環境に合わせて.rbenv-versionを書き換える事になります。  
自分の環境だとubuntuのパッケージから入れたので「1.9.3-debian」となります。  
これを設定しておかないと動きません。  
また、gemのインストールはbundlerを使って、vendor/bundle以下にインストールしています。  
参考：[http://octopress.org/docs/setup/](http://octopress.org/docs/setup/)  
```bash Octopressの初期設定
$ cd octopress
$ echo "1.9.3-debian" > .rbenv-version
$ sudo gem install bundler
$ bundle install --path vendor/bundle
$ bundle exec rake install
```
  
## プレビュー
以下のコマンドを実行します。
```bash
bundle exec rake preview
```
これで次のURLでプレビューできるようになります。  
[http://localhost:4000/](http://localhost:4000/)

## テーマ
テーマの設定が可能です。以下からお好みのテーマを探してみてください。  
[3rd party themes](https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes)

今回はこのテーマを使ってみます。  
[greyshade](https://github.com/shashankmehta/greyshade)

```bash テーマのインストール
$ git clone git@github.com:shashankmehta/greyshade.git .themes/greyshade
$ echo "\$greyshade: #073642;" >> sass/custom/_colors.scss //Substitue 'color' with your highlight color
$ bundle exec rake "install[greyshade]"
$ bundle exec rake generate
$ bundle exec rake preview
```

## コンフィグ
参考:[http://octopress.org/docs/configuring/](http://octopress.org/docs/configuring/)  
_config.ymlを編集し、各種設定を行います。変更した箇所だけを抜粋しています。  
```yaml _config.yml
url: http://sudix.github.io
title: Sudix Blog
subtitle: my blog about programming, IT, etc...
author: sudix
description: about programming, IT, etc...
date_format: "%Y/%m/%d"
email: "youremailaddress@example.com"
```

***

#Githubの設定

## create repository
手順:[http://octopress.org/docs/deploying/github/](http://octopress.org/docs/deploying/github/)

Github pages用のリポジトリを作っておく必要があります。  
「自分のユーザー名.github.io」という名前のリポジトリになります。  
自分の場合だと「sudix.github.io」ですね。  
octopressの上記手順で説明のある.comではなく.ioで作る必要があるようです。  
昔と変わったのでしょうか。  
[Github Pagesのマニュアル](https://help.github.com/articles/user-organization-and-project-pages)

リポジトリ作成はこちらから。[https://github.com/new](https://github.com/new)

***


# デプロイ＆公開
いよいよ公開してみます。

## リポジトリの登録
rake setup_github_pagesを実行するとRepository urlを入力しろと聞かれるので、  
先ほど作ったリポジトリ（git@github.com:sudix/sudix.github.io）を入力します。

```bash リポジトリの登録
$ bundle exec rake setup_github_pages
Enter the read/write url for your repository
(For example, 'git@github.com:your_username/your_username.github.io)
           or 'https://github.com/your_username/your_username.github.io')
Repository url: git@github.com:sudix/sudix.github.io
rm -rf _deploy
mkdir _deploy
cd _deploy
Initialized empty Git repository in /home/sudix/apps/octopress/_deploy/.git/
[master (root-commit) 5a3c02f] Octopress init
 1 file changed, 1 insertion(+)
 create mode 100644 index.html
cd -
---
## Now you can deploy to http://sudix.github.io/sudix with `rake deploy` ##
```

## デプロイ
以下のコマンドでデプロイされます。
```bash デプロイ実行
$ bundle exec rake deploy
```

### 公開されたか確認
Github Pagesを開いてみて、公開されたか確認してみましょう。  
なお、デプロイして10分ほど待つ必要があるみたいです。  
[http://sudix.github.io/](http://sudix.github.io/)


***

#コメント欄を追加
[DISQUS](http://disqus.com/)を使って、コメント欄を追加することもできます。  
[DISQUS](http://disqus.com/)に登録するとshort nameが発行されるので、  
それを_config.ymlに設定するだけです。  

```yaml _config.yml
# Disqus Comments                                                                                                                       
disqus_short_name: sudixsblog
disqus_show_comment_count: false
```
追加した後、generateし直せば、下にコメント欄が表示されるはずです。

***

# 記事を書く
さっそく記事を書いてみましょう。
手順:[http://octopress.org/docs/blogging/](http://octopress.org/docs/blogging/)

## ファイル作成
rakeでnew_postを実行すれば記事を記入するためのmarkdownファイルが作成されます。  
new_postの後にタイトルを入力しましょう。  
ここで指定したタイトルを使ってパーマネントリンクが作成されます。  
タイトルは日本語不可のようです。  

```sh 記事の作成
$ rake new_post["my first octopress article"]
```

## タイトルの編集
作成されたファイルを開くと、先頭にyamlで記述された設定があるので、  
タイトルを変更したい場合はその中のtitleを編集します。  

```yaml yyyy-mm-dd-my-first-octopress-article.markdown
---
layout: page
title: "Octopressでの初めての記事"
date: 2013-08-06 20:59
comments: true
sharing: true
footer: true
---
```

## 記事の編集と公開
あとはmarkdown形式で記事を書き、generateとdeployを実行すれば、記事が公開されます。

## おまけ コードスニペットの書き方
gist使ったり色々と書き方はあるみたいです。  
参考:[http://octopress.org/docs/blogging/code/](http://octopress.org/docs/blogging/code/)  
基本はバッククォートで囲みます。言語も指定可能です。

    ```ruby Test.rb
    def hoge
      puts "hogefuga"
      1 + 2
    end
    ```
と書けば、以下のように。
```ruby Test.rb
def hoge
  puts "hogefuga"
  1 + 2
end
```

Scalaだって  
    ```scala Hoge.scala
    object Hoge {
      def main(args: Array[String]) {
        println("hoge")
      }
    }
    ```
↓  
```scala Hoge.scala
object Hoge {
  def main(args: Array[String]) {
    println("hoge")
  }
}
```
綺麗に表示されて楽しいですね！  


