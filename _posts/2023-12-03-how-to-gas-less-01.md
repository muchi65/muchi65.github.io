---
layout: post
title: "「ガス代横取りされていませんか？」ガスレスフリーミントのやり方（ノーコード）【前編】"
categories: NFT Tips
tags: NFT フリーミント Web3.0 Tips
date: 2023-12-03 16:08 +0900
---
![何かを調べるルナのイラスト](/assets/images/CNP151.webp "何かを調べるルナのイラスト")

**目次**
* markdown unordered list
{:toc}

# はじめに
先日、[FDAというコレクションのガスレスフリーミント]({% post_url 2023-11-25-FDA %})を開催してました。
通常、thirdwebでフリーミントの設定をした場合でも、ミントする人にガス代がかかってしまいます。
そこで今回は、ガス代を製作者側で負担する仕組みを入れて「ガスレス」フリーミントに挑戦してみたので、そのやり方を紹介します。  
前編では、**ブログをやっていなくて、SNSしか持っていなくても**ミントサイトが公開できるようになるまでの方法を記載しています。
後編では、**ガスレスの設定を誤ると他人がガス代を横取りできる状態になってしまう**ので、その対応策を説明します。
thirdwebでミントの設定までできている方は後編の記事からご覧ください。

# thirdwebでフリーミントの設定
## NFTの作成
まずは、[thirdweb](thirdweb)を利用してコントラクトをデプロイしていきます。簡単に言うと、NFTをミントできるように画像データやメタデータを登録する作業です。画像の矢印の箇所からウォレットを接続します。その後、「Deploy contract」を選択、今回は「Drop NFT」を選びます。

![thirdwebスタートページ](/assets/images/gasless_startpage.jpg "thirdwebスタートページ")  

![thirdwebでのデプロイ](/assets/images/gasless_deploy.jpg "thirdwebでのデプロイ")  

![コントラクトの選択](/assets/images/gasless_explore.jpg "コントラクトの選択")  

{: align="right"}
*[thirdweb](thirdweb)*

「Drop NFT」を選択した場合、ERC721AのNFTとなります。
少し前はSignature Dropという項目があり、そちらがERC721Aでしたが、Drop NFTに統合されたようです。
ERC271Aは一度にたくさんミントしてもガス代が高くならない等の特徴があります。  
以下の、「Deploy now」を押すと、コントラクトをデプロイできます。OpenSeaで言うと最初にコレクションを作る段階ですね。
ここからは、ガス代が必要なのでガス代を用意してください。まずはテストネットで練習をしてみましょう。  
Mumbaiテストネットで使用するMATICは以下のリンクから入手できます。  
メタマスクにテストネットを接続する方法は[Metamask（メタマスク）にテストネット（Goerli・Mumbai）を接続する方法](https://seoblo.org/metamask-add-testnet/)が参考になります。

- [Polygon Faucet](https://faucet.polygon.technology/)  
- [Polygon Mumbai Faucet](https://mumbaifaucet.com/)  


![コントラクトの詳細](/assets/images/gasless_contract.jpg "コントラクトの詳細")  

{: align="right"}
*[thirdweb](thirdweb)*

ガス代が用意出来たら、コントラクトをデプロイします。デプロイ時の設定は以下の通りです。
- Image ... コレクションの画像
- Name ... コレクション名
- Symbol ... コレクションのシンボル
- Description ... コレクションの説明
- Royalties ... 二次流通時のロイヤリティ（多分）
- Primary Sales ... 最初に売れたときの受け取りアドレス（多分）
- Platform fees ... 他のプラットフォームでの販売された時のロイヤリティ（多分）
- Network / Chain ... 使用するネットワーク

コントラクトがデプロイできたら「NFTs」を選択して、NFTにしたい作品をアップロードしていきます。
「Single Upload」はOpenSeaのように1個ずつ作品をアップロードできます。
「Batch Upload」はcsvファイルに複数の作品の情報をあらかじめ記載することで、まとめてアップロードできます。

![NFTのアップロード](/assets/images/gasless_upload.jpg "NFTのアップロード")  

{: align="right"}
*[thirdweb](thirdweb)*

## NFTの公開
最後に、「Claim Conditions」からNFTの公開の設定をします。ALの設定などもここで可能です。FDAでは、フリーミントにするのでミント価格は0MATIC、同じ人にすべて持っていかれるのは困るので1アドレス1個までの設定としました。なお、フェーズは時間をずらして複数設定可能なので、1週間限定のフリーミントにするために、1週間後に0.5MATICの価格になるようにしました。以下、「Public」のフェーズを追加したときの項目です。
- Name  
フェーズの名前です。自分が分かればなんでも良いと思います。
- When will this phase start?  
フェーズが有効になり、NFTがドロップされる時間です。環境に合わせてくれるようで、タイムゾーンは日本時間でした。
- How many NFTs will you drop in this phase?  
このフェーズでドロップするNFTの総数です。Unlimitedは制限なしです。
- How much do you want to charge to claim each NFT?  
ミント価格です。フリーミントの場合は0に設定します。
- How many NFTs can be claimed per wallet?  
1つのウォレットあたりミントできる上限の数です。Unlimitedは制限なしです。

![NFTの公開設定](/assets/images/gasless_claim.jpg "NFTの公開設定")  

{: align="right"}
*[thirdweb](thirdweb)*

これでNFTの公開準備が完了しました。
もし、ガスレスにしなくても良いならこのまま「Embed」にある「Embed Code」をブログなどへ貼り付けるだけでミントページが完成します。
ホームページを持っていないという方でミントページのみをSNS等で公開したい場合は、「Embed Code」内に記載されている「src="https://..."」部分のURLに直接アクセスしてもミントが可能なので、そちらを共有すれば良いと思います。

![ミント用のコード](/assets/images/gasless_embed.jpg "ミント用のコード")  


# 後編
[thirdwebのブログ](https://blog.thirdweb.com/guides/setup-gasless-transactions/)を参考に、ガスレス設定について説明します。

[thirdweb]:https://thirdweb.com/

