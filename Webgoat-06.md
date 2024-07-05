# 加密错误

[TOC]

## Lesson 2

### Base64编码

核心：核心将6位二进制数据，转成8位可读字符。

编码过程：每3个字节一组，生成4个可读字符。不足三个字节，先补0补足三个字节，在编码。

编码完成后：补上的全0字节替换=。

### Assigment

使用burpsuit decoder解题。

![image-20240519184245624](./assets/image-20240519184245624.png)

奖励结算

![image-20240519184341323](./assets/image-20240519184341323.png)

## Lession 3

在这一章节还介绍了其他几种编码方式。

* URL encoding
* HTML encoding
* UUEncode
* XOR encoding

### Assigment

尝试破解给出的数据库密码信息`Oz4rPj0+LDovPiwsKDAtOw==`。先用base64解码获得到`;>+>=>,:/>,,(0-;`。尝试用Python暴力破解。

```python
def xor_string(input_str, key):
    # 将结果存储在一个列表中
    result = []
    # 对字符串中的每个字符进行异或操作
    for char in input_str:
        # 异或操作
        xor_char = chr(ord(char) ^ key)
        # 将结果添加到列表中
        result.append(xor_char)
    # 将列表中的字符连接成一个新的字符串
    return ''.join(result)

# 输入字符串
input_str = ";>+>=>,:/>,,(0-;"

# 调用函数并输出结果
for key in range(0xff):
    output_str = xor_string(input_str, key)
    print(hex(key) + "----->" + output_str)
```

部分输出信息截图所示。根据截图所示，可以找到真正的密码。

![image-20240519191843028](./assets/image-20240519191843028.png)

## Lesson 4

哈希是一种主要用于检测原始数据是否被更改的加密技术(cryptography)。哈希值是从原始数据生成的，基于不可逆的加密技术( irreversible cryptographic techniques)。如果原始数据哪怕只改变一个字节，生成的哈希值也会完全不同。

所以，从某种程度上看，这似乎是一种安全的技术。然而，当用于存储密码时，它不是，也绝对不是一个好的解决方案。问题在于，你可以从字典生成密码，并从这些密码计算出各种变体。对于每个密码，你可以计算一个哈希值，然后将其存储在大型数据库中。所以，每当你找到一个可能是密码的哈希值时，你只需在数据库中查找该哈希值，就能找到密码。

一些哈希算法不再适合使用，例如MD5、SHA-1。对于这些哈希算法，可以通过改变有效负载的方式使其产生相同的哈希值。尽管这需要大量的计算能力，但仍然是一种可行的选项。



### Assigment

网络搜索[在线彩虹表](http://tool.geekapp.cn/decrypt.php)。

![image-20240519202702497](./assets/image-20240519202702497.png)



![image-20240519202605582](./assets/image-20240519202605582.png)

![image-20240519201956852](./assets/image-20240519201956852.png)

## Lesson 6

### 对称加密（Symmetric encryption）

* AES
* 3DES

### 非对称加密（ASymmetric encryption）

* RSA
* DSA

## 签名

签名本质是一个hash，根据hash值检查某段数据的有效性。签名可以与其验证的数据分开提供，或者包含在同一文件中。 （在CMS签名 或 SOAP签名的文件中，文件的部分包含数据，部分包含签名）。

当完整性很重要时签名才背使用。这是为了保证A方发送给B方的数据不被更改。因此，A方通过计算数据的哈希值并使用非对称私钥对该哈希值进行加密来对数据进行签名。然后，B 方可以通过计算数据的哈希值并解密签名来比较两个哈希值是否相同来验证数据。

### RAW签名

裸签名通常由甲计算开始

* 创建数据的哈希值（例如 SHA-256 哈希值）
* 使用非对称私钥加密哈希（例如 RSA 2048 位密钥）
* (可选）使用 Base64 编码对二进制加密哈希进行编码

乙方也必须获得带有公钥的证书。这可能是以前交换过的。所以至少涉及3个文件：数据、签名和证书。

### CMS签名

CMS 签名是一种标准化方式，将数据 + 签名 + 带有公钥的证书全部放在一个文件中从甲方发送到乙方。只要证书有效且未被吊销，乙方就可以使用提供的公钥来验证签名。

### SOAP签名

SOAP 签名还包含数据和签名以及可选的证书。他们全部集中在一个 XML payload中。其计算数据的哈希值的方法涉及一些特殊步骤。特殊步骤涉及到：从一个系统发送到另一个系统的 SOAP XML 可能会引入额外的元素或时间戳。此外，SOAP 签名还提供了由不同方对消息的不同部分进行签名的可能性。

### Email签名

发送电子邮件很容易。只要安格式须填写一些数据并将其发送到转发服务器，最终邮件会到达目的地。为了避免发件人地址被伪造。您可以签署您的电子邮件。受信任的第三方将检查您的身份并颁发电子邮件签名证书。您在电子邮件应用程序中安装私钥并将其配置为签署您发送的电子邮件。该证书是在特定电子邮件地址上颁发的，收到此电子邮件的所有其他人都会看到发件人已验证的指示，因为他们的工具将使用受信任的第三方颁发的公共证书来验证签名。

### PDF或WORD签名

Adobe PDF 文档和 Microsoft Word 文档也是支持签名的示例。签名也与数据位于同一文档内，因此有一些关于什么是数据的一部分以及什么是元数据的部分的描述。政府通常会发送带有包含证书的 PDF 的官方文件。

### Assignment

#### kali通关

Here is a simple assignment. A private RSA key is sent to you. Determine the modulus of the RSA key as a hex string, and calculate a signature for that hex string using the key. The exercise requires some experience with OpenSSL. You can search on the Internet for useful commands and/or use the HINTS button to get some tips.

1. 私钥存文件private.key

   ```
   -----BEGIN PRIVATE KEY-----
   MIIEuwIBADANBgkqhkiG9w0BAQEFAASCBKUwggShAgEAAoIBAQCMruTu1H5AjBG2lVbZfE+ZADQOrpsQq8wsrpWtFcP8wVzwkhY5b+PasJDKI3W3Tyj2nmfnJRySH6Uu3xLQvqXhxPysTKTmgLeRET9G2VCS0kyFcW2JsnKkGgggEou9+b/UkWX81yRPpV9O26bBaqXORu53eRdUcJgpvfVTk7xZT9lFSbRL5r2Qlfnttqgm5VmNnMVYzvBesMeTBANIbYGZ7OjSry/3TkQzxq+9wGqtYHn+OXQQfR2YZ7eJMesGe4Ff/H/cTXTaahZRsjeSALpch6ul8inHUcvHyTibxSCytRtCejMlFNKD+3FSlFhul/DHUw7JqVZ8PwTttqOrvcGDAgEDAoIBABdye30jamAXWEkY487qDUQqs1fHxILHTLIdGPIuS1TK5NLDA7Q9UKRywsxbPkk33CkaZqaGL22v8N0lLc11G6Wg1MdiG3vAHpgtiovO4sMjDMDoPOxIaHCvAVqtwfT+9U4YO6oj22Kbj+J58SA8cPe2fRPpg+NoGVxKU43t9LmND4zOySSYy6P1AO0CenYzhN4dxpqh0KOrgTSKB3+E4gY7fqX+k2uLj1VTONRm0Q/PjNPtmn6kxWSvjsUSIYP5szC0ayIDazuxtpgaN6psmsqdy44ovqEBqYeZzwMa+zzSdAxg2Zdu2DtDQE4ZIJOh8cKZVSw8+6KYQ5UASvW+kmcCgYEAxua7CRXtYEfQqpfdhMPTMGY5Kcl4NHPjXXPy2WLsfXvwrQhxFtdwePtOiaS8+5JG6oEWrnBJ62r9EDTXa4iMW5D+agIhFZoUggZr4t4D37/6aGhT9IcNbEgVWscnOy74ZMckTQS9LltNJAen8eQNlMgdAZmbPRRHkaqhJBLAPC8CgYEAtRG19Fpkl3EHScfKRJ3dC/Kw8++L2BTIYuPU/Ohjt/iXQ+ZGpJqYbziE0R6aiLw8QoWRJqhWkdFNTrXttmYU8qrDkw4X29orTLqpBrVxOdzafeipvXo6ZlIZA8H8AhbN/jEQzJO+ksUay3ZVoxS+sW+eDiagM5hl3cUTvdaGFu0CgYEAhJnSBg6eQC/gcbqTrdfiIEQmG9uleE1CPk1MkOydqP1LHgWgueT1pfzfBm3TUmGEnFYPHvWGnPH+Cs3k8lsIPQtURqwWDma4Vq7ylz6tP9VRmvA3+FoI8tq45y9vfMn67doYM1h+HueIwq/FS+1eYzATVmZnfg2FC8cWGAyAKB8CgYB4tnlNkZhk9gTb2obYaT4H9yCin7KQDdrsl+NTRZfP+w+CmYRtvGWfewM2FGcF0tLXA7YZxY8L4N40eUkkRA33HIJiCWU9PBzd0cYEePYmkzxT8HEo/Cbu4WYCgVKsDzP+y2CIYn8Mg2cyTuPCDdR2SmlexGrNEEPpLg0pOa658wKBgEBrRE+IJfZH/d6Cb8xsTlnAd75GtncsQM7DYo3CXscXx4UBG866Jcqjls55eDnXedQuTLTyH3/yBvMsDdTCtjTlgPe+7AWLBbkfja3FgWSXwkDa2r0mwOE+6FFnCVy8G3rYoEkzPoFOZlIngKR6Hon7dddliWKKYTpxegdAUV7J
   -----END PRIVATE KEY-----
   ```

   

2. openssl生成公钥

   一个私钥可以生成多个公钥。这是因为在一对RSA密钥中，私钥包含了两个重要的参数：模数（modulus）和私钥指数（private exponent）。通过这些参数，可以生成多个公钥，每个公钥都与私钥相关联，但具有不同的公钥指数（public exponent）。

   然而，使用 OpenSSL 生成公钥时，默认情况下 OpenSSL 会生成一个具有标准公钥指数的公钥。如果你希望生成其他公钥，你可以使用 OpenSSL 的选项来指定不同的公钥指数。

   以上的话可以理解为，使用下面这条命令生成的公钥是一个具有标准公钥指数的公钥。也就是说两次执行下面这条命令，如果私钥不变，那么生成的公钥也不会发生改变。

   ```
   openssl rsa -in private.key -pubout > public.pub
   ```

   ![image-20240525021917984](./assets/image-20240525021917984.png)

3. openssl输出公钥信息

   ```
   ┌──(kali㉿kali)-[~/Documents]
   └─$ openssl rsa -in public.pub -pubin -modulus -noout
   Modulus=8CAEE4EED47E408C11B69556D97C4F9900340EAE9B10ABCC2CAE95AD15C3FCC15CF09216396FE3DAB090CA2375B74F28F69E67E7251C921FA52EDF12D0BEA5E1C4FCAC4CA4E680B791113F46D95092D24C85716D89B272A41A0820128BBDF9BFD49165FCD7244FA55F4EDBA6C16AA5CE46EE77791754709829BDF55393BC594FD94549B44BE6BD9095F9EDB6A826E5598D9CC558CEF05EB0C7930403486D8199ECE8D2AF2FF74E4433C6AFBDC06AAD6079FE3974107D1D9867B78931EB067B815FFC7FDC4D74DA6A1651B2379200BA5C87ABA5F229C751CBC7C9389BC520B2B51B427A332514D283FB715294586E97F0C7530EC9A9567C3F04EDB6A3ABBDC183
   ```

   ![image-20240525023419378](./assets/image-20240525023419378.png)
   
   4. 计算签名
   
      ```
      echo -n "8CAEE4EED47E408C11B69556D97C4F9900340EAE9B10ABCC2CAE95AD15C3FCC15CF09216396FE3DAB090CA2375B74F28F69E67E7251C921FA52EDF12D0BEA5E1C4FCAC4CA4E680B791113F46D95092D24C85716D89B272A41A0820128BBDF9BFD49165FCD7244FA55F4EDBA6C16AA5CE46EE77791754709829BDF55393BC594FD94549B44BE6BD9095F9EDB6A826E5598D9CC558CEF05EB0C7930403486D8199ECE8D2AF2FF74E4433C6AFBDC06AAD6079FE3974107D1D9867B78931EB067B815FFC7FDC4D74DA6A1651B2379200BA5C87ABA5F229C751CBC7C9389BC520B2B51B427A332514D283FB715294586E97F0C7530EC9A9567C3F04EDB6A3ABBDC183" | openssl dgst -sign private.key -sha256 | base64
      ```
      
      ![image-20240525031114043](./assets/image-20240525031114043.png)

​		![image-20240525065901841](./assets/image-20240525065901841.png)

#### windows通关

1. 安装openssl，[下载地址](https://slproweb.com/download/Win64OpenSSL_Light-3_3_0.exe)

2. 生成公钥，命令与kali有所区别，推荐使用第一种。

   ```
   这样输出公钥编码格式为utf-8，这样不会影响之后的使用。
   openssl rsa -in private.key -pubout -out public.pub
   这样输出公钥编码格式为utf-16，这会导致之后生成modulus失败，需要重新编码成utf-8
   openssl rsa -in private.key -pubout > public.pub
   ```

​	![image-20240525032809734](./assets/image-20240525032809734.png)

​	![image-20240525033031351](./assets/image-20240525033031351.png)

3. 获得模数，这一步与kali linux中命令相同

   ```
   openssl dgst  -sign private.key -sha256 -out sign.sha256 .\modulus.txt
   ```

4. 签名

   ```
   # 正确的方式
   openssl dgst  -sign private.key -sha256 -out sign.sha256 .\modulus.txt
   
   # 错误的方式
   echo -n "8CAEE4EED47E408C11B69556D97C4F9900340EAE9B10ABCC2CAE95AD15C3FCC15CF09216396FE3DAB090CA2375B74F28F69E67E7251C921FA52EDF12D0BEA5E1C4FCAC4CA4E680B791113F46D95092D24C85716D89B272A41A0820128BBDF9BFD49165FCD7244FA55F4EDBA6C16AA5CE46EE77791754709829BDF55393BC594FD94549B44BE6BD9095F9EDB6A826E5598D9CC558CEF05EB0C7930403486D8199ECE8D2AF2FF74E4433C6AFBDC06AAD6079FE3974107D1D9867B78931EB067B815FFC7FDC4D74DA6A1651B2379200BA5C87ABA5F229C751CBC7C9389BC520B2B51B427A332514D283FB715294586E97F0C7530EC9A9567C3F04EDB6A3ABBDC183" |openssl dgst -sign private.key -sha256 -out sign.sha256
   ```

​	产生错误的原因是使用了'|'管道符生成签名。由于windows管道符号编码格式并不是utf-8，与之前一样采用了utf-16编码，从而导致openssl签名出错。

![image-20240525064414892](./assets/image-20240525064414892.png)

5. base64编码

   ```
   certutil -encode  .\sign.sha256 .\sign.sha256.base64
   ```

   ![image-20240525065518225](./assets/image-20240525065518225.png)

​	![image-20240525065850881](./assets/image-20240525065850881.png)

### 总结

在Windows上使用openssl时不应该使用windows管道符，因为管道符引入的编码问题非常容易导致openssl运算出错。

## Lesson 7

KeyStore是一个用于存储密钥、证书和受信任的证书颁发机构（CA）的容器。Truststore的另一个名字是Truststore

### 依据存储类型划分Keystore

* 基于文件的Keystore
* 基于数据库的Keystore
* 基于硬件的Keystore

### 管理Keystore

当您访问一个网站并且浏览器显示证书正常时，这意味着该网站使用的证书是由受信任的证书颁发机构（CA）颁发的。然而，这些受信任的证书颁发机构的列表是被管理的。某些 CA 可能会被撤销或移除，这些更新会在浏览器更新时在后台进行。不仅浏览器维护一个受信任的证书颁发机构列表，操作系统也维护这样的列表。此外，Java 运行时环境也有自己的列表，保存在 `cacerts` 文件中。操作系统和 Java JRE 的更新会保持这个列表的最新状态。在企业环境中，这些列表通常由公司维护，并且还包含公司根证书。

### 使用 DNS CAA 记录对网站证书进行额外检查

有些公司会检查所有或大部分的互联网流量，即使是那些您认为是端到端安全连接的流量。其工作原理如下：员工打开浏览器并搜索一些信息。浏览器会使用 HTTPS 并访问 Google 的网站。链接看起来是真实的，浏览器中也显示了锁定图标。然而，如果您检查证书，可能会注意到它是由您的公司根 CA 颁发的！所以，您实际上是与公司的服务器建立了一个端到端的安全连接，而该服务器则与 Google 建立了安全连接。为了防止这种中间人攻击连接到您的服务器，现代浏览器现在还会检查 DNS CAA 记录，以确认某个颁发机构是否被允许为某个网站颁发证书。更多信息请参阅：Wiki DNS CAA。

### Let's Encrypt 提供的免费证书

[Let's encrypt](https://letsencrypt.org/)是一个免费、自动化和开放的证书颁发机构。它允许您为您控制的网站创建有效的证书。通过遵循和实施特定协议，您的身份将得到检查并颁发证书。证书是免费的，这样做是为了鼓励使用授权证书并降低互联网上自签名证书的使用。证书有效期为 90 天，因此需要自动更新。（这确保身份/所有权证明也经常进行



## Lesson 8

所有类型的系统中的一个大问题是使用默认配置。例如路由器中的默认用户名/密码、密钥库的默认密码、默认未加密模式等。

### Java cacerts

您是否曾经更改过Java cacerts的密码？给 `cacerts` 文件设置密码非常有效。当需要保护受信任的证书颁发机构，且不希望轻易添加未知的自签名证书颁发机构时，这一点尤为重要。

### 保护你的Id_rsa私钥

你是否为 GitHub 或其他网站使用 SSH 密钥，并将其未加密地存放在磁盘上？甚至放在云盘上？默认情况下，生成 SSH 密钥对会将私钥未加密地保存。这使得使用起来很方便，如果存储在只有你能访问的地方，也能提供足够的保护。然而，最好还是对密钥进行加密。当你想要使用该密钥时，需要再次提供密码。

### 连接到服务器的SSh的账号密码

当您从某个托管服务提供商处获取虚拟服务器时，通常会有许多不太安全的默认设置。其中之一是 SSH 服务运行在默认端口 22 上，并允许使用用户名/密码进行尝试。您应该做的第一件事之一是更改配置，使得无法以 root 用户身份进行 SSH 登录，且无法使用用户名/密码进行 SSH 登录，而只能使用有效且强壮的 SSH 密钥进行登录。否则，您会注意到不断有暴力破解尝试登录到您的服务器。

### Assigment

1. 安装docker

2. 运行命令`docker run -d webgoat/assignments:findthesecret`

   ![image-20240525075052842](./assets/image-20240525075052842.png)

3. 登录docker 虚拟机`docker exec -it -u root baefa95efd64 /bin/bash`，获取密钥

   ![image-20240525075932938](./assets/image-20240525075932938.png)

4. 解密

   ```
   echo "U2FsdGVkX199jgh5oANElFdtCxIEvdEvciLi+v+5loE+VCuy6Ii0b+5byb5DXp32RPmT02Ek1pf55ctQN+DHbwCPiVRfFQamDmbHBUpD7as=" | openssl enc -aes-256-cbc -d -a -kfile default_secret
   ```

   ![image-20240525080404842](./assets/image-20240525080404842.png)

5. 通关截图

   ![image-20240525080531104](./assets/image-20240525080531104.png)
