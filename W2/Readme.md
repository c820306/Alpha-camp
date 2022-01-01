# Week2: Content-based recommendation

## 專案目的


## 使用工具


## 使用資料
   1. 資料來源： 
       * `All_beauty.csv`： [連結](http://deepyeti.ucsd.edu/jianmo/amazon/categoryFilesSmall/All_Beauty.csv)
       * `meta_All_Beauty.json.gz`：[連結](http://deepyeti.ucsd.edu/jianmo/amazon/metaFiles2/meta_All_Beauty.json.gz)
   2. 資料描述：
       * `All_beauty.csv`：提供客戶商品申購、評論歷史資訊
          * 時間區間：2000-01-10 ~ 2018-10-02
          * 資料筆數：371,345
          * 訓練資料：2000-01-10 - 2018-09-01
          * 測試資料：2018-09-01 - 2018-09-30
        - 測試資料：2018-09-01 - 2018-09-30 (共 590)
       * `meta_All_Beauty.json.gz`：提供產品基本資訊
          * 時間區間：2000-01-10 ~ 2018-10-02
          * 資料筆數：32,892
## 資料清洗與EDA
   1. 去除metadata重複資料
   2. 篩選有用欄位內容，僅留下`description`、 `title` 、 `also_buy` 、 `brand`、 `rank`、 `price`
   3. 清洗 `rank`，拆解出排名與子分類
   4. 產品子分類 97%為 `Beauty  Personal Care`，篩選此類做推薦
   5. 清洗`price`，作為特徵值之一，將價格分布分為三桶(範圍為0~100)，主要分布在0~35區間，新增`price_catagory`價格分為`High`、 `Middle` 、 `Low` 
   6. 清洗 `brand`空值，以no_brand取代
   7. 檢視每月評論數，近年有下降趨勢，公司營收可能出現問題
   8. TF/IDF：
      * 思考`description`、 `title` 、 `brand`、 `price_catagory`對於商品標題、商品形容、隸屬牌子、價格區間，可能為使用者有興趣知道的商品訊息，因此合併為統一字串
      * 使用word_tokenize進行斷詞，使用stopword過濾不重要字詞
      * TfidfVectorizer計算tf-idf數值， min_df = 100 限制字頻>100
   9. 使用cosine_similarity計算相似度矩陣

##推薦規則
> 我們已完備設計Content-based推薦系統基礎資料，以下為思考脈絡說明


##結論







