# Flutter notları
## Aptal Hatalar
Image için couldn't load asset hatası veriyor. Fakat pubspec.yaml ve assets path'i doğru.
Paketleri silip tekrar yüklememiz lazım:
1. proje klasöründe terminali aç
2. `flutter clean` ve sonrasında `flutter pub get`
3. projeyi tekrar çalıştır.
4. hala düzelmediyse ide'yi kapatıp tekrar aç


Create widgets for all elements in list using map
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
An instance of MyWidget is a StatelessWidget, but in a sense, it is composed of a Column, two Container widgets, and a Placeholder. Through composition, we define what a MyWidget is. Many Flutter widgets, custom and core, are thin wrappers around a bunch of child widgets. This allows a great deal of flexibility and power when constructing an app's UI.

Inheritance relationships are often described as **is-a relationships**; an ArmoredAlienTemplate is an AlienTemplate. Composition relationships are **has-a relationships**; a LiveAlien has an AlienTemplate. Use all of these concepts when constructing your own classes, remembering that each is best for different situations.

<style>
.markdown-body {
  --md-code-background: #e3dcef;
  --md-code-text: #4a2b7b;
  --md-code-tabs: #c6b8dd;
  --md-code-radius: 4px;
</style>