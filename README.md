# Recommendation_System_Word2Vec

## ETL

### 欄位說明：
* **[outfit_Season]**：在日本，一年分為4個期間。3月至5月是春季，6月至8月是夏季，9月至11月是秋季，12月至2月是冬季。
* **[outfit_Like_adjust]**：依據照片上傳時間，按讚數除分母(N年前)。以2020為始(含2021年1月),不做調整(除以1)，2019年除以2，2018年除以3，以此類推
* **[out_Update]**：資料抓取時間，從2015年開始。2014年之前(含)的資料較舊。
* **[outfit_Bodypart >> outfit_Itemgroup >> outfit_Item]**：商品類別階層
  * **[outfit_Bodypart]**：Top, Buttom, Outerwear, Dress, Acc
  * **[outfit_Itemgroup]**：依據wear官網主類別
  * **[outfit_Item]**：商品顏色+wear官網子類別

## 推薦模型(Word2Vec)

### 模型使用說明：
1. 依據每位博主過往的穿搭商品，列成歷史商品清單，並利用Word2Vec模型推算出商品的相似度，依此推算出商品可能的搭配推薦。
2. 透過季節、按讚數排序進行篩選，列出推薦的URL
3. 特徵抽取：商品顏色+wear官網子類別。`'白色連帽外套', '棕色插肩外套', '黑色靴子', '白色連帽外套', '黑色靴子', '藍色運動衫', '綠色褲裝', '黑色靴子'`


### 模型訓練與主要參數

```py
model = Word2Vec(window = 2, sg = 1, hs = 0, #window=2
                 negative = 10, # for negative sampling
                 alpha=0.03, min_alpha=0.0007,
                 seed = 14)

model.build_vocab(upload_train, progress_per=200)

model.train(upload_train, total_examples = model.corpus_count, 
            epochs=10, report_delay=1)
```


### 訓練結果

```py
similar_products_byitem('summer',model['白色褲裝'])
```
    (('白色褲裝',
    ['https://wear.tw/yoahiru35/17301668/',
    'https://wear.tw/fwafwa7/17455693/',
    'https://wear.tw/maarimo196/17383511/'],
    1.0),
    [('黃色背心外套', ['https://wear.tw/maarimo196/17265532/'], 0.595501720905304),
    ('橘色套裝',
    ['https://wear.tw/ninomasa/17527184/',
    'https://wear.tw/ninomasa/17350060/'],
    0.5745309591293335),
    ('米色披肩',
    ['https://wear.tw/carmen1505/15176425/',
    'https://wear.tw/carmen1505/14977728/'],
    0.5720653533935547),
    ('白色套裝',
    ['https://wear.tw/chicchimo5/17126803/',
     'https://wear.tw/akari0302/17478080/',
    'https://wear.tw/chicchimo5/14983703/'],
    0.5645591020584106),
    ('米色連體套裝',
    ['https://wear.tw/sanki0102/17016617/',
    'https://wear.tw/cloud9nail/12556345/'],
    0.5632511377334595),
    ('紅色背心外套',
    ['https://wear.tw/yuriaaa5/17367330/',
    'https://wear.tw/ninomasa/14956278/'],
    0.5563393831253052),
    ('米色連帽長外套',
    ['https://wear.tw/keiru2000/17477143/',
     'https://wear.tw/mii207/17477758/',
    'https://wear.tw/harupi1230/13003274/'],
    0.5505149364471436),
    ('紫色細肩帶背心',
    ['https://wear.tw/haruxchi/17176576/',
    'https://wear.tw/murasesae/12550946/',
    'https://wear.tw/miniwen311/17558427/'],
    0.5485215187072754),
    ('紫色夾克／外套', ['https://wear.tw/38hnt/14871118/'], 0.5361681580543518),
    ('紫色坦克背心',
    ['https://wear.tw/unitarosu9876/17444836/',
    'https://wear.tw/pageboy_hinechi/17052452/',
    'https://wear.tw/imoko30/15109288/'],
    0.5339160561561584),
    ('白色尼龍夾克', ['https://wear.tw/takahashiai/4293731/'], 0.5309780836105347)])
