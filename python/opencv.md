# Opencv Notes

## 1. 使用opencv在图片上绘制矩形框

```python
# 对单帧样本画框
def save_bboxed_img(img_path, bbox, save_path='./visulizations/', phrase):
    
    img = cv2.imread(img_path)
    img_name = os.path.basename(img_path)
        
    cv2.rectangle(img, bbox[0], bbox[1], bbox[2], bbox[3])
    cv2.putText(img, phrase[0], (80, 80), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 2)
    
    save_path = os.path.join(save_path, img_name)
    cv2.imwrite(save_path, img)
```

## 2. 使用opencv在图片上打印文本

cv2图片中插入文本：

```python
cv2.putText(img, phrase[0], (80, 80), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 0), 2)
			图片  文本        坐标      字体                       大小  颜色         字体厚度
```

