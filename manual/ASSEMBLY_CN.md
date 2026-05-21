# Roadrunner 组装手册

在开始前，请准备好[材料清单](BOM_CN.md)中的所有零件。

### 1. 热压螺母

将热压螺母安装到 `body_part_a.stl` 和 `body_part_b.stl` 中。

<img src="../images/manual/001_assembled.jpg" height="200">

顶部还有一个螺母未在下图中显示：

<img src="../images/manual/002_assembled.jpg" height="200">

<img src="../images/manual/003_assembled.jpg" height="200">

### 2. 惰轮组件

<img src="../images/manual/008_parts.jpg" height="200">

将 `idler.stl` 与惰轮齿轮、滚针轴承和 3x20mm 轴组装在一起。

<img src="../images/manual/008_assembled.jpg" height="200">

### 3. 主体组装

将 5mm 轴承安装到 `body_part_a.stl` 和 `body_part_b.stl` 中。

<img src="../images/manual/004_assembled.jpg" height="200">

组装 5mm 轴、3x2 磁铁和磁铁支架部件。

<img src="../images/manual/005_assembled.jpg" height="200">

在安装 AS5600 板之前，将 5mm 轴安装到轴承中。

<img src="../images/manual/006_parts.jpg" height="200">

使用 M3 x 6mm BHCS 螺丝固定 AS5600 板，4 针接头应与左侧的开口对齐。确保螺丝拧紧后 5mm 轴可以自由旋转。

<img src="../images/manual/006_assembled.jpg" height="200">

将驱动齿轮安装到 5mm 轴上，注意拧紧紧定螺钉（使用乐泰胶）。同时安装 3x30mm 轴。

<img src="../images/manual/007_assembled.jpg" height="200">

通过将惰轮滑到 3mm 轴上完成主体组装，使用两个 M3 x 16mm SHCS 将主体 A 和 B 部分拧在一起。

<img src="../images/manual/010_assembled.jpg" height="200">

### 4. 电子元件

下一步是焊接电子元件，如下所示：

<img src="../images/schematics.jpg" height="350">

准备 IR 传感器，从板上移除 3 针 0.1" 接头。

<img src="../images/manual/011_parts.jpg" height="200">

添加 2mm 自攻螺钉，并确保 IR 传感器与惰轮臂对齐。

<img src="../images/manual/011_assembled.jpg" height="200">

准备 RP2040-Zero 和一段 4 针 90 度 0.1" 接头。中间 2 针应移除，因为它们在两块板之间不是直接连接的（稍后我们将反向焊接）。

<img src="../images/manual/012_parts.jpg" height="200">

将 90 度接头与两块板对齐并焊接到位。注意在引脚 9 和 12 上焊接（很容易偏移一个引脚并在错误的引脚上焊接）。

<img src="../images/manual/012_soldered_2.jpg" height="200">

此时应该可以一起取下两块电路板。连接器上的中间两个引脚仍然未焊接，这是正常的。

<img src="../images/manual/012_soldered_3.jpg" height="200">

将 RP2040 放在一边，在 IR 传感器板上焊接一些细线。

<img src="../images/manual/013_soldered_2.jpg" height="200">

将 RP2040 和 AS5600 板放回原位，并将 IR 传感器电线剪至合适长度。

<img src="../images/manual/014_wires.jpg" height="200">

红色（5V）和黑色（GND）电线应到达 PCB 的底部边缘，棕色（中间）电线应到达 GP0 引脚。两块板之间有一个凹槽，可以容纳多余的电线长度。

<img src="../images/manual/014_soldered_2.jpg" height="200">

在 RP2040 引脚 GP10/GP11 和 AS5600 引脚 SDA/SCL 之间添加两根电线。注意两块板上的 I2C 引脚顺序是相反的，因此电线应该交叉！

<img src="../images/manual/015_soldered.jpg" height="200">

将 AS5600 板上的 VCC 和 GND 焊盘焊接到 RP2040 上的 3V3 和 GND 焊盘。AS5600 板只能在移除 0Ω 电阻（R1）的情况下由 5V 供电！

<img src="../images/manual/015_soldered_2.jpg" height="200">

将母 JST-XH 接头添加到 `wire_cover.stl`，并将电线焊接到连接器上。红色（5V）和黑色（GND）电线焊接到 RP2040 5V 和 GND 焊盘，绿色和蓝色电线焊接到 RP2040 GP4/GP5 焊盘。

<img src="../images/manual/016_soldered_1.jpg" height="200">

确保按照图片所示焊接在正确的焊盘上。编号从 0 开始，丝印对齐可能会产生误导。

<img src="../images/manual/016_soldered_2.jpg" height="200">

将电线放入电线盖内置的通道中，确保电线不会被挤压。

<img src="../images/manual/016_assembled_2.jpg" height="200">

添加 M3 x 6mm BHCS 和 M3 x 10mm BHCS 固定电线盖。

<img src="../images/manual/016_assembled.jpg" height="200">

将电线整齐地推在一起，不要盖住 RP2040 按钮或 LED。

<img src="../images/manual/017_wires.jpg" height="200">

### 固件刷写和测试

现在是时候刷写 RP2040 并测试您的工作了！

1. 访问 [releases](https://github.com/EiNSTeiN-/roadrunner-filament-sensor/releases) 页面。
2. 下载适合您配置的固件，例如 `roadrunner_v1_uart_rgb.uf2`。
3. 使用 USB 电缆将 RP2040 连接到您的计算机。
4. 同时按下 `BOOT` 和 `RESET` 按钮，释放 `RESET` 按钮，1 秒后释放 `BOOT` 按钮。
5. 您的计算机上将出现一个名为 `RPI-RP2` 的驱动器。
6. 将 `.uf2` 文件复制到 `RPI-RP2` 驱动器，RP2040 将重新启动，LED 将亮起。

##### 通过 USB、UART 或 I2C 通信？

提供 USB 串行、UART 和 I2C 固件，为所有 MCU 和工具头板提供选项。两者各有优缺点，主要优缺点如下：

**USB 串行：**
* 优点：对于某些配置更方便（无需接线）。
* 缺点：当传感器连接到工具头板或主机没有可用的 USB 端口时不起作用。

**UART：**
* 优点：对于引脚数量有限或没有原生 I2C 支持的 MCU 是个好选择。I2C SDA/SCL 引脚通常是专用引脚，而任何 GPIO 都可以用于 UART TX/RX。
* 缺点：该协议使用位bang实现，与具有原生 I2C 支持的 MCU 相比效率较低。
* 缺点：UART 一次只能读取 4 字节，因此每次从传感器更新都需要多次读取，导致通信开销增加。
* 缺点：与 RP2040 通信时，实现更容易出错，由于重试导致通信开销更大。

**I2C：**
* 优点：可以使用仅 2 个引脚菊花链连接多个传感器（需要重新编译固件以获得唯一的 I2C ID）。
* 优点：可以一次性读取传感器的所有数据，从而降低 MCU 和主机之间的开销。
* 优点：在测试过程中，I2C 比 UART 更不容易出现通信故障。
* 缺点：I2C 错误无法恢复。如果 RP2040 停止响应或 i2c 总线上出现任何延迟，打印机将执行紧急停止并显示 `i2c timeout` 错误。

如果您的 MCU 上有可用的引脚，建议使用 I2C。否则使用 UART。

##### LED 闪烁红色（接线问题）

当 AS5600 板出现错误时，LED 将闪烁红色：

<img src="../images/manual/018_firmware_error.gif" height="200">

此状态的可能原因包括：
1. AS5600 未通过 I2C 总线响应。
2. 磁铁太弱、太强或根本未检测到。

如果您正确组装了传感器，磁铁应该在正确的位置，因此此错误最可能的原因是 AS5600 板的接线问题。

##### LED 显示稳定的红色、绿色或蓝色

如果所有焊接都正确，将惰轮移入和移出 IR 传感器应该会在蓝色和绿色之间切换 LED。

故障排除：
1. 如果 IR 传感器被遮挡时 LED 保持相同颜色，则 IR 传感器可能焊接不正确。
2. 如果 LED 在红色和蓝色之间切换而不是绿色和蓝色，则您可能有一个带有 GRB neopixel 的 RP2040-Zero 克隆。只需刷写 GRB 固件变体即可解决此问题。

一旦杠杆臂可以手动操作，用一段线材进行测试。稍微修剪杠杆臂，使插入线材会将臂推离 IR 传感器，移除线材会遮挡传感器。

<img src="../images/manual/019_IR_arm.jpg" height="200">

完成所有操作后，您的传感器应该可以工作了！

<img src="../images/manual/019_filament_demo.gif" height="200">

### 最后的修饰

添加盖子，用您喜欢的强调色打印。

<img src="../images/manual/020_assembled_1.jpg" height="200">

在顶部和底部添加 PC4-M5 接头。

<img src="../images/manual/021_assembled.jpg" height="200">

最后，添加弹簧组件。

<img src="../images/manual/022_assembled.jpg" height="200">

您的传感器已完成！

### 连接到打印机

连接到打印机取决于传感器将连接到哪个板。请查看[接线手册](./WIRING_CN.md)以获取有关如何将传感器连接到工具头板或打印机控制器板的信息。