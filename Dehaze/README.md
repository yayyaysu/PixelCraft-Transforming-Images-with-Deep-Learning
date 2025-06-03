# 單張影像去霧（Dark Channel Prior）


這是一個使用 暗通道先驗（Dark Channel Prior, DCP） 演算法的影像去霧專案，透過估計大氣光與傳輸率來還原較清晰的影像。


## 處理過程簡介:
暗通道計算：找出每個區塊中最暗的像素，推估霧區分布


大氣光估計：從暗通道中最亮的像素中選出最亮的 RGB 值


傳輸率估計與優化：估計霧的濃度，並使用導向濾波平滑結果


影像復原：根據模型反推出無霧影像


降噪處理：可選的雙邊濾波改善視覺效果

## 執行方式:
安裝必要函式庫


pip install opencv-python opencv-contrib-python numpy matplotlib

## 注意事項:
需使用 opencv-contrib-python 才能使用 cv2.ximgproc.guidedFilter


輸出影像未自動儲存

## 參考資料:
He, K., Sun, J., & Tang, X. (2009). Single Image Haze Removal Using Dark Channel Prior 


OpenCV 官方文件
