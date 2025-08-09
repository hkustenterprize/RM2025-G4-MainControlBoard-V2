<div align='center'>
    <h1>RM2025 G4主控板V2</h1>
</div>

<div align='center'>
    <h2>RM2024香港科技大学ENTERPRIZE</h2>
</div>

![alt text](image/image1.png)

**开源论坛链接：**

**欢迎加入开源交流QQ群：581184202**

## 文件内容

核心板：[点击在线预览](https://kicanvas.org/?github=https%3A%2F%2Fgithub.com%2Fhkustenterprize%2FRM2025-G4-MainControlBoard-V2%2Ftree%2Fmain%2FRM2025_G473_MCB)

扩展板：[点击在线预览](https://kicanvas.org/?github=https%3A%2F%2Fgithub.com%2Fhkustenterprize%2FRM2025-G4-MainControlBoard-V2%2Ftree%2Fmain%2FRM2025_G473_MCB_StdExtend)

~~~
RM2025-G4-MainControlBoard-V2
├── image
├── RM2025_G473_MCB             > 核心板工程文件
├── RM2024_G473_MCB_StdExtend   > 标准扩展板工程文件
├── LICENSE
└── README.md
~~~

开源工程文件设计软件为 **KiCAD 8.0**，可以导入立创EDA

每个工程文件文件夹中的内容:

~~~
xxx
├── ibom             > 焊接工具，由 "Interactive HTML BOM" 插件生成
├── production      > 生产文件，由 "Fabrication Toolkit" 插件生成，将其中的zip文件拖入JLC下单助手即可
├── xxx.kicad_pcb   > PCB
├── xxx.kicad_pro   > 主工程，通过双击这个启动KiCAD
└── xxx.kicad_sch   > 原理图
~~~


## 重要参数


主控板分为核心板（下板）和扩展板（上板），两板之间使用80pin BTB接口连接，尺寸上保持为与C板一致的 60x40mm；核心板包含所有核心芯片，扩展板用于引出接口或进行其他功能的扩展。

- MCU: STM32G473VET6
- IMU: ICM-42688P
- TTL: CH343P *2 
- FDCAN *3
- RS485 (硬件流控) *1
- FLASH: W25N01GVZEIG


## 主要变动

较比上代开源（[RM2024 G4主控板开源](https://github.com/hkustenterprize/RM2024-MainControlBoard)），2025赛季的G4主控板根据需求做出了一些变动和优化，主要变化如下：

- 核心板重新 layout，将 QSPI FLASH 的走线进行了等长
- 移除磁力计，移除 CH343P 的流控引脚连接
- 在核心板上增加两个 GPIO 接口
- 将核心板的 MCU 供电由 Buck 改为 LDO
- 扩展板重新 layout，增加了专用于舵轮分电板的 7pin 接口
- 扩展板增加四个 WS2812 LED用于指示状态，将蜂鸣器移至核心板上

## 电源网络


## 外设引脚分配




| <u>CAN</u> (SIT1042T/3) | CAN_TX | CAN_RX |
| ---- | ---- | ---- |
| **CAN1**   | PD1   | PD0   |
| **CAN2**   | PB6   | PB5  |
| **CAN3**   | PB4   | PB3   |


| <u>RS485</u> (SN75176AD) | TX <sup>1</sup> | RX <sup>1</sup> | DE <sup>2</sup>|
| ---- | ---- | ---- |---- |
| **RS485 (UART4)**   | PC10   | PC11  | PA15 |


1. 不使用RS485功能时 UART4 可作为 独立UART 使用。
2. 不使用RS485功能时 **DE引脚** 需要配置为 **高电平**，需要确保 **RS485接口** 没有连接。


| <u>TTL</u> (CH343P) | TX | RX |
| ---- | ---- | ---- |
| **TTL1 (UART2A)**   | PA2  | PA3  |
| **TTL2 (UART3A)**   | PD8  | PD9  |


- 两个TTL芯片均连有状态指示灯，在接口两侧。
- CH343P 接入USB线 (VBUS有供电) 后自动使能。


| <u>IMU</u> (ICM-42688P) | SCLK | MISO | MOSI | CS | DRDY |
| ---- | ---- | ---- | ---- | ---- | ---- |
| **IMU (SPI2)**   | PB13  | PB14  | PB15 | PB12 | PB10 |


- **IMU加热**开关由 **PA4** 引脚控制，可以使用 **TIM3_CH2** 或 **GPIO SET/RESET**。
- IMU朝向已在板背面标明


| <u>DR16/i10B</u> | RX <sup>1 2</sup> |
| ---- | ---- |
| **RX1 (UART1C)**   | PA10 |
| **RX2 (UART3C)**   | PB11 |


1. 两个引脚硬件上连接在一起，可使用任意一个引脚 (对应的串口) 读取接受机数据。
2. 使用一个引脚时，**另外一个引脚一定要悬空**。


| <u>FLASH</u> (W25N01GVZEIG) | CS | CLK | IO0 | IO1 | IO2 | IO3 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| **FLASH (QSPI1)**   | PE11  | PE10  | PB1 | PE13 | PE14 | PA6 |


| <u>其他外设</u>  | Pin |
| ---- | ---- |
| **VIN 24V ADC**   | PA8  |
| **5V ADC**   | PA9  |
| **Buzzer**   | PE3  |
| **LED**   | PC13  |
| **Button**   | PC15  |

## 串口分配


|       | TX | RX | DE | 功能 |
| ---- | ---- | ---- | ---- | ---- |
| **UART1A**   | PC4  | PC5  |-      | 裁判系统/图传 |
| **UART1B**   | PE0  | PE1  |- | 通用串口  |
| **UART1C**   | -  | PA10  |-  | 接收机  |
| **UART2A**   | PA2  | PA3  |-  | MiniPC1 TTL |
| **UART2B**   | PD5  | PD6  |-  | 通用串口  |
| **UART3A**   | PD8  | PD9  |-  | MiniPC2 TTL |
| **UART3B**   | PB9  | PE15  |-  | 通用串口  |
| **UART3C**   | -  | PB11  |-  | 接收机  |
| **UART4**   | PC10  | PC11  | PA15 | RS485/通用串口<sup>1</sup>  |
| **UART5**   | PC12  | PD2  |-  | 裁判系统/图传  |
