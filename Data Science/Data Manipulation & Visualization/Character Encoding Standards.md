
|           | ASCII<br>American Standard Code for Information Interchange | Unicode                                  | UTF-8<br>A realization of Unicode, compatible with ASCII |
| --------- | ----------------------------------------------------------- | ---------------------------------------- | -------------------------------------------------------- |
| range     | 128 chars: 英文字母（大小写）、数字、标点符号和一些控制字符                         | 149,000 chars+: all characters in theory |                                                          |
| size/char | 1 Byte                                                      | 2 or 4 Bytes                             | 1 Byte for ASCII chars<br>2-4 Bytes for other chars      |
# Unicode
从 `U+0000` 到 `U+10FFFF`，共计1,114,112个码点（code points）
​这些码点被划分为 17 个平面（Plane），每个平面包含 65,536 个码点。​最常用的是第 0 平面，称为基本多文种平面（BMP），范围为 `U+0000` 至 `U+FFFF`，涵盖了大多数常用字符。

| 范围 (十进制)      | Unicode范围   | 含义        |
| ------------- | ----------- | --------- |
| 0–31          | U+0000–001F | 控制字符（不可见） |
| 32–127        | U+0020–007F | 基本ASCII   |
| 128–255       | U+0080–00FF | 扩展拉丁字符    |
| 0x4E00–0x9FFF | 汉字区         | 常用 CJK 字符 |
## Realizations of Unicode
**UCS-2**: early realization
	2 Bytes/char
**UTF-8**: most used in the internet
	1 Bytes/char for ASCII code (UFT-8 is compatible with ASCII)
	3 Bytes per Chinese char
	Also use 2, 3 and 4 Bytes
**UTF-16**: 平衡空间和处理效率，特别是那些需要处理大量非拉丁字符的应用
	2 or 4 Bytes/char
**UTF-32**: ​快速随机访问字符
	4 Bytes/char
	因为定长可以直接计算索引，所以便于快速随机访问
	高空间占用，实际应用较少
# Others
GBK 国标码
ISO-8859-1 欧洲编码
	1 Byte, 128 chars