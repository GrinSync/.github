# Developer Guide
This is a guide to the developers of GrinSync. 

## Table of Contents
- ### Flutter - Creating Pages for the App
- ### Software Architecture
- ### Database Structure
- ### Languages and Frameworks
- ### API Documentation
- ### Coding Guidelines

## Flutter - Creating Pages for the App
Imagine a page in the app as one big widget. We will create this widget and add functionalities to it. 

This is the basic scheme for creating a new page. 

```dart
// Define a new class that extends the StatefulWidget class. This means that the page is a widget
// that can have mutable state.
class NEWPage extends StatefulWidget {
  const NEWPage({super.key}); // Define a constructor
  // Specify that when a new instance of the page is created, it should create a corresponding state
  // object of a new type that is defined below
  // (which will be how we manage the state of the widget of the page)
  @override 
  State<NEWPage> createState() => _NEWPage();
}

// Define a state class to manage the state of the widget 
class _NEWPage extends State<NEWPage> {
  // Variables that need to be stored for this page
  // late indicates that the variables are initialized later on
  late final TextEditingController ...;
  late final String ...;
  late List<String> ...;

  /// Initialize variables defined above here
  @override
  void initState() {
    ... 
    super.initState();
  }

  /// Dispose of the variable defined above when the page closes.
  @override
  void dispose() {
    ...
    super.dispose();
  }

  /// Here is where we build the page interface
  @override
  Widget build(BuildContext context) {
    return Scaffold(...)
  }
```
The widgets that make up the page UI/interface go within the Scaffold call. We suggest going through this tutorial to learn how to add UI elements: https://codelabs.developers.google.com/codelabs/flutter-codelab-first?hl=en#0 
