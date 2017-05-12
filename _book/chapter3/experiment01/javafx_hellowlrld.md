# 实验一. Hello World入门实验

-------

## 实验目的

- 掌握JavaFX应用程序编程流程;
- 掌握通过CSS来美好UI界面的方法;

## 实验环境

- 硬件：CBT-IOT-CTP 实验平台,PC机;
- 软件： IntelliJ IDEA ,JDK,Scene Builder;

## 实验内容

- 创建JavaFX示例应用程序

- 重命名控制类

- 设计用户界面

- 编写控制类

- 运行应用程序

- 使用CSS来美化界面

## 实验步骤

### 创建JavaFX示例应用程序

1. 从”File”菜单中选择”New Project”。

2. 在”JavaFX”应用程序分类中，选择”JavaFX Application”，单击”Next”按钮。

3. 将Project命名为”HelloWorld”并单击”Finish”按钮。

IDE将会打开Controller.java文件，点击左侧的Main.java。并使用基本的应用程序代码来填充其内容，如下所示。

```
package sample;

import javafx.application.Application;
import javafx.fxml.FXMLLoader;
import javafx.scene.Parent;
import javafx.scene.Scene;
import javafx.stage.Stage;

public class Main extends Application {

    @Override
    public void start(Stage primaryStage) throws Exception{
        Parent root = FXMLLoader.load(getClass().getResource("sample.fxml"));
        primaryStage.setTitle("Hello World");
        primaryStage.setScene(new Scene(root, 300, 275));
        primaryStage.show();
    }


    public static void main(String[] args) {
        launch(args);
    }
}

```

下面是理解JavaFX应用程序基本结构需要了解的一些重点：

- JavaFX应用程序的主类需要继承自application.Application类。start()方法是所有JavaFX应用程序的入口。

- JavaFX应用程序将UI容器定义为舞台(Stage)与场景(Scene)。Stage类是JavaFX顶级容器。Scene类是所有内容的容器。例4-1中创建了Stage和Scene，然后为Scene设置了大小并使其可见。

- 在JavaFX中，Scene中的内容会以由图形节点(Node)构成的分层场景图(Scene Graph)来展现。在本例中，root节点是一个StackPane对象，它是一个可以调整大小的layout节点。这就意味着在用户改变stage大小时，root节点可以随scene的大小变化而变化。

- root节点包含一个带文本的按钮子节点，按钮上添加了一个事件处理器(Event Handler)，它在点击按钮时会向控制台输出信息。

- 当JavaFX应用程序是通过JavaFX Packager工具打包时，main()方法就不是必需的的了，因为JavaFX Package工具会将JavaFX Launcher嵌入到JAR文件中。但是保留main()方法还是很有用的，这样你可以运行不带有JavaFX Launcher的JAR文件，例如在使用某些没有将JavaFX工具完全集成进去的IDE时。另外嵌入了JavaFX代码的Swing应用程序仍需要main()方法。

下图展示了Hello World应用程序的场景图(Scene Graph)。如果想了解关于Scene Graph的信息，请参考文档：《[使用JavaFX Scene Graph(Working with the JavaFX Scene Graph)](http://docs.oracle.com/javase/8/javafx/get-started-tutorial/get_start_apps.htm)》。


### 重命名Controller控制类

1. 将鼠标指针