---
title: 'HugoでWebサイトを作ってみた②｜GitHub Pagesで一般公開するまで'
date: '2026-08-27T18:55:28+09:00'
tags: ['Web制作', 'Git', 'Hugo']
featured_image: ""
description: "Hugoで作成したWebサイトをGitHub Pagesで一般公開するまでの手順をまとめます"
---

## この記事の内容

Hugoで作成したWebサイトをGitHub Pagesでの一般公開までの手順をまとめました。  
※前回の記事はこちら↓   
[HugoでWebサイトを作ってみた①｜Windowsでサイトを作成するまで](/posts/CreateWebPageFirst/)
<br>
<br>

<!--more-->

## 今回使用した環境
- Windows 11 (25H2)
- Git (2.55.0)
- Hugo (v0.165.0)
- GitHub
<br>
<br>

## 手順

#### 全体の流れ

1. Gitをインストール  
2. Hugoをインストール  
3. Hugoでサイトを作成  
4. GitHubへPush　　　**←今回はここから**  
5. GitHub Pagesで公開
<br>
<br>

### 4. GitHubへPush

では、前回作成したWebサイトをGitHubにPushしましょう。  
<br>

まず、PowerShellでHugoプロジェクトのディレクトリへ移動し、Gitの状態を確認します。  
（例）
```powershell
cd Documents/Blogs/myblog   #ドキュメント>Blogs>myblogディレクトリへ移動
git status                  #Gitステータスの確認
```
<br>

初のPushなので、ステータスは『まだ一度もコミット（変更の記録）が作成されていない』となります。  
```powershell
No commits yet
```
<br>

前回作成したhelloworld.mdを編集し、draft = falseに変更します。  
```text
date = '2026-08-26T21:01:06+09:00'
draft = false
title = 'Hello World'
```
<br>

draft = trueの状態では、Web上で表示されないので、必ずfalseに変更しましょう。  
<br> 
<br>

ここから、作成したサイトをGitにPushしていきます。  
```text
git add .                                                                       #全ファイルをステージング
git commit -m "Initial Hugo blog"                                               #Gitへコミット
git branch -M main                                                              #ブランチ名をmainに設定
git remote add origin https://github.com/ユーザー名/ユーザー名.github.io.git      #ローカルレポジトリとGitリポジトリを接続
git push -u origin main                                                         #GitへHugoリポジトリをアップロード
```
<br>

なお、それぞれの意味を説明すると、  
- ステージング：この変更を次のコミットに含めますよ
- コミット：ステージングの変更をローカルGit履歴として、確定・保存するよ
- プッシュ：GitHubへローカルで作成したコミットをアップロードするよ  

といった流れです。  
ブランチやリポジトリについては、今回は深く考えません。  
<br>

なお、**git push -u origin main**とすることで、次回以降の更新時、**git push**でプッシュできるようになります。
<br>
<br>

では、設定したHugoのBaseURLをhugo.tomlで確認しましょう。  
```text
baseURL = 'https://aldoworks88.github.io/'
locale = 'ja-JP'
title = 'アルドの備忘録'
theme = 'ananke'
```
<br>
<br>

### 5. GitHub Pagesで公開

実際にGitHub Pagesで公開していきましょう。  
ここからは、ブラウザからGithubにアクセスし、作業を進めていきます。  
<br>

GitHubのダッシュボードから、先ほど接続したリポジトリを選択します。 

<img src="/images/GitHubRepository.png">
<br>
<br>

上のタブからSetting>Pagesへ移動し、Build and deploymentのSource『GitHub Actions』に設定します。  
<img src="/images/Buildanddeployment.png">
<br>
<br>

それでは、ソースコードをhugoでHTMLにビルドしてから公開します。  
<br>

PowerShellで、.github/workflows/hugo.yamlを作成する。  
```powershell
mkdir .github\workflows -Force
notepad .github\workflows\hugo.yaml
```
<br>

下記のように、hugo.yamlに記述します。  
```text
name: Build and deploy

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest

    env:
      HUGO_VERSION: 0.165.0
      TZ: Asia/Tokyo

    steps:
      - name: Checkout
        uses: actions/checkout@v7
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v6

      - name: Install Hugo
        run: |
          curl -L -o hugo.tar.gz \
            "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
          sudo tar -C /usr/local/bin -xzf hugo.tar.gz hugo

      - name: Build
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v5
        with:
          path: ./public

  deploy:
    runs-on: ubuntu-latest
    needs: build

    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v5
```
※中身のことは気にしない笑  
<br>

PowerShellで、Publicを除外するための.gitignoreファイルを作成する。  
```powershell
notepad .gitignore
```
<br>

下記のように、hugo.yamlに記述します。  
```text
/public/
/resources/_gen/
.hugo_build.lock
```
<br>

Publicは、Hugoがビルド時に生成する完成HTMLです。  
Git Actionsが毎回生成してくれるため、管理対象から外します。  
要するに、Git Actionsが作ってくれるから、こちらでソース管理しなくていいよね。だから除外するよってことです。  
<br>
<br>


作成したworkflowsをGitHubへPushします。  

```powershell
git add .
git commit -m "Add GitHub Pages deployment"
git push
```
<br>

GitHubから、GitHub Pagesに追加が完了しているか確認してみましょう。  

上のタブからActions>All workflowsへ移動し、✔マークになっていればOKです。  
<img src="/images/ActionsDeployment.png">
<br>
<br>

BaseURLにアクセスして、サイトを確認してみましょう。  
```text
https://aldoworks88.github.io/  
```
<img src="/images/helloworld.png" class="article-image">  
<br>

これで、Webサイトを公開することができました。  
めでたし、めでたし。  
<br>
<br>
<br>

## まとめ

今回は、とりあえずGit PagesでWebサイトを公開することを目標に進めました。  

Hugo、GitHubともに、Webサイト作りをサポートしてくれる機能が豊富なので、比較的簡単に公開まで進めることができました。  

一方、深く考えず、とにかく進めた部分もありました。  
ブランチとか、リポジトリとか、yamlファイルとか...  
今後は、このあたりもソースコードの意味を理解していきたいですね。  

あとは、Content内のソースコードを編集して、作りたいようにWebサイトをカスタマイズしていきたいと思います。  

皆さんも、自分のWebサイトを公開してみてはいかがでしょうか？  
<br>
<br>
<br>

#### プロフィール

<img src="/images/Aldo_Icon00.png" width="100">

名前：アルド  
職業：会社員  
趣味：ゲーム・YouTube・旅行  

<a href="/aboutme/" class="profile-button">
    プロフィール詳細はこちら
</a>
<br>
<br>
<br>

## 最新の記事

{{< latest-posts >}}

