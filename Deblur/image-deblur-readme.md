# 圖像去模糊自編碼器模型

這是一個使用卷積自編碼器（Convolutional Autoencoder）來去除圖像模糊的深度學習模型。

## 模型架構

該模型採用編碼器-解碼器架構：
- **編碼器**：3層卷積層（64, 128, 256 filters），將 128×128×3 的圖像壓縮到 256 維的潛在向量
- **解碼器**：3層反卷積層，將潛在向量還原成清晰圖像

## 環境需求

```python
tensorflow
keras
numpy
opencv-python (cv2)
matplotlib
pandas
tqdm
scikit-learn
```

## 資料集準備

需要準備兩個資料夾：
- `sharp/`：包含清晰的原始圖像
- `defocused_blurred/`：包含對應的模糊圖像

支援的圖像格式：`.jpg`, `.jpeg`, `.png`

## 訓練步驟

1. **資料載入與預處理**
   - 將所有圖像調整為 128×128 像素
   - 將像素值正規化到 [0, 1] 範圍

2. **模型訓練**
   ```python
   # 設定參數
   batch_size = 32
   epochs = 100
   
   # 訓練模型（輸入為模糊圖像，目標為清晰圖像）
   autoencoder.fit(blurry_frames, clean_frames, ...)
   ```

3. **模型儲存**
   ```python
   autoencoder.save('deblur_autoencoder_model.keras')
   ```

## 使用模型進行預測

```python
# 載入模型
autoencoder = keras.models.load_model('deblur_autoencoder_model.keras', compile=False)

# 預處理輸入圖像
def preprocess_image(image_path):
    image = cv2.imread(image_path)
    image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
    image = cv2.resize(image, (128, 128))
    image = image.astype('float32') / 255.0
    return np.expand_dims(image, axis=0)

# 進行預測
input_image = preprocess_image('blurry_image.jpg')
deblurred_image = autoencoder.predict(input_image)[0]
```

## 訓練技巧

- 使用 MSE（均方誤差）作為損失函數
- 使用 Adam 優化器
- 實施學習率衰減策略（ReduceLROnPlateau）
- 訓練過程中同時監控訓練和驗證損失

## 注意事項

- 輸入圖像必須調整為 128×128 像素
- 模型適用於輕度到中度的模糊，對於嚴重模糊的效果可能有限
- 建議使用 GPU 加速訓練過程

## 模型評估

訓練完成後，可以透過比較輸入模糊圖像、真實清晰圖像和預測結果來評估模型效果。程式碼中包含了視覺化功能來展示這些比較結果。