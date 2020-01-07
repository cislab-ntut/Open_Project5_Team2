Project2-5
===
### 私鑰分割
#### 第2組 

## 前提

在做區塊鏈應用的時候，最常碰到的一個問題就是，怎麼保管私鑰，怎麼讓使用者方便，但又同時是安全的。第一個想法就是備份金鑰（不論是passphrase/keystore/私鑰），但是如果把使用者金鑰(加密)備份到自己的server，只要server的安全上有個不小心，使用者的金鑰就可能就被盜取了，就算是加密過的，也難保不會被破解。那如果切成好幾部分，有好幾份備份呢? 那怎麼切，才能確保安全呢？

## 定義
	機密共享是指在一組參與者之間分配機密的方法，每個參與者都分配了該機密的一部分。只有將足夠數量（可能是不同類型）的份額組合在一起時，才能重建機密。單獨的股份本身沒有用處。在一種類型的秘密共享方案中，有一個發牌人和n個玩家。經銷商將秘密的份額提供給玩家，但是只有當滿足特定條件時，玩家才能夠從他們的份額重新構造秘密

![](https://imgur.com/uBCzolF.png)

### Shamir's Secret Sharing - 介紹(Lagrange polynomial)
		Shamir's Secret Sharing is an algorithm in cryptography created by Adi Shamir. It is a form of secret sharing, where a secret is divided into parts, giving each participant its own unique part.
	To reconstruct the original secret, a minimum number of parts is required. In the threshold scheme this number is less than the total number of parts. Otherwise all participants are needed to reconstruct the original secret.

	Shamir’s Secret Sharing 是由 Adi Shamir 創建的密碼學演算法。Shamir’s Secret Sharing 是「Secret Sharing」的一種實現。具體工作如下：
	現有N個參與者，其中一個參與者將一段私鑰、密碼或是敏感訊息分為N個加密片段，設定恢復值為M，其中(M < N)，將每個唯一的片段分給每個參與者，各自妥善保管。要恢復原始私鑰、密碼或是敏感訊息，需要至少<個加密片段。通常我們將此類加密解密的方式稱為多簽，
	比如設置3/5多簽，需要至少3個人簽名（Shamir’s Secret Sharing Scheme 中叫做提供加密的片段）才能發送交易（Shamir’s Secret Sharing Scheme 中叫做解密原始私鑰、密碼或者敏感訊息

![](https://imgur.com/K2ltjnM.png)
## 主要步驟

	1.如何拆分
	2.如何還原

## 如何拆分

	先定義一下要我們接下來要幹嘛
	把秘密（secret）拆分成Ｎ份，並且只需Ｍ（M < N）份即可組回完整的秘密，被拆分過的每一份秘密叫做share。
	建立一個(M-1)次方程式
	假設，我們要傳遞的秘密是數字3，然後希望3份（M=3）就能回復完整的秘密，所以要建立一個一元二次（Ｍ-1）方程式，y=ax ²+bx+c。
	a跟b可以任選，而c是秘密（在我們的例子就是3），我們選a=2, b=1，所以方程式為y=2x ²+x+3。

![](https://imgur.com/KsJuZTo.png)
      
	接著，我們任意取三點（因為M=3），（1, 6）,（2, 13）,（-2, 9）

![](https://imgur.com/WeaQaJC.png)

	而這三個點座標就是三個share。最終可以由這三個點座標(share)還原秘密(secret)。

## 如何還原

	目標是，還原(M-1)次方程式。
	若能取得原本的方程式，即可拿到秘密。我們知道兩點可以成一直線，而三點可以定義一個拋物線。現在，我們有三個點（1, 6）,（2, 13）,（-2, 9），但不知道曲線

![](https://imgur.com/nkwRfKi.png)

	不過M=3，所以可以知道方程式為y=ax ²+bx+c，然後，我們知道三個座標（1, 6）,（2, 13）,（-2, 9），
	（1, 6） => c = a + b -6
	（2, 13） => c = 4a + 2b -13
	（-2, 9） => c = 4a -2b - 9

	過程就不推導了，最後就可以得出a=2, b=1然後我們的秘密數字c=3。

	另一種解法: Lagrange polynomial

![](https://imgur.com/h5dkkbA.png)
	我們目前已知三點
	f(1) = 6
	f(2) = 13
	f(-2) = 9
	帶入上述多項式，可得

![](https://imgur.com/FfrEE6K.png)
	最後得到一樣的方程式2x²+x+3。
	在Shamir Secret Sharing中，需要選定一質數Ｐ，而Secret需要小於質數。所以最後方程式得出的值需要再對質數Ｐ取餘數。


### Asmuth-Bloom Secret Sharing - 介紹(Chinese Remainder Theorem)	

## 主要步驟

	1.參數選取
	2.如何拆分
	3.如何還原


### 參數選取
	令p是一個大質數，m_{1}, m_{2}, ..., m_{n} 是n個嚴格遞增的數，且滿足下列條件：
![](https://imgur.com/9xYr4z7.png)


### 如何拆分

![](https://imgur.com/XjfAnLI.png)
### 如何還原

![](https://imgur.com/FilORQO.png)


# 額外想法
## 能否在非常數項放置secret

令一多項式為![](https://i.imgur.com/psKcMMI.png)，其最大次方為n。

- CASE1: secret放在常數項a0
    - 需要(n+1)個座標才能找出secret


- CASE2: secret放在其他項係數
    - 若n為**偶數**
        - **有機會**用n個點找出**奇數**次方項的係數
        - 也就是不需要到門檻，便有可能找出secret
    - 若n為**奇數**
        - 可用(n+1)個點找出**奇數**次方項的係數，跟基本門檻一樣
    


### 小結論
- 如果要在常數項以外的係數放置秘密
    - 放在**偶數**次方項的係數目前看起來是可行的
    - 但若要放在**奇數**次方項的係數，則n得為**奇數**


