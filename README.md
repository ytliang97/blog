# myblog


使用基於Poole的Lanyon，使用Jekyll製作。

要修改的話需要gem創立jekyll server環境，讓本地端的修改可以即時呈現在localhost:4000。

想將Lanyon加入留言區。因為github沒有資料庫的設定，所以留言區需要通常需要使用第三方插件儲存。

不過上個禮拜發現了utterance的utterances，可以將留言跟issue串接。

我看了[Utterances 免費開源、無廣告、無追蹤的網站留言系統 by jkgtw](https://www.jkg.tw/p3350/)的教學，直接點他提供的APP連結。

不太懂為什麼utteraces的github頁面找不到那個鏈結。

install後，就跑到下圖這個頁面。

![](https://i.imgur.com/4G7u8z9.png)

我輸入ytliang97/ytliang97.github.io


下面是留言配對issue的方式，我直接先選擇第一種，反正應該可以改。

![](https://i.imgur.com/E7od2Ug.png)

本來想選第三種的，但覺得還是用比較unique的好了。



Issue label留空預設Comments

![](https://i.imgur.com/kh85z0i.png)


comments可以有很多種顏色

![](https://i.imgur.com/B68PRUD.png)

我選了個很炫的橘色XDD


我直接把`<script></script>`放在page.html的最下方，不過jekyll server看上去沒變化。

所以直接推看看。


沒用，我去看有人有bug是在網域名有點點`.`的情況下，我有兩個點。

不過也有人提到說已經fixed了，所以我還是新增一篇文章試試好了。


但還是不work。


所以我就去查lanyon的設定要怎麼加入comment

[6. Integrating Disqus comments by Nikhita Raghunath](https://www.nikhita.dev/build-blog-using-github-jekyll#disqus)

我查了comments的用法，把default.html加入include的語法，然後在`_includes/`把之前用失敗的disqus改成comments.html

再把page.html那裡的script移過去，希望能work。


這次有來信報錯`The page build failed for the `master` branch with the following error:`

我把dufault.html那裡的disqus相關的code刪除，再把if page.comment的語法加到comments.html那裡