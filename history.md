# 介绍发展历史
利用同态加密来保护数据的私密性，这个概念最早是由Rivest等人于1978年提出。该想法的来源是通过加密函数的同态性质，实现对加密后的数据作运算的同时保护数据的私密性。
这个概念被提出后，密码学家设计出很多具有同态性质的加密方案，如实现电子投票的体制、保密信息检索协议等。近几年，云计算受到广泛关注，而它在实现中遇到的问题之一是
如何保证数据的私密性。全同态加密可以在一定程度上解决这个技术难题。
全同态加密指同时满足加同态和乘同态性质，可以进行任意多次加和乘运算的加密函数。
2009年，IBM的研究人员Gentry第一次设计出一个真正的全同态加密体制，即可以在不解密的条件下对加密数据进行可以在明文上进行的运算。使得对加密信息仍能进行深入和无限
的分析，而不会影响其保密性。经过这一突破，存储他人机密电子数据的电脑销售商就能受用户委托来充分分析数据，不用频繁与用户交互，也
不必看到任何隐私数据。同态加密技术将允许公司将敏感信息存储在远程服务器上，既可以避免本地的主机端发生泄密，又可以保证信息的使用和搜索。用户也得以使用搜索引擎进
行查询并获取结果。而不用担心搜索引擎会留下自己的查询记录。
## 研究现状
Gentry的同态加密体系是基于理想格设计的，主要思想是先构造一个部分同态加密的体制，该体制仅能保证对明文进行较低次数的多项式运算时的同态性，然后利用Squash技术降
低解密好事的多项式次数，实现Bootstrapping，通过在不知道密钥的情况下更新密文，从而减少扰动，实现全同态加密。该体制在期望的安全强度上远达不到实用程度。

## 最新发展