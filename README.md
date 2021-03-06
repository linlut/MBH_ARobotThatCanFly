# MBH_ARobotThatCanFly
<br>
<br>这是一个开源的四轴飞行器控制项目，使用树莓派3B型做四轴飞行器的控制器，通过GPIO引脚读取摇控器的输入PWM信号，计算MPU6050陀螺仪的读数，然后对欧拉角和旋转角速度做平衡修正，最后通过GPIO引脚并对四轴上的4个电调输出。
<br>
<br>使用方法：
<br>
<br>1、下载源代码，并拷贝到你的树莓派上，用网线连接到莓派上，并通过PC机ssh远程到树莓派上执行这些命令。
<br>
<br>2、修改include/driver.h文件中摇控器接收机和电调的GPIO引脚编号，编号是WiringPi的编号。
<br>
<br>3、修改include/dirver.h文件中摇控器PWM信号零点时长（不是必需的，你可以通过--ctl参数查看当前摇控器的读数）。
<br>
<br>4、执行make命令，编译源代码。如果编译成功话会生成一个可执行文件release/bin/quadcopter。
<br>
<br>5、执行su -c 'release/bin/quadcopter --test 15 100 3000'，测试你的GPIO和电机，你的显示器会显示如下内容：
<br>	--test [WringPi引脚编号] [电机运转速度0~1000] [测试时长0~30000毫秒]
<br>	
<br>6、执行su -c 'release/bin/quadcopter --ctl'，显示你的摇控器3个通道输入PWM信号时长。
<br>	[BF: 1600 LR: 1600 PW: 1100]
<br>	BF:表示“前/后”，也就是摇控器上控制“前/后”通道的数值，你应该尽量让它接近1600，或者修改include/driver.h中的CTL_BF默认数值让它接近你的实际读数。
<br>	LR:表示“左/右”，也就是摇控器上控制“左/右”通道的数值，你应该尽量让它接近1600，或者修改include/driver.h中的CTL_LR默认数值让它接近你的实际读数。
<br>	PW:表示“油门”，也就是摇控器上控制“上下”通道的数值，你应该尽量让它接近1100，因为在开始时油门通常都会被调节到最低，或者修改include/driver.h中的CTL_PW默认数值让它接近你的实际读数。
<br>	
<br>7、执行su -c 'release/bin/quadcopter --fly'开始飞行模式。这时你会看到这样的输出内容：
<br>	[xyz:  +6.800 +12.000  +0.000][gxyz:  +0.000  +0.000  +0.000][s0:   0   0   0   0][c_xy: +6.80 +12.00]
<br>	从左到右分别为：[xyz轴欧拉角] [xyz轴角速度] [当前电机转数] [pid参数]。最后一列的值显示的是当前欧拉角的PID参数，而键盘上的4个按键q、w、e、r（注意按键的大小写）可以用于切换显示不同的参数：
<br>	q : [pid: +9.40 +1.90 +8.00]	——欧拉角PID参数
<br>	w : [pid_v: +9.30 +5.40 +8.30]	——角速度PID参数
<br>	e : [pid_z: +3.60 +0.80 +2.80]	——抑制自旋PID参数
<br>	r : [c_xy: +6.80 +12.00]		——陀螺仪中心点校准补偿
<br>	处于不同的显示参数状态下，按下小键盘上的数字按钮可以对这些参数做相应的调整，例如：
<br>	在PID参数调整下，按“7”则对Kp加0.1；按“4”则对Kp减0.1；按“8”则对Ki加0.1；按“5”则对Ki减0.1；按“9”则对Kd加0.1；按“6”则对Kd减0.1。
<br>	在PID_V参数调整下，按“7”则对Kp_v加0.1；按“4”则对Kp_v减0.1；按“8”则对Ki_v加0.1；按“5”则对Ki_v减0.1。按“9”则对Kd_v加0.1；按“6”则对Kd_v减0.1；
<br>	在PID_Z参数调整下，按“7”则对Kp_z加0.1；按“4”则对Kp_z减0.1；按“8”则对Ki_z加0.1；按“5”则对Ki_z减0.1。按“9”则对Kd_z加0.1；按“6”则对Kd_z减0.1；
<br>	在C_XY参数调整下，按“7”则对Cx加0.1；按“4”则对Cx减0.1；按“8”则对Cy加0.1；按“5”则对Cy减0.1。
<br>	按“+”键则对所有电机的转数加10，按“-”键则对有电机的转数减10，按“0”电机停转（在电机转数为0时，不对电机做任何的平衡处理）。
<br>
<br>8、在调整完你满意的参数之后按“S”保存当前参数，按“L”载入上次保存的参数（注意按键的大小写）。
<br>
<br>9、如果拔掉树莓派上的网线（如果你插入了网线的话）。
<br>
<br>9、缓慢推动摇控器的油门起飞，四轴飞行器的螺旋桨逐渐加速，起飞。
<br>