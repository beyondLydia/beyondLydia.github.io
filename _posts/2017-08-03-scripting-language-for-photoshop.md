---
layout: post
title: js scripting in photoshop cs6
---

<div class="message">
  因为需要批量处理dds文件，开始学习写ps中的脚本文件.jsx，本质上是JavaScript，编辑器可以用sublime，也可以用Adobe ExtendScript Toolkit CS6，后者据说是可以直连ps cs6调试，但目前为止没出现这个美好的效果，程序里调试也无效，会提示报错，所以我目前是前往ps中的菜单-文件-浏览，手动选择所要运行的脚本。
</div>


【在中文版ps中获取通道的方法】
//通过调用[channels]对象的[getByName]方法，获得当前图像的红色通道。
var channelRefR = app.activeDocument.channels.getByName("红");
var channelRefG = app.activeDocument.channels.getByName("绿");
var channelRefB = app.activeDocument.channels.getByName("蓝");

//通过调用[channels]对象的[getByName]方法，获得当前图像的[Alpha 1]通道。
var channelRefA = app.activeDocument.channels.getByName("Alpha 1");

//通过调用[alert]命令，弹出警告框，显示通道的信息。
alert(channelRefA.kind);

【创建新颜色并把它赋给前景色】
var black = new SolidColor();
//black.rgb[“hexValue”] = “000000”;
black.rgb.red = 0;
black.rgb.green = 0;
black.rgb.blue = 0;
app.foregroundColor = black;

【关于颜色的文档】
Working with colors

Your scripts can use the same range of colors that are available from the Photoshop user interface. Each color model has its own set of properties. For example, the RGBColor class contains three properties: red, blue and green. To set a color in this class, you indicate values for each of the three properties. You can also express an RGB color as a single hexadecimal value; the RGBColor.hexValue property can contains a string with three pairs of hexadecimal numbers that represent red, blue, and green, in that order.
Creating color objects

The SolidColor class contains a property for each color model (cmyk, rgb, and so on), which contains a color object of that type; for example, the SolidColor.cmyk property contains a CMYKColor object, which in turn contains cyan, magenta, yellow, and black properties. To make a color, create an instance of  SolidColor and set the specific color values in the color properties of the appropriate color-model object.

This example sets a color using the CMYK color class:

// create a solid color array in CMYK color space
var solidColorRef:SolidColor = new SolidColor();
solidColorRef.cmyk.cyan = 20;
solidColorRef.cmyk.magenta = 90;
solidColorRef.cmyk.yellow = 50;
solidColorRef.cmyk.black = 50;

app.foregroundColor = solidColorRef;
Converting colors

Once a color model has been assigned to a SolidColor object, you cannot  reassign a different color model for that object. However, once you have set a color in a particular color space, the other color model properties of the object contain equivalent colors in their own color spaces.

This example converts an RGB color that is being used as the application's foreground color to its CMYK equivalent, by looking in the cmyk property of the original SolidColor object:

var cmykValue:CMYKColor = app.foregroundColor.cmyk;

var newColorRef:SolidColor = new SolidColor();
newColorRef.cmyk = cmykValue;

Use the SolidColor.nearestWebColor property to convert to a web-safe color:

var webSafeColor:RGBColor = new RGBColor();
webSafeColor = app.foregroundColor.nearestWebColor;
Comparing colors

The SolidColor.isEqual() method allows you to compare colors, even if they are in different color spaces. It returns true if the colors are visually equivalent. For example:

if (app.foregroundColor.isEqual(backgroundColor)) {...}


【reference】
优设网的教程 http://www.uisdc.com/manipulate-an-image-with-scripting
adobe bridge的开发套件 http://www.adobe.com/devnet/bridge.html
adobe脚本指南 http://www.adobe.com/devnet/photoshop/scripting.html
-包含各个版本的官方文档，很靠谱，附有少量案例
