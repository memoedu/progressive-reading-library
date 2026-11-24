使用opencv进行视频流IO是非常常用的操作，比如使用CV的单帧预测算法施加在视频上，使用webcam采集视频，都是非常常用的操作。介于opencv的视频流IO不是那么地简洁（主要指`cv2.VideoWriter`），我觉得很有必要记录一下，我只给出常用地视频流IO代码模板，这种东西没必要记住，知道就行：

```python
import numpy as np
import cv2

INPUT_VIDEO = "./video/test.mp4"
OUTPUT_VIDEO = "./output.mp4"

FORMAT = "mp4v"

cap = cv2.VideoCapture(INPUT_VIDEO)
fourcc = cv2.VideoWriter_fourcc(*FORMAT)

width = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
height = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))
fps = cap.get(cv2.CAP_PROP_FPS)
print("width:",width, "height:", height)

out = cv2.VideoWriter(OUTPUT_VIDEO, fourcc, fps, (width, height))

while (cap.isOpened()):
    ret, frame = cap.read()
    if not ret:
        break 
    # 操作frame
    out.write(frame)
    
cap.release()
out.release()
cv2.destroyAllWindows()
```

其中`FORMAT`代表目标视频的编码格式，目前我还遇到了很多bug，已经确定的`FORMAT`和编码关系的对照表如下：

|目标视频编码格式|`FORMAT`取值|
|:---:|:---:|
|mp4|"mp4v"|
|aiv|"MJPG"|



---

# 参考

[1] [python opencv写视频——cv2.VideoWriter()_翟羽嚄的博客-CSDN博客_cv2.videowriter](https://blog.csdn.net/mao_hui_fei/article/details/107573021)