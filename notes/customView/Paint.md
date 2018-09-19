# Paint 基础

## Paint 基础 API

- `Paint.setStyle(Style style)` 设置绘制模式
- `Paint.setColor(int color)` 设置颜色
- `Paint.setStrokeWidth(float width)` 设置线条的宽度
- `Paint.setTextSize(float textSize)` 设置文字大小
- `Paint.setAntAlias(boolean aa)` 设置抗锯齿开关

### 绘制模式

Paint.setStyle(Style style)

Style 默认值为 FILL

- FILL 填充模式
- STROKE 画线模式（只画边缘线条）
- FILL_AND_STROKE 既画线又填充（可以理解为前两种模式的综合）
  
### 绘制颜色

Paint.setColor(int color)

Paint.setARGB(int a, int r, int g, int b)

两种方式用来设置绘制内容的颜色 常用第一种

