---
date: 2016-03-25T18:41:00+08:00
title: 构建Nios II系统
---

我最近找了一块载有[EP4CE6F17C8](https://www.altera.com/content/dam/altera-www/global/en_US/pdfs/literature/hb/cyclone-iv/cyiv-51001.pdf)的FPGA开发板，并试图在上面构建Nios II系统，做着玩玩。

{{< figure src="/media/nios-1.jpg" >}}

因为没有经验，所以走了很多弯路，记录在这里，供以后查阅。

<!--more-->

# 开发套件

在[Altera下载中心](http://dl.altera.com/?edition=lite)可以下载免费的Altera Prime Lite版软件。目前的最新版本为15.1。Lite版软件缺少一些高级功能，但是用于构建Nios II系统已经足够。

在下载中心至少需要下载以下组件：

* Quartus Prime (includes Nios II EDS)
* Cyclone IV device support

建议下载Update 2补丁包。

我用的是Cyclone IV系列芯片。如果使用Cyclone III或者更早版本的芯片，则必须使用旧版本的软件。这在Altera的下载中心也都有提供。

版本较新有一个缺点：市面上能找到的教程主要针对旧版本的软件，一些旧的开发工具，例如SOPC builder和Nios II IDE已被淘汰，取而代之的Qsys和Nios II EDS则缺少参考资料，需要自己摸索。

但是话说回来，如果已经知道某个技术在未来是一定要被淘汰的，那就没有必要再从旧技术入门。

# 开发流程

构建Nios II系统需要下面几个步骤：

1. 在Qsys中构建Nios II系统，包括处理器和一些外围组件，并生成系统的HDL文件。
2. 用Quartus Prime添加其他必要硬件，并为输入/输出端口分配引脚。将整个系统编译为硬件电路，并写入开发板。
3. 用Nios II EDS编写软件，编译并写入开发板。
4. 通过Flash烧写装置将所开发的系统写入配置芯片，这样当开发板掉电再启动时，能够自动运行所构建的系统。

下面分开详述。

## Qsys

启动Quartus Prime，创建一个新的工程。然后点击工具栏上的Qsys图标（或者Tools->Qsys菜单项）。

一般而言，需要构建的系统往往包含以下组件：

* 系统时钟
* 处理器核
* 系统ID
* 片上内存
* EPCS控制器
* SDRAM控制器
* JTAG UART
* 并行IO

这些组件可以在Qsys界面左侧的IP Catalog中找到。双击对应的项目即可将其添加到系统中。添加时，首先在配置窗口中设置相关参数，然后就可以在Qsys的主界面中看到该组件。

一般而言，对组件常进行如下操作：

* 重命名。右键单击组建名称，点击Rename。
* 修改参数。双击组件，在Parameter窗口中修改。
* 连线。在Qsys界面中，各组件左侧有一些浅色的线条，点击交叉点处的圆点可以将几个组件的端口互相连接。一般用于连接时钟、数据控制端等。
* 设置IRQ。在Qsys界面中，有些组件的右侧有通向Nios II处理器和的浅色线条，在圆点处点击可以生成IRQ编号。

### 系统时钟 (Clock Source)

Qsys启动后会创建一个缺省的系统时钟，因此无需手工添加。

* Clock frequency：所需的时钟频率，例如我填写的是100000000Hz。这表示整个系统将运行于100MHz的时钟频率。

### 处理器核 (Nios II Processor)

处理器核可以选择Nios II/e或者Nios II/f。前者占用资源少但性能较低，后者反之。

如果需要把Nios II/f固化到配置芯片中，需要向Altera公司购买许可。Nios II/e则无此限制。

### 系统ID (System ID Peripheral)

* 32 bit System ID：一个自定义的编号，来标识该系统。

系统ID的作用是防止实际的硬件版本和软件所需的硬件版本不一致造成的问题。

### 片上内存 (On-Chip Memory (RAM or ROM))

* Data width：数据宽度。根据芯片参数设置。
* Total memory：系统所使用的片上内存总量。可以根据需要设置，但不能超过芯片所能提供的总量。

对于我的EP4CE6芯片，Data width为16，Total memory一般设置为10KB~30KB。

可见，片上内存的资源是很稀少的，可能连标准C的运行库都放不下。为了能够运行C语言程序，一般需要在系统中配置SDRAM。在我的开发板上有一片32MB的SDRAM芯片，相对而言空间就比较充足了。

### EPCS控制器 (Legacy EPCS/EPCQx1 Flash Controller)

如果需要开发板断电重启后能够自动加载所设计的系统，就需要将它写入EPCS芯片。要实现此功能，需要在系统中加入EPCS控制器。

需要导出(Export)其external管线。

如果不需要这个功能，那么可以不添加EPCS控制器。但是，建议在进行软件设计之前确定系统中是否需要EPCS控制器，因为这可能会对后面的操作产生影响。

### SDRAM控制器 (SDRAM Controller)

为了弥补片上内存空间的不足，需要SDRAM控制器将SDRAM芯片来扩充内存空间。

其中的参数需要根据SDRAM芯片的参数来修改。我的开发板使用HY57V2562GTR芯片。

需要导出其wire管线。

### JTAG UART

这个组件可以通过JTAG下载器实现开发板和计算机之间的数据通信。例如C语言程序如果需要使用stdin, stdout等设备，可以通过JTAG UART，从计算机的键盘读取并显式在计算机的屏幕上。这对于一些调试工作来说非常有用。

### 并行IO (PIO (Parallel I/O))

这个组件可以用于控制一般用途的I/O。例如我的开发板上有4个LED灯。我可以建立一个PIO组件，其宽度设为4，方向设为输出。这样，我在C语言程序中只需向特定的地址写入数据，就可以控制4个LED的亮灭。

需要导出其external\_connection管线。

---

设置好上述各组件后，需要进行连线工作。通常需要连接的线路有：

* 所有组件的时钟(clk)端口，连接到系统时钟的输出。
* 所有存储控制器(片上内存、EPCS或SDRAM控制器)的描述为“Avalon Memory Mapped Master”的端口，连接到Nios II处理器核的instruction\_master端口。
* 所有含有描述为“Avalon Memory Mapped Master”端口的组件，将该端口连接到Nios II处理器核的data\_master端口。
* 所有重置端口。这部分连线可以通过System菜单中的Create Global Reset Network来完成。
* 所有IRQ编号。通过点击IRQ列中的圆点来分配IRQ编号。

接下来为各个组件分配地址，这可以通过System菜单中的Assign Base Addresses来完成。

在Nios II处理器核的参数设置中，将Reset Vector设置为epcs，Exception Vector设置为on chip memory或者sdram。

如果你没有添加EPCS控制器，那么你的软件一定是通过连接的计算机写入芯片内存的，计算机会将处理器跳转到起始地址，那样的话，不设置Reset Vector也无所谓。

如果一切设置正确，则Qsys界面下方的Messages窗格中不会有任何错误，一般也没有警告。这时候，可以将系统保存为.qsys文件，然后点击Generate HDL...按钮，再点击Generate，生成HDL文件。生成完毕后就可以退出Qsys了。

## Quartus Prime硬件配置

退出Qsys后，有一个对话框会提示你，需要将一个qip文件添加到工程中。可以用Project菜单中的Add/Remove Files in Project来把它添加进工程。

接下来，按照一般教程的做法，这时应该创建一个原理图文件(.bdf)作为顶层实体(top-level entity)，然后添加刚才创建的系统模块。但是，由于未知的原因，这个版本的Quartus Prime无法在原理图中添加这个模块。

可行的办法是使用Verilog来描述顶层实体。创建一个Verilog文件(.v)，保存后将其设为顶层实体。并在其中输入

```
module toplevel(
// 输入输出端口
);

// 相关组件

endmodule
```

在这个Verilog文件中，需要配置2个组件。

### Nios II系统

打开之前保存的.qsys文件，选择Generate菜单中的Show Instantiation Template项目，可以看到一个Verilog模板代码。我的是

```
    kernel u0 (
        .clk_clk                            (<connected-to-clk_clk>),                            //                         clk.clk
        .epcs_controller_external_dclk      (<connected-to-epcs_controller_external_dclk>),      //    epcs_controller_external.dclk
        .epcs_controller_external_sce       (<connected-to-epcs_controller_external_sce>),       //                            .sce
        .epcs_controller_external_sdo       (<connected-to-epcs_controller_external_sdo>),       //                            .sdo
        .epcs_controller_external_data0     (<connected-to-epcs_controller_external_data0>),     //                            .data0
        .led_pio_external_connection_export (<connected-to-led_pio_external_connection_export>), // led_pio_external_connection.export
        .reset_reset_n                      (<connected-to-reset_reset_n>),                      //                       reset.reset_n
        .sdram_controller_wire_addr         (<connected-to-sdram_controller_wire_addr>),         //       sdram_controller_wire.addr
        .sdram_controller_wire_ba           (<connected-to-sdram_controller_wire_ba>),           //                            .ba
        .sdram_controller_wire_cas_n        (<connected-to-sdram_controller_wire_cas_n>),        //                            .cas_n
        .sdram_controller_wire_cke          (<connected-to-sdram_controller_wire_cke>),          //                            .cke
        .sdram_controller_wire_cs_n         (<connected-to-sdram_controller_wire_cs_n>),         //                            .cs_n
        .sdram_controller_wire_dq           (<connected-to-sdram_controller_wire_dq>),           //                            .dq
        .sdram_controller_wire_dqm          (<connected-to-sdram_controller_wire_dqm>),          //                            .dqm
        .sdram_controller_wire_ras_n        (<connected-to-sdram_controller_wire_ras_n>),        //                            .ras_n
        .sdram_controller_wire_we_n         (<connected-to-sdram_controller_wire_we_n>)          //                            .we_n
    );

```

可以将其复制后粘贴到顶层实体的Verilog文件中的“相关组件”位置。

### 锁相环

锁相环能产生不同频率的时钟，例如我可以用它生成100MHz的时钟，使得处理器可以工作在更高的频率。锁相环的另一个用处是生成相位不同的时钟信号，这在连接SDRAM控制器时将会起到作用。

在Quartus Prime界面的IP Catalog窗格里面找到ALTPLL组件，双击。选择一个合适的文件名，例如`pll.v`，IP variation选择Verilog，单击OK。

这时会弹出一个MegaWizard Plug-in Manager窗口。在这里可以进行对锁相环的配置。

首先，第一页中的What is the frequency of the inclk0 input?中，填写开发板的晶振所提供的频率，我填写的是50.000MHz。

然后，在下一页中取消Create an 'areset' input to asynchronously reset the PLL和Create 'locked' output的勾选。

一路Next直到进入c0的配置界面，在这里，将Clock multiplication factor设置为2，即2倍频，可以生成100MHz的时钟信号。这是因为我之前在Qsys中把系统时钟设置成了100MHz。

然后Next，进入c1的配置界面。如果需要使用SDRAM控制器，那么此处需要将c1启用，Clock multiplication factor同样设置为2，但是Clock phase shift需要设置为-73 deg。这个数字我是从教程上看到的，至于为什么，我还没有弄清楚。这样设置，对于我的开发板是可以工作的，对于其他的硬件，可能需要调整。

设置完以上这些，就可以直接按Finish完成锁相环的配置了。当然，如果用不到SDRAM控制器，并且希望整个系统就工作在默认的时钟频率下，那么也可以不加入锁相环组件。

锁相环配置完毕后，回到顶层实体的Verilog文件中，在“相关组件”位置增加如下代码

```
wire pll_c0;	// 这句放在Nios II模块之前

pll pll0(
    .inclk  (CLOCK),	// 晶振时钟
    .c0     (pll_c0),	// c0时钟 -- 处理器
    .c1     (S_CLK)	// c1时钟 -- SDRAM
);
```

修改刚才粘贴的模板，例如我是这样修改的

```
kernel nios2(
    .clk_clk                            (pll_c0),    // 系统时钟
    .reset_reset_n                      (RESET),     // 复位按钮
    .sdram_controller_wire_addr         (S_A),       // SDRAM 地址总线
    .sdram_controller_wire_ba           (S_BA),      // SDRAM BANK 选择地址
    .sdram_controller_wire_cas_n        (S_NCAS),    // SDRAM 列地址选通
    .sdram_controller_wire_cke          (S_CKE),     // SDRAM  时钟使能
    .sdram_controller_wire_cs_n         (S_NCS),     // SDRAM  片选
    .sdram_controller_wire_dq           (S_DB),      // SDRAM 数据总线
    .sdram_controller_wire_dqm          (S_DQM),     // SDRAM 数据输入/输出掩码
    .sdram_controller_wire_ras_n        (S_NRAS),    // SDRAM 行地址选通
    .sdram_controller_wire_we_n         (S_NWE),     // SDRAM 写使能
    .led_pio_external_connection_export	(LED),       // LED 控制端
    .epcs_controller_external_dclk      (DCLK),
    .epcs_controller_external_sce       (SCE),
    .epcs_controller_external_sdo       (SDO),
    .epcs_controller_external_data0     (DATA0)
);
```

括号左侧的名字是模板提供的，根据Qsys设计时所设置的各组件的名称不同，可能会与此处的名字有所出入。所以不宜直接复制粘贴这里的代码。

括号内的名字，模板中是一些由尖括号括起的名字，意思是需要自己修改。我改成了一些特殊的大写字母名字。这些名字是我的开发板的参考手册提供的，使用这些名字可以便于引脚分配工作。

注意，锁相环的`c0`端口和Nios II的`clk_clk`端口都连接到了`pll_c0`连线，即表示这两个端口是相连的。

设置好Nios II和锁相环以后，再填写总电路的“输入输出端口”部分。我是这样填写的

```
module toplevel
(
	input CLOCK, RESET,
	output S_CLK,
	output [12:0] S_A,
	output [1:0] S_BA,
	output S_NCAS, S_CKE, S_NCS,
	inout [15:0] S_DB,
	output [1:0] S_DQM,
	output S_NRAS, S_NWE,
	output [3:0] LED,
	input DATA0,
	output DCLK, SCE, SDO
);
```

特别要注意的是input, output和inout分别制定输入、输出、双向端口，不能搞错。之前我误将`S_DB`设置成了output导致SDRAM无法正常工作。这个错误比较隐蔽，花了我很多时间才找出来。

编写好Verilog文件后，保存。然后点击Pin Planner图标，或者Assignments菜单的Pin Planner选项。在Pin Planner为各个端口配置硬件引脚。我的开发板提供了一个tcl脚本，可以自动配置引脚。可以通过Tools菜单的Tcl Scripts来运行这个脚本。我的端口命名就是依照这个Tcl脚本来的。

然后，选择Assignments菜单的Device...项目，点击Device and Pin Options，一般需要做如下的配置：

* Unused Pins类别中，设置Reserve all unused pins as input tri-stated。这一步的效果，根据一些教程，是可以防止写入电路以后，某些输出被配置为有效电平。例如蜂鸣器会响起，等等。但我不知道其中的原因。
* Dual-Purpose Pins类别中，将所有引脚设置为Use as a regular I/O。这个也是一些教程上写的，我也不知道原因。

全部设置完毕以后，就可以按工具栏上的Start Compilation按钮，编译整个硬件电路。如果一切顺利，就会在output\_files文件夹中生成一个.sof文件。

连接好JTAG下载器和开发板电源，打开Programmer（Tools菜单中的Programmer项目），设置好硬件，选择刚才生成的.sof文件，就可以把生成的电路写入FPGA芯片。

## 编写软件

在Quartus Prime界面中选择Tools菜单中的Nios II Software Build Tools for Eclipse，启动一个定制的Eclipse集成开发环境。工作目录设置为Quartus Prime工程所在的目录。

Eclipse启动后，可以通过模板创建一个新的工程。选择File/New/Nios II Application and BSP from Template。模板可以选择Hello World。如果没有设置SDRAM芯片，则内存可能不足以容纳C运行库，这时也可以选择Hello World Small模板。

SOPC Information File name选择由Qsys生成的.sopcinfo文件。输入一个合适的Project name，点击Finish就生成了一个Hello World工程和相应的BSP工程。

BSP指Board Support Package，它提供一些最基础的硬件抽象。这个工程中的所有文件都是自动生成的，用户无需操心。

打开hello\_world工程的hello\_world.c文件，可以看到一个简单的C语言程序，它调用了C标准库中的`printf`函数。（如果是Hello World Small模板，则没有C标准库，调用的是BSP提供的`alt_printf`函数。）

点击工具栏上的Run按钮，或者Ctrl+F11快捷键运行程序。首次运行时，会显示Run As对话框，这时选择Nios II Hardware。如果出现了Run Configurations对话框，在Project选项卡中选择hello\_world工程，并在Target Connection选项卡中查看硬件是否连接正常。如果正常，那么按Run按钮。就可以运行程序了。

Eclipse集成开发环境会自动地编译相关的文件，生成一个.elf程序文件，然后把它下载到开发板的内存中，并运行这个程序。如果一切正常，则可以看到Eclipse界面下方出现了Nios II Console，并且打印出了`printf`所打印的字符串。

从效果来看似乎和运行本地程序没有什么区别，但实际上这个程序是被开发板上的处理器执行的，通过JTAG UART将结果显示在计算机的屏幕上，而不是由计算机执行的。如果想更加明确这一点，那么可以尝试操作PIO输出端口，向其中写入一些数据。例如，我在构建硬件的时候将一个PIO连接到了LED的引脚上，那么，我向其中写入一些数据，就可以控制LED的亮灭。

## 烧写Flash

在Nios II Software Build Tools窗口(即Eclipse集成开发环境)中选择Nios II菜单中的Flash Programmer项目，打开Flash烧写器。

在Nios II Flash Programmer界面中选择File/New，然后选择集成开发环境自动生成的.bsp文件，单击OK做好硬件设置。

接着，在Files for flash conversion列表的右侧点击Add...，分别加入Quartus Prime生成的.sof文件（硬件配置）和Eclipse生成的.elf文件（程序文件）。保持默认设置，点击Start按钮开始烧写。

需要注意的几个问题：

* 确保硬件正确地连接。
* 必须先用Quartus Prime的Programmer把.sof硬件写入FPGA芯片。因为Flash Programmer需要硬件中的EPCS Controller支持，所以要先把硬件写入FPGA芯片。
* 烧写过程中有可能会出现硬件烧写成功而软件烧写失败的情况。一般的解决方法如下：在Files for flash conversion列表中先只添加.sof文件，按Start按钮烧写，这时可以烧写成功。成功以后，按开发板上的复位按钮或者断开电源再通电，使开发板复位。然后再添加.elf文件，再次烧写就可以成功。至于为什么这么做可以解决问题，我就不清楚了。

我的EPCS配置芯片型号是[M25P16](https://www.micron.com/~/media/Documents/Products/Data%20Sheet/NOR%20Flash/Serial%20NOR/M25P/M25P16.pdf)，擦写寿命约为10万次。因为EPCS配置芯片的寿命是有限的，而且烧写的过程比较费时，所以一般在调试程序功能的时候可以直接通过Eclipse界面的Run按钮来运行程序，等到软件已经成型之后，在进行Flash的烧写。
