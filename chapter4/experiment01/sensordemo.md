# 实验一. 传感器SensorDemo串口处理软件开发实验

----------
##  实验目的
- 属性JavaFX软件开发流程;
- 掌握Scene Builder设计FXML界面布局流程;
- 熟悉模型 - 视图 - 控制器（MVC）模式构造应用的架构；

##  实验环境
* 硬件：CBT-IOT-CTP实训台,PC机;
* 软件： IntelliJ IDEA，Scene Builder;

##  实验内容
- 设计传感器串口处理界面及功能，实现对实训台传感器设备的数据采集及解析。


##  实验步骤

### 创建一个新的 JavaFX 项目

在IDEA中点击**File | New |Project** 并选择**JavaFX Application**。指定这个项目的名称(**e.g. SensorDemo**)并点击**Finish**。   

之后将sample包删除。

### 创建包

Model-View-Controller (MVC)是一个非常重要的软件设计原则。按照MVC模式可以将我们的应用程序划分成3个部分，
然后为这每一部分建立自己的包 (在源代码文件夹上右键， 选择 新建 | 包):

  - edu.iot.sensordemo - 放置所有的控制器类(也就是应用程序的业务逻辑)
  - edu.iot.sensordemo.model - 放置所有的模型类
  - resources.view - 放置所有界面和控件类   

- 将resources文件夹设置为资源根目录，方法：右键**resources | Make Directory as | Resources Root**。

注意: **view**包里可能会包含一些控制器类，它可以直接被单个的**view**引用，我们叫它**视图-控制器**。




### 创建FXML布局文件   

有两种方式来创建用户界面，一种是通过FXML文件来定义，另外一种就是直接通过java代码来创建. 这两种方式你都可以在网上搜到.
 我们这里将使用FFFXML的方式来创建大部分的界面。因为这种方式将会更好的将你的业务逻辑和你的界面开来，以保持代码的简洁。
 在接下来的内容里，我们将会介绍使用Scene Builder(所见即所得)来编辑我们的XML布局文件，它可以避免我们直接去修改XML文件。

打开**Scene Builder**软，点击**File | New**新建空白FXML文件；

![](/chapter4/experiment01/create_fxml_file.png)


点击**File | Save as**，保存到本地SensorDemo软件工程的view包目录下，把它命名为MainOverview。

### 用Scene Builder来设计你的界面   

> 注意: 你可以下载这部分教程的源码，它里面已经包含了设计好的布局文件。

![](/chapter4/experiment01/main_layout.png)

1. 在左侧**Library**栏中拖拽一个**_ArchorPane_**到中间的工作区域。在左下方的**Hierarchy**选中该组件（或在中间工作区选择），就可以在右边
的属性设置栏中对它的尺寸进行修改：

![](/chapter4/experiment01/Layout_set1.png)

2. 从**Scene Builder**的左边控件栏中拖拽一个 **_Splite Pane(Horizontal Flow)_** 到界面设计区域，在Builder的右边视图结构中选择刚添加的**Pane**，
在弹出的右键菜单中选择 **_Fit to Parent_** 。

![](/chapter4/experiment01/Layout_set2.png)

3. 选中**Split Pane**组件，在右侧**Properties**中将**Divider Positions**属性设置为0.5，使该组件左右两侧比例平分。

![](/chapter4/experiment01/Layout_set3.png)

4. 同样从左边的控件栏中拖拽一个 **VBox** 到 **SplitPane** 的左边，选择这个**VBox**(而不是它的列)对它的布局进行设置，
你可以在 **AnchorPane** 中对这个**VBox**四个边的外边距进行调节。
([more information on Layouts][javafx layout api])。


![](/chapter4/experiment01/Layout_set4.png)

5. 设计串口操作框，同上方法拖拽**HBox**到**VBox**下，将**Layout**设置栏中的**Spacing**属性设置为**10**(表示在**HBox**
组件下的子组件间的间隙值)。
**tips**：可用鼠标点击该属性名称会跳转到该属性的API具体介绍网页界面。继续设计该串口框中的其他组件，如下图所示：

![](/chapter4/experiment01/Layout_set5.png)


6. 基于上述UI组件设置方法设计其他组件，**Hierarchy**框架如下图所示。完成界面设置后，可以通过**_Preview_**来预览该界面，并检查各控件是否设计正确。



![](/chapter4/experiment01/Layout_set6.png)


### 创建主应用程序

#### 创建一个**main java class**用来加载**MainOverview.fxml**,作为这个应用程序的入口。

鼠标右键**edu.iot.sensordemo**包，**New | Java Class**,命名为**MainLauncher**。

继承**Application**方法，按下<kbd>Alt<kbd>+<kbd>Enter<kbd>键，自动补全实现的方法。：




这里最重要的方法是 **start(Stage primaryStage)** ，它将会在应用程序运行时通过内部的 main 方法自动调用。

正如你所看到的，这个**start(...)** 方法会接收一个 **Stage** 类型的参数，下面的图向你展示了一个JavaFX应用程序的基本结构。

![New FXML Document](/chapter4/experiment01/javafx-hierarchy.png)



Image Source: [http://www.oracle.com][oracle]


一切看起来象是剧场里表演: 这里的 **Stage** 是一个主容器，它就是我们通常所认为的窗口（有边，高和宽，还有关闭按钮）。在这个**Stage** 里面，你可以放置一个 **Scene**，
当然你可以切换别的 **Scene**，而在这个 **Scene** 里面，我们就可以放置各种各样的控件。

更详细的信息，你可以参考 [Working with the JavaFX Scene Graph][JavaFX Scene Graph]。

#### 打开**MainLauncher.java**,将已有的代码替换成下面的代码：


```java
 package edu.iot.sensordemo;

import javafx.application.Application;
import javafx.application.Platform;
import javafx.fxml.FXMLLoader;
import javafx.scene.Parent;
import javafx.scene.Scene;
import javafx.scene.image.Image;
import javafx.stage.Stage;
import javafx.stage.StageStyle;

/**
 * Created by LuffyCheung on 2017/2/17.
 */
public class MainLauncher extends Application{

    private static Stage windows;

    @Override
    public void start(Stage primaryStage) throws Exception {
        windows = primaryStage;
        Parent root = FXMLLoader.load(getClass().getClassLoader().getResource("views/MainOverview.fxml"));
        primaryStage.setTitle("SensorDemo : Client version 0.1");

        Scene mainScene = new Scene(root);
        mainScene.setRoot(root);

        primaryStage.setScene(mainScene);
        primaryStage.show();
        
        primaryStage.setOnCloseRequest(e -> Platform.exit());

    }

    public static void main(String[] args) {
        launch(args);
    }

    public static Stage getPrimaryStage() {
        return windows;
    }

}

```

#### 可能遇见的问题

- 如果你的应用程序找不到你所指定的 fxml 布局文件，那么系统会提示以下的错误：   
 
```
java.lang.IllegalStateException: Location is not set.
```

**解决方法**：检查一下你的 fxml 文件名是否拼写错误。
> `如果还是不能工作，请下载这篇教程所对应的源代码，然后将源代码中的fxml文件替换掉你的`

- 如果找不到资源目录，那么系统会提示以下错误：

```
Caused by: java.lang.NullPointerException: Location is required.
```

**解决方法**：右键**resources | Make Directory as | Resources Root**。
若无此选项可打开`Project Structure`设置界面，选择**Modules | SensorDemo | Resources**。
定位到`src`目录下的`resources`文件夹，右键选择`Resources`,点击下方的`Apply`和`OK`按键应用。

![](/chapter4/experiment01/resources_set.png)





### 创建模型类

>- 创建一个模型类。
>- 在界面右侧**AnchorPane->VBox**使用模型类。
>- 使用**Controllers**在**ChoiceBox**中显示数据。


#### 创建模型类

我们需要一个模型类来保存传感器信息。在模型包中(`edu.iot.sensordemo.model`) 添加一个叫`Sensor`的类。
将以下代码添加到类。在代码后，将解释一些 **JavaFX** 的细节。

**Sensor.java**

```
package edu.iot.sensordemo.model;

import javafx.beans.property.BooleanProperty;
import javafx.beans.property.StringProperty;

/**
 * Created by LuffyCheung on 2017/2/17.
 */
public class Sensor {
    private final BooleanProperty ctlFlag; //是否为可控类传感器
    private final StringProperty sensorType;
    private final StringProperty sensorIndex;
    private final  StringProperty frameData;
    private final   StringProperty sensorValue;
    private final   byte[] bytesData;
    private final   byte[] dataOn,dataOff,dataUpper;
    private final   byte bType;
    private final   byte bIndex;


    public Sensor(BooleanProperty ctlFlag, StringProperty sensorType, StringProperty sensorIndex, StringProperty frameData, StringProperty sensorValue, byte[] bytesData, byte[] dataOn, byte[] dataOff, byte[] dataUpper, byte bType, byte bIndex) {
        this.ctlFlag = ctlFlag;
        this.sensorType = sensorType;
        this.sensorIndex = sensorIndex;
        this.frameData = frameData;
        this.sensorValue = sensorValue;
        this.bytesData = bytesData;
        this.dataOn = dataOn;
        this.dataOff = dataOff;
        this.dataUpper = dataUpper;
        this.bType = bType;
        this.bIndex = bIndex;
    }

    public boolean isCtlFlag() {
        return ctlFlag.get();
    }

    public BooleanProperty ctlFlagProperty() {
        return ctlFlag;
    }

    public void setCtlFlag(boolean ctlFlag) {
        this.ctlFlag.set(ctlFlag);
    }

    public String getSensorType() {
        return sensorType.get();
    }

    public StringProperty sensorTypeProperty() {
        return sensorType;
    }

    public void setSensorType(String sensorType) {
        this.sensorType.set(sensorType);
    }

    public String getSensorIndex() {
        return sensorIndex.get();
    }

    public StringProperty sensorIndexProperty() {
        return sensorIndex;
    }

    public void setSensorIndex(String sensorIndex) {
        this.sensorIndex.set(sensorIndex);
    }

    public String getFrameData() {
        return frameData.get();
    }

    public StringProperty frameDataProperty() {
        return frameData;
    }

    public void setFrameData(String frameData) {
        this.frameData.set(frameData);
    }

    public String getSensorValue() {
        return sensorValue.get();
    }

    public StringProperty sensorValueProperty() {
        return sensorValue;
    }

    public void setSensorValue(String sensorValue) {
        this.sensorValue.set(sensorValue);
    }

    public byte[] getBytesData() {
        return bytesData;
    }

    public byte[] getDataOn() {
        return dataOn;
    }

    public byte[] getDataOff() {
        return dataOff;
    }

    public byte[] getDataUpper() {
        return dataUpper;
    }

    public byte getbType() {
        return bType;
    }

    public byte getbIndex() {
        return bIndex;
    }
}
```

**代码说明**

- 在JavaFX中,对一个模型类的所有属性使用 Properties是很常见的。
 打个比方, 当 `sensorType` 或其他属性被改变时自动收到通知, 这有助于我们保持视图与数据的同步，
 阅读 [Using JavaFX Properties and Binding][Using JavaFX Properties and Binding] 学习更多关于 Properties 的内容。




[oracle]: http://www.oracle.com

[JavaFX Scene Graph]: http://docs.oracle.com/javase/8/javafx/scene-graph-tutorial/scenegraph.htm

[javafx layout api]: http://docs.oracle.com/javase/8/javafx/layout-tutorial/builtin_layouts.htm

[Using JavaFX Properties and Binding]: http://docs.oracle.com/javase/8/javafx/properties-binding-tutorial/binding.htm