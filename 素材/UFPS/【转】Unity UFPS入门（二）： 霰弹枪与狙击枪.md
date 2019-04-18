> 原文（中）：[UFPS入门： Unity FPS 教程](https://blog.csdn.net/kmyhy/article/details/72846348)
原文（英）：[Introduction To UFPS: Unity FPS Tutorial](https://www.raywenderlich.com/151418/introduction-ufps-unity-fps-tutorial) 
原作者：[Anthony Uccello](https://www.raywenderlich.com/u/anthonyuccello)

`原文标题: Introduction To UFPS: Unity FPS Tutorial`
`本文在参考原（中英）文的基础下对部分翻译做了一些增减及修改，以便更好理解本文内容`


> In this Unity FPS tutorial you’ll learn the basics of working with UFPS to build the basis for a solid first-person shooter in Unity3D.

## 添加霰弹枪

编写一个不让主角拥有短距离毁灭性武器的 FPS 游戏有什么意义？那还不如不要写。没有这种武器的只可能是一个非常小、非常小的 FPS 游戏。

回到 UFPS 上来，你将不废吹灰之力就添加一个枪械模型。

在项目浏览器中，输入 **2Shotgun** 然后将 **2Shotgun** 预制件拖到结构视图的 1Pistol 游戏对象的下面。

![](http://upload-images.jianshu.io/upload_images/13611890-ec5945eccfeaa05c.gif?imageMogr2/auto-orient/strip)

你可能会发现 `vp_WeaponShooter` 没有任何选项，那是因为预制件被禁用了。将 2Shotgun 左边的选项框勾上。

![](http://upload-images.jianshu.io/upload_images/13611890-f6144eb6e76d2be1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你可能也想到了，你只需要运行游戏然后按 2 就可以装备上霰弹枪了？差不多吧！但是 `SimplePlayer` 预制件只配置了手枪。

为了让新枪能够生效，你需要添加了一个特殊的组件。在结构视图中点击 **SimplePlayer** 游戏对象，然后在检视器中点 **Add Component**。输入 **vp_FPWeaponHandler**，选中这个脚本。这个脚本允许玩家切换枪械。

![](http://upload-images.jianshu.io/upload_images/13611890-bec893cf58bc24a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将 **Start Weapon** 的值设置为 **1**，即手枪。现在，你可以点击 **Play** 然后按 **2** 就可以从手枪切换到霰弹枪。

![](http://upload-images.jianshu.io/upload_images/13611890-8491834742ae6a27.gif?imageMogr2/auto-orient/strip)

等等……有点不对劲——枪的位置不太对劲！在结构视图Hierachy中选中 **2Shotgun**，在检视器Inspector中展开 `vp_FPWeapon` 的 **PositionSprings** 节，将 **Offset** 设为 **(X** = **-0.04**, **Y** = **-0.46**, **Z** = **4.89)**。 展开 **RotationSprings** 节，将 **offset** 设置为 **(X** = **-1.09**, **Y** = **-93.3**, **z** = **2.5)**。

![](http://upload-images.jianshu.io/upload_images/13611890-46090ec5e799398c.gif?imageMogr2/auto-orient/strip)

霰弹枪会发射一片子弹。一把霰弹枪一次只射出一颗子弹有什么意思呢。

展开 **vp_FPWeaponShooter** 下面的 **Projectdile**，将 **Count** 设置为 **10**, **Spread** 设置为 **2**。

展开 **MuzzleFlash** 将预制件设置为 **MuzzleFlashShotgun**。

![](https://koenig-media.raywenderlich.com/uploads/2016/12/muzzleflash.png)

将 **Position** 设为 **(X** = **-0.08**, **Y** = **-0.24**, **Z** = **8.88)** ，**Scale** 为 **(X** = **1**, **Y** = **1**, **Z** = **1)**。如果你想修改这些值，可以在试玩模式下编辑 MuzzleFlash `Position`，然后停止游戏后再修改 Position——这样枪焰就会精确地出现在你所指定的位置。

展开 **Sound** 小节，将 **Fire** 设置为 **ShotgunFire**。

![](http://upload-images.jianshu.io/upload_images/13611890-1a699160df50ea28.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 **Play** 并按下数字 **2**，你可以向靶子开火了。

![](https://koenig-media.raywenderlich.com/uploads/2016/12/shotgunfire.gif)

Zoom 是一种特殊的状态，在 UFPS 中它被预设为鼠标右键（RMB，right mouse button）来触发。点击右键后，会将所有组件设置为 Zoom 状态，这些组件必须监听这种状态的改变。

你可以添加自己的 Zoom 状态。来试一下，点击 **Play**，按下 **2** 换成散弹枪。修改 **vp_FPWeapon** 的 **Position Springs** 和 **Rotation Springs** **Offset** 值，让镜头指向枪管下面。点击位于 `vp_FPWeapon` 底部的 **Save** 按钮，将文件名命名为 **ShotgunZoom** 然后点击 **Save**。

![](https://koenig-media.raywenderlich.com/uploads/2016/12/shotgunzoom.gif)

> 注意：如果你无法让 Zoom 显示正常，或者你使用的 Mac 上无法看到 **vp_FileDialog** 窗口中的 **Save** 按钮，请下载这个文件 [ShotgunZoom](https://koenig-media.raywenderlich.com/uploads/2016/12/ShotgunZoom.txt)并将它拖到 Unity 的 Assets 文件夹。

设置完 Zoom，但是散弹枪并不知道怎样去使用它。停止游戏，点击结构视图中的 **2Shotgun**, 在检视器中展开 **vp_FPWeapon** 的 **States** 小节。将 **ShotgunZoom** 拖到 **Zoom** 字段。

![image](http://upload-images.jianshu.io/upload_images/13611890-79944696fde473a9.gif?imageMogr2/auto-orient/strip)

UFPS 利用开始值和终止值自动插值创建动画。点击 **Play**，当你换上散弹枪并切换到 **Zoom** 模式下时，它会以动画方式移动到你所保存的 Zoom 状态的位置。

![](http://upload-images.jianshu.io/upload_images/13611890-ebc776d1d710e700.gif?imageMogr2/auto-orient/strip)

## 添加狙击枪

任何优秀的 FPS 游戏都会提供一系列武器以便玩家能够在各种情况下消灭敌人。这次当你想要在一个安全距离下一个接一个地搞定敌人，没有什么比一直大狙更好的武器了！

在项目浏览器中搜索 **3Sniper** 并将 **3Sniper** 预制件拖到结构视图的 **2Shotgun** 下面。在检视器Inspector中，添加 **vp_FPWeapon** 和 **vp_WeaponShooter** 脚本。

![](http://upload-images.jianshu.io/upload_images/13611890-0ddadcb10f37bcff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在项目浏览器中展开 **Assets/Prefabs** 文件夹，将狙击步枪的模型添加到 `vp_FPWeapon` 的 `Rendering` 节。

在结构视图中，点击 **3Sniper** ，在检视器中展开 **vp_FPWeapon** 组件。展开 **Rendering** 节，找到 **1st Person Weapon(Prefab)** 字段。将 m24 预制件从项目浏览器中拖到 **1st Person Weapon（Prefab)** 字段。

![](http://upload-images.jianshu.io/upload_images/13611890-7fca411bd543ae8a.gif?imageMogr2/auto-orient/strip)

在 `vp_FPWeapon` 下面，将 **Position Springs** 设置为 **(X** = **1.4**, **Y** = **-0.81**, **Z** = **4.88)** ，**Rotation Springs** 设为 **(X** = **-2.7**, **Y** = **79.62**, **Z** = **-5.01)**。

展开 **vp_FPWeaponShooter** 的 **Sound** 小节，将 **Fire** 设置为 **SniperShot**。

然后就可以使用大狙了。点击 **Play**，按数字 **3** 试一试。

![](https://koenig-media.raywenderlich.com/uploads/2016/12/sniperadd-1.gif)

你是不是觉得“可以用” 是有点夸张了？你会发现当你在右键瞄准的时候却没有瞄准镜！而使用狙击枪的时候非常有必要添加一个瞄准镜。

接下来添加一个 Sniper 组件，用于瞄准。这是一个特殊脚本，为了能够正常工作，它需要监听玩家事件。

打开 Scripts 文件夹，在任何地方右键，使用弹出菜单创建一个新的C#脚本文件，命名为 Sniper。双击这个文件打开，将内容编辑为：

```
using UnityEngine;
using System.Collections;

public class Sniper: MonoBehaviour
{
  //1
  public vp_FPPlayerEventHandler playerEventHandler;
  public Camera mainCamera;
  private bool isZooming = false;
  private bool hasZoomed = false;

  //2
  void Update()
  {
    if (playerEventHandler.Zoom.Active && !isZooming)
    {
      isZooming = true;
      StartCoroutine("ZoomSniper");
    } 
    else if (!playerEventHandler.Zoom.Active)
    {
      isZooming = false;
      hasZoomed = false;
      GetComponent<vp_FPWeapon>().WeaponModel.SetActive(true);
    }

    if (hasZoomed)
    {
      mainCamera.fieldOfView = 6;
    }
  }

  //3
  IEnumerator ZoomSniper()
  {
    yield return new WaitForSeconds(0.40f);
    GetComponent<vp_FPWeapon>().WeaponModel.SetActive(false);
    hasZoomed = true;
  }
}
```

代码解释如下:

1.  这些变量用于保存 `PlayerEventHandler` 和镜头的引用。两个布尔变量用于保存 zoom 状态。
2.  这段代码判断 zoom 状态是否启用。当枪处于 zoom 状态时，它会将视图的这个字段设置为 6，这就触发了 zoom 状态。
3.  这个子程序追踪判断什么时候 zoom 动画才完成——这里大约需要 0.4 秒左右——然后隐藏枪支模型。这会在切换枪支模型与瞄准镜之前给玩家一个缓冲时间。

保存文件，返回 Unity。将 **Sniper** 基本拖到 **3Sniper** 这个游戏对象。

![](http://upload-images.jianshu.io/upload_images/13611890-537482b9aa8540dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将 **SimplePlayer** 预制件拖到 **Sniper** 组件的 **PlayerEventHandler** 上。然后，将 **FPSCamera** 拖到 **Simper** 组件的 **Main Camera** 字段。

![](http://upload-images.jianshu.io/upload_images/13611890-b118dd83da28f4a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在结构视图Hierarchy中，你会发现有一个名为 **UI** 的 游戏对象，它是用一个 **SniperZoom** 的贴图创建的。

进入 Scripts 文件夹，创建一个新的 C# 脚本文件。命名为为 GameUI。在结构视图中选中 UI 对象，将 GameUI 脚本从项目浏览器中拖到检视器中。

![](http://upload-images.jianshu.io/upload_images/13611890-17765c1def5c2dca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开 GameUI 脚本，编辑如下内容：

```
using UnityEngine;
using UnityEngine.UI;

public class GameUI: MonoBehaviour
{
  public GameObject sniperZoom;
  public vp_PlayerEventHandler playerEventHandler;

  public void ShowSniperZoom() 
  {
    sniperZoom.SetActive(true);
    sniperZoom.GetComponent<Image>().enabled = true;
  }

  public void HideSniperZoom()
  {
    sniperZoom.SetActive(false);
    sniperZoom.GetComponent<Image>().enabled = false;
  }
}
```

两个 public 方法分别用于显示和隐藏 sniper scope 贴图——这里保存了一个 scope 纹理的引用和事件处理器的引用。Sniper 脚本经过配置后会调用这两个方法。保存脚本，返回 Unity。

在结构视图中，展开 **UI** 对象。将 **SniperZoom** 拖到检视器中的 **SinperZoom** 字段，然后将 **SimplePlayer** 拖进 **PlayerEventHandler** 字段。

![](http://upload-images.jianshu.io/upload_images/13611890-46b424680a131af8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开 **Sniper** 脚本。在头部添加变量：

```
public GameUI gameUI;

```

现在，这个脚本将引用 GameUI。

在 else if (!playerEventHandler.Zoom.Active) 语句下面，加入：

```
gameUI.HideSniperZoom();

```

修改之前是 :

```
else if (!playerEventHandler.Zoom.Active)
{
  isZooming = false;
  hasZoomed = false;
  GetComponent<vp_FPWeapon>().WeaponModel.SetActive(true);
}
```

修改之后变成了:

```
else if (!playerEventHandler.Zoom.Active)
{
  gameUI.HideSniperZoom();
  isZooming = false;
  hasZoomed = false;
  GetComponent<vp_FPWeapon>().WeaponModel.SetActive(true);
}
```

当处于“非-zoom”状态时，sniper zoom 图片将隐藏。

在 GetComponent().WeaponModel.SetActive(false); 后面添加：

```
gameUI.ShowSniperZoom();

```

修改之前的代码是:

```
IEnumerator ZoomSniper()
{
  yield return new WaitForSeconds(0.40f);
  GetComponent<vp_FPWeapon>().WeaponModel.SetActive(false);
  hasZoomed = true;
}
```

修改之后变成了:

```
IEnumerator ZoomSniper()
{
  yield return new WaitForSeconds(0.40f);
  GetComponent<vp_FPWeapon>().WeaponModel.SetActive(false);
  gameUI.ShowSniperZoom();
  hasZoomed = true;
}
```

这句在狙击步枪进入 zoom 状态之后显示 sniper scope 图片。保存，返回 Unity。选择 **3Sniper** 对象，将它拖到 **UI** 对象的 **GameUI** 字段。

![](http://upload-images.jianshu.io/upload_images/13611890-ba509a6c814e5407.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所有的工作是为了在玩家进入瞄准状态时把枪隐藏，同时显示瞄准器视野。运行游戏，按下数字键 **3**，换成大狙，点击 **RMB** （鼠标右键）切换到 **zoom** 状态。

![](http://upload-images.jianshu.io/upload_images/13611890-ea533095a14e377f.gif?imageMogr2/auto-orient/strip)

起作用了，但还不完善。你可以通过一些设置进行改进，让玩家在瞄准的时候能够看一眼瞄准镜。

点击 **Play**，按 **3** 切到大狙。展开 **3Sniper** 的 **Position Spring** 和 **Rotation Springs**,调整位置和角度的 **offset** 值，直到镜头前有瞄准镜显示出来。

点击 **vp_FPWeapon** 组件底部的 **Save** 按钮。文件命名为 **SniperZoom** 然后点 **Save**。

我们将大狙进入瞄准状态时的状态保存到了文件里。这里，你将枪上移，进入镜头前面显示出来，就像玩家在低头看瞄准镜一样，**SniperZoom** 中刚好保存了这个状态。



> 注意：如果你无法创建或保存你的设置，请下载此[SniperZoom](https://koenig-media.raywenderlich.com/uploads/2016/12/SniperZoom.txt)文件。

现在，你已经设好了状态，但还需要创建要用到的状态。选中 **3Sniper**，展开 **vp_FPWeapon**，展开 **State** 一节。

点击 **Add State** 按钮，将 **Untitled with Zoom** 替换成 **new State** 字段。

从 **Assets** 文件夹将 **SniperZoom.txt** 文件拖到 **3Sniper** 的 **vp_FPWeapon** 组件的 **Text Asset** 字段。

![](http://upload-images.jianshu.io/upload_images/13611890-43bf8ae648a08b48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 吃枪子去吧！

在你能够狙击猎物之前，还有几个工作要做。在枪里面装入子弹是个不错的注意——当然，你可以跳过这一步，但这就不是狙杀了。前面的手枪和散弹枪都创建过子弹。我们从头开始创建大狙的子弹，因此你需要设置子弹所用的预制件。

仍然是在 **3Sniper** 中，展开 **vp_FPWeaponShooter**，再展开 **Projectile** 一节。点击 **Prefab** 右边的小圆圈按钮。找到并选择 **PistolBullet** 预制件。

![](http://upload-images.jianshu.io/upload_images/13611890-a107b8ca94923d77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 **Play**,瞄准距离最远的枪靶——你可能需要转个方向。按下 **RMB** （右键）瞄准、开枪！


现在，在进入 zoom 状态之前，你会看见狙击镜显示。干得不错！

## 结束

噢，又到说再见的时候了。从何说起呢？UFPS 是一个强大的框架，这篇教程仅仅论及皮毛。

接下来的步骤应该是添加一只突击步枪和令人满意的动画。试着修改 vp_FPWeapon 中的滑竿和值直到感觉满意。记住，它的射速要快，每次射击后会产生快而小的后弹动作。

你学会了基本的概念，但在 UFPS 文档中还有大量的东西需要学习。比如，你还不知道什么是 vp_FXBullet。在 FPS 游戏中有一个关键特性就是库存和弹药管理，它设计到 UI 的使用。

你也没有体验到许多 UFPS 的超酷的特性——因此请继续学习它们。

关于 Unity 的 UI 系统，你可以参考以下教程：

[Unity UI Part 1](https://www.raywenderlich.com/149464/introduction-to-unity-ui-part-1) 
[Unity UI Part 2](https://www.raywenderlich.com/149464/introduction-to-unity-ui-part-2) 
[Unity UI Part 3](https://www.raywenderlich.com/149464/introduction-to-unity-ui-part-3)

或者购买[Unity Games by Tutorials](https://blog.csdn.net/kmyhy/article/details/72846348)一书，学习如何编写棒棒的游戏。

正如我之前所说的，reflection 是一个高级技术，它在你的编码工具箱中是个很好的工具，因此请你有机会一定要[阅读它](https://msdn.microsoft.com/en-us/library/mt656691.aspx)。

通过这个官方 Unity 事件教程([https://www.youtube.com/watch?v=3NBYqPAA5Es](https://www.youtube.com/watch?v=3NBYqPAA5Es))，你可以更深入地学习事件系统。

有任何问题、建议或需要更多的 UFPS 教程，请在论坛中留言。你的评价将决定我们下一个主题会介绍什么——请告诉我！

## 致谢

特别感谢 Wojciech Klimas 提供了枪械模型，以及其他资源：

[网站](http://www.blendswap.com/user/veti) 
[散弹枪的 Asset](http://www.blendswap.com/blends/view/52432) 
[狙击步枪的 Asset](http://www.blendswap.com/blends/view/17879)
