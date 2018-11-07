# 1. Ray Tracing in One Weekend

## 1.1 Output an Image

**PPM格式**

```
P3
# The P3 means colors are in ASCII, then 3 columns and 2 rows,
# width == 3, height == 2
# then 255 for max color, then RGB triplets
3 2
255
255	0	0
0	255	0
0	0	255
255	255	0
255	255	255
0	0	0
```

**图像库**

`ppm`格式以`ASCII`存储，文件较大，考虑采用图像的无损压缩格式`png`来存储图像。

可使用开源库 `stb_image` 和 `stb_image_write` 来读写图像

> https://github.com/nothings/stb

**图像实时显示**

如果只使用图像来检查结果，则需等待计算完毕；而光线追踪算法耗时较长，故考虑实时显示。

图形接口使用的是OpenGL，我将功能封装在了 `Utility/RayTracing/ImgWindow` 中。

**实现**

```c++
int main(int argc, char ** argv) {
	ImgWindow imgWindow(str_WindowTitle);
	Image & img = imgWindow.GetImg();
	const size_t val_ImgWidth = img.GetWidth();
	const size_t val_ImgHeight = img.GetHeight();
	const size_t val_ImgChannel = img.GetChannel();

	auto imgUpdate = Operation::ToPtr(new LambdaOp([&]() {
		for (size_t i = 0; i < img.GetWidth(); i++) {
			for (size_t j = 0; j < img.GetHeight(); j++) {
				float r = i / (float)img.GetWidth();
				float g = j / (float)img.GetHeight();
				float b = 0.2;
				img.SetPixel(i, j, Image::Pixel<float>(r, g, b));
			}
		}
	}, false));

	imgWindow.Run(imgUpdate);

	return 0;
}
```

结果输出在 `data/out` 中