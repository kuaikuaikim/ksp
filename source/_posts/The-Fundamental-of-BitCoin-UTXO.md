---
title: The Fundamental of BitCoin UTXO
date: 2017-05-24 23:52:09
tags: 比特币
---
### 比特币UTXO原理
&emsp;&emsp;刚刚看了下比特币的官方文档，终于摸清了点门道。说白了，UTXO就是一个数据结构，包含交易数据和执行脚本(Pubkey scripts)。如下图所示:

![utxo](/img/cvbhdadvsas.jpg)  

&emsp;&emsp;用户上传的图片中间标蓝的那个"可形象化"意义的UTXO。其中TX 1 Output 的 Public Key Hash就是Bob的Full Public Key的Hash,别忘了比特币的地址是公钥的BASE58编码(双向)。可以把比特币的交易链想象成一个水管的管道网络，把UTXO表示网络中的一个交汇接口，这个接口上有一个阀门(Pubkey Script)，默认是关闭的，水不能从这个接口流向其他管道，而打开这个阀门需要一把钥匙(Bob的private Key)，这样才能打开阀门，让水流向另外的接口(UTXO)。  

&emsp;&emsp;假设以下情形，Bob有2个跟他比特币地址对应(属于他)的UTXO(Ua和Ub),其中Ua有2个比特币，Ub有3个比特币。如果Bob想要发送给David发送4个比特币怎么办? 比特币系统中是可以同时打开多个UTXO,把Ua和Ub都激活发给David比特币。那么剩下的1个比特币去哪了呢? 同时比特币系统中会生成一个UTXO给付费的人(就是自己)，里面对应的比特币数量就是这次交易的余额。其实现实中比特币的最小交易单位是satoshis(很小的一个单位)。  

#### 那么UTXO中的Pubkey Script是如何被打开激活的呢?

&emsp;&emsp;其实Pubkey Script就是一种简单的基于栈的脚本语言（很多人都以为比特币不像以太坊那样带有脚本语言），每个比特币客户端都有一个虚拟机来执行Pubkey Script，想象一下java的虚拟机jvm只是一个基于堆栈带gc的虚拟机^-^(多了个堆和gc)。而比特币系统的脚本语言也非常简单。如下是一条标准的脚本。
```ruby
OP_DUP OP_HASH160 <PubkeyHash> OP_EQUALVERIFY OP_CHECKSIG
```
其中每个指令的结果都是入栈或者出栈。而付费的人(消费utxo)需要把自己的私钥签名和完整的公钥作为脚本指令的前缀，也当做两条指令处理。如下是带签名和公钥的脚本。
```ruby
<Sig> <PubKey> OP_DUP OP_HASH160 <PubkeyHash> OP_EQUALVERIFY OP_CHECKSIG
```
那么上面这条指令具体代表上面意思呢?  上个图或许你就明白了。

![opcode](/img/fdjsahfjshg.jpg)  

* 首先把sig和PubKey入栈。  
* OP_DUP：把栈顶的PubKey复制一份，再入栈。这时栈顶有两个PubKey。  
* OP_HASH160：把栈顶的PubKey哈希编码(hash160),代表图中下面那个Pk Hash。然后脚本中的那个Pk Hash入栈。  
* OP_EQUALVERIFY：比较栈顶的两个Pk Hash是否一样。验证地址拥有者的初步合法性。如果不通过把FALSE压入栈顶。  
* CHECKSIG：验证私钥和公钥是否匹配。    

&emsp;&emsp;如果最终结果的栈顶没有FALSE，就代表这笔交易通过，意味着Bob可以打开这个UTXO来发送比特币给另外的UTXO，而且Bob也会放入自己的Pubkey Script来个下一位想要花费UTXO的人出难题。
