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
