# 区块链隐私保护
区块链的账本是具有分布式的特点，需要多个节点参与账本的存储与验证，而这容易导致人们对账本隐私的担忧。尤其是金融行业对隐私保护会更加注重。隐私问题成为区块链应用落地的主要障碍之一。本文将介绍现有区块链应用主要的隐私保护方案。
## 链外存储
  链外存储是将要保护的隐私数据存到链外，可以公开的部分数据放在分布式账本上。

  一种是将原文存到链外，对应的摘要信息存到分布式账本上。这种策略结合安全的哈希算法计算出摘要信息，即便有摘要信息也无法直接逆向推导出原文。但这样简单处理还可能存在隐私泄露的问题，尤其是对于身份证号、性别等取值有限比较通用的数据，容易让攻击者实施字典攻击和暴力破解。对于这类数据需要加上盐值（SALT）再进行哈希后上链。链外存储拥有较强的隐私保护，通常使用在数据存证领域。但这种方法也有缺点，因为原文不在链上，原文的安全存储需要各方花精力自行维护。同时如果在举证的时候一方原文丢失，对手方很可能考虑自身经济利益故意不提供原文，无法真正做到存证，举证的效果。

  另外一种是状态旁路 (State Channels)。在这种策略下，分布式账本上可见的只是粗粒度的“批发”，可以类比出入备付金操作，而真正细粒度的双边或有限多边交易明细，则不作为“交易”记录在分布式账本上， 而仅仅作为有争议事件发生时备查的“信息”单据，通过状态旁路的方式“曲线”执行。比特币体系下的“闪电网络”是在比特币脚本逻辑表达能力受到限制的情况下不得不借助“精巧”的设计实现的事实上的状态旁路。这种策略不仅为交易隐私提供了保护，更提升了交易处理能力。但部分数据还在分布式账本上，状态旁路只能达到部分保护效果。
账本隔离
  账本隔离是将具有不同隐私需求的账本，分别存放到不同的分布式账本上。

  一种是使用多通道（子链模式），隔离账本。Fabric利用多通道(Channel)的机制，实现账本隔离保护隐私性。Channel代表了一个私有的广播通道，保证了消息的隔离性和私密性，不同的链码（Chaincode）关联主体只知道自己Chaincode相关交易和执行交易验证，共识服务只接收相关主体的广播请求和执行对相关主体的消息送达，节点只记录与其相关的Chaincode的状态。

  Fabric在多通道模式下，共识节点会接收所有通道的交易数据，需要对共识节点进行适当的安全管理和技术控制，防止信息泄露。更好的做法是不将全部账本数据都传给共识节点，只提供共识用到的部分信息，通过结合同态加密等技术实现排序共识等功能。以防止在共识节点泄露隐私数据。
  
  例如，如图1所示，peer 1，2和N订阅红色通道，并共同维护红色账本；peer 1和N订阅蓝色通道并维护蓝色账本；类似地，peer 2和peerN在黑色通道上并维护黑色账本。

  另一种是业务数据仅传给参与人，非全网传播，业务数据只落在参与人的账本中。
  
  ## 加密保护
  加密保护是利用密码学算法，对账本数据进行加密做到只有相关方才能够解密查看。加密保护这种策略现在用得比较多。对应的加密算法包括对称加密（如：AES256、SM4）和非对称加密（如:RSA2048、SM2）。  账本加密应该选择加强度高的算法，金融行业需要支持国密算法。由于对称加密速度快，但相对容易破解，而非对称加密算法则相反。所以实际应用中一般会将对称加密和非对称加密算法结合使用。数字信封就是其中一个例子，它可以解决将一个私密数据共享给多个对手方，做到只有这几个对手方能解密查看，其他人都无法获知数据明文。
  除了对账本存储要进行加密，账本传送过程也需要加密。传送过程只有做到端对端加密才是安全有效的，如果依赖于平台或中间节点完成加密，这些节点将可以获取隐私数据。涉及加密还需要考虑密钥的安全性。密钥如果被盗，数据隐私则无从谈起。密钥文件可以保存在服务器、手机钱包、USBKey、加密机等，一般的硬件设备只能使用密钥，不能读取密钥，其安全性将更高。利用门限算法和区块链结合的方式实现了分布式的密钥管理服务，解决用户密钥丢失，无法解密账本的问题。基于门限算法保证了只有凑齐一定数量的密钥分片，才能恢复出密钥，可以防止个别的主体窃取密钥获取隐私数据。
  加密保护的策略也有一定的局限，一方面很难向非技术人员证明加密的安全性，另一方面加密算法的安全级别随着技术的进步将逐步降低。比如量子计算机出来之后，过去许多无法破解的密文将会被量子计算机破解。量子计算机概念的出现也使得人们开始研究在量子计算机下依然安全的密码学方案。目前有量子密码学与革密码学两个重要的分支。
  
  
  ## 部分明文
  部分明文是将分布式账本数据分为敏感部分与非敏感部分，对敏感部分进行隐私保护。

  Corda的一种隐私保护方案采用抽离(Tearoff)部分敏感内容的类盲签名技术，该技术采用把敏感字段和非敏感字段分组哈希，再分层构建Merkel树的方式，使得去掉敏感字段后，剩余的Merkel树仍然具有树状结构和针对非敏感字段的验证价值，可在其基础上达到类似盲签名的效果。意思是说要把交易发给一个对手方时，可以把不需要他知道的具体数据从发送的内容中删除，却仍然不影响他对整个交易签名。有了这个机制，就可以实现交易的隐私保护。那么，没有交易的全部数据，怎么实现对整个交易进行签名呢？Corda将交易的签名结构做成一棵MerkleTree，从而可以实现将一个保留了必要签名的分支发送给签名者，使他仍然能按照签名结构完成对整个交易的签名。同时一旦发生法律纠纷，如已去除的敏感字段内容被伪造，该Merkel树还可用作鉴别证据真伪之用。

## 身份混淆
  身份混淆是将在区块链上交易用户的身份隐匿起来。
  Fabric使用交易证书（TCerts）即每个交易的短期证书，满足一次一密、不可伪造、无关联性和可跟踪性。使得用户不仅以匿名方式参与到系统中，而且阻止了交易之间的关联性。
  还有使用群签名进行身份匿名。群签名是指一个群体中的任意一个成员可以以匿名的方式代表整个群体对消息进行签名。与其他数字签名一样，群签名是可以公开验证的，而且可以只用单个群公钥来验证。
环签名可以认为是特殊的群签名，环签名解决了对签名者完全匿名的问题，环签名允许一个成员代表一组人进行签名而不泄漏签名者的信息。而群签名不能解决这个问题，因为群签名的生成需要群成员的合作，群管理者可以打开签名。对于验证者来说，签名人是完全正确匿名的。环签名的这种无条件匿名性在对信息需要长期保护的一些特殊环境中非常有用。
## 其他方案
1. 零知识证明(zero-knowledge proof)：指的是证明者能够在不向验证者提供任何有用的信息的情况下，使验证者相信某个论断是正确的。区块链的隐私将通过使用“零知识证明”得到进一步提升，除了声明的有效性，这个验证方法并不会透露出其他的信息。这样的项目包括Zcash，一个在公共区块链上的开源加密货币促进支付系统，但是发送方、接收方以及交易的金额，这些信息都是保密的。
2. 同态加密：是基于数学难题的计算复杂性理论的密码学技术。对经过同态加密的数据进行处理得到一个输出，将这一输出进行解密，其结果与用同一方法处理未加密的原始数据得到的输出结果是一样的。如果说，一种加密算法，对于乘法和加法都能找到对应的操作，就称其为全同态加密算法。但直到目前还没有真正可用的全同态加密算法。
3. 安全多方计算平台（MPC）：是保证多个参与方的数据无需先归集后分析，将数据保存在本地进行协同计算。
4. 基于属性加密（ABE）：又称模糊的基于身份的加密（Fuzzy Identity-Based Encryption）。它把身份标识看做是一系列的属性。IBE中解密者，只有当自己的身份信息和信息加密者描述的信息是一致的时候，才可以解密加密者加密的信息。和IBE不同的是，采用基于属性加密后，当用户拥有的属性超过加密者所描述的预设门槛时，用户是可以解密的。在某些特殊场合可以适用。从总体上看，理想的隐私保护策略，如零知识证明、同态加密等大都基于较为复杂的密码学技术，目前在实际应用中有待进一步完善。
  
