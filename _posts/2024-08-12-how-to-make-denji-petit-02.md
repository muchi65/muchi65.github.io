---
layout: post
title: 具体例で学ぶ！ゲーム序盤のつくりかた【ゲームデザインのコツ】
date: 2024-08-12
categories: game
tags: ゲーム制作 でんぷち！
image: /assets/images/den_petit/eye_catch02.png
---

![具体例で学ぶ！ゲーム序盤のつくりかた【ゲームデザインのコツ】]({{ page.image }} "具体例で学ぶ！ゲーム序盤のつくりかた【ゲームデザインのコツ】")

**目次**
* markdown unordered list
{:toc}


# はじめに

先日、[「ついやってしまう」体験のつくりかた](https://amzn.to/3W3TECF) で学んだゲームを面白くする3つの秘訣を投稿しました。

ただ、ゲームを面白くする方法って説明されてもよくわからないんですよね。

そこで今回から、3つの秘訣を盛り込んだ自作ゲーム「でんぷち！」の意図について解説します。

まず、最初に

**ゲームって序盤が一番大事！**

なぜならゲームを始めてくれた時点で、ある程度まではやってくれるから。

人間の脳って一度始めたら、その場ですぐやめるってあんまりしないんですよね。

つまり、序盤だけは工夫なしでも「ついやってしまう」フィーバータイムです。

序盤でプレイヤーの心を掴めれば、最後までプレイしてくれる確率は格段に上がります！

ということで、今回はぼくの自作ゲームを例に、序盤のゲームデザインについて解説します。

ネタバレしかないので、もしよければ先にゲームをプレイしてから読んでみてください！

{% include link-card.html 
  url="http://muchi65.starfree.jp/denji_petit/"
  title="でんぷち！【無料ゲーム】"
  description="無料で遊べるフリーゲームです。"
  image="/assets/images/den_petit/den_petit.jpg"
%}

前の記事を読んでない方は、こちらもぜひ！

{% capture target_url %}{% post_url 2024-07-31-how-to-make-denji-petit-01 %}{% endcapture %}
{% for post in site.posts %}{% assign candidate_url = post.url | relative_url %}{% if candidate_url == target_url %}
[{{ post.title }}（{{ post.date | date: "%B %-d, %Y" }}）]({{ target_url }}){% break %}{% endif %}{% endfor %}

# チュートリアルを作らない

「でんぷち！」はタップ操作のみで敵を倒すシンプルなゲームです。

最初に決めたのは、操作方法を丁寧に教えてくれるチュートリアルがなくてもやり方がわかるようにすることです。

理由はシンプル。「わかりやすい」がもっとも重要だと思っているから。  
それに、チュートリアルを作るは大変だから。。。

とにかくどうすればプレイヤーに伝わるかを考えました。

**「シャオランを守れ！」**

これがこのゲームでプレイヤーに示される唯一のルールです。

![denji_petit_01.jpg](/assets/images/den_petit/denji_petit_R01.jpg)

- 手を振るパンダのリーリー（視線誘導）
- 手元にターゲットマーク
- 「◀シャオランの家」という文字
- 右から近づいてくる謎の男（デンジ）
- タップすると殴る効果音と共にターゲットマークに黒い丸が出現

この情報によってプレイヤーに

**左側から来る敵をターゲットマークのところで殴ればいいのか？**

という仮説を立ててもらいます。

これは **「直感のデザイン」** です。

Xでの投稿を含めて、右からくる敵を殴るという説明は一切していませんが、「やり方がわからない」という方はいませんでした。

# 実はラウンド20までがチュートリアル

チュートリアルを作らないと言いましたが、実はラウンド20までがチュートリアルになっています。

え、どういうこと？

と思うかもしれません。これは、

**ラウンド20までの間にゲーム性をプレイヤーに理解してもらう**

という意味です。

無事に敵を殴り飛ばしたプレイヤーの次なる敵は、移動速度が速いお尻です。

![denji_petit_02.jpg](/assets/images/den_petit/denji_petit_R02.jpg)

もしちょっとでもあなたが笑ってしまったらしめたものですが、少なくとも

「敵はどうにかしてプレイヤーの守りをすり抜けて、シャオランの家に向かおうとしている」

と理解してもらえれば十分。

このままデンジとお尻を殴り飛ばしているとラウンド10では桃が登場します。

![denji_petit_10.jpg](/assets/images/den_petit/denji_petit_R10.jpg)

実はこの桃はスルーしてシャオランの家に届けることでスコアが上がるアイテムになっています。

殴ると「ブブー」という音で殴るのは間違っているよ。と伝えます。

殴っても「シャオランを守れ！」というルールには反していないので、ゲームオーバーにはなりません。

このゲームは**殴るのは正義だけど、スルーしてもいいヤツもいる**のです。

スルーできることに気付かない人もいるかもしれませんが、それはそれで問題ありません。

いつか気が付いたときに、またゲームをやりたくなる要素になります。

チュートリアルの最後、ラウンド20では背景が変化し、デンジが桃を運んできます。

![denji_petit_R20.jpg](/assets/images/den_petit/denji_petit_R20.jpg)

背景が変わったことと合わせて、プレイヤーをちょっとだけドキッとさせます。

おそらくキャラクターが2体同時に出現するとは思っていなかったでしょう。

これは **「驚きのデザイン」** です。

プレイヤーにキャラクターは1体だと誤解させて、びっくりさせる。

脳に飽きられてしまうのを刺激を与えてリセットさせるわけです。

同時に、

**10ラウンドごとに新しいキャラクターが出現する**

というルールもなんとなく理解してもらえているはずです。

ラウンド20までのチュートリアルをまとめると、

- 敵は何とかしてプレイヤーの守りを突破しようとしてくる
- 敵は基本的には殴ればいいが、スルーしていいやつもある
- ラウンドが進むと新しいキャラクターが出現する

このことを理解してもらえれば

- うわ、やられた！
- 桃をスルーするのは難しいから全部殴ろう
- 新しいキャラクターはどんなのかな？

というプレイヤー自身の体験に繋がってきます。

# 小ボスとコンティニュー

チュートリアルが終わった後に出現する敵が、ヘルメットデンジです。

![denji_petit_R30.jpg](/assets/images/den_petit/denji_petit_R30.jpg)

小ボスの位置づけでこのタイミングに設定しています。

プレイヤーはここまでのチュートリアルで、 **敵は1回殴れば倒せると誤解** しているはずです。

しかし、いざ殴ってみると1回では倒せません。ヘルメットが飛んでいくだけです。

「ヘルメット＝防御」という直感的な情報を与えているつもりですが、このあたりでゲームオーバーになった方もいると思います。

ゲームオーバーになると出てくる画面がこちらです。

![denji_petit_GameOver.jpg](/assets/images/den_petit/denji_petit_GameOver.jpg)

イラストはNinjaDAOのれおんさん（[@Neue_r_s
](https://x.com/Neue_r_s)）に描いていただきました。

哀愁漂うリーリーで思わず助けてあげたくなりませんか？

実はこのコンティニューのアイデアは初版公開時にみんみんさん（[@minmin_4410
](https://x.com/minmin_4410)）から頂きました。

ここでのこだわりポイントは、なるべくゲームの世界から離れている時間を短くするために **コンティニューからの再開を短くする** ことです。

リロードなどを極力減らして、内部処理を高速化してあります。

また、ラウンドとスコア表示を強調し、どの敵にやられたのかわかるよう敵を最前面に表示しています。

ハイスコアや新しい敵にやられたときなど **「誰かに伝えたくなる要素」** がリザルト画面に出ていることで、拡散してくれないかな？と期待してます。

コンティニューの再開は1, 11, 21, …と1のつくラウンドからです。

新しい敵は10ラウンドごとに出現するので、突破するたびにオートセーブされます。

新しい敵を見るためには、今回のこの敵を倒さないといけない。

こうやればコイツを倒せるのでは？

そんなことを考えてもらいながら、10ラウンドごとに新しく出現する敵を倒してボスまでいってもらう。

それが「でんぷち！」のゲームデザインの基本になっています。

# まとめ

まずは直感のデザインに沿って「わかりやすい」ゲームを設計を意識しました。

ある程度進んだ段階で、プレイヤーの誤解を利用した驚きのデザインで飽きさせないよう工夫を入れています。

物語のデザインも入れてありますが、それについてはまた次回説明します！

{% include link-card.html 
  url="http://muchi65.starfree.jp/denji_petit/"
  title="でんぷち！【無料ゲーム】"
  description="無料で遊べるフリーゲームです。"
  image="/assets/images/den_petit/den_petit.jpg"
%}

{% capture target_url %}{% post_url 2024-07-31-how-to-make-denji-petit-01 %}{% endcapture %}

<!-- START MoshimoAffiliateEasyLink -->
<script type="text/javascript">
(function(b,c,f,g,a,d,e){b.MoshimoAffiliateObject=a;
b[a]=b[a]||function(){arguments.currentScript=c.currentScript
||c.scripts[c.scripts.length-2];(b[a].q=b[a].q||[]).push(arguments)};
c.getElementById(a)||(d=c.createElement(f),d.src=g,
d.id=a,e=c.getElementsByTagName("body")[0],e.appendChild(d))})
(window,document,"script","//dn.msmstatic.com/site/cardlink/bundle.js?20220329","msmaflink");
msmaflink({"n":"「ついやってしまう」体験のつくりかた 人を動かす「直感・驚き・物語」のしくみ [ 玉樹 真一郎 ]","b":"","t":"","d":"https:\/\/thumbnail.image.rakuten.co.jp","c_p":"","p":["\/@0_mall\/book\/cabinet\/6167\/9784478106167.jpg"],"u":{"u":"https:\/\/item.rakuten.co.jp\/book\/15986916\/","t":"rakuten","r_v":""},"v":"2.1","b_l":[{"id":1,"u_tx":"楽天市場","u_bc":"#bf0000","u_url":"https:\/\/item.rakuten.co.jp\/book\/15986916\/","a_id":4323525,"p_id":54,"pl_id":27059,"pc_id":54,"s_n":"rakuten","u_so":1},{"u_bc":"#0054ff","u_tx":"Yahoo!ショッピング","u_url":"\/\/ck.jp.ap.valuecommerce.com\/servlet\/referral?sid=3710551\u0026pid=890752427\u0026vc_url=https%3A%2F%2Fstore.shopping.yahoo.co.jp%2Febookjapan%2Fb00162217086.html","s_n":"custom_3","u_so":3,"a_id":0,"p_id":0,"pc_id":0,"pl_id":0,"id":3},{"u_bc":"#ff9900","u_tx":"Amazon","u_url":"https:\/\/amzn.to\/3ymEcJi","s_n":"custom_4","u_so":4,"a_id":0,"p_id":0,"pc_id":0,"pl_id":0,"id":4}],"eid":"tiVk4","s":"s"});
</script>
<div id="msmaflink-tiVk4"></div>
<!-- MoshimoAffiliateEasyLink END -->

{% for post in site.posts %}{% assign candidate_url = post.url | relative_url %}{% if candidate_url == target_url %}
{% include link-card.html 
  url=target_url
  title=post.title
  description=post.description 
  image=post.image 
%}
{% break %}{% endif %}{% endfor %}
