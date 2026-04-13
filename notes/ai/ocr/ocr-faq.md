---
tags:
  - FAQ
---

# OCR FAQ

- 扫描件通常是 300DPI
  - 转换为 72DPI 相当于缩小 4 倍
- 视觉模型有最小限制
  - Qwen2.5-VL
    - https://github.com/QwenLM/Qwen2.5-VL/blob/d2240f11656bfe404b9ba56db4e51cd09f522ff1/qwen-vl-utils/src/qwen_vl_utils/vision_process.py#L26-L36

| 纸张 | 72 DPI (Web)  | 90 DPI         | 150 DPI (平衡) | 300 DPI (高清) |
| :--- | :------------ | :------------- | :------------- | :------------- |
| A4   | 595 x 842 px  | 744 x 1052 px  | 1240 x 1754 px | 2480 x 3508 px |
| A3   | 842 x 1191 px | 1052 x 1488 px | 1754 x 2480 px | 3508 x 4961 px |

```bash
# 缩小 50
convert a.jpg -resize 50% a.50.jpg
# 缩小 4 倍
convert a.jpg -resize 25% a.25.jpg
# 缩小 4 倍，限制高度为 30
convert a.jpg -resize 25% -resize 'x30<' a.output.jpg

# -compress Group4
# -density 300 增加 meta 信息
# -lat 20x20+3% 可能需要调整来到最佳效果
# 轻微高斯模糊有时可以帮助后续的阈值处理，平滑噪点
convert 1.png \
  -colorspace Gray \
  -deskew 40% \
  -normalize \
  -gaussian-blur 0.5 \
  -adaptive-sharpen 0x1.5 \
  -density 300 \
  -lat 20x20-3% \
  -trim +repage \
  1.o.png

magick identify 1.png 1.o.png
magick identify -verbose 1.png 1.o.png
exiftool 1.png 1.o.png
echo 1.png 1.o.png | xargs -n 1 ffprobe -hide_banner
```

## DPI

- 72 / 90 DPI：适合网页展示、移动端预览，或作为 LLM 视觉模型 (如 GPT-4o-mini, Qwen2.5-VL) 的预缩放目标，平衡 Token 消耗与识别效果。
- 150 DPI：复杂版面文字识别的底线，兼顾速度与准确度。
- 300 DPI：OCR 行业标准。大多数 OCR 引擎（Tesseract, PaddleOCR）在此分辨率下表现最稳定。
- 600 DPI：仅用于极小字体（如票据微缩文字）或高精度手写体分析，普通文档处理没必要，会显著拖慢处理速度。

## OCR 预处理 {#preprocess}

- 二值化
  - threshold - 阈值
    - 需要掌握 threshold
    - otusu - 大津法
    - sauvola - 自适应阈值
    - adaptive threshold
  - negate - 反色
- 对比度增强与光照均衡
  - normalize
    - 拉伸全局对比度
  - clahe - Contrast Limited Adaptive Histogram Equalization
- 噪声去除
  - median
  - blur
- 锐化
  - sharpen
- 图像歪斜校正 - Deskew
  - rotate
  - OpenCV、Leptonica
- 形态学操作 (Morphological Operations)
  - dilation - 膨胀
  - erosion - 腐蚀
  - opening - 开运算
  - closing - 闭运算
- 分辨率
  - DPI
    - 300DPI
  - 72DPI
- trim
- 参考
  - https://github.com/lovell/sharp/issues/609
  - https://github.com/IgorMeloS/OCR
