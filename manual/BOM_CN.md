# 材料清单

### 打印件

推荐设置：
* 0.2mm 层高
* 0.5mm (250%) 线宽
* 立方体填充，60%
* 启用支撑

| 零件 | 数量 | 备注 |
|---|---|---|
| [a]_cover.stl | 1 | 用强调色打印 |
| [c]_led_diffuser.stl | 1 | 用透明材料打印 |
| body_part_a.stl | 1 | 用主色打印 |
| body_part_b.stl | 1 | 用主色打印 |
| idler.stl | 1 | 用主色打印 |
| magnet_holder.stl | 1 | 用主色打印 |
| wire_cover.stl | 1 | 用主色打印 |

### 电子元件

订购以下物品中最便宜的：

| 数量 | 零件 | 预估价格 | 描述 | 链接 |
|----|----|----|----|----|
| 1 | AS5600⁽¹⁾ | 1$ | 磁性编码器分线板，用于旋转检测 | [aliexpress](https://vi.aliexpress.com/item/1005003080438185.html)
| 1 | MK3S IR 传感器⁽²⁾ | 5$ | 用于线材存在检测的 IR 传感器 | [aliexpress](https://vi.aliexpress.com/item/1005002551124258.html)
| 1 | Waveshare RP2040-Zero | 3$ | 仅订购板，不带预焊接接头 | [aliexpress](https://vi.aliexpress.com/item/1005004281549886.html)
| 1 | 电线 | | 一些长度的电线用于连接所有部件（需要焊接） | |
| 1 | JST-XH 公头 4 针 | | (可选) 适合提供的底部盖子 | |

注意：
1. 链接的 AS5600 分线板带有 3mm 直径的磁铁。如果必须单独购买磁铁，请确保其磁极沿直径方向定向，而不是沿厚度方向。
2. IR 传感器不需要任何通常的 MK3S 硬件，尽管 Aliexpress 上的许多套件都附带额外的硬件。只需要一个小的 2mm 自攻螺钉来固定 PCB。

### 五金件

| 数量 | 零件 | 预估价格 | 描述 | 链接 |
|----|----|----|----|----|
| 1 | BMG 挤出机套件⁽¹⁾ | 5$ | 套件应包括翼形螺钉组件、驱动齿轮、滚子轴承、滚针轴承、3 x 20mm 和 3 x 30mm 轴。 | [aliexpress](https://vi.aliexpress.com/item/1005005443742333.html)
| 1 | M5 x 25mm 杆⁽¹⁾ | 2$ | 订购更长的长度并切割成 24 至 26mm 之间的尺寸 | [aliexpress](https://vi.aliexpress.com/item/100500500541338002.html)
| 2 | M3 x 16mm SHCS | | 连接传感器主体 A 和 B 部分。建议使用内六角螺钉 | |
| 6 | M3 x 6mm BHCS | | 用于 AS5600、顶部和底部盖子。需要圆头螺钉以留出间隙 | |
| 1 | M3 x 10mm BHCS | | 用于底部盖子。 | |
| 10 | M3 X D4.2 X L4.0 热压螺母 | | 这些比 Voron 标准螺母小！ | [aliexpress](https://vi.aliexpress.com/item/4000761483243.html) |
| 2 | PC4-M5 接头 | | 用于在顶部和底部连接 PTFE 管 | [aliexpress](https://vi.aliexpress.com/item/1005005646446620.html) |

注意：
1. 可以使用随 BMG 挤出机套件附带的大型塑料齿轮的轴，而不是单独购买 5mm 杆。只需推出塑料齿轮并打磨轴的带齿部分，使其适合 5mm 轴承。