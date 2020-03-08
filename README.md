
打水漂
===
作者：[蘇庭葦](https://github.com/tingwei-fly)、[高聖傑](https://github.com/KaoShengChieh)、[蔡宥杏](https://github.com/tsai-you-shin):monkey:

:point_right: [程式細述 Code Description](Stone_Skipping.ipynb)<br>
:point_right: [搶先體驗 Run on GlowScript](http://www.glowscript.org/#/user/B06902117/folder/Public/program/FinalProject)<br>
:point_right: [解說影片 Presentation Video](https://www.youtube.com/watch?v=JO-nTd-2gdg)<br>
:point_right: [獲獎訪談 Award Interview](https://vphysics.ntu.edu.tw/post.php)

> 朝水面扔出扁平的石頭，讓它在水面上跳躍，這是人類古老的遊戲之一，也是許多人的童年回憶—打水漂。但究竟要怎麼樣才能將石頭打得更遠呢？這引起了我們的興趣。

## 目標

藉由分析石頭接觸水面時及在空中的受力，模擬出石頭在水面上的運動軌跡，並統整歸納出不同初始條件，例如石頭的初速、角度等等對水漂彈起次數或距離的影響。

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d5/Skipping_stones.svg/745px-Skipping_stones.svg.png)
> Image source: [Stone skipping - Wikipedia](https://en.wikipedia.org/wiki/Stone_skipping)

:bulb:何謂一個**厲害的**打水漂，我們以**彈跳次數多**作為指標:bulb:

## 大綱

- [目標](#目標)
- [研究假設](#研究假設)
- [物理分析與推導](#物理分析與推導)
- [程式架構](#程式架構)
- [結果分析](#結果分析)
- [延伸思考](#延伸思考)
- [開發時程](#開發時程)
- [參考資料](#參考資料)
- [附錄](#附錄一、和線性參數有關的碰撞之分析)

## 研究假設
* 水面沒有其他擾動
* 石頭為圓柱體，且不考慮其厚度
* 不考慮[馬格努斯效應(Magnus effect)](https://en.wikipedia.org/wiki/Magnus_effect)
* 石頭上方不會被水面覆蓋

## 物理分析與推導
打水漂的碰撞可以分成以下兩種：

* [和線性參數有關的碰撞](#附錄一、和線性參數有關的碰撞之分析)
* [和轉動參數有關的碰撞](#附錄二、和轉動參數有關的碰撞之分析)

由於篇幅過大，我們將完整的分析放在附錄，我們會在附錄中分析這兩種碰撞，以及其碰撞所造成之能量消耗或終端條件。

## 程式架構

![](https://i.imgur.com/fsIuRTg.png)

我們將模擬打水漂分成主程式及四個函式，包含石頭、碰撞、空氣、水波。
* **主程式**模擬石頭由出發點開始的斜向拋射，同時依據不同的拋射角度、石頭角度、初速度作為初始狀態的變因進行探討，並由空氣函式作為石頭飛行時空氣阻力的因素。
* 接著利用**石頭函式**操縱石頭的初始狀況，在此函式中，我們設定整個模擬的石頭為圓柱體，並可改變其圓面積或厚度。
* 然後藉由**碰撞函式**計算石頭接觸水面時的彈射角度和石頭與水平面的夾角。
* **水波函式**則是在石頭與水面碰撞時，利用波函數使水面產生水波，並套入隨機參數模擬水花濺起的現象，力求接近現實生活中的打水漂。

<img src="https://i.imgur.com/HQTPD5S.png" width="600">

## 結果分析

![](https://i.imgur.com/KOynQSW.png)

藉由操縱**初速**、**轉速**和**角度**三項變因，分析石頭在不同狀態下的彈跳次數及彈跳距的改變。以下圖為例，橫座標為彈跳次數，縱座標為每次彈跳距離相對於首次彈跳距離的比值，並將三項變因組合成六種情況進行分析。

![](https://i.imgur.com/zAIvKA7.png)

* 由上圖發現隨次數的增加，綠線的下降幅度漸增，而紫線的下降幅度趨緩。意即，綠線接近凹向下的曲線，紫線則接近凹向上。
* 另外，在初速及轉速大、角度理想(~10°)的情況下，彈跳次數最多，而水漂後期的彈跳距離相較於一開始都非常短，對照真實裝況我們也可以觀察到這個現象，職業打水漂人士給它一個名字：[Pitty-Pat](https://stoneskipping.com/glossary/)。

![](https://i.imgur.com/yqWmbby.png)

接著，我們可以藉由石頭在空氣中和水面時的v-t圖分析石頭能量的消耗。
* 在空氣中時，石頭受到的阻力會與速率呈正相關，因此一開始速率下降幅度大，隨著石頭受阻力影響，後期下降幅度也逐漸變小。
* 在水面時，起初石頭速率較快受水的黏度影響小，因此前期速率下降幅度小，隨著石頭速率變慢，後期受水的黏滯現象較明顯，導致速率下降幅度逐漸增加。

由此即可驗證我們實驗所得之特性：由於初速較小、或轉速較小、或角度不適當導致在彈跳次數較少時，石頭會像是被水面「黏住」，因此受到水面的影響較大，圖形會較為凹向下。反之，如果彈跳次數較多時，石頭在碰觸水面後，會彷彿彈性碰撞般立即向上反彈，而不會被被水面「黏住」，受到空氣的影響較大，因此圖形會較為凹向上。

## 延伸思考
完成了原本的研究目標，我們還不滿足，我們思考著要如何讓電腦模擬有不一樣的火花。我們想到不一定要使用空氣和水作為介質，我們可以用一些生活中幾乎不可能用來做實驗的介質來模擬打水漂。因此，我們的程式提供了以下幾種液體介質選擇，這些選擇是考慮密度、互溶性、黏度等物理和化學特性所挑選出來的。

* **上方介質：** 空氣、橄欖油
* **下方介質：** 水、汽油、紅酒、苯

下圖為橄欖油(上方介質)與水(下方介質)的模擬狀況：

![](https://i.imgur.com/eGtgZcs.png)
![](https://i.imgur.com/HmcTPAp.png)

這裡更呼應了稍早的論述。上方圖中的紅線為原本「初速很大、轉速很大、角度理想」在空氣與水的狀況；黃線為將上方介質改為橄欖油，其餘初始條件皆與紅線相同的狀況。我們可以清楚地觀察到，由於上方介質的阻力變得更大，使得圖形凹向上的趨勢更為明顯。

更多的模擬可以到我們的[GlowScript](http://www.glowscript.org/#/user/B06902117/folder/Public/program/FinalProject)親自體驗。

## 開發時程 
* Week 1 \~ 2 (2017/11/22 \~ 2017/12/05)
  * Establish an executable program with prototypes of primary functions we need. Simulate the complete motion of stone skipping by neglecting air dragging and assuming that stone skipping at the water surface is elastic collision.
  
* Week 3 \~ 4 (2017/12/06 \~ 2017/12/19)
  * Build up user interface including run button, reset button, condition selecting menu, condition setting sliders, etc.
  * Considering air and water dragging.
  * Dividing stone skipping at the water surface into collision about linear parameters and collision about spin parameters.
  
* Week 5 (2017/12/20 ~ 2017/12/26)
  * Simulate various conditions by modifying parameters, such as stone’s angle versus horizon, initial velocity of projection, spin angular velocity,etc.
  * Add graph, labels, meter lines and so on to make physics phenomenon more clear and make our analysis more intuitive.
  * Technically, we have already finished our project.
  
* Week 6 (2017/12/27 ~ 2018/01/02)
  * Add some special fluid including olive oil, gasoline, benzene, wine and so on.
  * Add splash when stone collides at water interface.

## 參考資料

1. [The physics of stone skipping](https://arxiv.org/pdf/physics/0210015.pdf)
2. [雷諾數與阻力係數關係](http://developer.hanluninfo.com:8088/2005/fluid_mech/chapter08/inside_08_04_02_m.htm)
3. 雷諾數 [(resource 1)](https://en.wikipedia.org/wiki/Reynolds_number) [(resource 2)](https://www.grc.nasa.gov/www/BGH/reynolds.html) [(resource 3)](https://www.engineeringtoolbox.com/reynolds-number-d_237.html)
4. [阻力係數與形狀](https://www.engineeringtoolbox.com/drag-coefficient-d_627.html)
5. [Kinematic viscosity](https://www.engineeringtoolbox.com/kinematic-viscosity-d_397.html)
6. Viscosity of different fluid [(resource 1)](https://physics.info/viscosity/) [(resource 2)](https://en.wikipedia.org/wiki/Viscosity)
7. [Lift coefficient](https://en.wikipedia.org/wiki/Lift_coefficient)
8. [Coefficient of friction](https://en.wikipedia.org/wiki/Friction)
9. [Gyroscope effect](https://en.wikipedia.org/wiki/Gyroscope)

## 附錄一、和線性參數有關的碰撞之分析

### 正方形的石頭

由於圓形的石頭在計算過程中會出現大量的二階微分方程，使得計算變得非常的繁雜，因此我們首先考慮「正方形」的石頭。同樣不考慮石頭的厚度。

<img src="https://i.imgur.com/yzqqJqy.png" width="250">

這種形狀極大地簡化計算過程，並包含了打水漂所涉及的物理機制(mechanism)。

![](https://i.imgur.com/YQqNXqA.png)

我們找到了正方形石頭的終端條件。當然，根據經驗，我們打水漂都是選擇圓型的石頭。

<img src="https://i.imgur.com/3XB0EYc.png" width="200">

因此，接下來我們思考如何藉由正方形石頭的計算，來幫助我們找到圓型石頭打水漂的終端條件。

### 圓形的石頭

初步的受力分析和正方形石頭是完全相同的，接下來，我們針對圓形的石頭設定一些參數

![](https://i.imgur.com/1JUHPcS.png)

我們分析各項參數的物理因次，發現以「無因次」的假設最能簡化我們的計算，並能極佳地模仿剛剛所完成的正方形石頭的計算。以下三個設定為接下來計算圓形石頭所需要之核心參數，並沒有實際的物理意義，其中第一個設定加負號也是方便我們後面解微分方程。

![](https://i.imgur.com/PRZJLrH.png)

## 附錄二、和轉動參數有關的碰撞之分析

我們忽略[馬格努斯效應(Magnus effect)](https://en.wikipedia.org/wiki/Magnus_effect)所產生的石頭在流體中轉動所受到的力，僅考慮[陀螺儀效應](https://en.wikipedia.org/wiki/Gyroscope)的影響。在運轉中的陀螺儀，如果外界施一作用或力矩在轉子旋轉軸上，則旋轉軸並不沿施力方向運動，而是順著轉子旋轉向前90度垂直施力方向運動。

![](https://i.imgur.com/uGcb7JW.png)
![](https://i.imgur.com/25kyOXu.png)
