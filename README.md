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

