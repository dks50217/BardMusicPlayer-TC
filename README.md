
<details>
  <summary>目錄</summary>
  <ol>
    <li><a href="#about">關於 BardMusicPlayer-TC</a></li>
    <li><a href="#tc">繁體中文版 FFXIV 支援</a></li>
    <li><a href="#plugin">DalamudDoot</a>
      <ul>
        <li><a href="#prerequisites">前置需求</a></li>
        <li><a href="#installation">安裝步驟</a></li>
        <li><a href="#usage">使用 DalamudDoot</a></li>
      </ul></li>
    <li><a href="#contributing">貢獻</a></li>
  </ol>
</details>

<section id="about">

# 關於 BardMusicPlayer-TC
  <p>BardMusicPlayer 是一個自動化 MIDI 音樂播放器，使用 FFXIV 遊戲內的詩人演奏模式來播放 MIDI 歌曲。</p>
  <p>本分支 (<strong>-TC</strong>) 在原版 BardMusicPlayer 基礎上，新增對<strong>最終幻想XIV 繁體中文版</strong>的支援。</p>
</section>

<section id="tc">

# 繁體中文版 FFXIV 支援

本分支針對 TC 版本客戶端做了以下修改：

* 新增 `GameRegion.TC` 地區識別
* 新增 Sharlayan 記憶體讀取的 TC 專用 Signatures / Structures JSON
* Seer 自動偵測 TC 客戶端（透過 USERJOY 登錄檔、安裝路徑、語言檔案等方式識別）
* 設定檔路徑對應至 `我的文件\My Games\FINAL FANTASY XIV - TC\`

</section>

<section id="plugin">

# DalamudDoot
BardMusicPlayer 可搭配 <a href="https://github.com/BardMusicPlayer/DalamudDoot">DalamudDoot</a> 外掛使用，以獲得更多進階功能。

<details>
<summary>進階功能</summary>

    * 輸出歌詞
    * 演奏中使用聊天
    * 直接開啟 / 關閉樂器
    * 直接進行合奏準備 / 確認
    * 改良的音符演奏
    * 圖形設定切換
    * 靜音切換

    以及更多功能！
</details>
</section>

<section id="prerequisites">

### 前置需求

* 安裝<a href="https://www.finalfantasyxiv.com/" alt="Final Fantasy XIV">最終幻想XIV 繁體中文版</a>
* 安裝 <a href="https://github.com/goatcorp/FFXIVQuickLauncher#how-to-install-the-launcher" alt="XIVLauncher">XIVLauncher</a>
* 新增自訂<a href="#installation" alt="repository">套件庫</a>
</section>

<section id="installation">

### 安裝步驟
* 複製以下套件庫連結：<br>
  `https://dl.bardmusicplayer.com/dalamuddoot` <br><br>
* 在遊戲聊天框輸入 `/xlsettings` 開啟 Dalamud 設定。
* 切換至「Experimental」頁籤。
  <br><br><a><img src="https://i.imgur.com/FDlwtbe.png" /></a><br><br>
* 將連結貼入「Custom Plugin Repositories」下方的輸入框。
* 確認連結右側的核取方塊已勾選。
  <br><br><a><img src="https://i.imgur.com/YvPZ7cN.png" height="180" /></a><br><br>
* 點擊設定視窗右下角的「Save changes & close」。
* 在 `/xlplugins` 插件瀏覽器的「All Plugins」頁籤搜尋 `DalamudDoot`。
* 點擊「DalamudDoot」並選擇「Install」。
* 外掛現已準備好使用。
</section>


<section id="usage">

### 使用 DalamudDoot
在設定中勾選「DalamudDoot Compatibility」即可啟用所有進階功能。
  <br><br><a><img src="https://i.imgur.com/XdK3f8G.png" alt="Settings"/></a><br>
</section>

<section id="contributing">

# 貢獻
歡迎對本專案提出貢獻，請透過 [pull request](https://github.com/BardMusicPlayer/BardMusicPlayer/pulls) 提交。
</section>
