# OBS Glitch Slideshow

專為 OBS Studio 瀏覽器來源設計的本地 HTML 投影片播放器，受 [Codrops CSSGlitchEffect](https://github.com/codrops/CSSGlitchEffect) 的驚豔故障轉場特效啟發。

## 功能特色

- **本地 HTML 檔案** - 無需伺服器，直接從檔案系統執行
- **可自訂播放清單** - 透過 JSON 檔案控制投影片順序與顯示時間
- **Codrops 風格故障轉場** - 3 秒轉場時間，包含多層次故障動畫
- **漸進式圖片替換** - 新圖片在故障特效播放時逐漸淡入
- **透明背景** - 非常適合在 OBS 中作為疊加層使用
- **響應式設計** - 適應任何解析度

## 檔案結構

```text
obs-glitch-slideshow/
├── index.html       # 主要投影片播放器 HTML 檔案
├── playlist.json    # 投影片播放設定
├── slide1.jpg       # 你的圖片檔案
├── slide2.jpg
├── slide3.jpg
└── ...
```

## 播放清單設定

在與 `index.html` 相同目錄下建立 `playlist.json` 檔案：

```json
[
  {
    "image": "slide1.jpg",
    "duration": 5
  },
  {
    "image": "slide2.jpg",
    "duration": 8
  },
  {
    "image": "slide3.jpg",
    "duration": 5
  }
]
```

### 播放清單選項

| 屬性       | 類型   | 說明                                         |
| ---------- | ------ | -------------------------------------------- |
| `image`    | string | 圖片檔名（相對於 index.html 的路徑）         |
| `duration` | number | 顯示時間（秒），在轉場開始前的靜止顯示時間   |

## OBS 設定

1. 將所有檔案複製到本地資料夾
2. 將你的圖片新增至該資料夾
3. 使用你的圖片與顯示時間設定 `playlist.json`
4. 在 OBS Studio 中：
   - 新增**瀏覽器**來源
   - 勾選**本地檔案**
   - 瀏覽並選擇 `index.html`
   - 設定寬度／高度以符合你的畫布（例如 1920x1080）
   - 取消勾選**不可見時關閉來源**（選用）
   - 取消勾選**取得焦點時更新瀏覽器**（選用）

## 瀏覽器相容性

專為基於 Chromium 的瀏覽器（OBS Studio 瀏覽器來源使用）設計。已測試於：

- OBS Studio 32.0.4

## 授權條款

### 程式碼

<img src="https://github.com/user-attachments/assets/717be0f4-5f3b-42b0-9b75-5e59149ab4b8" alt="gplv3" width="300" />

[GNU GENERAL PUBLIC LICENSE Version 3](LICENSE)

Copyright (C) 2026 Jim Chen <Jim@ChenJ.im>.

本程式為自由軟體：您可以依據由自由軟體基金會發布的 GNU 通用公共授權條款（第 3 版，或您選擇的任何後續版本）重新發佈及/或修改本程式。

本程式以期望其有用而發佈，但不提供任何保證；甚至不包含對適銷性或特定用途適用性的默示保證。詳情請參閱 GNU 通用公共授權條款。

您應已隨本程式收到一份 GNU 通用公共授權條款副本。如果沒有，請參見 <https://www.gnu.org/licenses/>。

### 圖片

三張圖片 (slide1.webp, slide2.webp, slide3.webp) 由[須多夜花](https://sudayoruka.com/)版權所有，僅供此範例使用。
