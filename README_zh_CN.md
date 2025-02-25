<h1 align="center">
  tokencore
</h1>
<p align="center">

  <a href="https://travis-ci.com/galaxyzxcv/tokencore">
    <img src="https://travis-ci.com/galaxyzxcv/tokencore.svg?branch=master">
  </a>

  <a href="https://github.com/galaxyzxcv/tokencore/issues">
    <img src="https://img.shields.io/github/issues/galaxyzxcv/tokencore.svg">
  </a>

  <a href="https://github.com/galaxyzxcv/tokencore/pulls">
    <img src="https://img.shields.io/github/issues-pr/galaxyzxcv/tokencore.svg">
  </a>

  <a href="https://github.com/galaxyzxcv/tokencore/graphs/contributors">
    <img src="https://img.shields.io/github/contributors/galaxyzxcv/tokencore.svg">
  </a>

  <a href="LICENSE">
    <img src="https://img.shields.io/github/license/paipaipaipai/tokencore.svg">
  </a>

</p>


# 联系方式

- [我的Telegram](https://t.me/xzadcx) / @Telegram

# 语言选择
- en [English](README.md)
- zh_CN [简体中文](README_zh_CN.md)

# 使用案例

交易所钱包后台 https://github.com/galaxyzxcv/java-wallet

# tokencore介绍

##### 区块链钱包后台核心组件，支持BTC,OMNI,ETH,ERC20,TRX,TRC20,BCH,BSV,DOGE,DASH,LTC,FILECOIN

# tokencore使用方式

#### 引入本库

- gradle方式 在你的build.gradle里面

```
    repositories {
        maven { url 'https://jitpack.io' }
    }
    
    dependencies {
        compile 'com.github.galaxyzxcv:tokencore:1.2.7'
    }
```

- maven方式

```
	<repositories>
		<repository>
		    <id>bintray</id>
		    <url>https://dl.bintray.com/tronj/tronj</url>
		</repository>
		<repository>
		    <id>jitpack.io</id>
		    <url>https://jitpack.io</url>
		</repository>
	</repositories>
	
	<dependency>
	    <groupId>com.github.galaxyzxcv</groupId>
	    <artifactId>tokencore</artifactId>
	    <version>1.2.7</version>
	</dependency>
```
#### 测试样例
[https://github.com/paipaipaipai/tokencore/blob/master/src/test/java/org/consenlabs/tokencore/Test.java](https://github.com/paipaipaipai/tokencore/blob/master/src/test/java/org/consenlabs/tokencore/Test.java)
#### 初始化身份

```java
    try{
        Files.createDirectories(Paths.get("${keyStoreProperties.dir}/wallets"))
        }catch(Throwable ignored){
        }
        //KeystoreStorage是接口，实现它的getdir方法
        WalletManager.storage=KeystoreStorage();
        WalletManager.scanWallets();
        String password="123456";
        Identity identity=Identity.getCurrentIdentity();
        if(identity==null){
        Identity.createIdentity(
        "token",
        password,
        "",
        Network.MAINNET,
        Metadata.P2WPKH
        );
        }
```

#### 生成钱包

```java
        Identity identity = Identity.getCurrentIdentity();
        String password = "123456";
        Wallet wallet = identity.deriveWalletByMnemonics(
        ChainType.BITCOIN,
        password,
        MnemonicUtil.randomMnemonicCodes()
        );
        System.out.println(wallet.getAddress());

```

#### 离线签名

- 比特币

```java
        String password = "123456";
        String toAddress = "33sXfhCBPyHqeVsVthmyYonCBshw5XJZn9";
        int changeIdx = 0;
        long amount = 1000L;
        long fee = 555L;
        //utxos需要去节点或者外部api获取
        ArrayList<BitcoinTransaction.UTXO> utxos = new ArrayList();
        BitcoinTransaction bitcoinTransaction = new BitcoinTransaction(
        toAddress,
        changeIdx,
        amount,
        fee,
        utxos
        );
        Wallet wallet = WalletManager.findWalletByAddress(ChainType.BITCOIN, "33sXfhCBPyHqeVsVthmyYonCBshw5XJZn9");
        TxSignResult txSignResult = bitcoinTransaction.signTransaction(
        String.valueOf(ChainId.BITCOIN_MAINNET),
        password,
        wallet
        );
        System.out.println(txSignResult);
```

- 波场

```java
        String from = "TJRabPrwbZy45sbavfcjinPJC18kjpRTv8";
        String to = "TF17BgPaZYbz8oxbjhriubPDsA7ArKoLX3";
        long amount = 1;
        String password = "123456";
        Wallet wallet = WalletManager.findWalletByAddress(ChainType.BITCOIN, "TJRabPrwbZy45sbavfcjinPJC18kjpRTv8");
        TronTransaction transaction = new TronTransaction(from, to, amount);
        //离线签名，不建议签名和广播放一块
        TxSignResult txSignResult = transaction.signTransaction(String.valueOf(ChainId.BITCOIN_MAINNET), password, wallet);

        System.out.println(txSignResult);
```

#### 注意：这只是一个数字货币的功能组件！！！只供学习使用，不提供完整的区块链业务功能





