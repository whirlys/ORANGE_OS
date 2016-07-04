## int 10 中断

参考 [这篇int 10中断的功能](http://www.cnblogs.com/magic-cube/archive/2011/10/19/2217676.html)

<table style="width: 100%;" bgcolor="#993333" border="0" cellpadding="0" cellspacing="1">
<tbody>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center"><span style="color: #cc0033;" color="#cc0033">AH</span></div>
</td>
<td class="pt9-black" width="160">
<div align="center"><span style="color: #cc0033;" color="#cc0033">功 能</span></div>
</td>
<td class="pt9-black" width="309">
<div align="center"><span style="color: #cc0033;" color="#cc0033">调用参数</span></div>
</td>
<td class="pt9-black" width="207">
<div align="center"><span style="color: #cc0033;" color="#cc0033">返回参数 / 注释</span></div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">1</div>
</td>
<td class="pt9-black" width="160">
<div align="left">　置光标类型</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　（CH）0―3 = 光标开始行<br> 　　（CL）0―3 = 光标结束行</div>
</td>
<td class="pt9-black" width="207">
<div align="left"></div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">2</div>
</td>
<td class="pt9-black" width="160">
<div align="left">　置光标位置</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　BH = 页号 <br> 　　DH = 行<br> 　　DL = 列 　</div>
</td>
<td class="pt9-black" width="207">
<div align="left"></div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">3</div>
</td>
<td class="pt9-black" width="160">
<div align="left">　读光标位置</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　BH = 页号</div>
</td>
<td class="pt9-black" width="207">
<div align="left">　CH = 光标开始行<br> 　CL = 光标结束行<br> 　DH = 行<br> 　DL = 列</div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">4</div>
</td>
<td class="pt9-black" width="160">
<div align="left">&nbsp;&nbsp; 读光笔位置</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　</div>
</td>
<td class="pt9-black" width="207">
<div align="left"><span style="color: #8080c0;" color="#8080c0">AH=0 光笔未触发<br>
            =1 光笔触发<br>
            CH=象素行<br>
            BX=象素列<br>
            DH=字符行<br>
            DL=字符列</span></div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" height="26" width="65">
<div align="center">5</div>
</td>
<td class="pt9-black" height="26" width="160">
<div align="left">　显示页</div>
</td>
<td class="pt9-black" height="26" width="309">
<div align="left">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; AL = 显示页号</div>
</td>
<td class="pt9-black" height="26" width="207">
<div align="left"></div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">6</div>
</td>
<td class="pt9-black" width="160">
<div align="left">　屏幕初始化或上卷</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　AL = 上卷行数 <br> 　　AL =0全屏幕为空白 <br> 　　BH = 卷入行属性<br> 　　CH = 左上角行号 <br> 　　CL = 左上角列号 <br> 　　DH = 右下角行号 <br> 　　DL = 右下角列号</div>
</td>
<td class="pt9-black" width="207">
<div align="left"></div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">7</div>
</td>
<td class="pt9-black" width="160">
<div align="left">　屏幕初始化或下卷</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　AL = 下卷行数<br> 　　AL = 0全屏幕为空白 <br> 　　BH = 卷入行属性<br> 　　CH = 左上角行号 <br> 　　CL = 左上角列号 <br> 　　DH = 右下角行号 <br> 　　DL = 右下角列号</div>
</td>
<td class="pt9-black" width="207">
<div align="left"></div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">8</div>
</td>
<td class="pt9-black" width="160">
<div align="left">　读光标位置的属性和字符</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　BH = 显示页</div>
</td>
<td class="pt9-black" width="207">
<div align="left">　AH = 属性<br> 　AL = 字符</div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">9</div>
</td>
<td class="pt9-black" width="160">
<div align="left">　在光标位置显示字符及其属性</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　BH = 显示页<br> 　　AL = 字符<br> 　　BL = 属性<br> 　　CX = 字符重复次数</div>
</td>
<td class="pt9-black" width="207">
<div align="left"></div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">A</div>
</td>
<td class="pt9-black" width="160">
<div align="left">　在光标位置只显示字符</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　BH = 显示页<br> 　　AL = 字符 <br> 　　CX = 字符重复次数</div>
</td>
<td class="pt9-black" width="207">
<div align="left"></div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">E</div>
</td>
<td class="pt9-black" width="160">
<div align="left">　显示字符(光标前移)</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　AL = 字符<br> 　　BL = 前景色</div>
</td>
<td class="pt9-black" width="207">
<div align="left">　光标跟随字符移动</div>
</td>
</tr>
<tr bgcolor="#ffffff">
<td class="pt9-black" width="65">
<div align="center">13</div>
</td>
<td class="pt9-black" width="160">
<div align="left">　显示字符串</div>
</td>
<td class="pt9-black" width="309">
<div align="left">　　ES:BP = 串地址 <br> 　　CX = 串长度 <br> 　　DH， DL = 起始行列 <br> 　　BH = 页号<br> 　　AL = 0，BL = 属性 <br> 　　串：Char，char，……，char<br> 　　AL = 1，BL = 属性 <br> 　　串：Char，char，……，char <br> 　　AL = 2 <br> 　　串：Char，attr，……，char，attr <br> 　　AL = 3 <br> 　　串：Char，attr，……，char，attr</div>
</td>
<td class="pt9-black" width="207">
<div align="left">
<p></p>
<p><br> <br> <br> <br> 　光标返回起始位置<br> <br> 　光标跟随移动<br> <br> <br> 　光标返回起始位置<br> <br> <br> 　光标跟随串移动</p>
</div>
</td>
</tr>
</tbody>
</table>
<hr>
<h3>AH=00H</h3>
<p>AH=00/INT 10H 是用来设定显示模式的服务程序，AL 寄存器表示欲设定的模式：</p>
<p></p>
<table class="gen" align="center" border="2">
<tbody>
<tr align="center">
<td class="cellgen">AL</td>
<td class="cellgen">文字/图形</td>
<td class="cellgen">分辨率</td>
<td class="cellgen">颜色</td>
</tr>
<tr>
<td class="cellgen">00</td>
<td class="cellgen">文字</td>
<td class="cellgen">40*25</td>
<td class="cellgen">2</td>
</tr>
<tr>
<td class="cellgen">01</td>
<td class="cellgen">文字</td>
<td class="cellgen">40*25</td>
<td class="cellgen">16</td>
</tr>
<tr>
<td class="cellgen">02</td>
<td class="cellgen">文字</td>
<td class="cellgen">80*25</td>
<td class="cellgen">2</td>
</tr>
<tr>
<td class="cellgen">03</td>
<td class="cellgen">文字</td>
<td class="cellgen">80*25</td>
<td class="cellgen">16</td>
</tr>
<tr>
<td class="cellgen">04</td>
<td class="cellgen">图形</td>
<td class="cellgen">320*200</td>
<td class="cellgen">2</td>
</tr>
<tr>
<td class="cellgen">05</td>
<td class="cellgen">图形</td>
<td class="cellgen">320*200</td>
<td class="cellgen">4</td>
</tr>
<tr>
<td class="cellgen">06</td>
<td class="cellgen">图形</td>
<td class="cellgen">640*200</td>
<td class="cellgen">2</td>
</tr>
</tbody>
</table>

<h3>AH=01H</h3>
<p>您可以把光标想成一个小的矩形，平时这个矩形扁平位于某字底部，但藉由此功能可以改变其大小与位置。光标起始处与终止处分别由 CL 与 CH 的 0 到 4 位表示，参考下图：</p>
<center></center>
<p>而 CH 的第 7 位必须是 0，第 5、6 位表示光标属性：</p>
<pre class="code">位 6&nbsp;&nbsp;&nbsp;&nbsp;   位 5&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   属性
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   正常
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   0&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   隐形
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   0
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   闪烁缓慢</pre>
<h3><a name="02"><span style="color: #000000;" color="#000000">AH=02H</span></a></h3>
<p>此功能是设定光标位置，位置用 DH、DL 表示，DH 表示列号，DL 表示行号。由左至右称之为『列』，屏幕最上面一列为第零列，紧靠第零列的下一列称为第一列……；由上而下称之为『行』，屏幕最左边一行称之为第零行，紧靠 第零行右边的一行为第一行。故最左边，最上面的位置为 DH=0 且 DL=0；最左边第二列，DH=1，DL=0。如果是文字模式时，BH 为欲改变光标位置的显示页，如果是图形模式，BH 要设为 0。</p>
<p>以行列来说明 DH、DL 之意义，小木偶常常搞混，底下以座标方式解释。在文字模式下，字符的位置类似数学直角座标系的座标，但是 Y 轴方向相反，Y 轴是以屏幕最上面为零，越下面越大，直到 24 为止，存于 DH 内。X 轴和直角座标系相同，越右边越大，存于 DL 内，其最大值视显示模式而变。</p>
<h3>AH=03H</h3>
<p>AH=03H/INT 10H 这个中断服务程序返回时，会在 DX 里面有光标的行列位置，CX 内有光标的大小，DX、CX 之数值所代表的意义和 AH=02H/INT 10H、AH=01H/INT 10H 相同。</p>
<h3>AH=04H</h3>
<p>此功能是探测光笔之位置，似乎只有 CGA 卡有接上光笔？？</p>
<h3>AH=05H</h3>
<p>这个功能是把指定的显示页显示于屏幕上，欲显示的显示页于 AL 寄存器中指定。此功能只能在文字模式下才能发生作用。</p>
<h3>AH=06H/07H</h3>
<p>这个服务程序的功用是把某一个设定好的矩形区域内的文字向上或向下移动。先说明向上移动，即调用 AH=06H/INT 10H。当此服务程序工作时，会使矩形区域的文字向上移动，而矩形区域底端移进空格列。向上移动的列数存入 AL 中 ( 如果 AL 为零，表示使矩形区域的所有列均向上移 )，底端移入空格列的属性存于 BH，矩形区域是藉由 CX、DX 来设定左上角与右上角的座标，左上角的行与列分别由 CL、CH 设定，右下角的行与列由 DL、DH 设定。</p>
<p>AH=07H/INT 10H 和 AH=06H/INT 10H 相似，只是卷动方像不同而已。</p>
<h3>AH=08H</h3>
<p>这个服务程序是用来取得光标所在位置的字符及属性，调用前，BH 表示欲读取之显示页，返回时，AL 为该位置之 ASCII 字符，AH 为其属性。有关属性的说明，请参考<a>注一</a>。</p>
<h3>AH=09H</h3>
<p>这个功能是在光标位置显示字符，所要显示字符的 ASCII 码存于 AL 寄存器，字符重复次数存于 CX 寄存器，显示页存于 BH 寄存器，属性存于 BL 寄存器，其属性使用与 AH=08/INT 10H 一样。</p>
<h3>AH=0AH</h3>
<p>这个功能和 AH=09H/INT 10H 一样，差别在 AH=0AH 只能写入一个字符，而且不能改变字符属性。</p>
<h3>AH=0BH</h3>
<p>这个服务程序是选择调色盘。显示模式 5 是 320*200 的图形模式，最多可以显示 4 种颜色，这四种颜色的意思是最多可以『同时』显示一种背景色及三种前景色，而这三种前景色有两种方式可供选择，因此事实上，在显示模式 5 有两种调色盘可供选择。就好像您去买 12 种颜色的水彩，但可在调色盘上以任意比例搭配出许多种颜色。</p>
<p>调色盘 0 的三色是绿、红、黄；调色盘 1 的三色是青、紫红、白。背景色有 16 六种可供选择，这 16 种就是<a>注一</a>的 16 色。调用此中断时，先决定要设定背景色抑或调色盘，</p>
<ul>
<li>要设定背景色时，则使 BH 为 0，再使 BL 之数值为 0 到 0fh 之间表示<a>注一</a>的 16 色之一。</li>
<li>要设定调色盘时，则使 BH 为 1。再设定 BL 为零或一表示选择那一种调色盘。</li>
</ul>
<p>背景色只有在前景色为 0 时才会显现出来。</p>
<h3>AH=0CH</h3>
<p>AH=0Ch/INT 10H 是在绘图模式中显示一点 ( 也就是写入点像，write graphics pixel )，而 AH=0DH/INT 10H 则是读取点像 ( read graphics pixel )。</p>
<p>写入时，要写入位置 X 座标存于 CX 寄存器，Y 座标存于 DX 寄存器，颜色存于 AL 寄存器。和文字模式相同，萤光幕上的 Y 座标是最上面一列为零，越下面越大，X 座标则和数学的定义相同。CX、DX、AL 值之范围与显示模式有关：</p>
<p></p>
<table style="width: 60%;" class="gen" align="center" border="2" cellpadding="0" cellspacing="0">
<tbody>
<tr>
<td class="field" align="center" width="20%">显示模式</td>
<td class="field" align="center" width="30%">X 座标</td>
<td class="field" align="center" width="30%">Y 座标</td>
<td class="field" align="center">颜色</td>
</tr>
<tr align="center">
<td class="cellgen">4</td>
<td class="cellgen">0～319</td>
<td class="cellgen">0～199</td>
<td class="cellgen">0、1</td>
</tr>
<tr align="center">
<td class="cellgen">5</td>
<td class="cellgen">0～319</td>
<td class="cellgen">0～199</td>
<td class="cellgen">0～3</td>
</tr>
<tr align="center">
<td class="cellgen">6</td>
<td class="cellgen">0～639</td>
<td class="cellgen">0～199</td>
<td class="cellgen">0、1</td>
</tr>
</tbody>
</table>
<p>AH=0DH/INT 10H 则是读取某一位置之点像，您必须指定 CX、DX，而 INT 10H 会传回该位置点像之颜色。</p>
<h3>AH=0EH</h3>
<p>这个子程序是使显示器像打字机一样的显示字符来，在前面用 AH=09H/INT 10H 和 AH=0AH/INT 10H 都可以在萤光幕上显示字符，但是这两奘方式显示字符之后，光标位置并不移动，而 AH=0EH/INT 10H 则会使光标位置移动，每显示一个字符，光标会往右移一格，假如已经到最右边了，则光标会移到最左边并移到下一列，假如已经移到最下面一列的最右边，则屏幕 会向上卷动。</p>
<p>AL 寄存器存要显示的字符，BH 为目前的显示页，如果是在图形模式，则 BH 须设为 0，假如是在图形模式下，也可以设定 BL 来表示文字的颜色，文字模式下的 BL 则无功能。</p>
<h3>AH=0FH</h3>
<p>这个服务程序是得到目前的显示模式，调用前只需使 AH 设为 0fh，当由 INT 10H 返回时，显示模式存于 AL 寄存器 ( 参考 AH=00H/INT 10H 的显示模式表 )，目前的显示页存于 BH 寄存器，总字符行数存于 AH 寄存器。</p>
<hr>
<p><a name="note1"><span class="note"><span style="color: #000000;" color="#000000">注一：</span></span></a> 所谓属性是指字符的颜色、背景颜色、是否闪烁、有没有底线等性质。在彩色显示卡 ( CGA/EGA/VGA 等 ) 的文字模式中，颜色是用 4 个位表示，故可以表现出 16 种颜色，如下表：</p>
<p></p>
<table style="width: 80%;" class="gen" align="center" border="2" cellpadding="0" cellspacing="0">
<tbody>
<tr align="center">
<td class="field" width="15%">二进制数</td>
<td class="field" width="15%">颜色</td>
<td class="field" width="20%">例子</td>
<td class="field" width="15%">二进制数</td>
<td class="field" width="15%">颜色</td>
<td class="field" width="20%">例子</td>
</tr>
<tr align="center">
<td class="cellgen">0000</td>
<td class="cellgen">黑色</td>
<td class="cellgen" style="color: black;">black</td>
<td class="cellgen">1000</td>
<td class="cellgen">灰色</td>
<td class="cellgen" style="color: #808080;">gray</td>
</tr>
<tr align="center">
<td class="cellgen">0001</td>
<td class="cellgen">蓝色</td>
<td class="cellgen" style="color: #000080;">blue</td>
<td class="cellgen">1001</td>
<td class="cellgen">淡蓝色</td>
<td class="cellgen" style="color: #0000ff;">light blue</td>
</tr>
<tr align="center">
<td class="cellgen">0010</td>
<td class="cellgen">绿色</td>
<td class="cellgen" style="color: #008000;">green</td>
<td class="cellgen">1010</td>
<td class="cellgen">淡绿色</td>
<td class="cellgen" style="color: #00ff00;">light green</td>
</tr>
<tr align="center">
<td class="cellgen">0011</td>
<td class="cellgen">青色</td>
<td class="cellgen" style="color: #008080;">cyan</td>
<td class="cellgen">1000</td>
<td class="cellgen">淡青色</td>
<td class="cellgen" style="color: #00ffff;">light cyan</td>
</tr>
<tr align="center">
<td class="cellgen">0100</td>
<td class="cellgen">红色</td>
<td class="cellgen" style="color: #800000;">red</td>
<td class="cellgen">1100</td>
<td class="cellgen">淡红色</td>
<td class="cellgen" style="color: #ff0000;">light red</td>
</tr>
<tr align="center">
<td class="cellgen">0101</td>
<td class="cellgen">紫红色</td>
<td class="cellgen" style="color: #800080;">magenta</td>
<td class="cellgen">1101</td>
<td class="cellgen">淡紫红色</td>
<td class="cellgen" style="color: #ff00ff;">light magenta</td>
</tr>
<tr align="center">
<td class="cellgen">0110</td>
<td class="cellgen">棕色</td>
<td class="cellgen" style="color: #808000;">brown</td>
<td class="cellgen">1110</td>
<td class="cellgen">黄色</td>
<td class="cellgen" style="color: #ffff00;">yellow</td>
</tr>
<tr align="center">
<td class="cellgen">0111</td>
<td class="cellgen">银色</td>
<td class="cellgen" style="color: #c0c0c0;">light gray</td>
<td class="cellgen">1111</td>
<td class="cellgen">白色</td>
<td class="cellgen" style="color: #ffffff;">white</td>
</tr>
</tbody>
</table>
<p>在彩色显示器里，如 CGA、EGA、VGA 等，常用一个字节 ( 8 个位 ) 来表示文字颜色和背景颜色，通常以第 0～3 位表示文字本身颜色；第 4～6 位表示背景颜色，背景颜色只有上表左栏的 8 种而已；第 7 个位，表示是否闪烁，0 表示不闪烁，1 表示闪烁。</p>
<p>但是在单色显示器里，如 MDA 和 Hercules 卡中，这些颜色表并无意义，所以属性解释方式不同，请看下表：</p>
<p></p>
<table style="width: 50%;" class="gen" align="center" border="2" cellpadding="0" cellspacing="0">
<tbody>
<tr align="center">
<td class="field" width="30%">数值</td>
<td class="field">属性</td>
</tr>
<tr>
<td class="cellgen" align="center">00H</td>
<td class="cellgen">空格，不显示任何数据</td>
</tr>
<tr>
<td class="cellgen" align="center">77H</td>
<td class="cellgen">显示白色方块</td>
</tr>
<tr>
<td class="cellgen" align="center">07H</td>
<td class="cellgen">正常的黑底白字</td>
</tr>
<tr>
<td class="cellgen" align="center">70H</td>
<td class="cellgen">反白的白底黑字</td>
</tr>
<tr>
<td class="cellgen" align="center">01H</td>
<td class="cellgen">加底线</td>
</tr>
</tbody>
</table></div><div id="MySignature"></div>
<div class="clear"></div>
<div id="blog_post_info_block">


----------------------------------
