# Week2: Content-based recommendation


## 專案目的

透過Amazon購物網站的美妝消費資料，萃取`客戶購買之商品`、`評論歷史資訊`、`產品基本資訊`等影響推薦成效之欄位，建置以內容為基礎的過濾(Content-based)的方式，替指定消費者，依據過去歷史購買的商品，推薦類似的k個商品，並使用`是否實際購買推薦商品`作為評估recall score成效指標

## 使用工具
   Python、 Google Colab

## 使用資料
   1. 資料來源： 
       * `All_beauty.csv`： [連結](http://deepyeti.ucsd.edu/jianmo/amazon/categoryFilesSmall/All_Beauty.csv)
       * `meta_All_Beauty.json.gz`：[連結](http://deepyeti.ucsd.edu/jianmo/amazon/metaFiles2/meta_All_Beauty.json.gz)
   2. 資料描述：
       * `All_beauty.csv`：提供客戶商品申購、評論歷史資訊
          * 時間區間：2000-01-10 ~ 2018-10-02
          * 資料筆數：371,345
          * 訓練資料：2000-01-10 ~ 2018-09-01
          * 測試資料：2018-09-01 ~ 2018-09-30
        - 測試資料：2018-09-01 - 2018-09-30 (共 590)
       * `meta_All_Beauty.json.gz`：提供產品基本資訊
          * 時間區間：2000-01-10 ~ 2018-10-02
          * 資料筆數：32,892
## 資料清洗與EDA
   1. 去除metadata重複資料
   2. 篩選有用欄位，metadata留下`description`、 `title` 、 `also_buy` 、 `brand`、 `rank`、 `price`
   3. 清洗 `rank`，拆解出排名與子分類
   4. 產品子分類 97%為 `Beauty  Personal Care`，metadata篩選留下此類型
   5. 清洗`price`，作為特徵值之一，觀察價格分布並拆分為三個區間(篩選price<100)，多數產品銷售分布於0~33元區間，新增`price_catagory`價格分為`High`、 `Middle` 、 `Low` 
   6. 清洗 `brand`空值，以no_brand取代
   7. 檢視每月評論數，近年有下降趨勢，公司營收可能出現問題
   8. TF/IDF：
      * 思考`description`、 `title` 、 `brand`、 `price_catagory`對於商品標題、商品形容、隸屬牌子、價格區間，可能為使用者有興趣知道的商品訊息，可作為內容相似度推薦的關鍵，故合併為一個字串
      * 使用word_tokenize進行斷詞，使用stopword過濾不重要字詞
      * TfidfVectorizer計算tf-idf數值， 以min_df = 100 限制字頻>100進行計算
   9. 使用cosine_similarity計算相似度矩陣

## 推薦規則
> 已備好Content-based推薦系統的基礎材料，以下為思考脈絡說明：

1. 套用「Content-based推薦系統」：
    * 以9月份資料計算推薦成效，使用者主要93% 新客、舊客佔 7%(38位），僅舊戶有歷史商品銷售資料套用Content-based推薦
    * 嘗試以推薦不同K個類似商品[2, 5, 10, 15, 20, 25, 30]，當中K＝10已可達到最佳，表現為0.005
    * 9月份主要為新客，當僅舊戶使用Contnet-based，導致資料集表現成效不佳
2. 套用「Content-based + Rule-based推薦系統」：
    * 新客戶使用Content-based，舊客戶使用Rule-based進行推薦
    * Rule-based使用兩個維度：購買資料的時間遠近、商品推薦數，得出資料時間區間2018-08-01～2018-09-01'、推薦30個，recall表現最好，分數達0.264
    * 最終套用Content-based + Rule-based推薦邏輯，recall可達0.255

## 結論
1. 結合content-based & rule-based表現(recall：0.255)，遠比content-based佳(recall：0.005)，但若相較全使用rule-based(recall：0.264)，整合兩個推薦表現仍略差於rule-based
2. 針對此電商的客戶銷售樣態（多新客、少舊客），建議公司應多推出舊客回流行銷活動，促動舊戶消費，content-based才有更好展現運用機會






