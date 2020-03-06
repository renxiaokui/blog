---
title: 抽象工厂模式
date: 2019-12-25 11:22:35
categories: 设计模式
tags:
	- 设计模式
	- 创建型模式
---

## 设计模式之抽象工厂模式
#### 产品等级结构与产品族
&emsp;&emsp;工厂方法模式通过引入工厂等级结构，解决了简单工厂模式中工厂类职责太重的问题，但由于工厂方法模式中的每个工厂只生产一类产品，可能会导致系统中存在大量的工厂类，势必会增加系统的开销。此时，我们可以考虑将一些相关的产品组成一个**“产品族”**，由同一个工厂来统一生产，这就是抽象工厂模式的基本思想。如一个电器工厂，它可以生产电视机、电冰箱、空调等多种电器，而不是只生产某一种电器。先引入两个概念：
- **产品等级结构**：产品等级结构即产品的继承结构，如一个抽象类是电视机，其子类有海尔电视机、海信电视机、TCL电视机，则抽象电视机与具体品牌的电视机之间构成了一个产品等级结构，抽象电视机是父类，而具体品牌的电视机是其子类。
- **产品族**：在抽象工厂模式中，产品族是指由同一个工厂生产的，位于不同产品等级结构中的一组产品，如海尔电器工厂生产的海尔电视机、海尔电冰箱，海尔电视机位于电视机产品等级结构中，海尔电冰箱位于电冰箱产品等级结构中，海尔电视机、海尔电冰箱构成了一个产品族。
![alt](/images/abstract-factory1.jpg)

### 定义
&emsp;&emsp;**抽象工厂模式(Abstract Factory Pattern)：提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类。抽象工厂模式又称为Kit模式，它是一种*对象创建型模式*。**
<!--more-->
### 了解
&emsp;&emsp;当系统所提供的工厂生产的具体产品并不是一个简单的对象，而是多个位于不同产品等级结构、属于不同类型的具体产品时就可以使用抽象工厂模式。抽象工厂模式是所有形式的工厂模式中最为抽象和最具一般性的一种形式。抽象工厂模式与工厂方法模式最大的区别在于，工厂方法模式针对的是一个产品等级结构，而抽象工厂模式需要面对多个产品等级结构，一个工厂等级结构可以负责多个不同产品等级结构中的产品对象的创建。**当一个工厂等级结构可以创建出分属于不同产品等级结构的一个产品族中的所有对象时，抽象工厂模式比工厂方法模式更为简单、更有效率。**
![alt](/images/abstract-factory3.jpg)

&emsp;&emsp;在上图中可以发现，每一个具体工厂可以生产属于一个产品族的所有产品，例如生产颜色相同的正方形、圆形和椭圆形，所生产的产品又位于不同的产品等级结构中。如果使用工厂方法模式，上图所示结构需要提供15个具体工厂，而使用抽象工厂模式只需要提供5个具体工厂，极大减少了系统中类的个数。

<!--more-->
### 结构图
在抽象工厂模式中，每一个具体工厂都提供了多个工厂方法用于产生多种不同类型的产品，这些产品构成了一个产品族
![alt](/images/abstract-factory4.jpg)

1. **AbstractFactory**（抽象工厂）：它声明了一组用于创建一族产品的方法，每一个方法对应一种产品。
2. **ConcreteFactory**（具体工厂）：它实现了在抽象工厂中声明的创建产品的方法，生成一组具体产品，这些产品构成了一个产品族，每一个产品都位于某个产品等级结构中。
3. **AbstractProduct**（抽象产品）：它为每种产品声明接口，在抽象产品中声明了产品所具有的业务方法。
4. **ConcreteProduct**（具体产品）：它定义具体工厂生产的具体产品对象，实现抽象产品接口中声明的业务方法。

### 重载的工厂方法
&emsp;&emsp;与工厂方法模式一样，抽象工厂模式也可为每一种产品提供一组重载的工厂方法，以不同的方式对产品对象进行创建。

### 示例
![alt](/images/abstract-factory5.jpg)

如果需要增加新的皮肤，只需增加一族新的具体组件并对应提供一个新的具体工厂，修改配置文件即可使用新的皮肤，原有代码无须修改，符合“开闭原则”

### 开闭原则的倾斜性
&emsp;&emsp;在抽象工厂模式中，增加新的产品族很方便，但是增加新的产品等级结构很麻烦，抽象工厂模式的这种性质称为“开闭原则”的倾斜性。
&emsp;&emsp;正因为抽象工厂模式存在“开闭原则”的倾斜性，它以一种倾斜的方式来满足“开闭原则”，为增加新产品族提供方便，但不能为增加新产品结构提供这样的方便，因此要求设计人员在设计之初就能够**全面考虑**，不会在设计完成之后向系统中增加新的产品等级结构，也不会删除已有的产品等级结构，否则将会导致系统出现较大的修改，为后续维护工作带来诸多麻烦。

### 总结
&emsp;&emsp;抽象工厂模式是工厂方法模式的进一步延伸，由于它提供了功能更为强大的工厂类并且具备较好的可扩展性，在软件开发中得以广泛应用，尤其是在一些框架和API类库的设计中，例如在Java语言的AWT（抽象窗口工具包）中就使用了抽象工厂模式，它使用抽象工厂模式来实现在不同的操作系统中应用程序呈现与所在操作系统一致的外观界面。抽象工厂模式也是在软件开发中最常用的设计模式之一。
#### 优点
1.抽象工厂模式隔离了具体类的生成，使得客户并不需要知道什么被创建。由于这种隔离，更换一个具体工厂就变得相对容易，所有的具体工厂都实现了抽象工厂中定义的那些公共接口，因此只需改变具体工厂的实例，就可以在某种程度上改变整个软件系统的行为。
2. 当一个产品族中的多个对象被设计成一起工作时，它能够保证客户端始终只使用同一个产品族中的对象。
3. 增加新的产品族很方便，无须修改已有系统，符合“开闭原则”。

#### 缺点
1. 增加新的产品等级结构麻烦，需要对原有系统进行较大的修改，甚至需要修改抽象层代码，这显然会带来较大的不便，违背了“开闭原则”。

#### 适用场景
1. 一个系统不应当依赖于产品类实例如何被创建、组合和表达的细节，这对于所有类型的工厂模式都是很重要的，用户无须关心对象的创建过程，将对象的创建和使用解耦。
2. 系统中有多于一个的产品族，而每次只使用其中某一产品族。可以通过配置文件等方式来使得用户可以动态改变产品族，也可以很方便地增加新的产品族。
3. 属于同一个产品族的产品将在一起使用，这一约束必须在系统的设计中体现出来。同一个产品族中的产品可以是没有任何关系的对象，但是它们都具有一些共同的约束，如同一操作系统下的按钮和文本框，按钮与文本框之间没有直接关系，但它们都是属于某一操作系统的，此时具有一个共同的约束条件：操作系统的类型。
4. 产品等级结构稳定，设计完成之后，不会向系统中增加新的产品等级结构或者删除已有的产品等级结构。

### code
```java
import javafx.scene.control.Skin;

/**
 * @author Wangzy
 * @Description: 启动类
 * @date 17:45 2019/12/12
 */
public class Boot {

    public static void main(String[] args) {
//        SkinFactory skinFactory = new SpringSkinFactory();
        // 通过配置文件方式实现符合“开闭原则”的代码
        SkinFactory skinFactory = (SkinFactory) XMLUtil.getBean();
        Button button = skinFactory.createButton();
        TextField textField = skinFactory.createTextField();
        ComboBox comboBox = skinFactory.createComboBox();
        button.display();
        textField.display();
        comboBox.display();
    }
}
```
```java
/**
 * @author Wangzy
 * @Description: 按钮产品抽象类
 * @date 17:32 2019/12/12
 */
public interface Button {

    /**
     * @Description: 显示
     * @author Wangzy
     * @date 17:33 2019/12/12
     */
    void display();
}

/**
 * @author Wangzy
 * @Description: 复合文本框产品抽象类
 * @date 17:34 2019/12/12
 */
public interface ComboBox {

    /**
     * @Description: 显示
     * @author Wangzy
     * @date 17:35 2019/12/12
     */
    void display();
}

/**
 * @author Wangzy
 * @Description: 文本框产品抽象类
 * @date 17:33 2019/12/12
 */
public interface TextField {

    /**
     * @Description: 显示
     * @author Wangzy
     * @date 17:34 2019/12/12
     */
    void display();
}
```
```java
/**
 * @author Wangzy
 * @Description: 春天按钮
 * @date 17:39 2019/12/12
 */
public class SpringButton implements Button {

    @Override
    public void display() {
        System.out.println("spring button display！");
    }
}

/**
 * @author Wangzy
 * @Description: 春天复合文本框
 * @date 17:40 2019/12/12
 */
public class SpringComboBox implements ComboBox {

    @Override
    public void display() {
        System.out.println("spring combobox display！");
    }
}

/**
 * @author Wangzy
 * @Description: 春天文本框
 * @date 17:39 2019/12/12
 */
public class SpringTextField implements TextField {

    @Override
    public void display() {
        System.out.println("spring textfield display！");
    }
}

/**
 * @author Wangzy
 * @Description: 夏天按钮
 * @date 17:39 2019/12/12
 */
public class SummerButton implements Button {

    @Override
    public void display() {
        System.out.println("summer button display！");
    }
}

/**
 * @author Wangzy
 * @Description: 夏天复合文本框
 * @date 17:40 2019/12/12
 */
public class SummerComboBox implements ComboBox {

    @Override
    public void display() {
        System.out.println("summer combobox display！");
    }
}

/**
 * @author Wangzy
 * @Description: 夏天文本框
 * @date 17:39 2019/12/12
 */
public class SummerTextField implements TextField {

    @Override
    public void display() {
        System.out.println("summer textfield display！");
    }
}
```
```java
/**
 * @author Wangzy
 * @Description: 春天界面工厂
 * @date 17:36 2019/12/12
 */
public class SpringSkinFactory implements SkinFactory {

    @Override
    public Button createButton() {
        System.out.println("create spring button！");
        return new SpringButton();
    }

    @Override
    public TextField createTextField() {
        System.out.println("create spring textfield！");
        return new SpringTextField();
    }

    @Override
    public ComboBox createComboBox() {
        System.out.println("create spring combobox！");
        return new SpringComboBox();
    }
}

/**
 * @author Wangzy
 * @Description: 夏天风格产品工厂
 * @date 17:42 2019/12/12
 */
public class SummerSkinFactory implements SkinFactory {

    @Override
    public Button createButton() {
        System.out.println("create summer button！");
        return new SummerButton();
    }

    @Override
    public TextField createTextField() {
        System.out.println("create summer textfield！");
        return new SummerTextField();
    }

    @Override
    public ComboBox createComboBox() {
        System.out.println("create summer combobox！");
        return new SummerComboBox();
    }
}
```
```java
/**
 * @author Wangzy
 * @Description: 抽象界面工厂
 * @date 17:28 2019/12/12
 */
public interface SkinFactory {

    /**
     * @Description: 创建按钮
     * @author Wangzy
     * @date 17:30 2019/12/12
     */
    Button createButton();

    /**
     * @Description: 创建文本框
     * @author Wangzy
     * @date 17:30 2019/12/12
     */
    TextField createTextField();

    /**
     * @Description: 创建复合文本框
     * @author Wangzy
     * @date 17:31 2019/12/12
     */
    ComboBox createComboBox();
}
```
```java
import javax.xml.parsers.*;

import org.w3c.dom.*;
import org.xml.sax.SAXException;

import java.io.*;

/**
 * @author Wangzy
 * @Description: xml工具类
 * @date 17:49 2019/12/12
 */
public class XMLUtil {

    /**
     * @Description: 该方法用于从XML配置文件中提取具体类类名，并返回一个实例对象
     * @author Wangzy
     * @date 17:50 2019/12/12
     */
    public static Object getBean() {
        try {
            // 创建文档对象
            DocumentBuilderFactory dFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = dFactory.newDocumentBuilder();
            Document doc;
            doc = builder.parse(new File("src/config.xml"));

            // 获取包含类名的文本节点
            NodeList nl = doc.getElementsByTagName("className");
            Node classNode = nl.item(0).getFirstChild();
            String cName = classNode.getNodeValue();

            // 通过类名生成实例对象并将其返回
            Class c = Class.forName(cName);
            Object obj = c.newInstance();
            return obj;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

}
```
```xml
<?xml version="1.0"?>
<config>
    <className>SpringSkinFactory</className>
</config>
```

### 相关资料
https://blog.csdn.net/lovelion/article/details/9319181
https://blog.csdn.net/lovelion/article/details/9319323
https://blog.csdn.net/lovelion/article/details/9319423
https://blog.csdn.net/lovelion/article/details/9319481
https://blog.csdn.net/lovelion/article/details/9319571
