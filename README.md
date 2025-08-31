# ELRS-NoSMD-Receiver
这款「全是模块 elrs 接收机」DIY 方案，ESP3—C3芯片，仅需两模块（各十块钱），20 元内搞定，无需焊接任何分立元器件，手搓难度极低，无需对付贴片小元件，功能全，还能秒变发射机。
过了英文摘要就是详细中文内容


> **A revolutionary ExpressLRS receiver design** eliminating surface-mount soldering,**No discrete components** needed at all
> built by creatively combining **two commercial PCBA modules** with **full configurability**，while keeping **costs extremely low**（perhaps only in China）

## 🚀 Key Innovations
- **No SMD Soldering Required**: Uses pre-built ESP32-C3 & RF modules only  
- **Dual-Function Hardware**: Software-configurable as receiver or transmitter  
- **Beginner-Friendly Assembly**: Connect modules via jumper wires or custom PCB  
- **Cost Efficiency**: Total BOM under **¥20 (≈$3) in China** 
- **Interface types**: UART, PWM, I²C expansion, voltage sensing

## 📦 Core Hardware
| Module | Specification | Key Notes |
|--------|---------------|-----------|
| **ESP32-C3 Super Mini** | 4MB Flash, USB-C, 13 GPIO | ⚠️ Avoid fake boards without flash memory |
| **RF Module** | SX1280/1281(E28/A28) and LR1121（E80）| ❌ PA modules not supported |

## ⚡ Quick Start
1. **Flash Firmware** via USB using ELRS Configurator or ELRS Web Flasher
   - Target: `Generic ESP32-C3`
   - Select `Standard` or `PWM Output` firmware
2. **Connect Modules** with 9 wires (SPI+RST+BUSY+DIO)  
   - GPIO mapping freedom 
3. **Configure Hardware** at `10.0.0.1/hardware.html`
4. **Mount & Fly**! Supports Betaflight/INAV/ArduPilot/PX4 （CRSF protocols）

---

### 🌍 Cost Comparison by Region(All countries except China are for reference only.)
| Region       | Modules Cost | Shipping | Est. Total | Availability |
|--------------|--------------|----------|------------|--------------|
| China Mainland | ¥20          | ¥0       | **$3**   | 2-3days (Sent from Shenzhen)    |
| USA          | $9           | $4       | **$13**    | 2-3 weeks    |
| EU           | €8           | €5       | **€13**    | 3-4 weeks    |

---
Even in China, this is a very low price – three times cheaper than commercially available finished products.



## 🔗 Resources
- [📺 Video Tutorial](https://b23.tv/cTPp4Ne) · [📁 PCB Files](https://oshwhub.com/jianchibuyongduo/board2)  
- [💬 QQ Group 902021691] · [📚 Full Docs](https://xcnmlw0olagh.feishu.cn/wiki/SgmEwbcjciPRihklynUcdX4qnAJ)  

---
**License**: GPL-3.0 · **Designed by** [ServolessWaverider](https://github.com/ServolessWaveRider)  
> *Proudly created by a Chinese high school student – innovation has no age barriers.I want to boost global drone enthusiasts' innovation!*  

---

**For complete technical details, assembly guides, and advanced configurations, continue reading below ↓**  
*(Note: Following content is in Chinese. International users may use browser translation)*



前所未有！！我能让你永远不用买任何「成品elrs设备」
作者：坚持不用舵ServolessWaverider，本人是一名喜爱电子和无人机的高中生，本文章和本项目均为原创，允许转载，但必须注明来源及作者。欢迎复刻，二创，但必须遵守开源协议，不要把本作品说成是自己造的哦。
[图片]
[图片]

  我这个方案可以说是小学生都会手搓了，因为它只需要两个模块，连在一起就能构成一个完整的elrs接收机。它可以串口连接飞控，可以有六通道PWM，还能做的电池电压检测，甚至是额外连接一个气压计或GPS等模块用于在不用飞控的情况下回传数据。不过这还不是最牛逼的功能，得益于它是esp32芯片，而不是esp8266或8285，所以它可以被软件定义成高频头，然后塞进你的遥控器里作为发射机使用。我们可以打一块PCB板把两个模块双面贴装，当然最简单粗暴的方法是直接飞线连接。该DIY方案彻底解决了要用镊子对付蚂蚁大小的元器件的问题，大大降低手搓难度，而且一样保持的30元不到的极低成本。是以往开源elrs方案前所未有的，所以我将其称之为「全是模块elrs接收机」。

  第一个模块是一个叫做super mini的esp32c3开发板，虽然体积非常的小，但集成度很高，而且15个GPIO都引出了，还自带USB接口。

[图片]
不过有一个你一定要注意的大坑，就是市面上存在部分假货开发板，使用的是这种内部没有flash存储器的esp32c3芯片，这就像一个没有硬盘的电脑，运行不了一点程序，所以买之前一定问清楚卖家，到底有没有内置4MB的flash？

[图片]
我们分析一下它的电路：
[图片]
[图片]
[图片]

首先最大的这个就是esp32c3 soc芯片，他有WiFi功能和15个GPIO，这个板子的设计将其中两个固定用于USB，所以我们实际可用13个GPIO。然后是一个有500m带载的能力的3.3v线性稳压器，不过也有些esp32c3 super mini开发板的线性稳压器只有100mA带载能力，需要你格外检查稳压器的丝印。这个接收机在我的设计中，射频芯片和esp32芯片都由它供电。两个指示灯，红的是电源指示灯，蓝的则是由GPIO8控制亮灭。还有两个开关，一个rst开关连接到芯片的chip EN使能管脚，我们可以快速按它三次让接收机进入对频状态，而不需要让飞机所有航电设备承受三次上电冲击。另一个boot开关可用于升级固件或在elrs系统里设置为button。最底下还有一个2.4G贴片天线用于wifi连接，不过这个天线的效果不是特别好，所以在设计时尤其要注意RF净空区，不能有一点遮挡。

[图片]

[图片]
[图片]
[图片]
[图片]

另一个是射频模块，这正是本设计所前所未有的地方！我创造性的在一块区域内设计了三合一封装，你可以将三种模块的其中一种焊接到板上，从而实现高兼容性，打一次板就能做出不同的接收机。由于射频芯片和esp32共用一个LDO，所以我们不能使用加装了PA功率放大器的射频模块，因为PA所需电流较大。会使LDO过载。目前理论上支持sx1280芯片，以及2.4G和915mhz的LR1121芯片。其中2.4G的模块采用成都泽耀的A28和成都亿佰特的E28，双频模块目前市面上无可用型号，虽然搭载LR1121芯片的模块有亿佰特的E80，但是经过我的实测，它在适配elrs系统时晶振供电时序存在BUG，只能寄希望于elrs官方更新固件。
[图片]

不过如果你使用成都泽耀的A28模块时，需要覆盖并绝缘这四个焊盘（如图所示，我的方案是贴PI耐高温胶带，你也可以用绿油，甚至直接割断这几个焊盘的走线），否则可能会出现问题，相信聪明的你，看完PCB版图就知道为什么要这么做了
[图片]
  我们先给空白芯片的开发板烧录固件。由于这个接开发板自带USB功能，我们可以直接把它连到电脑上刷写固件，完全不需要USB转UART的转串口模块。注意：我们需要在上电瞬间或者rst重启瞬间保持boot按钮处于按下状态（相当于在芯片启动瞬间要拉低GPIO9）让芯片进入flash download固件下载模式，不过此方法主要用于第一次给空白芯片刷入固件。如果芯片内已经有elrs固件，则完全可以使用wifi连接esp32，使用elrs系统提供的OTA无线升级功能。连接到电脑后，你需要打开浏览器，搜索“elrs中国镜像站”并进入，选择接收机，在Device vendor（设备厂商）一栏，选择Generic Target used as a base(用作基础的通用目标设备)。在Hardware Target（硬件目标）”选项卡中找到Generic ESP32 C3，可以看到有四种固件，分别是有功率放大器的固件，普通固件，PWM输出固件和双射频真分集固件。正常连接飞控需要选择第二种，如果你想用pwm输出则选择第三种。然后翻到下一页，填入对频密码，无线电标准（要填Fcc），WiFi名和密码，固件刷写方式（这里要选串行Uart）。在下面的高级设置里面，可以选择在上电多少秒进入wifi调参，这里保持默认60秒。还可以选择将其刷写为高频头还是发射机。不勾选就是接收机。然后是uart串口波特率，这里也可以保持默认420000波特率。再翻到下一页，按照提示打开串口，选择JTAG 点击Flash按钮烧入固件。等待进度条跑完，如果此时蓝色指示灯亮起，那就大功告成了！


[图片]

  接下来开始布线，射频模块和开发板之间需要连接9根线，这9根线分别是四根spi通讯线，用于传输数据。一根rst线，用于重启射频模块。一根busy线，用于告诉esp32我现在很忙，别给我发新的数据。还有一个dio线，用于触发中断控制或者额外功能。
[图片]
那具体该怎么连接9根线呢，答案其实是你怎么爽怎么连。因为esp32芯片有一个比较NB的功能，叫做GPIO交换矩阵，它可以将外部信号与内部片内控制模块（如硬件uart收发器，spi控制器，i2c控制器）进行任意的映射和路由，注意这不是软件模拟出来的接口。
[图片]
不过随意连接也不是绝对爽，它有以下限制：
一、芯片存在三个strapping管脚，分别为GPIO2，8，9。在上电瞬间，他的锁存值影响芯片的启动模式，所以板子上为他们各自都安排了一个10kΩ上拉电阻。这个上拉电阻在这条线路两头都各自提供有明确电平且非高速通信的情况下并不会影响什么，但是在高频信号通信时会影响这个线路上的阻抗并导致上升沿缓慢。因此它不能用于高波特率的UART的Tx和Rx线。也不能用于SPI的miso，mosi ，clk线，但是片选信号线CS可以，因为它的速度足够低。对于开漏输出的I2C接口的scl和sda线也不推荐使用，因为它会影响线路原本4.7kΩ的上拉强度，所以它们不能连接外置i2c气压传感器
[图片]

二、GPIO8已经用于控制蓝色指示灯，不能连接其他东西。如果你非要用它连接其他设备，它只能用于输出舵量pwm，控制舵机或电调。指示灯功能会在解锁后自动失效，GPIO8开始输出信号， 因为那时候飞机已经上天了，所以你也不会看指示灯。不过别忘了它是有上拉电阻的，所以最好不要用它输出高频的Dshot数字协议电调信号。
三、如果你需要动力电池电压检测电路，那么你需要一个有ADC功能的管脚，在开发板上。这5个管脚有adc功能，去除掉刚刚说的GPIO2，剩下4个你随便选一个，接入到双电阻分压电路的分压节点。该管脚用于测量电压后，不能用于其他任何信号的通信。
以上就是所有接线规则了，你可以根据这些接线规则，将开发板和射频模块连接起来。连接好两个模块的7信号和2根供电线后，现在13个GPIO焊盘还剩6个，它们可以被配置为两个UART接口，I2C接口，高/低电平输出，舵量pwm输出，全占空比pwm输出或者one shot/Dshot电调协议输出。（主串口用于和飞控通信，副串口用于连接其他设备，比如GPS。i2c接口可以用于连接气压传感器，如果你把他刷成高频头，也可以连接使用i2c接口的ssd1306 OLED屏幕用来显示发射机信息）。
[图片]
确定好所有接线定义和顺序后，你此时就得到了一个完整的原理图了。接下来，你需要把这个接线定义写入接收机，先给接收机供电，这里注意：可以通过5v焊盘接口供电，也可以通过USB供电，但是只能二选一！如果你用手摸esp 32芯片发现它很烫，八成就是它在发射WiFi信号，不烫反倒是有问题的。此时你打开手机WiFi列表，应该可以看到一个叫做ExpressLRS的WiFi名，连接它，输入密码expresslrs（就是WiFi名自己）。成功连接后，此时接收机的esp32芯片就变成了一个WiFi接入点，你需要在浏览器输入内网地址「10.0.0.1/hardware.html」，进入硬件配置网页，把你设计好的esp32管脚定义输入，输入好后，点击网页最底下的reboot保存。
现在你的接收机理论上就完工了，你可以再输入网址「10.0.0.1」进入常规参数配置界面，调整混控，电调输出协议，串口协议，对频信息等等常规参数。如果你的手搓接收机能够正常使用，那么恭喜你成功了。快去把接收机装在进飞机或车模，或者刷成高频头，装进遥控器里吧！

[图片]
[图片]
最后想说，如果你也针对该方案做了衍生版本的改进，或者你成功复刻了，请你一定要分享出来，无论是视频，图文，哪怕是在群里发张照也好，让我们一起促进开源事业的进步吧。
别走啊，还有呢：
一、B站介绍视频&手把手教你焊接：
[图片]
https://b23.tv/cTPp4Ne
二、立创开源硬件平台&PCB及原理图获取：
[图片]
https://oshwhub.com/jianchibuyongduo/board2
三、欢迎大家加入QQ讨论群：
902021691
[图片]
四、GITHUB没图片，如果文档有更新，飞书这里最全：
https://xcnmlw0olagh.feishu.cn/wiki/SgmEwbcjciPRihklynUcdX4qnAJ
鸣谢：感谢@长江几号，他是和我住的比较近的本地模友，在我研制接收机的过程中， 提供了高频头支持测试，因为我本没有高频头，就没法测试接收机，在此感谢！


