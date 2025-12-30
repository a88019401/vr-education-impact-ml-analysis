# Impact of Virtual Reality on Education — EDA, Supervised Learning & Clustering
> 虛擬實境在教育中的影響：資料探索、監督式學習與分群分析

## 📌 Project Overview 專案簡介
本專案使用 Kaggle 資料集 **Impact of Virtual Reality on Education**，包含不同教育機構導入 VR 教學後的：
- 學生人口統計（年齡、性別、年級、地區）
- VR 使用情況（是否使用、每週時數、設備可近性）
- 學習與心理相關變項（參與度、學習成果、壓力、創造力、教師回饋、持續學習意願等）

進行：
1) **資料前處理**（Label Encoding / One-Hot Encoding）  
2) **統計分析與視覺化（EDA）**  
3) **監督式學習（分類：預測學習成果是否改善）**  
4) **非監督式學習（KMeans / DBSCAN / PCA：探索分群結構）**



---

## 📂 Dataset 資料集來源
- Kaggle: https://www.kaggle.com/datasets/waqi786/impact-of-virtual-reality-on-education
- 原始檔名（Kaggle 下載後常見）：`Virtual_Reality_in_Education_Impact.csv`


---

## 🧾 Data Columns 資料欄位（節錄）
- `Student_ID`, `Age`, `Gender`, `Grade_Level`, `Field_of_Study`
- `Usage_of_VR_in_Education`, `Hours_of_VR_Usage_Per_Week`
- `Engagement_Level`, `Improvement_in_Learning_Outcomes` (Target)
- `Instructor_VR_Proficiency`, `Perceived_Effectiveness_of_VR`
- `Access_to_VR_Equipment`, `Impact_on_Creativity`, `Stress_Level_with_VR_Usage`
- `Collaboration_with_Peers_via_VR`, `Feedback_from_Educators_on_VR`
- `Interest_in_Continuing_VR_Based_Learning`, `Region`
- `School_Support_for_VR_in_Curriculum`

---

## 🧼 Preprocessing 資料前處理
本專案測試兩種處理策略：

### A) Label Encoding（快速，但可能引入「假順序」）
- 對類別欄位使用 `LabelEncoder()`  
- 優點：維度不爆炸、速度快  
- 缺點：模型可能把類別值當成有大小順序（例如 Female=0, Male=1 會被誤解成「男 > 女」）

### B) One-Hot Encoding（較合理，但維度暴增）
- 使用 `OneHotEncoder()` 展開所有類別
- 優點：避免假順序、較符合統計/機器學習常見處理  
- 缺點：維度可能非常大，需要特徵篩選或降維

---

## 📊 EDA & Visualization 統計分析與資料視覺化
我們主要關注：
- **年級 / 領域** 是否影響 VR 使用與學習成果
- **VR 使用時數** 與學習成果改善的關係
- **參與度 vs VR 感知效果** 的關聯
- **地區資源 / 學校支援** 對 VR 設備與學習成果的影響

> 初步觀察：多數變項與 `Improvement_in_Learning_Outcomes` 的線性相關性偏低，暗示「非線性」或「資料本身模式不強」的可能。

---

## ✅ Supervised Learning 監督式學習（分類）
### 🎯 Task 目標
預測 `Improvement_in_Learning_Outcomes`（Yes/No）

### Models 使用模型
- KNN + GridSearchCV
- SVC + GridSearchCV
- Random Forest + GridSearchCV
- Random Forest Feature Importance（特徵篩選）

### Notes 重要提醒
- 你們的結果曾出現 **One-Hot + KNN = 100% accuracy** 這種「過於完美」的情況  
  建議在報告/README 中務必加註可能原因，例如：
  1) **資料洩漏（Data Leakage）**：像 `Student_ID` 這類識別欄位若被納入特徵，可能讓模型「記住答案」  
  2) 資料可能是**合成/規則生成**，導致 target 可被特徵完全推回  
  3) 前處理或切分方式使 train/test 重複或高度相似

✅ 建議加做（加分項）：
- 移除 `Student_ID` 後重跑
- 顯示 confusion matrix、precision/recall/F1
- 用不同 random seed / Stratified split 驗證穩定性

---

## 🧩 Unsupervised Learning 非監督式學習（分群）
### Methods 使用方法
- KMeans（含 Elbow / Silhouette 選 K）
- DBSCAN（含 K-distance graph 找 eps）
- PCA（2D 視覺化與主成分負荷量分析）

### Objective 分群目的
探索「VR 參與/壓力/感知效果/學科背景」是否存在自然群集，
並用群集平均值解釋各群特徵輪廓（persona-like clusters）。

---

## 🛠️ How to Run 如何執行
### 1) Environment 環境需求
- Python 3.9+（建議 3.10/3.11）
- Jupyter Notebook / JupyterLab

### 2) Install 安裝套件
```bash
pip install -r requirements.txt
