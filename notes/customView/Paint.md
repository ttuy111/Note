# Paint 基础

## Paint 基础 API

- `Paint.setStyle(Style style)` 设置绘制模式
- `Paint.setColor(int color)` 设置颜色
- `Paint.setStrokeWidth(float width)` 设置线条的宽度
- `Paint.setTextSize(float textSize)` 设置文字大小
- `Paint.setAntAlias(boolean aa)` 设置抗锯齿开关

### 绘制模式

`Paint.setStyle(Style style)`

`Style` 默认值为 `FILL`

- `FILL` 填充模式
- `STROKE` 画线模式（只画边缘线条）
- `FILL_AND_STROKE` 既画线又填充（可以理解为前两种模式的综合）
  
### 绘制颜色

`Paint.setColor(int color)`

`Paint.setARGB(int a, int r, int g, int b)`

两种方式用来设置绘制内容的颜色 常用第一种

### 着色器

`Paint.setShader(shader)`

可以设置一套着色方案用于绘制颜色，常用的有：

1. ##### `LinearGradient` 线性渐变 

    ###### 构造方法：

   `LinearGradient(float x0, float y0, float x1, float y1, int color0, int color1, Shader.TileMode tile)`  

   ###### 参数：
   `(x0,y0)` `(x1,y1)`：渐变的两个端点坐标
   `color0` `color1`：渐变的两个端点颜色
   `title`：渐变规则（`CLAMP`、`REPEAT`、`MIRROR` ）
   <br>
2. ##### `RadialGradient` 镭射渐变
3. ##### `SweepGradient` 扫描渐变

4. ##### `BitmapShader` 用 `Bitmap` 着色

5. ##### `ComposeShader` 混合着色器
  
     混合着色器就是把两个 `Shader` 一起使用


### 设置线条的宽度

`Paint.setStrokeWidth(float width)`

默认单位为 `px` 像素

在绘制模式为 `FILL` 或者 `FILL_AND_STROKE` 下有效

### 设置文字大小

`Paint.setTextSize(float textSize)`

### 设置抗锯齿开关

`Paint.setAntiAlias(boolean aa)`

`Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG)`


