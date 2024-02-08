# LearningQML

One can learn a lot from QML seeing the examples from QT createor.
source : Udemy : QML For Beginners

# Sample Code

## C++ Backend
```
QGuiApplication app(argc, argv);
QQmlApplicationEngine engine;
const QUrl url(u"qrc:/buildq-2-8/Main.qml"_qs);
QObject::connect(
    &engine,
    &QQmlApplicationEngine::objectCreationFailed,
    &app,
    []() { QCoreApplication::exit(-1); },
    Qt::QueuedConnection);

engine.load(url);
return app.exec();
```


**QGuiApplication** : It is a subclass of QCoreApplication and is helpful in integrating QT and QML.
**QMLApplicationEngine** : It serves as an engine for loading and running QML-based applications.
**QObject::connect** : objectCreationFailed singal is connected with exit slot.

Finally we are loading the engine.

## QML Code

```
import QtQuick 2.0

Window {
    visible: true
    height: 200
    width: 200

    Rectangle {
        id: page
        width: 100; height: 100
        color: "lightgray"
        anchors.centerIn: parent

        Text {
            id: helloText
            text: "Hello world!"
            anchors.centerIn: page
            font.pointSize: 10;
        }
    }
}
```
NOTE : 
1. height and width values are device independent units, sometimes measured in DIPs.
2. Window is needed in case you are developing a standalone application else in case of component level design you do not need it.

# Basics
1. width : parent.width is a **property binding**
2. The component at the top is parent
3. QT uses parent child memory hirerchy for memory management, so if parent is destroyed, all the children are destroyed. That's why multiple parent thing does't work

## Signal and Slots in QML
```
Window{
    visible: true
    height: 200
    width: 200
    TextInput    {
        id : editText
        text: "Default Text"
        anchors.centerIn: parent
    }
    Text {
        id: nameText
        text: editText.text
        font.pixelSize: 25
    }
}
```

In the code above text of id nameText is binded with text of TextInput using *Property Binding* . Changes mades in exitText will reflect in nameText. This behaves similar to *signal and slots* .

## X,Y, Z Positioning

x,y,z (0,0,0) by default is the top left corner.
Higher x means downward, higher y means towards right. 
If z is not specified then the compenent definer first gets to be on top.

```
Window{
    visible: true
    height: 200
    width: 200

    Rectangle    {
        id : rect1
        width: 100
        height : 100
        color : "red"
        x : 50          //Here we are positioning the rectangle corder to 50,50
        y : 50
        opacity: 0.5
        z: 1            //Higher z value gets to be on top
    }

    Rectangle    {
        id : rect2
        width: 100
        height : 100
        color : "blue"
        x : 100         //Here we are positioning the rectangle corner to 150,150
        y : 100
        opacity: 0.5
        z : 0
    }
}
```

NOTE:
If there is a parent child relationship between components then the x,y,z would be relative to that parent and not the super parent window.

## TapHandler

It is a QML Type, just like rectangle and handles taps and cliks.
source of QML types : https://doc.qt.io/qt-6/qtquick-qmlmodule.html

```
Rectangle  {
    id : rect1
    width: 100
    height : 100
    color : tapHandleText.pressed ? "red" : "blue"
    opacity: tapHandleText.pressed ? 0.5 : 1
    
    TapHandler{
        id : tapHandleText
    }
}
```

# Basic Types
## Item Type
Item type is a basic type for all visual types but has no visual appearance for itself.
It has all basic features like anchoring, x,y,z etc whatever we have been playing with so far.

It can be said as container of other QML objects. E.g. can contain multiple Image QML types.

>When to use

It is helpful in making our own **custom object**.
Item is useful for grouping items together. You can use Item to create an invisible container to hold other components. 
*So it facilitates handling children and processing properties such as width and height.*

```
Window{
    visible: true
    height: 200
    width: 200

    Item {
        id: myCustomObject
        width: 100
        height: 100
        anchors.centerIn: parent
        //color: "red"          //You cannot set color as it has no visual appearance

        Rectangle
        {
            id : myrect
            color: "red"
            anchors.fill: parent
        }
    }
}
```
In the code above we have made our custom object using Item and filled with Rectangle.

## Component

The component encapsulates the QML types within, as if they were defined in a separate QML file, and is not loaded until requested (in this case, by the two Loader objects). Because Component is not derived from Item, you cannot anchor anything to it.

When declaring delegates, Component is used because there are several delegate **items** that must be created. A single Item doesn't work here. You can think of Component as a template that you can create objects from.

As a QML concept, all reusable things are called components. You can define a component in multiple ways, but one easy way is to create a .qml file and name it as you name your component. i.e: Button.qml or Switch.qml. When the QML engine loads that file, you can use it as buttons or switches.

Another way to define a component is to use Component {} in a .qml file. This allows you to define a new component inline. 

```
Item {
    id: itemWidget

    Rectangle { id: one }
    Rectangle { id: two }
}
```
```
Component {
    id: componentWidget

    Rectangle { id: one }
    Rectangle { id: two }
}
```

## Rectangle 
Let's create a circle from a Rectangle QML type

```
Rectangle
    {
        id : myrect
        width: 100
        height: 100
        color: "red"
        anchors.centerIn: parent
        radius: width            //This is the one that makes the rectangle a circle

        gradient : Gradient
        {
            GradientStop {position : 0.0; color : "red"}
            GradientStop {position : 1.0; color : "blue"}
        }
    }
```

Note : If we simply make *Gradient{}* object and not use *gradient:Gradient{}* then it won't show the result. *gradient* is a property of Rectangle and it needs to be populated with *Gradient{}* object.

## Image
qrc : QT resource collection file.
Everything in qrc file get compiled right into the binary executable.

```
Image {
        id: myImage
        source: "Images/smile"

        height: 100
        fillMode: Image.PreserveAspectFit
    }
```

PreserveAspectFit will keep the width value adjusted if height is already defined so that the original aspect ratio is maintained.

```
import QtQuick 2.12


Window{
    visible: true
    height: 200
    width: 200
    title: "Hello World"
    property int middle : height / 2  

Image {
        id: myImage
        source: "Images/smile"
        height: 100
        fillMode: Image.PreserveAspectFit
        
        //positioning the image
        x : 100
        y : middle
    }
     
    Image {
        id: remoteImage
        source: "https://someonlineimage.svg"
        height: 100
        fillMode: Image.PreserveAspectFit
        
        //positioning the image
        x : 300
        y : middle 
         
        //Online image may take time to Loader
        //Let's track its progress via javascript

        onProgressChanged: console.log(remoteImage.progress)
        onStatusChanged: if(remoteImage.status == Image.Ready) console.log("Remote image was loaded")
    }
}
```

## Text

```
   Text {
        id: myText
        anchors.centerIn: parent
        font.pixelSize: 20

        //You can insert html in the text

        text: ("<html> <b> Hello </b> <i> <font color = 'green'> World </font> </i> </html> <br>
                    <a href='http://www.google.com'> web link </a>")


        onLinkActivated:
        {
            Qt.openUrlExternally(link) //If we don't put this then link wouldn't be clikable
        }

        onLinkHovered:
        {
            console.log("Hovering over : " + link)

            if(link)
                myText.font.bold = true
            else
                myText.font.bold = false
        }
    }
```

> Singals used here:

If we check in QT documentation of Text : https://doc.qt.io/qt-6/qml-qtquick-text.html
we would find signals like :
```
lineLaidOut(object line)
linkActivated(string link)
linkHovered(string link)
```
These signals are always emitted on making Text {} qml object in a qml file.

To access these in a a slot we use keyworks like 
```
onLineLaidOut : {}
onLinkActivated : {}
onLinkHovered : {}
```
withing {} we add *javascript* code
