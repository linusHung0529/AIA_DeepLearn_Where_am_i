# 前言
這篇github，用於紀錄我在台灣人工智慧學校助教培訓中，第2、3周測驗題目，其中解題思路、遇到的問題、解決辦法。<br>
題目為：Where_am_i 圖片資料集自動分類<br>
train資料集，總共分布於15個類別資料夾。而test資料集，存在一起，目標:分類test資料。<br>

kaggle：https://www.kaggle.com/c/aia-tc-image-cla-where-am-i/leaderboard
<br>
# 解題步驟
1.load_data<br>
2.架設model<br>
3.用各式方法提高model成功率
## load_data
在最初遇到的第一個問題就是載入train資料集，以往遇到的題目，train、test資料集都是可以直接從套件上load.data就可以了，但這次的題目train分布在各資料夾中。<br>
經過很長一段時間的奮鬥，最終將load data這個問題分解成幾個步驟。<br>
1.先透過mapping.txt取得所有資料夾名稱。<br>
2.用os套件造訪mapping中所有資料夾，並生成所有資料夾中圖片路徑。<br>
3.用cv2套件，造訪所有圖片，並將它們match成train dataset<br>
## 架設model
在解決了load_data問題後，遇到的第二個問題是model的選擇，因為我是初學者，所以只知道MLP network，在一開始用MLP training之後，調了各種參數，
predict的結果依然只有20%左右。代表著我已經碰到了MLP在這個問題上的極限了，之後google了許久，發現圖片分類，有兩個方向。<br>
1.遷移式學習 trainfer Learning<br>
2.卷積式網路 CNN<br>
最後選擇了CNN做為新的模組，因為trainfer Learning都是引用別人寫好的套件，再調參數，對於初學者的我來說，我想光是先實作自己的model的能力都還不夠，
所以選擇CNN讓我更透徹的學習其中的原理(儘管我知道trainfer Learning預測一定比較好)<br>
在建CNN模組上，我依然選擇tensorflow作為框架，原因有兩個:<br>
1.我在學MLP時就用它了，跟它比較熟<br>
2.kares在建構模組上，省略了很多語法，但我認為對初學者來說，tensorflow的宣告雖然很長，但比較好看出架構。<br>
在我的第一版CNN模組建起來時，我採用的架構是兩層卷積層、兩層池化層、底下接著一層hidden層(1024神經元)，但train結果並不理想。傳入的圖片大小是32*32，
overfitting的情況非常嚴重，因為圖片不大，資訊不多，所以模組透過400epcho左右，就能完全被起來train set了。為了解決這個問題，首先我加大了圖片大小到128*128，
隨著圖片增大，我也加深了CNN的層數，目的是讓CNN抽取出來的特徵高階，hidden層我也改用兩層分別為512、216，這樣出來的結果大大提升了準確率，讓準確率來到45%左右。
接著我調適參數(epcho、batch_size、lr)，其準確率有提升至50%左右。但也差不多到CNN對於這個問題的極限了。接著後續為了提升準確率，我選擇從train dataset下手。