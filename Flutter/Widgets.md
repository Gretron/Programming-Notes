# Widgets

## Stateless & Stateful Widgets

### 1. Stateless Widgets

To make a StatelessWidget, i.e. an app root widget, use the following example:

```dart
import 'package:flutter/material.dart';

void main() => runApp(App());

class App extends StatelessWidget {
    @override
    Widget build(BuildContext context) {
        return MaterialApp(home: Text('Hello World!'));
    }
}
```

The only problem with a StatelessWidget is that it does not re-run the 'build()' function upon data manipulation, see following example:

```dart
import 'package:flutter/material.dart';

void main() => runApp(App());

class App extends StatelessWidget {
   	List<String> words = [ 
        'Hello', 
        'World',
    ];
    
    int index = 0;
    
    @override
    Widget build(BuildContext context) {
        return MaterialApp(home: Scaffold(
            body: Column(
                children: [
                    Text(words[index]), // Not updated
                    ElevatedButton(
                        onPressed: () {
                            index++; // Does nothing visually
                        },
                        child: Text('Increase'),
                    ),
                ],
            ),
        ));
    }
}
```



### 2. Stateful Widgets

To make a StatefulWidget, use the following example:

```dart
import 'package:flutter/material.dart';

void main() => runApp(App());

class App extends StatelessWidget {
    @override
    State<StatefulWidget> createState() {
        return AppState();
    }
}

class AppState extends State<App> {
    List<String> words = [ 
        'Hello', 
        'World',
    ];
    
    int index = 0;
    
    @override
    Widget build(BuildContext context) {
        return MaterialApp(home: Scaffold(
            body: Column(
                children: [
                    Text(words[index]), // Not updated
                    ElevatedButton(
                        onPressed: () {
                            index++; // Does nothing visuall
                        },
                        child: Text('Increase'),
                    ),
                ],
            ),
        ));
    }
}
```

To visually update the values, we must call 'setState()' and pass in a function, use following example:

```dart
import 'package:flutter/material.dart';

void main() => runApp(App());

class App extends StatelessWidget {
    @override
    State<StatefulWidget> createState() {
        return AppState();
    }
}

class AppState extends State<App> {
    List<String> words = [ 
        'Hello', 
        'World',
    ];
    
    int index = 0;
    
    @override
    Widget build(BuildContext context) {
        return MaterialApp(home: Scaffold(
            body: Column(
                children: [
                    Text(words[index]), // Updated
                    ElevatedButton(
                        onPressed: () {
                            setState(() { // Calls 'build()'
                               index++;
                            }); 
                        },
                        child: Text('Increase'),
                    ),
                ],
            ),
        ));
    }
}
```

