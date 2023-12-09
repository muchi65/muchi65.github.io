---
layout: post
title: "「ガス代横取りされていませんか？」ガスレスフリーミントのやり方（ノーコード）【後編】"
categories: NFT Tips
tags: NFT フリーミント Web3.0 Tips
date: 2023-12-09
image: /assets/images/gasless/gasless02.png
---

![ガス代横取りされていませんか？【後編】]({{ page.image }} "ガス代横取りされていませんか？【後編】")

**目次**
* markdown unordered list
{:toc}

# はじめに
先日、[FDAというコレクションのガスレスフリーミント]({% post_url 2023-11-25-FDA %})に挑戦してみたので、そのやり方を紹介します。
[前編][gasless01]では、**ミントサイトとして使用できるブログ等を持っていない人でも**ミントサイトが公開できるようになるまでの方法を記載しました。
今回、後編では、**ガスレスの設定によっては他人がガス代を横取りできる状態になってしまう**ので、その対応策を説明します。
thirdwebでミントの設定のやり方は[前編][gasless01]の記事をご覧ください。

なお、OpenZepplin Relayer v2が出てますが、v1での対応です。（執筆中に気づきました…）

# ガスレスフリーミントの設定
## 準備
[thirdwebのブログ](https://blog.thirdweb.com/guides/setup-gasless-transactions/)を参考に進めていきます。  
今回は、[OpenZeppelin Defender][defender]が必要になります。
OpenZeppelin Defenderの役割としては、ミントするユーザーに代わってガス代を負担する自動プログラムです。
ウォレットがあればすぐにアカウントを作れるので、アカウントを作成してログインしてください。この後、3ステップで設定が完了します。

## 負担するガス代の送金
まずは、負担するガス代をデポジットしておくRelayerを作成します。左側のタブの「Relay」を選択し、「Create Relayer」を押します。Relayerの作成画面が出てくるので、
名前とネットワークを設定します。名前は、後から分かるようにtestや、ネットワーク名を入れると良いと思います。ネットワークはフリーミントしたいNFTに合わせて設定してください。以下の画像はMumbaiテストネットを選択しています。

![Relayerの作成](/assets/images/gasless/gasless_create_relayer.jpg "Relayerの作成")
![Relayerの設定](/assets/images/gasless/gasless_relayer_settings.jpg "Relayerの設定")

{: align="right"}
*[OpenZeppelin Defender][defender]*

無事Relayerが作成できたら、Relayerに負担するガス代を送金します。
ここで送金する**ネットワークとアドレスを絶対に間違えないよう**に注意してください。
最悪、GOXという暗号資産が失われた状態になり、取り戻せなくなります。
送金先のアドレスは下の画像の枠の箇所にありますので、必ずコピペしましょう。
ちなみに、「Withdraw funds」で引き出すこともできますのでご安心ください。
ただし、送金・引き出しの際にガス代はかかるので注意してくさい。  

送金の方法は、メタマスクから送金を選択してアドレスを入力すれば良いです。
この時、右上のネットワークの切り替えで、今のネットワークを確認してください。
送金後、Relayerの表示に反映されるまでは少し時間がかかります。

![Relayerのアドレス](/assets/images/gasless/gasless_relayer_address.jpg "Relayerのアドレス")

{: align="right"}
*[OpenZeppelin Defender][defender]*

![メタマスクからの送金](/assets/images/common/metamask_transfer.jpg "メタマスクからの送金")

{: align="right"}
*[MetaMask](https://metamask.io)*


## ガス代を負担するプログラムの設定
次に、ミントされるときに自動でガス代を負担するプログラムを設定をします。
左側のタブの「Autotask」を選択し、「Create Autotask」を押します。Autotaskの設定画面が出てくるので、以下の通りに設定していきます。

- Name  
後から見て分かりやすい名前を設定しましょう。

- Trigger  
Webhookを選択します。

- Connect to a relayer  
先ほど作成した「Relayer」を選択します。

- Dependency version  
初期設定のままでOKです。

- Code  
ここが非常に重要なので、後ほど説明します。最初から記載されているコードは全て削除してください。

![Autotaskの作成](/assets/images/gasless/gasless_create_autotask.jpg "Autotaskの作成")

{: align="right"}
*[OpenZeppelin Defender][defender]*

さて、ノーコードと言っておきながら、ここでコードが登場しますが、コピペでOKです。
**「ガス代を横取りされる」**ポイントはここのコードの部分です。
thirdwebから公開されている[コード](https://raw.githubusercontent.com/thirdweb-dev/ozdefender-autotask/main/src/forwarder_handler.js?ref=blog.thirdweb.com)は汎用性が高い設計になっています。
詳細は省きますが、送信先アドレス等の制限がないため、他人がこのプログラムを使ってミントすることができてしまいます。
そこで、対応できるアドレスを制限するようにしたコードを[こちら](https://raw.githubusercontent.com/muchi65/thirdweb_relayer/main/forwarder_handler.js)で公開しています。

まずは、[こちら](https://raw.githubusercontent.com/muchi65/thirdweb_relayer/main/forwarder_handler.js)のコードを上記のコードの箇所へ全てコピペします。
コピペしたら、上から10行目の「allowedAddress」の部分を、自分のNFTのコントラクトアドレスに変更します。
”ダブルクォーテーション”で囲む必要があるのと、末尾のセミコロン;は削除しないよう注意してください。

```js
// Enter your NFT address here
const allowedAddress = "0x5B660d122038F1eE1eb704d15349ACC53e9ABBaC";
```

![コントラクトアドレス](/assets/images/gasless/gasless_contract_address.jpg "コントラクトアドレス")

{: align="right"}
*[thirdweb][thirdweb]*

もう一度記載しますが、ここで記載するのはNFTのコントラクトアドレスです。
自分のウォレットアドレスではないので注意してください。
コントラクトアドレスは、thirdwebの「Contracts」から確認できます。
ちなみに、作成したRelayerとAutotaskは設定から動作を停止させることも可能なので、使用しない間は停止させるようにするとより安全だと思います。


## thirdwebでの設定
最後に、設定したAutotaskと[OpenZeppelin Defender][defender]のミントサイトを繋げます。
まず、Autotask側にある「Webhook URI」をコピーします。
その後、thirdwebの「Contracts」からコントラクトを選択し、「Embed」を選択します。
「Gasless」の項目を「OpenZeppelin Relayer」に変更し、「Relayer URL」にコピーしたAutotaskのURIを貼り付けます。

これで、Embed Codeが自動的に変わり、ガスレスフリーミントができるようになります。
Embed Codeをブログに張り付ける等、ミントの設定は[前編][gasless01]で説明したとおりです。


![URIの取得](/assets/images/gasless/gasless_copy_webhook.jpg "URIの取得")

{: align="right"}
*[OpenZeppelin Defender][defender]*

![URIの貼り付け先](/assets/images/gasless/gasless_embed_URL.jpg "URIの貼り付け先")

{: align="right"}
*[thirdweb][thirdweb]*

ちなみに、フリーミントではない場合（ミント価格を0円にしていない場合）は、ガスレスでミントできませんでした。
できるのかどうか分からなかったので、ご存じの方がいらっしゃれば教えてください。

![ミント失敗](/assets/images/gasless/gasless_mint_fail.jpg "ミント失敗")

{: align="right"}
*[thirdweb][thirdweb]*

# まとめ
thirdwebとOpenZepplin Defenderを使って、ガスレスでフリーミントをする方法を紹介しました。
今回は、thirdwebで公開されているコードにコントラクトアドレスによる制限を加えて、横取りを防ぐ方法を紹介しました。
OpenZepplin Defenderで作成したWebhook URIは、上記のやり方では公開されます。
一般的にも隠す実装は難しいようなので、公開される前提でコントラクトアドレスによる制限を加えています。

そもそもthirdwebでのNFTの作り方がわからない方は[前編][gasless01]もご覧ください。

[defender]:https://defender.openzeppelin.com
[thirdweb]:https://thirdweb.com
[gasless01]:{% post_url 2023-12-03-how-to-gas-less-01 %}