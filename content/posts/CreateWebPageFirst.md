---
title: 'HugoでWebサイトを作ってみた①｜Windowsでサイトを作成するまで'
date: '2026-08-26T10:55:28+09:00'
tags: ['Web制作', 'Git', 'Hugo']
featured_image: ""
description: "Windows環境でHugoを使ってWebサイトを作成する手順をまとめます"
---

## この記事の内容

Git、Hugoの導入から、Hugoを使ってWebサイトを作成する手順をまとめました。  
次回の記事はこちら↓  
[HugoでWebサイトを作ってみた②｜GitHub Pagesで一般公開するまで](/posts/createwebpagesecond/)  
<br>
<br>

<!--more-->

## はじめに

学んだこと、経験したことを記録しておきたいと考え、自分のWebページ作成に着手しました。  
知識の整理ができるし、後で見返すこともできる。  
その上で、誰かの役に立てばと思いまして…笑  

ということで、本ページを作成しましたので、その手順についてまとめました。
<br>
<br>

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
3. Hugoでサイトを作成 　**←今回はここまで**  
4. GitHubへPush  
5. GitHub Pagesで公開
<br>
<br>

### 1. Gitをインストール

まずは、Gitのインストールです。  
これをインストールしないことには始まりません。  
<br>

ダウンロードページから、Gitをダウンロードする。  
[Gitダウンロード](https://git-scm.com/install/windows)  
<br>

PowerShellで、下記コマンドを実行し、バージョンを確認する。  
```powershell
git --version
```
<br>

バージョン情報が表示されれば、インストール成功です。  
（例）
```powershell
git version 2.55.0
```
<br>
<br>

### 2. Hugoをインストール

次は、Hugoのインストールです。  
今回は、Webサイトを作成するためにこちらを使用します。  
Markdown形式で記述できるので、HTMLより楽だと思います。  
<br>

PowerShellで、wingetコマンドを使用し、Hugo Extendedをダウンロードする。  
```powershell
winget install Hugo.Hugo.Extended
```
<br>

下記コマンドを実行し、バージョンを確認する。  
```powershell
hugo version
```
<br>

バージョン情報が表示されれば、インストール成功です。  
（例）
```powershell
hugo v0.165.0 ... +extended windows/amd64
```
<br>

Sass/SCSSをCSSに変換するLibSassをサポートしているということで、Extended版をインストールしましたが、現在は、LibSass非推奨。  
将来的に削除予定とのことです。  
であれば、Standard版で問題なさそうです...  
<br>
<br>

### 3. Hugoでサイトを作成

さて、ここからはHugoを使用して、実際にWebサイトを作成していきます。  
難しいことは、一旦置いておいて、ブラウザでサイトを見れるようにすることを目指します。  
<br>

PowerShellで、ブログを置きたい場所に移動します。  
（例）ドキュメント>Blogsに作成する。  
```powershell
cd Documents/Blogs      #Blogsディレクトリへ移動
```
<br>

Hugoコマンドで新しいサイトを作成し、初期化する。  
（例）myblogを作成する。 
```powershell
hugo new site myblog    #myblogというHugoサイトのひな型を新規作成
cd myblog               #myblogディレクトリへ移動
git init                #Gitリポジトリとして初期化
```
これで、ブログのフォルダ構成を作成できました。
<br>
<br>
<br>

次に、デザインの手間を省くために、既存のテーマを使用するよう設定します。  
<br>

Git Submoduleコマンドで、テーマを追加する。  
（例）Anankeテーマを追加する。  
```text
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```
<br>

myblogフォルダ内のhugo.tomlを開き、テーマと基本情報を設定する。  
```text
baseURL = 'https://aldoworks88.github.io/'
locale = 'ja-JP'
title = 'アルドの備忘録'
theme = 'ananke'
```
これで、基本情報とテーマの設定は完了です。
<br>
<br>
<br>

ここからは、実際にページの作成に取り掛かります。  
<br>

Hugoコマンドで、新しいページを作成します。  
（例）content>posts下に、hello-world.mdを作成する。  
```powershell
hugo new content posts/hello-world.md
```
これで、ページの作成ができました。  
<br>
<br>
<br>

それでは、実際にブラウザで確認してみましょう。  
<br>

Hugoコマンドで、サイトを起動する。
```powershell
hugo server -D             #ローカルPC上で確認するためコマンド
```
<br>

下記の結果を確認する。  
```text
Built in 148 ms
Environment: "development"
Serving pages from disk
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```
<br>

ブラウザで、URL（http://localhost:1313/）を開き、Webサイトを確認する。  

<img src="/images/helloworld.png" class="article-image">  

これで、新しいサイトを作成できました。  
今後は、このhelloworld.mdを編集して、Webサイトの表示をカスタマイズしていきます。  
<br>
<br>
<br>

## まとめ
初めてHugoを使用してみましたが、
- Hugoコマンドでひな型作成
- Markdown形式で直感的に記述
- 既存テーマで労力削減  

と、非常にラクに、Webサイトの作成ができました。  
HTMLで記述した際は、`<p>`や`<h1>`のようなタグをつけるのが面倒臭かったので、Markdownの方が、記述しやすいですね！
<br>
<br>

次回は、  
[HugoでWebサイトを作ってみた②｜GitHub Pagesで一般公開するまで](/posts/createwebpagesecond/)  
で、  

4. GitHubへPush  
5. GitHub Pagesで公開  

についてのまとめです。
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
