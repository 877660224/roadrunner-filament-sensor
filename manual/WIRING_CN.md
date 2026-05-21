# 接线

与 Roadrunner 传感器通信有三种方式：
* 连接到任何 MCU 的 I2C 或 UART，或
* 通过 USB 串行连接到主机。

每种方式都有相应的固件变体，根据 rp2040 板上刷写的变体，您需要正确将 Roadrunner 连接到打印机。

### UART

任何 2 个 GPIO 都可以用于 UART 通信，这在某些板上非常方便。UART 通信可能不太可靠，需要更多开销，因此如果可能，首选 I2C。找到您的 Roadrunner 传感器将连接到的 MCU 的原理图，并找到任何具有 `5V`、`GND` 和两个可用 GPIO 的 4 针（或更多）连接器。

### I2C

本机 I2C 通信需要微控制器上有专用引脚。有些板有专门用于 I2C 的连接器，但许多板没有。

要找出哪些总线编译到您的 klipper 版本中，请下载 `klipper.log` 并搜索 `BUS_PINS_i2c`。每个 MCU 都会有一行，列出所有 i2c 总线的名称和相应的引脚。

以下是您可能想要连接此传感器的一些常见板。

### USB 串行

只需通过 USB 连接，您可以省去底部的电线盖。

#### BTT SB2209/SB2240 (stm32 mcu)

原理图：[SB2209](https://github.com/bigtreetech/EBB/blob/master/EBB%20SB2240_2209%20CAN/SB2209/Hardware/SB2209.png), [SB2240](https://github.com/bigtreetech/EBB/blob/master/EBB%20SB2240_2209%20CAN/SB2240/Hardware/SB2240.png)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c1_PB6_PB7 | Endstop | Stop1 (PB6) | Stop3 (PB7) | 不太方便，因为每个引脚位于不同的连接器上 |
| i2c1_PB8_PB9 | BLTouch | PROBE (PB8) | SERVOS (PB9) | |
| i2c1_PA9_PA10 | SPI_OUT | IO (PA9) | NSS (PA10) | |
| i2c2_PB10_PB11 | SPI_OUT | CLK (PB10) | MOSI (PB11) | |

#### BTT SB2209 (rp2040 mcu)

原理图：[SB2209](https://github.com/bigtreetech/EBB/blob/master/EBB%20SB2209%20CAN%20(RP2040)/Hardware/EBB%20SB2209%20CAN%20V1.0%EF%BC%88RP2040%EF%BC%89-Pin.png)

该板上没有好的 I2C 选项，最不坏的选择是使用 USB-C 与 klipper 主机通信，这释放了 CAN-L/CAN-H 引脚通过 I2C 连接传感器。除非您只需要一个热端风扇或可以将一个风扇重新定位到另一个引脚。

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c0b | CAN port | CAN-L (gpio5) | CAN-H (gpio4) | 仅当板通过 USB-C 连接时可用 |
| i2c0d | SB0000 | Parts cooling fan (gpio13) | TACH (gpio12) | 仅当 SB0000 上不使用 PWM 风扇和零件冷却风扇时可用 |
| i2c1d | SB0000 | PWM (gpio15) | Hotend Fan (gpio14) | 仅当 SB0000 上不使用 PWM 风扇和热端风扇时可用 |

#### BTT EBB36/42 V1.0 (stm32 mcu)

原理图：[EBB36](https://github.com/bigtreetech/EBB/blob/master/EBB%20CAN%20V1.0%20(STM32F072)/EBB36%20CAN%20V1.0/Hardware/EBB36%20CAN%20V1.0-PIN.png), [EBB42](https://github.com/bigtreetech/EBB/blob/master/EBB%20CAN%20V1.0%20(STM32F072)/EBB42%20CAN%20V1.0/Hardware/EBB42%20CAN%20V1.0-PIN.png)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c1_PB6_PB7 | I2C | PB6 | PB7 | 最佳选择 |
| i2c1_PB8_PB9 | CAN | RX (PB8) | TX (PB9) | 仅当板通过 USB-C 连接时可用 |

#### BTT EBB36/42 V1.1 / V1.2 (stm32 mcu)

原理图：[EBB36](https://github.com/bigtreetech/EBB/blob/master/EBB%20CAN%20V1.1%20(STM32G0B1)/EBB36%20CAN%20V1.1/Hardware/EBB36%20CAN%20V1.1%26V1.2-PIN.png), [EBB42](https://github.com/bigtreetech/EBB/blob/master/EBB%20CAN%20V1.1%20(STM32G0B1)/EBB42%20CAN%20V1.1/Hardware/EBB42%20CAN%20V1.1%26V1.2-PIN.png)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c3_PB3_PB4 | I2C | PB3 | PB4 | 最佳选择 |
| i2c1_PB6_PB7 | Endstop | PB6 | PB7 | |
| i2c1_PB8_PB9 | Probe | PB8 | PB9 | |

#### Mellow Fly SB2040 V2 (rp2040 mcu)

原理图：[Fly SB2040 v2](https://www.fabreeko.com/cdn/shop/products/Mellow-Fly-SB2040-V1-Board-For-Voron-2-4-R2-Trident-Stealthburner-CW2-Extruder-Klipper-Hotend.jpg_Q90.jpg__4_550x.webp?v=1675264365)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c0d | EXP | FAN0 (gpio13) | RGB (gpio12) | |
| i2c0e | EXP | PWM1 (gpio17) | PWM0 (gpio16) | |
| i2c0h | Endstop | gpio29 | gpio28 | |

#### Fystec SB CAN Toolhead Board (stm32 mcu)

原理图：[SB CAN TH](https://wiki.fysetc.com/SB%20CAN%20ToolHead/)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c1 | IN.0 / IO.2 | IN.0 (PB6) | IO.2 (PB7) | |

#### BTT SKR v1.1 (lpc176x mcu)

原理图：[SKR v1.1](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.1/blob/master/hardware/SKR%20V1.1%20PINOUT.pdf)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c1 | E1 motor | STEP (P0.1) | DIR (P0.0) | |
| i2c1a | Z motor | STEP (P0.20) | EN (P0.19) | |

#### BTT SKR v1.3 (lpc176x mcu)

原理图：[SKR v1.3](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/blob/master/BTT%20SKR%20V1.3/hardware/SKR-V1.3-pinout.jpg)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c1 | E1 motor | STEP (P0.1) | DIR (P0.0) | |
| i2c1a | Y motor | DIR (P0.20) | STEP (P0.19) | |

#### BTT SKR v1.4 (lpc176x mcu)

原理图：[SKR v1.4](https://github.com/bigtreetech/BIGTREETECH-SKR-V1.3/blob/master/BTT%20SKR%20V1.4/Hardware/SKR-V1.4-pinout.jpg)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c1 | I2C | SCL (P0.1) | SDA (P0.0) | 该板的最佳选择 |
| i2c1a | Y motor | DIR (P0.20) | STEP (P0.19) | |

#### BTT SKR 2 (stm32 mcu)

原理图：[SKR 2](https://github.com/bigtreetech/SKR-2/blob/master/Hardware/BIGTREETECH%20SKR%202-Pin.pdf)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c1 | Fans | FAN1 (PB6) | FAN2 (PB7) | |
| i2c1a | I2C | PB8 | PB9 | 该板的最佳选择 |
| i2c2 | Wifi | PB10 | PB11 | |

#### BTT SKR 3 / SKR 3 EZ (stm32 mcu)

原理图：[SKR 3](https://github.com/bigtreetech/SKR-3/blob/master/Hardware%20(SKR%203)/BIGTREETECH%20SKR%203-PIN.pdf), [SKR 3 EZ](https://github.com/bigtreetech/SKR-3/blob/master/Hardware%20(SKR%203%20EZ)/BIGTREETECH%20SKR%203%20EZ%20V1.0-PIN.pdf)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c1 | Fans | FAN1 (PB6) | FAN0 (PB7) | |
| i2c2 | Wifi | PB10 | PB11 | |

#### BTT Octopus v1.0 / Octopus Pro (stm32 mcu)

原理图：[Octopus v1.0](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/blob/master/Hardware/BIGTREETECH%20Octopus%20-%20PIN.pdf), [Octopus Pro](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro/blob/master/Hardware/BIGTREETECH%20Octopus%20Pro%20V1.1-Pin.jpg)

| Klipper 总线名称 | 连接器 | SCL | SDA | 备注 |
|---|---|---|---|---|
| i2c1 | Probe | PB6 | PB7 | |
| i2c1a | I2C | PB8 | PB9 | 该板的最佳选择 |

#### 其他

在任何板上找到合适的连接器相对容易，您可以按照以下步骤为您的特定板操作：

1. 找到板上的处理器类型，例如 BTT SB2209 的 STM32，BTT SKR 1.4 的 lpc1769 等。
2. 在 [klipper 仓库](https://github.com/Klipper3d/klipper/tree/master/src) 中，在您的处理器类型的子文件夹中找到名为 `i2c.c` 的文件，例如 [`src/lpc176/i2c.c`](https://github.com/Klipper3d/klipper/blob/master/src/lpc176x/i2c.c)。
3. 文件中列出了总线名称和相应的引脚名称，例如行 `DECL_CONSTANT_STR("BUS_PINS_i2c1", "P0.1,P0.0");` 表示名为 `i2c1` 的总线的 SCL 和 SDA 引脚分别在 `P0.1` 和 `P0.0` 上。
4. 找到您板的原理图，其中所有连接器都标有引脚。
5. 使用原理图，检查板上哪些可用连接器映射到 I2C 总线。