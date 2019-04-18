> 原文（中）：[UFPS入门： Unity FPS 教程](https://blog.csdn.net/kmyhy/article/details/72846348)
原文（英）：[Introduction To UFPS: Unity FPS Tutorial](https://www.raywenderlich.com/151418/introduction-ufps-unity-fps-tutorial) 
原作者：[Anthony Uccello](https://www.raywenderlich.com/u/anthonyuccello)

`原文标题: Introduction To UFPS: Unity FPS Tutorial`
`本文在参考原（中英）文的基础下对部分翻译做了一些增减及修改，以便更好理解本文内容`


> In this Unity FPS tutorial you’ll learn the basics of working with UFPS to build the basis for a solid first-person shooter in Unity3D.

用一支散弹枪(shotgun)轰杀大片凶恶的敌人或者在战场上小心翼翼地狙杀(sniping)你的对手是一种爽到极点的游戏体验。以动作+射击为主的游戏被称作第一人称射击游戏（FPS,First-Person-Shooter）。它有一个类似的种类，即第三人称射击游戏——区别在于你看到的画面是角色的后背还是枪管的下方。

在编写 FPS 游戏时，毫无疑问需要做大量工作。但是，我们没有必要从 0 开始，你可以使用一个叫做 **UFPS**(**Ultimate First Person Shooter**,终极FPS)的框架！

在本文中，我们将介绍 UFPS 是什么以及如何用它创建一个基本的 Unity FPS “沙盒”(sandbox)游戏——“沙盒”一词意指玩家被限制在一个有限的空间内。

读完本文，你将学会：

*   用简单的 UFPS 脚本让玩家移动和开火
*   使用 `vp_FP` 组件
*   当子弹击到平面时显示弹孔
*   为玩家添加内置的 FPS 动作
*   添加新的枪具并配置属性
*   添加带瞄准镜的大狙（最好的枪）

## 预备知识

你应当熟悉 C# 脚本并关联组件到 GameObject 对象。如果你没接触过 Unity 或者需要重温一下，请参考我们的 [getting started in Unity 教程](https://www.raywenderlich.com/110263/unity-tutorial-getting-started-part-1)。

你还需要一份 Opsive 公司的 “UFPS: Ultimate FPS” 许可协议，这份协议你可以从[Unity Asset 商店](https://www.assetstore.unity3d.com/en/) 获得。这篇教程是用 [UFPS 1.7](https://assetstore.unity.com/packages/templates/systems/ufps-ultimate-fps-2943)编写的。
(目前Opsive 公司已经推出了[UFPS Version2版本](https://assetstore.unity.com/packages/templates/systems/ufps-ultimate-fps-106748)，[文档](https://opsive.com/support/documentation/ultimate-character-controller/character/))。

最后，你需要使用 Unity 5.6.0 或以上的版本。

## 开始

UFPS 是一个 Unity 插件(plugin)，包含用于支持典型 FPS 游戏的核心逻辑。不需要自己为每个场景编写脚本，你可以通过修改预置脚本来处理常见的任务比如：

*   装备枪支（Equipping guns）
*   枪的移动动画和开火（Animating and firing guns）
*   管理弹药和库存（Managing ammo and inventory）
*   玩家的移动（Moving the player）
*   管理相机视角（Managing the camera’s perspective）
*   跟踪相机（Tracking the camera）
*   管理事件，比如被激光枪击中（Managing events, such as getting hit by a ray gun）
*   管理平面，比如滑坡和水道（Managing surfaces, e.g., slippery slopes and waterways）
*   尸体及重力物理（Ragdoll and gravity physics）

好的游戏不可能一蹴而就。做游戏的最好方式是把注意力集中在基本功能上，比如核心玩法，然后以此为基础逐步完善。UFPS 能够帮你打好这个基础并且还提供一个更加庞大的框架给你使用。

下载开始项目：[ReadyAimFire_Unity3D_starter](https://koenig-media.raywenderlich.com/uploads/2017/02/ReadyAimFire_Unity3D_starter.zip)。

在 Unity 中打开项目，双击 **Main** 场景加载游戏场景。

> 注意：我们不能再次分发 UFPS 的源文件，因此这个教程中不会有一个最终完成后的项目，它们也不包含在开始项目中。你需要自己完成整个游戏:]

（UFPS是收费的，而且可能会有一些版权问题，所以原作者不能提供UFPS的project）

在 Unity Asset 商店中找到 UFPS，找到下载栏，点击**下载**按钮。下载完成后，点击 **Import** 按钮，稍等一会显示出包的内容，然后点 **OK**。包很大，请耐心一点。

> 注意：你可能会看到一个关于“ importing a complete project ”的警告： 
> ![image](http://upload-images.jianshu.io/upload_images/13611890-9ce090bbd79dae85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> 
> 这是因为 asset 中包含了一些项目设置，这些设置会覆盖你当前 open project 设置。在这里，只有 Graphics 和 Tag 设置会被覆盖，因此你只需要点击 **Import** 按钮继续导入 UFPS 即可。

导入完成后，你可能会看到几个警告。这些警告来自于 UFPS，直接忽略即可。

从 **File** 菜单选择 **Save Scenes**，保存修改，或者按`Ctrl+S`保存。(这里的保存修改的意思是保存开始项目ReadyAimFire和UFPS项目在一个项目里)

点击 Play 按钮，感受一下全新的项目。

![](http://upload-images.jianshu.io/upload_images/13611890-820d686686e618fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 重要概念

你肯定非常想射击（或者编写脚本），但理解 UFPS 框架的一些关键概念非常重要。

### FP 组件

**FP** 表示组件属于管理真实的第一人称射击对象（即玩家）的一系列脚本，这些组件都以 **vp_FP** 开头。其中最重要的组件是 **vp_FPController** 脚本，在玩家对象被创建的时候就关联了这个脚本。

### 状态

状态是一个容易混淆的概念。在许多 `vp_FP` 组件中，你都会找到另一个名为 State 的属性，例如：

![An area where states can be added](http://upload-images.jianshu.io/upload_images/13611890-32ccc72f631e8362.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

某些 `vp_FP` 脚本用一个 state 字段来记录追踪一些本地变量的值，这些变量用于表示组件在指定时间点的状态。设想一下：当你放大/机瞄并使用狙击步枪的瞄准镜时，你所处的一种“Zoom”状态—— 这个状态下的预设值应当变大，同时改变枪的位置和角度以对准中心位置。当你 “Zoom” 的时候，它也会让武器以动画形式的呈现出移动的效果。

### 事件

事件是对象之间“交流”的信号，用于表示某些重要的事情发生了，其它对象必须做出响应。例如，当某样东西被射中时。“被射中”就是一种事件——也可以说一个信号。根据你的游戏设定，其它对象会对此做出响应。这篇教程不会过多介绍事件。

## 创建玩家预制件

在项目浏览器的搜索栏中输入 **SimplePlayer**，然后将 SimplePlayer 预制件拖到场景中，放到靶子前面。

![](http://upload-images.jianshu.io/upload_images/13611890-ebe20d641b9ddf9b.gif?imageMogr2/auto-orient/strip)

在结构视图中选中 **SimplePlayer** 游戏对象，在**Transform**组件中设置它的位置参数**Position** 为 **(X = -26.6, Y = 1.3, Z = -21.3)** ，角度参数**Rotation** 为 **(X = 0, Y = 0, Z = 0)**。

![](http://upload-images.jianshu.io/upload_images/13611890-43f437716ebe8fcf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从结构视图Hierarchy中**删除** **Camera** 这个游戏对象。我们不需要它，因为在 SimplePlayer 上的镜头会给你一个第一人称视角。

![](http://upload-images.jianshu.io/upload_images/13611890-82b96c127532831b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 **Play**，你创建了一个 FPS 游戏并且可以运行了！

在开枪或使用瞄准镜的时候按下 **play**,会将光标锁定到屏幕上。按下 **Esc** 又会重新获得光标。

使用 **WASD** 键移动或者按下**鼠标左键**开枪，**鼠标右键**打开瞄准镜——你可以在打开瞄准镜的同时开枪。还可以按住 **Shift** 键奔跑，按**空格键**跳越。确保你的子弹都打在了靶子上:]

> 注意：在编写本教程的时候，UFPS 有一个 Bug，当你第一个试玩一个场景时，不能看到枪口焰。解决办法是点击鼠标右键打开瞄准镜一次——后面就正常了。

在其后面有许多逻辑，但 UFPS 好的地方就是它会为你完成许多脏活累活。它让你把注意力放在更重要的事情上，比如为武器添加自定义的行为。

你会惊喜地发现根本不需要为玩家的移动和武器的开火创建任何动画。你所看见的这些动作使用了一些物理引擎。当然这也是由 UFPS 替你完成的！

UFPS 开始只有一把小手枪，但开始项目中还包含散弹枪和狙击步枪的预制件。你可以简单地拖入这两个预制件并修改 UFPS 设置以完善动画效果。

尽管不需要管枪支的动画，但你需要自己实现手部的动画。这篇教程不会设计手的动画，UFPS 也不会包含手的动画。但是，如果你要做一个正式的 FPS 游戏，可能就需要考虑实现手部的动画。

最后，你可以为枪支模型添加复杂动画。这样，你就需要在模型中包含手的模型。

## 添加弹孔

现在你可以开火并且听到枪声，同时还能看到弹壳跳出！在枪口还会有很酷的枪焰喷出！UFPS 为你实现了这些，它有一个特性允许你轻易地添加和配置新枪。

尝试向某些东西开火，看看到底还差了点什么？这把枪难道是射到了空气中吗？为什么没有弹孔？

任何时候当你射到某样物体时都会留下一些痕迹。停止运行游戏。在结构视图 **Hierarchy** 中，点击 **Environment** 游戏对象左边的下拉箭头，选择子对象 **Wall** 。

在检视器中，点击 **Add Component**，在搜索栏中输入 
 **vp_SurfaceIdentifier** 。点击 **vp_SuerfaceIdentifier** ——唯一的脚本——然后添加它。点击 Surface Type 字段右边的**小圆点**，在弹出菜单中，选择 **Assets** 标签，然后选择 **Metal**。

![Adding Bullet Holes](http://upload-images.jianshu.io/upload_images/13611890-7cb4db33a9d4ae44.gif?imageMogr2/auto-orient/strip)

在结构视图**Hierachy**中选中 **Ground** 这个游戏对象，重复上述步骤，但在最后一步选择 **Wood**。

枪靶需要一个木制的表面痕迹，因此在结构视图中选中 **Target** 和 **BackTarget**，点击旁边的下拉箭头，选择它们的子对象，然后添加 **vp_SurfaceIdentifier** 脚本组件，表面类型设置为 **Wood**。

点击 Play 然后分别向标靶、墙和地板射击，现在你可以检查你的手艺如何了 :]

![shotall-1](https://koenig-media.raywenderlich.com/uploads/2016/12/shotall-1.gif)


## 如何使用 UFPS 手枪

仅仅用小手枪是无法打败一群可怕的怪兽的。过一会你会添加散弹枪，但我们先来理解如何使用手枪。

在结构视图 **Hierachy** 中，展开 **SimplePlayer**，然后展开 **FPSCamera** ，选择 **1Pistol** 。

武器前面的编号 **1** 表示玩家在游戏期间可以按这个数字来装备手枪。当你添加新武器的时候，你可以向上增加这个数字，UFPS会自动处理切枪的问题。

**1Pistol** 有几个重要组件:

*   `AudioSource` 让枪械在开火时发出 bang 的声音。
*   `vp_FPWeapon` 将该游戏对象识别为 UFPS 框架中的一种武器并决定了和这个武器所对应的外观。
*   `vp_FPWeaponShooter` 负责枪焰、弹壳弹跳和子弹的效果。
*   `vp_FPWeaponReloader` 负责装弹时的声音和动效。

> 注意: 弹药和装弹不属于本文内容，但它们也属于创建游戏时需要考虑的内容。在 UFPS 文档中关于这些 `vp_FP` 组件的完整介绍包含了许多章节，因此当你遇到问题时，请首先参考这个文档。

### vp_FPWeapon

在检视器**Inspector**中，展开 **vp_FPWeapon** 脚本组件的 **Position Springs** 一节。这个组件用于设置枪械在屏幕上的位置，它是根据当前枪械的状态而改变的——例如，当手枪在 Zoom 状态下移动的时候。`Offset` 就是枪械当前在屏幕上的位置。

点击 **Play**，通过修改 `Offset` 的值随意调整枪的位置。通过这种方式，可以让你在试玩模式下不会搞乱游戏的真实值。

在试玩模式下修改设置是一种测试和调试的好方法——唯有一点，如果你真的想修改这些设置，请不要在试玩模式下修改。你很容易被许多事情包围而忘记自己仍然处于试玩模式，这往往会导致白给并让你wdnmd。 XD

![](https://koenig-media.raywenderlich.com/uploads/2016/12/pistolpos-1.gif)

从这个组件往下看，找到 **Spring2 Stiffn** 和 **Spring2 Damp** — 这是枪的后坐力设置。Spring2 Stiffn 即 ‘弹性’, 或者开火后枪如何弹回它原来的位置。Spring2 Damp 是弹回的速度有多快。最简单的办法是通过实际操作来体验:]

点击 **Play** 然后拖动 `Spring2 Stiffn` 和 `Spring2 Damp` 值，感受不同的效果。

![](https://koenig-media.raywenderlich.com/uploads/2016/12/pistolspr2.gif)

要使最终的动画满意，可能需要做一些尝试并付出点代价，因此，如果您的前几次尝试使枪像一只弹簧一样弹来弹去，不要感到奇怪。

你已经熟悉 **Position** 了，接下来我们看一下 **Rotation**。在检视器中，展开 **vp_FPWeapon** 的 **RotationSprings** 节。找到 `Offset` 值，它和你在 `PositionSrping` 节中看到的是一样的。所不同的是这些值决定了枪的角度。

点击 **Play** 并修改一下 `Offset` 值试试。

![](http://upload-images.jianshu.io/upload_images/13611890-17c0ac7f7392383e.gif?imageMogr2/auto-orient/strip)

试想一下当你开枪的时候会发生什么。UFPS 通过插值的方式在两个状态间实时创建动画。

你所需要的仅仅是在 `vp_FPWeapon` 组件中设置各个状态值——不需要创建或导入导致这种效果的动画。你会在添加散弹枪和狙击步枪的时候亲身体会这一点。

现在让我们停止游戏。

进行一点深入讨论。在 `vp_FPWeapon` 的检视器中展开 **States** 节，看一下已有的状态及与之关联的数据文件。数据文件在状态改变时 `vp_FPweapon` 中的什么记录值会发生改变。

以 **Zoom** 状态为例。双击 **WeaponPistolZoom**，看一下脚本文件——它会使用Unity 默认的代码编辑器打开。打开后是一个简单数据文件：

ComponentType vp_FPWeapon 
ShakeSpeed 0.05 
ShakeAmplitude 0.5 0 0 
PositionOffset 0 -0.197 0.17 
RotationOffset 0.4917541 0.015994 0 
PositionSpringStiffness 0.055 
PositionSpringDamping 0.45 
RotationSpringStiffness 0.025 
RotationSpringDamping 0.35 
RenderingFieldOfView 20 
RenderingZoomDamping 0.2 
BobAmplitude 0.5 0.4 0.2 0.005 
BobRate 0.8 -0.4 0.4 0.4 
BobInputVelocityScale 15 
PositionWalkSlide 0.2 0.5 0.2 
LookDownActive false

当 `vp_FPWeapon` 进入 Zoom 状态时肯定会有某些变化发生。 看起来有点吓人，但其实非常简单——你可以在试玩期间改变枪的位置，然后保存状态文件。

### vp_FPWeaponShooter

在检视器中展开 **vp_FPWeaponShooter** 下面的 **Projectile** 节。这个决定了枪的开火方式。

*   `Firing Rate` 是多长时间触发一次动作。
*   `Tap Firing` 允许玩家不需要等待开火的动画完成直接射击——它修改了开火动画。
*   `Prefab` 是子弹。子弹的脚本位于 vp_BulletFX 下，要了解更多请参考这个文档。

展开 Muzzle Flash 小节。你可以配置这个美工预制件，它的大小、位置以在玩家开枪后实现完美的枪焰效果。

展开 **Shell 小结**，看一下这些设置，它们负责完成当弹壳从枪管跳出后的动作。

最后，展开 **Sound** 小节。在这里你可以配置负责开枪时和无弹药时的声效。

在这里还会有一个 `States` 节。它和 vp_WPWeapon 节的状态栏类似。如果你不知道它们的用途，你可以查看源代码，里面包含了一个高级主题 Refelection。

