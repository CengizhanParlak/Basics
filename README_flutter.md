# Flutter notları
### Aptal Hatalar
- Image için couldn't load asset hatası veriyor. Fakat pubspec.yaml ve assets path'i doğru.
Paketleri silip tekrar yüklememiz lazım:
  1. proje klasöründe terminali aç
  2. `flutter clean` ve sonrasında `flutter pub get`
  3. projeyi tekrar çalıştır.
  4. hala düzelmediyse ide'yi kapatıp tekrar aç
  
- Create widgets for all elements in list using map

  <details class="markdown-body">

  <summary>Click for code!</summary>


  ```dart
  final messages = <Message>[
    Message(text: "Message 1"),
    Message(text: "Message 2"),
    ImageMessage(
      text: "Message 3",
      imageUrl: "https://example.com/image1.jpg",
    ),
  ];

  class MessageList extends StatelessWidget {
    final List <Message>; messages;

    const MessageList({Key key, @required this.messages}) :
      super(key: key);

    @override
    Widget build(BuildContext context) {
      return Column(
        children: messages.map((Message msg) {
          final text = Text(msg.text);

          if (msg is ImageMessage) {
            return Row(
              children: [
                Image.network(msg.imageUrl),
                text,
              ],
            );
          }

          return text;
        }).toList(),
      );
    }
  }
  ```

  </details>


### Composition in Flutter
- An instance of MyWidget is a StatelessWidget, but in a sense, it is composed of a Column, two Container widgets, and a Placeholder. Through composition, we define what a MyWidget is. Many Flutter widgets, custom and core, are thin wrappers around a bunch of child widgets. This allows a great deal of flexibility and power when constructing an app's UI.

- Inheritance relationships are often described as **is-a relationships**; an ArmoredAlienTemplate is an AlienTemplate. Composition relationships are **has-a relationships**; a LiveAlien has an AlienTemplate. Use all of these concepts when constructing your own classes, remembering that each is best for different situations.

### Composition over Inheritance
- *Flutter uses composition over inheritance.* [stackoverflow](https://stackoverflow.com/questions/51476234/subclass-a-class-that-extends-statelesswidget-or-statefulwidget-class#:~:text=23-,As%20stated%20by%20Gunter,-%2C%20flutter%20uses%20composition)
- *This means that instead of extending a Widget, you should create a smaller one and then reuse it. A practical example would be a base button :*
  ```dart
  class MyButton extends StatelessWidget {
    final Color color;

    MyButton({this.color = Colors.grey, Key key}): super(key: key);

    @override
    Widget build(BuildContext context) {
      return Container(
        color: color,
        child: Text("My Button"),
      );
    }
  }
  ```
- *Then reused using composition to create a more specific type of button :*
  ```dart
  class OutlineButton extends StatelessWidget {
    final Color color;

    OutlineButton({this.color = Colors.grey, Key key}) : super(key: key);

    @override
    Widget build(BuildContext context) {
      return DecoratedBox(
        decoration: BoxDecoration(
          border: Border.all(
            color: color,
            width: 2.0,
            style: BorderStyle.solid,
          ),
        ),
        child: MyButton(    //composing other widgets to current one
          color: color,
        ),
      );
    }
  }
  ```

### Services in Flutter
- *Services are just normal Dart classes. Don’t think of them in the Android sense of the word, that is, a long-running background task. They are just classes that you write to do some specialized task in your app. You don’t even have to call them services. I’m just calling them that because that’s what FilledStacks calls them. You could call them helpers or repositories, but services is a good name and it matches the idea of microservices. Instead of being spread out across the network, though, they are contained within your app.* [medium article](https://medium.com/flutter-community/creating-services-to-do-the-work-in-your-flutter-app-93d6c4aa7697#:~:text=Services%20are%20just,within%20your%20app.)
- *Here are a few common examples you might write a service to handle:*
    - Read and write to local storage (database, shared preferences, files)
    - Access a web API
    - Log in a user
    - Perform some heavy calculation
    - Wrap Firebase or some other third-party package

### GlobalKeys in Flutter
- *In Flutter GlobalKeys can be used to access the state of a StatefulWidget and that's what we'll use to access the NavigatorState outside of the build context. We'll create a NavigationService that contains the global key, we'll set that key on initialization and we'll expose a function on the service to navigate given a name. Let's start with the NavigationService.*

### Inherited Widget and good practice

- *Use const to build your widgets* [medium | mehmet fidanboylu]([https://link](https://medium.com/@mehmetf_71205/inheriting-widgets-b7ac56dbbeb1#:~:text=It%20is%20important%20to%20note%3A))

### Awesome widgets

- Snackbar WillPopScope

  ```dart
  DateTime? currentBackPressTime;

  Future<bool> _onWillPop(BuildContext context) {
  var snackBar = SnackBar(
    content: const Text('Awesome Snackbar!'),
    action: SnackBarAction(
      label: 'Action',
      onPressed: () {
        // Code to execute.
      },
    ),
  );
  DateTime now = DateTime.now();
  if (currentBackPressTime == null ||
      now.difference(currentBackPressTime!) > Duration(seconds: 2)) {
    currentBackPressTime = now;
    ScaffoldMessenger.of(context).showSnackBar(snackBar);

    return Future.value(false);
  }
  return Future.value(true);
  }
  ```
