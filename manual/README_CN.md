# Roadrunner: 高分辨率线材运动传感器

![预览](../images/assembly.png)

### 特性

* **高分辨率**：亚毫米级运动检测
* **低成本**：在 Aliexpress 上购买零件约需 12 美元
* **方向检测**：可检测挤出或回抽方向（不同于基于脉冲的运动传感器）
* **高精度**：基于 24mm 旋转距离，可检测 0.7 度或 0.04mm 的变化
* **Neopixel 状态指示器**：显示线材移除、线材插入、线材移动状态
* **小巧轻便**：尺寸仅为部分商业产品的一半
* **故障原因分析**：可区分线材耗尽、线材堵塞或部分挤出不足
* **双线通信**：支持 UART 或 I2C 与 MCU 通信（适用于大多数主板）
* **最大流量校准**：提供实用的 gcode 命令进行校准

大多数 3D 打印机运动传感器体积庞大、触发缓慢、容易误报，且检测距离较大，这意味着在实际检测到耗尽之前已经挤出了大量材料，导致层间附着力差，耗尽后打印失败。

Roadrunner 运动传感器基于磁性旋转编码器，可检测线材的亚毫米级移动以实现精确的挤出长度测量，此外还配备了 IR 传感器，可立即检测线材耗尽。通过结合这两种传感器和专用的 RP2040-Zero 板收集数据，Roadrunner 可以检测影响打印的问题类型，包括线材用完、线材不再移动或线材移动但挤出速率低于预期。

利用传感器的高精度，可以确定预期的长度、速度和体积流量，并与实时测量值进行比较，从而合理估算线材的挤出不足程度。提供简单易用的 `CALIBRATE_MAX_FLOW` 命令进行自由空气挤出测试，并自动对任何温度/喷嘴/材料组合的最大体积流量进行基准测试。

### 构建指南

这是一个开源项目，您可以自己构建！

请参阅[材料清单](BOM_CN.md)和[组装手册](ASSEMBLY_CN.md)。

### Klipper 安装

克隆仓库并创建指向传感器代码的符号链接：

```
cd ~
git clone --depth 1 --single-branch https://github.com/EiNSTeiN-/roadrunner-filament-sensor.git
~/roadrunner-filament-sensor/install.sh
```

重新启动 klipper 以识别新的传感器类型：
```
sudo service klipper restart
```

要在 moonraker 中获得自动更新，请在 `moonraker.conf` 末尾添加以下块：
```ini
[update_manager roadrunner-filament-sensor]
type: git_repo
path: ~/roadrunner-filament-sensor
origin: https://github.com/EiNSTeiN-/roadrunner-filament-sensor.git
install_script: install.sh
primary_branch: main
managed_services: klipper
```

### 配置

将以下配置添加到您的 `printer.cfg` 中：

```
[high_resolution_filament_sensor roadrunner]
extruder: extruder
#  此传感器连接的挤出机
uart_rx_pin: P0.0
uart_tx_pin: P0.1
#  UART 通信的 TX 和 RX 引脚，如果使用 i2c 或 USB 串口则省略（见下文）。
#i2c_address: 64
#  传感器在 I2C 总线上的地址，默认为 64 (0x40)。
#  只有在同一 I2C 总线上连接多个传感器时才需要更改。
#i2c_mcu: mcu
#  传感器连接到的 MCU。
#i2c_bus: i2c1
#  MCU 上传感器连接的 I2C 总线。
#serial: /dev/serial/by-id/usb-Raspberry_Pi_Pico_E66130100F655734-if00
#  传感器连接的串口。使用 `ls -alh /dev/serial/by-id` 查找。
pause_on_runout: False
#  当触发耗尽条件时是否自动暂停打印。
runout_gcode: RESPOND TYPE=command MSG='检测到线材耗尽'
#  检测到耗尽条件时执行的 gcode。
invert_direction: False
# 如果测量的距离/速度值为负，将此设置为 True。
rotation_distance: 23.4
# 对应于磁体在旋转编码器上完整旋转 360 度的线材长度。
# 一个好的起点是传感器中 BMG 齿轮的周长。
underextrusion_max_rate: 0.5
# 可以容忍的挤出不足率，取值范围为 0.0 到 1.0。
# 该速率通过比较挤出机位置与测量的线材长度在一定时间段内计算得出。
# 速率为 0.0 表示测量的线材运动与该时间段内预期的挤出距离匹配。
# 速率为 1.0 表示当挤出机预期有运动时根本没有检测到线材运动。
underextrusion_period: 5
# 挤出不足率超过最大速率后，触发耗尽前应持续多长时间。
hysteresis_bits: 3
# 忽略磁性旋转编码器角度数据的位数。
# 此设置决定传感器的最小可检测位置变化。
# 配置过低可能导致即使线材不移动也在两个相邻位置之间抖动。
```

每个传感器会创建两个虚拟引脚，可用于配置 [`filament_motion_sensor`](https://www.klipper3d.org/Config_Reference.html#filament_motion_sensor) 或 [`filament_switch_sensor`](https://www.klipper3d.org/Config_Reference.html#filament_switch_sensor)。这主要是为了方便与 mainsail/fluid 等用户界面配合使用。虚拟运动传感器将在传感器移动最小可检测距离时触发。开关传感器将在 IR 传感器检测到线材存在时触发。这两个对象都独立于 `high_resolution_filament_sensor` 中实现的耗尽检测算法工作，该算法考虑了挤出不足等其他因素。

示例配置如下：

```
[filament_motion_sensor roadrunner_motion]
detection_length: 2
extruder: extruder
switch_pin: virtual_motion_sensor:roadrunner
pause_on_runout: False
runout_gcode: RESPOND TYPE=command MSG='线材运动传感器检测到耗尽'
event_delay: 0.1

[filament_switch_sensor roadrunner_switch]
switch_pin: virtual_switch_sensor:roadrunner
pause_on_runout: False
runout_gcode: RESPOND TYPE=command MSG='线材开关传感器检测到线材不再存在'
event_delay: 0.1
```

### 使用方法

##### 校准速查表

以下是完整校准流程的示例：

```
# 禁用一些功能以进行测试
SET_PRESSURE_ADVANCE EXTRUDER=extruder ADVANCE=0
SET_Z_THERMAL_ADJUST TEMP_COEFF=0 ENABLE=0

# 可选：归位打印机并移动到安全 Z 位置
G32
G0 Z100

# 检查传感器信息
QUERY_FILAMENT_SENSOR SENSOR=roadrunner

# 查找 rotation_distance 值，不要忘记将其保存在传感器配置中并重启。
CALIBRATE_FILAMENT_SENSOR_ROTATION_DISTANCE SENSOR=roadrunner TEMP=250 LENGTH=30 SPEED=30 COUNT=10

# 查找此挤出机的最大流量，保存收集数据的图表以供将来参考（这需要一些时间）
CALIBRATE_MAX_FLOW SENSOR=roadrunner TEMP=250 START=1 STOP=40 SAVE_GRAPH=1

# 测量高速 G1 命令（例如 11.5mm/s = 690mm/m）。
# 使用在前一个图表的非线性部分产生流量的值。
FILAMENT_SENSOR_MEASURE SENSOR=roadrunner TEMP=250 SPEED=690 LENGTH=100 SAVE_GRAPH=1

# 使用 `CALIBRATE_MAX_FLOW` 建议的系数启用非线性挤出
ENABLE_NONLINEAR_EXTRUSION SENSOR=roadrunner ENABLE=1 COEFFICIENTS=-0.1106867,1.1197162,-0.0347608,0.0028625

# 再次进行相同测量，测量的长度和速度应显示很小偏差。
FILAMENT_SENSOR_MEASURE SENSOR=roadrunner TEMP=250 SPEED=690 LENGTH=100 SAVE_GRAPH=1

# 现在通过生成新图表检查挤出补偿的结果（这需要一些时间）
CALIBRATE_MAX_FLOW SENSOR=roadrunner TEMP=250 START=1 STOP=40 STEP=2 COUNT=2 SAVE_GRAPH=1
```

##### 校准传感器的旋转距离：

首先要做的是校准传感器本身的 `rotation_distance`。这通过 `CALIBRATE_FILAMENT_SENSOR_ROTATION_DISTANCE` 完成：

```
CALIBRATE_FILAMENT_SENSOR_ROTATION_DISTANCE SENSOR=roadrunner TEMP=250 LENGTH=30 SPEED=60 COUNT=10
```

* `TEMP`: 挤出机加热器加热到的温度。默认为 200。
* `LENGTH`: 要回抽的线材长度。默认为 25。
* `SPEED`: 挤出速度，单位为转/分钟（例如 300 表示 5mm/s）。默认为 30（0.50mm/s）。
* `COUNT`: 执行测试的次数。默认为 10。

此宏会将挤出机加热器加热到指定温度，多次回抽线材，并计算一些统计数据。测量在回抽线材时进行，以避免因喷嘴中的背压引入误差。确保在挤出机齿轮仍能夹住线材的情况下，至少可以回抽 `LENGTH` mm 长的线材。为获得最佳结果，请以低速在足够长的距离上执行此测试，使传感器的齿轮能够完成一整圈旋转。

获取距离测量后，按频率排序，使用频率最高的测量值计算新的 `rotation_distance`。例如，如果 8/10 次测量在预期 25mm 时读取 25.62mm，则 `rotation_distance` 过高，会自动建议新值。将新的 `rotation_distance` 保存在传感器配置中后，使用相同的宏参数重复测试应测量正确的距离。测量之间的偏差相对于最小可检测位置变化报告，小偏差表示可重复的测量。虽然传感器精度很高，但其多次读数的准确性和可重复性可能受到现实因素的影响，例如 PTFE 管的弹性、传感器与挤出机主体的连接等。

![预览](../images/example_rotation_distance.png)

##### 测量 G1 命令

使用 G1 命令挤出一些线材，并打印传感器测量的结果。

```
FILAMENT_SENSOR_MEASURE SENSOR=roadrunner TEMP=250 LENGTH=10 SPEED=120 COUNT=1
```

* `TEMP`: 挤出机加热器加热到的温度。默认为 200。
* `LENGTH`: 要回抽的线材长度。默认为 10。
* `SPEED`: 挤出速度，单位为转/分钟（例如 300 表示 5mm/s）。默认为 120（2mm/s）。
* `COUNT`: 执行测试的次数。默认为 1。
* 使用 `SAVE_GRAPH=1` 参数，所有测量的图表将保存到 klipper 的当前工作目录（通常为 `~/klipper/`）。

该宏将运行一个或多个 G1 命令，直到挤出所需长度的线材。宏将打印传感器测量的每次移动的统计数据。

如果保存了图表，输出将如下所示：

![预览](../images/FILAMENT_SENSOR_MEASURE_timeline_0_20240118_100732.png)

##### 最大流量校准

此宏的目标是帮助确定给定线材、喷嘴和温度组合的最大体积流量。开始前，请确保挤出机的 `filament_diameter` 正确，以及线材传感器的 `rotation_distance` 正确。为获得最佳结果，`STOP` 值应高于或接近挤出机预期的最大体积流量率。

```
CALIBRATE_MAX_FLOW SENSOR=roadrunner TEMP=250 START=1 STOP=50 SAVE_GRAPH=1
```

* `TEMP`: 挤出机加热器加热到的温度。默认为 200。
* `DURATION`: 每次测试挤出的持续时间，用于确定挤出长度（长度 = 速度 * 持续时间）。默认为 10 秒。
* `START`: 要测试的最小体积流量。默认为 5。
* `STOP`: 要测试的最大体积流量。默认为 25。
* `STEP`: 每次测试之间体积流量的增量。默认为 1。
* `COUNT`: 每个体积流量值执行测试的次数。默认为 5。
* `MAX_SPEED_DEVIATION`: 预期速度和测量速度之间可接受的偏差百分比。用于推荐最大体积流量。默认为 0.05（5%）。
* 使用 `SAVE_GRAPH=1` 参数，所有测量的图表将保存到 klipper 的当前工作目录（通常为 `~/klipper/`）。

![预览](../images/example_max_flow.png)

当给定的体积流量太高而无法以该速率进行所有测量时，此宏将发出警告。这通常对应于挤出机的物理极限，此时背压过高，挤出机无法跟上并咬入线材。

获取测量后，此宏将查找预期速度和测量速度之间偏差低于 `MAX_SPEED_DEVIATION` 百分比的速度范围。在较高速度下，喷嘴中的背压会导致挤出不足，这可以在数据图中（使用 `SAVE_GRAPH=1`）直观地识别为预期速度和测量速度开始急剧偏离的曲线部分。将多项式曲线拟合到预期速度与测量速度之间的关系，可用于补偿较高速度下的挤出不足。

如果保存了图表，输出将如下所示：

![预览](../images/CALIBRATE_MAX_FLOW_measured_vs_expected_speed_20240117_195539.png)

##### 启用非线性挤出补偿（实验性）

必须添加 `[non_linear_extrusion]` 配置部分，此命令才能与给定的挤出机一起工作：
```
[non_linear_extrusion t0]
extruder: extruder
# 可以选择启用非线性挤出补偿的挤出机名称。
```

启用非线性挤出补偿后，挤出机中压力的增加将通过增加 G1 命令的请求速度来补偿。这适用于纯挤出移动以及常规打印移动。补偿仅在使用相对坐标且指定正 E 距离时应用，因为回抽移动不受喷嘴压力的影响。

```
ENABLE_NONLINEAR_EXTRUSION EXTRUDER=extruder ENABLE=1 COEFFICIENTS=-0.1106867,1.1197162,-0.0347608,0.0028625
```

* `ENABLE`: 1 启用，0 禁用
* `COEFFICIENTS`: 逗号分隔的多项式系数列表。