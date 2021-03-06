---
title: Github Pagesでmiddleman-blogをホストする
layout: single-layout
desc: GithubでMiddlemanで作成したblogをホストしたときの手順
---

Middlemanとmiddleman-blogでブログを始めるにあたって、
Github Pagesでホストしてみようと思った。  

Middlemanのインストールとmiddleman-blogのインストールについては省略。  

Middlemanのインストールについては <a href="https://middlemanapp.com/jp/basics/install" target="_blank">ここ</a>。   
middleman-blogのインストールについては <a href="https://middlemanapp.com/jp/basics/blogging/" target="_blank">ここ</a>。    

## リポジトリを用意する

まず、Githubにリポジトリを用意する。  
リポジトリの名前を
{ユーザー名}.github.io
とすることで、{ユーザー名}.github.ioというURLのGithub Pageが生成される。

## middleman-deployの追加

Gemfileに以下を追記して、bundle install する。

    gem "middleman-deploy"

次にconfig.rbに以下を追記する。  
deploy.remoteには先程作ったリポジトリを指定する。  
deploy.userは指定しなくても大丈夫。（なはず）  

    activate :deploy do |deploy|
      deploy.method = :git
      deploy.remote = 'git@github-torisashi:torisashi/torisashi.github.io.git'
      deploy.branch = 'master'
      deploy.user  = 'torisashi' # no default
    end

## 独自ドメインを設定する

URL {ユーザー名}.github.io を独自ドメインに変更したい場合は、  
対象ドメインのAレコードに192.30.252.153と192.30.252.154を追加する。(自分はお名前で取得したので、お名前のコンパネで設定した)  

最後に、sourceディレクトリにCNAMEという名前でファイルを作成する。
このファイルには以下のように希望するドメイン名を書いておく。

    torisashi.net

以上で準備完了。

## ビルドしてデプロイする
以下のコマンドを実行すると、build/ ディレクトリ内にhtmlが生成され、  
それらがremoteリポジトリのmasterブランチpushされる。  
指定したドメインにアクセスしてページは表示されていれば完了。

    bundle exec middleman build
    bundle exec middleman deploy
