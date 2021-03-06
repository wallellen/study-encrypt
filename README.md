摘要和加密算法
==============

 

信息安全
--------

-   在当前TCP/IP协议架构下，数据报文的内容是必然会泄露的，泄露的报文有可能导致如下问题：

    -   信息泄露：

    -   信息伪造：

    -   信息篡改：

     

-   为了信息安全，在互联网上传输敏感信息，都应该采用一种安全方案，以达到：

    -   报文泄露但信息不会泄露；

    -   可以识别被伪造的信息；

    -   可以发现被篡改的信息；

 

摘要算法
--------

-   简介：摘要算法的目的不是为了加密信息，而是将整个信息的内容“提取为”一段简短的数据，这段数据称为原信息的“摘要”，目前的摘要算法主要是“单向散列”算法。

-   特点：

    -   摘要具有不可逆性，即可以通过原信息得到摘要，但无法通过摘要推导出原信息；

    -   摘要具有可重复性，即原信息相同，则得出的摘要数据一定是相同的；

    -   可以认为摘要和原信息具备一对一关系，即大部分情况下，不同信息得出的摘要一定是不同的；

-   用途：

    -   作为信息的索引；

    -   防止信息被篡改；

    -   比较信息是否相同；

-   碰撞：

    -   所谓的碰撞表示使用两组不相同的信息却能得到相同的摘要结果，目前MD5碰撞的方法已被找到，但条件非常苛刻，所以可以认为MD5，SHA这类算法仍然是非常可靠的；

-   常用算法：

    -   MD5(Message-Digest Algorithm
        5)：是一种单向散列算法（Hash算法），由MD4发展而来，可以通过任意长度的byte序列得到一个长度为128bit的散列值；MD5的优点是计算速度较快（主要是通过位运算和位移运算，经过多次迭代而成），缺点是Hash结果长度较短，被计算出碰撞的可能性较大；

    -   SHA1(Secure Hash Algorithm
        1)：由于MD5的碰撞算法被公布，SHA算法被提出，但不久SHA的碰撞算法也被找到，所以SHA的一个改进版SHA1被提出。SHA1和MD5的原理类似，只是在计算Hash时增加了一些“填充因子”，以求得到分布性更好的结果。SHA1可以得到160bit长度的散列值，计算性能和MD5相当；

    -   SHA2：SHA1的碰撞算法被公布后，SHA2的应用被提上日程，包括Gun，Apache，Google等组织都推荐使用SHA2来替代SHA1。SHA2是SHA1的小改动版本，根据得到散列值的长度，被分为SHA-224，SHA-256，SHA-384和SHA-512；

-   安全性：

    -   作为信息摘要算法和数字签名算法，上述的散列算法都比较安全；

    -   作为信息认证算法，由于存在碰撞的可能性，应当尽量选择散列结果较长的算法；

    -   作为密钥验证算法，由于存在暴力破解字典，应当选择一些变体算法，避开常见的破解字典；

 

 

加密算法
--------

 

### 对称加密

-   原理：

    -   对称加密表示，加密解密使用同一套密钥。对称加密会破坏原有信息，将原信息的信息位进行位置变换或者值置换，而进行这些操作依赖于密钥动态产生密码表或置换规则表；

    -   加密操作是可逆的，通过密钥产生的规则，可以将密文通过加密的逆运算得到原文，也就是解密；

    -   加密、解密是相关于密钥的一对可逆运算，即`z=fx(x,y)`的关系，所以已知其中任意两个变量，都可以推导出第三个，但如果只有一个变量（即只知道密文），则很难推断出其它两个变量，这就是对称加密的基本安全保证；

-   用途：

    -   由于加密的算法是公开的，所以对称加密并不保证不被解密，但可以保证难以解密（即在有限资源有限时间内无法解密）；

    -   对称加密不应用来长时间加密重要信息，每隔一段时间都应该更改密钥或者更换加密算法；

    -   对称加密的算法效率都比较高，所以可以用来一次性加密大量信息；

-   常用算法：

    -   DES/3DES：老牌的加密算法，居有较强的抗破解能力和高性能。算法简单，便于通过硬件实现，被大量的用于金融行业（如ATM机），安保系统（如门禁）。DES的最大缺点是密钥长度过短（只有56位），可以在有限时间内被暴力破解，而3DES则弥补了这个问题，利用三组密钥，通过DES对信息进行处理（第一组密钥加密，再用第二组密钥解密，最后用第三组密钥再次加密），将密钥的长度增加到156位；

    -   AES：是DES的替代算法，是美国政府现行的加密算法标准。该算法可以根据需要选用128\~256位密钥长度，进行不同强度的加密；

    -   RC2/RC4/RC5：一组由RSA实验室推出（或参与设计）的加密算法，目前RC2/RC4都不再推荐使用，RC5是一种分组加密算法，可以对信息进行分组加密（而非一次性加密完毕），并且可以对分组大小，密钥长度和加密迭代次数进行定义，非常适合流式数据加密，被广泛用于无线通信领域（例如WIFI，手机通讯等）；

    -   其它算法：诸如IDEA，SKIPJACK等加密算法也在某些领域广泛使用；

-   密码：

    -   密钥和密码指的是不同的含义；

    -   密码是获取密钥的途径，而非加密数据的参数；

    -   一般来说，密钥都居有固定的长度，居有一些强度要求（例如不允许出现重复数据段），但密码只是相当于密钥的助记符；

-   安全性：

    -   常用的对称加密算法在安全性上都没有问题；对于对称加密的主要攻击方式在于获取密钥，而非暴力破解密文；

    -   获取密钥的主要方式在于：密钥存储漏洞，密钥分发漏洞；

        -   从存储密钥的介质上获取密钥（例如U盘）；

        -   获取密钥对应的密码（口令），从而获取密钥；

        -   通过在互联网上劫持信息获取密钥；

     

### 非对称加密

-   原理：

    -   非对称加密主要解决了对称加密的密钥分发和存储问题。非对称加密采用一对密钥（A和B）：其中A密钥自己保存，不进行任何分发，保证不被泄露，称为私钥；而B密钥则无需保密，可以任意分发；

        -   公钥：公钥持有方利用公钥对数据进行加密，但无法通过该公钥进行解密，只能将密文交由私钥持有方进行处理；

        -   私钥：私钥持有方可以解密通过对应公钥加密的数据；

-   算法：

    -   非对称加密主要是用RSA算法，目前没有任何已知方法可以非暴力破解RSA加密的数据，而RSA的密钥长度可以达到4096位甚至更长，有效的抵抗了暴力破解，RSA是ISO指定的非对称加密标准；

    -   RSA算法由Ron Rivest，Adi Shamir和Leonard
        Adleman共同创建，主要利用了数字的互质关系和欧拉函数；

    -   ECC算法利用椭圆曲线问题计算公钥和密钥，效率比RSA要高，据说会在未来取代RSA；

-   用途：

    -   数字签名：使用摘要进行的数字签名无法避免被篡改的问题，但如果对数字签名结果用私钥加密，在验证签名时用公钥解密，由于除了签名方无人可持有私钥，所以用公钥解密的结果如果可以验签成功，则一定可以保证信息是私钥方发布的原始信息，绝无篡改；如验签失败，则一定是信息被篡改或签名被篡改；

    -   数据加密：公钥持有方对数据进行加密后可将数据任意发布，但只有私钥持有方获得数据后可以对其进行解密，其它任何人无法成功完成解密；

    -   身份验证：持有私钥的一方可以在任意时刻可以向公钥持有方证明自己的身份，而无需向任何人公布自己的公钥，只需要将一段双方都认可的信息（例如Hello
        World）用私钥加密，发给公钥持有方，如果能成功解密，则公钥方可以放心认可私钥方的身份；

-   证书：

    -   证书是一种保存密钥的文件格式，包括：

        -   产生密钥的组织机构

        -   密钥内容

        -   密钥的签名

        -   密钥的有效期

    -   证书可以只包括公钥，也可以包含公钥和私钥，取决于双方采用何种方式通信（单向信任或双向信任）；

 

SSL/HTTPS
---------

-   安全套接字的目标是改善TCP/IP协议本身的安全性问题，采用基于证书的单向信任方式：

    -   A产生一对密钥，并将其中的公钥给B；

    -   B产生一个对称密钥，利用公钥加密后给A；

    -   A将得到的对称密钥签名后给B，B进行验签，确认A持有的对称密钥合法；

    -   A和B通过对称密钥进行安全通讯；

-   基于SSL的HTTP通讯称为HTTPS通讯，该方式可以完全保证客户端到服务器的通讯是安全可靠的；
