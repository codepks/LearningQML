# LearningQML

One can learn a lot from QML seeing the examples from QT createor.

# C++ Backend

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

