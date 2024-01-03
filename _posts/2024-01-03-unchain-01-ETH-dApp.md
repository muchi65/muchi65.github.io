---
layout: post
title: 一生コメントが残る掲示板を作ってみた
date: 2024-1-3
categories: Web3
tags: Web3.0 JavaScript Solidity
image: /assets/images/web3/20240103.jpg
---

![一生コメントが残る掲示板を作ってみた]({{ page.image }} "一生コメントが残る掲示板を作ってみた")

**目次**
* markdown unordered list
{:toc}

# UNCHAIN
[UNCHAIN](https://app.unchain.tech)というサイトをご存じでしょうか？
UNCHAINはWeb3の勉強が無料でできる学習サービス＆コミュニティです。
私もDiscordで分からないことを質問したりしています。

今回は、UNCHAINの学習コンテンツからWeb3アプリケーションを作成するコースにチャレンジしました。詳細は、[こちら](https://app.unchain.tech/learn/ETH-dApp/)をご覧いただく方が良いと思うので、つまづいた点をまとめていきます。

今回作った掲示板アプリがありますので、よろしければコメントを残していってください。
ソースコードは[こちら](https://github.com/muchi65/ETH-dApp)にあります。

今回の実装環境は以下の通りです。
- Windows11
- WSL2 ※後述しますがWSL1でやったらつまづきました
- Node.js 20.9.0
- React 16.14

# 掲示板
Mumbaiテストネットで動作する掲示板を作成しました。
荒らされるのは避けたいので、承認プロセスを追加しております。
コメントの通知が来ないので、もしコメントしたらXで[@muchi65_][X]まで連絡ください！
私の独断と偏見で掲載するか判断します。よろしくお願いします。

なお、今回の掲示板はMumbaiテストネットでデプロイしているため、一生は残らないと思います。
Polygonとかの本番環境で作れば、ブロックチェーン上にコメントを残せるアプリケーションになります。

Mumbaiテストネットで使用するMATICは以下のリンクから入手できます。  
メタマスクにテストネットを接続する方法は[Metamask（メタマスク）にテストネット（Goerli・Mumbai）を接続する方法](https://seoblo.org/metamask-add-testnet/)が参考になります。

- [Polygon Faucet](https://faucet.polygon.technology/)  
- [Polygon Mumbai Faucet](https://mumbaifaucet.com/)  

<div id="root"></div><script defer="defer" src="/assets/js/board/static/js/main.93af3606.js"></script><link href="/assets/js/board/static/css/main.59363bcc.css" rel="stylesheet">

[X]: https://twitter.com/{{ site.twitter_username }}

# つまづきポイント
私のつまづきポイントをセクションの番号と共に箇条書きで列挙しておきます。

## 【S-1-1】Hardhatのコンパイラがダウンロードできない

    yarn test

でいきなり以下のエラー。

    Error HH502: Couldn't download compiler version list. Please check your internet connection and try again.

ググった情報でトップの方に来る対応策がすべてNGでした…。
- Proxy設定→そもそもしていない
- DNS設定→問題なさそう
- 再起動してみる
- Windowsファイアウォールの設定→問題なさそう


**対策**  
Hardhatの[公式サイト](https://hardhat.org/hardhat-runner/docs/guides/project-setup)に「we strongly recommend using WSL2
」と書いてありました。最初はWSL1を使っていたので、これが原因のようです。
[こちらのリンク](https://qiita.com/matarillo/items/61a9ead4bfe2868a0b86)を参考にWSL2に変更して、無事突破できました。


## 【S-1-1】テストでエラーが出る

    yarn test

で以下のエラー。

    Error: Timeout of 40000ms exceeded. For async tests and hooks, ensure "done()" is called; if returning a Promise, ensure it resolves.

**対策**  
これは処理が遅く、デフォルトの40000ms以内に処理が終わらないために発生していました。
Lock.jsの30行目付近に```this.timeout(60000);```を追加して時間を延ばしたらpassしました。

```js
  describe("Deployment", function () {
    it("Should set the right unlockTime", async function () {
      this.timeout(60000); // ←ここの数字を変更！
      const { lock, unlockTime } = await loadFixture(deployOneYearLockFixture);

      expect(await lock.unlockTime()).to.equal(unlockTime);
    });
```


## 【S-1-3】実行結果が表示されない

    yarn contract run:script

で実行結果が出てこなくて、あれ？となりました。

**対策**  
まず、ルートフォルダで実行する必要があるので、./ETH-dApp/の階層で実行する必要があります。
また、デプロイに時間がかかるので、1分くらい待機が必要でした。

## 【S-2-3】Webアプリケーションのエラー①

最後のテストで、Webアプリケーション側でエラー。

    Error: invalid address or ENS name (argument="name", value=3.5629627477341506e+47, code=INVALID_ARGUMENT, version=contracts/5.7.0)

**対策**  
コントラクトアドレスが""で正しく囲われていなかったのが問題でした。空白とか入った場合もこのエラーが出るようです。

NG
```js
  const contractAddress = 0x3e68e0a589b64286e55aa30ae1bbb1b99f408c71;
```

OK
```js
  const contractAddress = "0x3e68e0a589b64286e55aa30ae1bbb1b99f408c71";
```

## 【S-2-3】Webアプリケーションのエラー②

    error: call revert exception [ see: https://links.ethers.org/v5-errors-call_exception ] (method="getallwaves()", data="0x", errorargs=null, errorname=null, errorsignature=null, reason=null, code=call_exception, version=abi/5.7.0)

コントラクトアドレスを間違えただけでした…。


## 【S-3-1】クライアントアプリケーションがデプロイできない

ガス代が高すぎてテスト用の通貨が足りなくなっただけでした。


## 【S-3-1】クライアントアプリケーションでメッセージが送信できない

    Error: call revert exception [ See: https://links.ethers.org/v5-errors-CALL_EXCEPTION ] (method="getAllWaves()", data="0x", errorArgs=null, errorName=null, errorSignature=null, reason=null, code=CALL_EXCEPTION, version=abi/5.7.0)

**対策**  
今回のアプリケーションは、ネットワークの変更を促してくれません。これは、接続したウォレットのネットワークが間違っていたことが原因でした。私が公開しているアプリケーションはネットワークの変更を促す処理を入れています。

```js
  const chainId = 80001; // Mumbai

  const changeNetwork = async () => {
    if (window.ethereum) {
      await window.ethereum.request({
        method: 'wallet_switchEthereumChain',
        params: [{ chainId: Web3.utils.toHex(chainId) }],
      }
    }
  }

  // ネットワークを切り替えたい箇所で
  await changeNetwork();
```

## 【S-3-2】コントラクトのテストがうまくいかない

    yarn contract run:script

で以下のエラー。

    Error: non-payable constructor cannot override value (operation="overrides.value", value={"type":"BigNumber","hex":"0x016345785d8a0000"}, code=UNSUPPORTED_OPERATION, version=contracts/5.4.1)

**対策**  
WavePortal.solのconstructorに```payable```をつけ忘れただけでした。


## 【S-3-2】メッセージを送るトランザクションに失敗

etherscanを確認したところ以下のログが残っていました。

    Fail with error 'Trying to withdraw more money than the contract has.'

**対策**  
テスト用アカウントにガス代がなかったからでした…


## その他
クライアントアプリ（Webサイト側）を作成中にアプリの起動に時間がかかりすぎて、トライ&エラーが大変でした。
そこで、```packages/client/package.json```の以下の箇所に、```WATCHPACK_POLLING=true```と入れるとホットリロードという機能が有効になります。
ホットリロードは、コンパイル後に、Web画面を開いている状態でソースコードを変更、保存すると、Webアプリが自動で更新される素敵な機能です。  
なお、Reactのバージョンによっては、```CHOKIDAR_USEPOLLING=true```の場合もあるようです。

```json
  "scripts": {
    "start": "WATCHPACK_POLLING=true react-scripts start",
    "build": "react-scripts build",
```

