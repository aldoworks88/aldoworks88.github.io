---
title: 'Hugoで作成したWebサイトに記事を追加してみた｜ブログ記事の作成'
date: '2026-08-29T17:53:28+09:00'
tags: ['Web制作', 'ブログ', 'Git', 'Hugo']
featured_image: ""
description: "Windows環境でHugoで作成したWebサイトに新しい記事を追加する方法をまとめました"
---

## この記事の内容

Hugoで作成したWebサイトに新しい記事を作成し、公開する手順をまとめました。  
Webサイトの作り方は[こちら](/posts/createwebpagefirst/)
<br>
<br>
<!--more-->


## はじめに

Hugoで作成したWebサイトをブログとして運用するためには、記事の追加が必要ですよね。  
であれば、記事を追加できるようにならなければなりません（当たり前）  

その方法をまとめました。  
私自身も忘れたときに、見直せるように。  
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

1. HugoでMarkdownファイルを作成  
2. 新しい記事を作成
3. GitHub Pagesで公開
<br>
<br>

### 1. HugoでMarkdownファイルを作成 

まず、新しい記事を書くための真っ新なMarkdownファイルを作成しましょう。

PowerShellで、作成済みのHugoプロジェクトのディレクトリへ移動します。  
（例）ドキュメント>Blogs>（Hugoプロジェクト）
```powershell
cd Documents/Blogs/myblog   #Hugoプロジェクト（myblog）へ移動
```
<br>

新しいMarkdownファイルを作成します。
```powershell
hugo new content posts/AddNewArticle.md     #Markdownファイルをcontent>postsディレクトリに作成
```
<br>

エクスプローラー上でも、作成したMarkdownファイルが確認できます。  
<img src="/images/ExploreAddNewArticleCapture.png">
<br>

これで、記事を書くための土台は完成しました。  
簡単ですね！  
<br>
<br>
<br>

### 2. 新しい記事を作成

では、新しい記事の作成に取り掛かります。  
先ほど作成したMarkdownファイルを開きましょう。  

下記のような状態になっていると思います。  
```text
---
title: ''                           #記事のタイトル
date: '2026-08-29T17:53:28+09:00'   #記事の作成日時・公開日時
tags: []                            #記事に設定するタグ
featured_image: ""                  #記事のアイキャッチ画像
description: ""                     #記事の概要・説明
---
```
<br>

ここに、必要な項目を記述していきます。  
アイキャッチ画像は設定しないので`featured_image`は空欄です。また、`description`は、記事の概要を記載する項目で、テーマによっては一覧表示や検索向けの説明にも使われるので、概要を記載しておくと良いと思います。  
```text
---
title: 'Hugoで作成したWebサイトに記事を追加してみた｜ブログ記事の作成'
date: '2026-08-29T23:00:00+09:00'
tags: ['Web制作', 'ブログ', 'Git', 'Hugo']
featured_image: ""
description: "Windows環境でHugoで作成したWebサイトに新しい記事を追加する方法をまとめました"
---
```
<br>

PowerShellでサイトを起動し、確認してみましょう。  
```powershell
hugo server -D
```
<br>

新しく追加された記事が表示されていますね。  
クリックして開いてみましょう。  
<img src="/images/NewArticleCapture01.png" class="article-image">
<br>

新しい記事が表示されましたね。  
Markdownファイルに記述したタイトル、日時、タグの表示が確認できます。  
<img src="/images/NewArticleCapture02.png" class="article-image">
<br>

これにて、記事の追加は完了です。  
あとは、Markdownファイルに記事の内容を記述していけば、本記事のようなページを作成できます。
<br>
<br>
#### 【Markdownファイルの編集ソフトについて】
Markdownファイルを編集するソフトは、メモ帳、さくらエディタなど、何を使用してもOKですが、**VS Code（Visual Studio Code）がおすすめ**です。ヘッダー`##〇〇`や改行`<br>`に色を付けてくれるので、とても見やすいですね。  

[VS Codeダウンロードページ](https://code.visualstudio.com/download?_exp_download=fb315fc982)

<img src="/images/VSCodeCapture.png">
<br>
<br>
<br>

### 3. GitHub Pagesで公開

それでは、新しく作成した記事をGitHub Pagesで公開しましょう。  
最初に、`git push -u origin main`でプッシュしていれば、下記のコマンドで公開できます。  

【参考】[Webサイトの公開方法](/posts/createwebpagesecond/)
```powershell
git add .
git commit -m "Add GitHub Pages New Article"
git push
```
<br>

GitHub上のworkflowsのステータスが✔マークになったら、Webサイトに記事が追加されていることを確認してみましょう。  
<img src="/images/NewArticleCapture02.png" class="article-image">
<br>
<br>
<br>

## まとめ
HugoでWebサイトへの記事の追加は**簡単！**、という感想です。  
最初の1歩であるサイトの公開をちゃんとできていれば、記事の追加もあまり苦戦せずに済みそうです。  

あとは、どんな記事を書くか、どうサイトを装飾していくか、といった点を考えながら、経験値を溜めていければ、よりスムーズに記事を増やしていけそうですね。
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

