---
layout: post
comments: true
title: manjaro使用心得、中文(注音)輸入法調整
excerpt: "最近決定要將正式從windows畢業，投入linux的懷抱。在接受擁抱前，我決定先用虛擬機測試，寫下調整環境的方法。"
---

最近決定要將正式從windows畢業，投入linux的懷抱。在接受擁抱前，我決定先用虛擬機測試，寫下調整環境的方法。

我使用manjaro時，所看到的第一個優點是，他在虛擬機中的畫面清晰，而且視窗會自動放大，全螢幕看起來跟真的安裝了作業系統沒兩樣。而且滑鼠很可愛。:smile:

<br>

如果想安裝manjaro作為你的作業系統，建議可以先在虛擬機上安裝、試用。
先下載你喜歡的[Manjaro的`.iso`檔](https://manjaro.org/get-manjaro/)、[虛擬機環境(virtual box)](https://www.virtualbox.org/wiki/Downloads)。
我選擇使用Manjaro 17.1.11 KDE，因為就預覽畫面來看，他比較接近windows作業系統給我的感覺，所以決定先使用它。

<br>

[TOC]



### Step1. 安裝virtual box
網路上已有很多教學，在此略過不談。


### Step2. 新增一台電腦(僅硬體，且為虛擬的)

我的配置：

OS: Arch linux 64-bit
Ram: 4 GB
Rom: 40 GB

我在安裝時選擇：

時區： Asia/Taipei
語言選擇： English(Default)
(密碼: manjaro(Default))
<br>
會選擇英文安裝的原因是之前在安裝ubuntu時，中文安裝容易出錯，為求保險，因此選擇安裝完畢再進行語言切換。


[切換語言(圖形介面)](#切換語言(圖形介面))
[切換語言(文字介面)](#切換語言(文字介面))


#### 切換語言(圖形介面)

語言切換的方法是點選左下角，在出現的工具列中的搜尋輸入`language`，即可找到修改的工具。

![](https://i.imgur.com/s4SsbjO.png)

點選右下方`Add languages`，在跳出的選單中選擇對應語言。

(如果你跟我一樣，選單出來就是最小化的視窗，要記得點擊視窗的上方或下方拖放，才選得到語言)
<br><br>

![](https://i.imgur.com/uFciCCk.png)

接著將選擇的語言調整至最上方，按下`Apply`。之後只要再次登入設定就會生效。

<br>

#### 切換語言(文字介面)

```
sudo pacman -S vim
```
首先，我是vim的愛用者，所以在編輯檔案前，會先安裝vim。令我意外的是，pacman使用`tab`鍵沒有辦法確認可安裝的套件有哪些，要按下`Enter`才能知道有沒有這個套件。

透過修改`/etc/locale.gen`文件，將需要的語系前的註解(#)刪除。然後將已經解除註解的語系重新註解。

希望環境是中文的話可以將語系en_US.UTF-8註解，將語系zh_TW.UTF-8前的註解刪除。

```
#en_US.UTF-8
zh_TW.UTF-8
```

接著執行locale-gen，重新設定語系。
```
locale-gen
```
<br>

### Step3. 安裝中文輸入法
因為看到一篇文章，[讓 Linux 下的中文輸入法更接近微軟新注音使用體驗（以Ubuntu、Linux Mint為例）](http://goodjack.blogspot.com/2013/08/linux-phonetic-setting.html)，hime輸入法系統經過調整後可以較接近新注音的使用方式。



首先，manjaro屬於Arch Linux，他的套件管理為pacman以及**A**rch **U**ser **R**epository(簡稱AUR)，因為AUR需要使用者對套件安裝原理了解較深，因此較不適合新手。

另外有一個**Y**et **A**n**O**ther **U**ser **R**epository **T**ool(簡稱Yaourt)作為使用AUR的輔助。等等就是需要Yaourt來安裝hime-git。

<br>







好吧，其實我閱讀完我的參考資料(第一個)，本以為需要寫下一長串的安裝步驟，但沒想到2018/07/19已經可以直接使用pacman安裝`yaourt`了。如果讀者無法使用`pacman`進行`yaourt`安裝的話，請參考[此文章](https://cms.35g.tw/coding/arch-linux%E5%AE%89%E8%A3%9Dyaourt%E9%9D%9E%E5%AE%98%E6%96%B9%E5%A5%97%E4%BB%B6%E7%AE%A1%E7%90%86%E5%93%A1/)。

```
sudo pacman -S yaourt
```


選擇1個packages安裝。很可惜的是，我並不清楚選擇packages安裝的原因。
```
yaourt hime-git

# 1 n n y y y y
```


![](https://i.imgur.com/H4dcokr.png)

不用編輯(Edit)檔案，使用預設的值就好，然後同意建置(building)以及安裝(installation)。

<br>

下方是我當時安裝時的選擇，可以對照著做。

![](https://i.imgur.com/lbLw0lL.png)
不編輯`PKGBUILD`。

![](https://i.imgur.com/Pc7YorG.png)
不編輯`hime-git.install`。

![](https://i.imgur.com/Hta6RSl.png)
繼續建置`hime-git`。

![](https://i.imgur.com/M4udQo8.png)
開始安裝。這一段的安裝會比較久，可以起身去倒一杯茶，再回來就差不多好了。

![](https://i.imgur.com/0yVpzuD.png)
繼續安裝。

![](https://i.imgur.com/VjSuoa8.png)
繼續安裝。

<br>

### Step4. 調整輸入法(新注音)

接下來，為了開機登入後，開啟每個頁面都可以直接使用hime輸入法，需要一些環境變量自動在窗口管理器(window manager)執行前匯出。


:::warning
在繼續進行下去之前，我要提醒讀者，接下來的一段需要有十足的實驗精神，為了未來良好的打字體驗，請耐心的按照下方指示進行環境變量的匯出。
:::



讀者可以先閱讀[Gnome/KDE/Xorg 底下的個人啟始命令稿: 到底是 .xinitrc .xsession 還是 .xprofile?](https://newtoypia.blogspot.com/2012/06/xinitrc-xsession-xprofile.html)，明白自己到底應該要將環境變量寫入哪一個檔案之中，才不會試了又試，徒勞無功又對著電腦生氣。:smile:

<br>

以下是我判別我應當使用哪一個檔案的方法：
我先使用
```
ls -a
```
將家目錄下的`.隱藏檔`列出。

<br>

我本身有`.xinitrc`，也有`.xprofile`，先將他們使用`cp`指令將他們備份起來。如果讀者的作業系統有`.xsession`，也將之備份起來。

```
cp .xinitrc .xinitrc.origin
cp .profile .profile.origin
```

我寫`.origin`並沒有什麼特殊功能，只是為了方便標示這一份是原本的檔案，讀者想要改寫成`cp .xinitrc .xinitrc.copy`也可以，方便辨識即可。

<br>

我使用的是`vim`編輯器，我使用至少30次的`dd`將`.xinitrc`中的指令刪除（如果不清楚`vim`編輯器的使用法，因為已有很多教學，此處再不贅述），除了最後一行`exec`需要的變數以外，其餘的指令全數刪除。

<br>

接著一次在一個檔案的頂部加入：
```
(echo -n "$0 is called at " ; date) >> ~/startup.log
```
然後登入再登出，檢視`startup.log`內有無新增紀錄。

<br>

選擇有出現紀錄的`.隱藏檔`(以Manjaro KDE 為例，是`.xession`)，加入以下變量：

我在live開機時是`.xession`沒錯，但是正式安裝卻變成要使用`.xprofile`。
```typescript
export LANG="zh_TW.UTF-8" #如果在安裝作業系統時，已經選擇中文，那麼LANG, LC_CTYPE變數都不需要export，刪除此兩行即可。
export LC_CTYPE=zh_TW.UTF-8

export GTK_IM_MODULE=hime
export QT_IM_MODULE=hime
export XMODIFIERS=@im=hime

hime &
```
<br>

這是我的`.xession`，`.xprofile`，供大家參考。`hime &`是為了讓hime程式一直執行。
```typescript
export GTK_IM_MODULE=hime
export QT_IM_MODULE=hime
export XMODIFIERS=@im=hime

hime &
```

<br>

重新登入後，應該可以看到hime已在下方工作列執行，所使用的輸入法是詞音，預設切換方式為`ctrl alt + 6`，我並沒有更動切換方式。

讀者可以按照[104.10.08 Kali Linux2.0 (三)中文輸入(hime)安裝過程](https://aben20807.blogspot.com/2015/10/1041008-kali-linux20_8.html)指示進行調整，我只有一個地方不同，我並沒有選擇`所有程式共用相同的輸入法狀態`。

可以在linux環境下打新注音了！

<br>

###### 參考資料：
[Arch Linux Yaourt AUR套件管理](https://cms.35g.tw/coding/arch-linux%E5%AE%89%E8%A3%9Dyaourt%E9%9D%9E%E5%AE%98%E6%96%B9%E5%A5%97%E4%BB%B6%E7%AE%A1%E7%90%86%E5%93%A1/) by danny
[讓 Linux 下的中文輸入法更接近微軟新注音使用體驗（以Ubuntu、Linux Mint為例）](http://goodjack.blogspot.com/2013/08/linux-phonetic-setting.html) by 林小克
[107.02.20 manjaro 安裝 vim](https://aben20807.blogspot.com/2018/02/1070220-manjaro-vim20.html) by 黃柏瑄 
[HIME Input Method Editor](https://github.com/hime-ime/hime) by hime-ime
[107.02.20 manjaro 安裝中文輸入法 hime](https://aben20807.blogspot.com/2018/02/1070220-manjaro-hime20.html) by 黃柏瑄
[在 Arch Linux 安裝 Hime](https://www.willychen.org/165/install-hime-on-arch-linux/)
[Window manager](https://wiki.archlinux.org/index.php/Window_manager) by Arch Wiki
[Xprofile](https://wiki.archlinux.org/index.php/Xprofile) by Arch Wiki
[解析环境变量XMODIFIERS/GTK_IM_MODULE](https://blogs.gnome.org/raywang/2007/01/26/%E8%A7%A3%E6%9E%90%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8Fxmodifiersgtk_im_module/) by raywang
[104.10.08 Kali Linux2.0 (三)中文輸入(hime)安裝過程](https://aben20807.blogspot.com/2015/10/1041008-kali-linux20_8.html) by 黃柏瑄 
[Gnome/KDE/Xorg 底下的個人啟始命令稿: 到底是 .xinitrc .xsession 還是 .xprofile?](https://newtoypia.blogspot.com/2012/06/xinitrc-xsession-xprofile.html)
[Localization/Traditional Chinese (正體中文)](https://wiki.archlinux.org/index.php/Localization/Traditional_Chinese_(%E6%AD%A3%E9%AB%94%E4%B8%AD%E6%96%87)) by Arch Wiki


###### tags: `Blog`