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
Rectangle    {
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
