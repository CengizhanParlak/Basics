# Flutter notları


- [Flutter notları](#flutter-notları)
  - [Kaynaklar](#kaynaklar)
  - [Hatalar](#hatalar)
  - [Structure Related (Design Patterns, Class compositions, Inheritance, Page and App Structure)](#structure-related-design-patterns-class-compositions-inheritance-page-and-app-structure)
    - [Composition in Flutter](#composition-in-flutter)
    - [Composition over Inheritance](#composition-over-inheritance)
    - [Services in Flutter](#services-in-flutter)
  - [Navigation, Routing, Passing Data to Other Pages](#navigation-routing-passing-data-to-other-pages)
    - [GlobalKeys in Flutter (used in Routing)](#globalkeys-in-flutter-used-in-routing)
    - [Inherited Widgets and good practices while building Custom Widgets](#inherited-widgets-and-good-practices-while-building-custom-widgets)
    - [Awesome widgets](#awesome-widgets)
    - [Production and Uploading to a Store](#production-and-uploading-to-a-store)
  - [Database (NoSQL, Firebase) and Backend related](#database-nosql-firebase-and-backend-related)
    - [Introduction](#introduction)
    - [Considerations](#considerations)
      - [Deleting a document does not delete its subcollections](#deleting-a-document-does-not-delete-its-subcollections)
      - [Delete collection by deleting individual documents](#delete-collection-by-deleting-individual-documents)
    - [Document deletion limit: max 20K/day on free Spark plan](#document-deletion-limit-max-20kday-on-free-spark-plan)
    - [Cannot query across subcollections](#cannot-query-across-subcollections)
    - [Data nesting can be up to 100 levels deep](#data-nesting-can-be-up-to-100-levels-deep)
    - [Choosing data structure - nested data in document vs. subcollections vs.  root-level collections](#choosing-data-structure---nested-data-in-document-vs-subcollections-vs--root-level-collections)
    - [No automatic ordering by creation date](#no-automatic-ordering-by-creation-date)
    - [Transactions](#transactions)

## Kaynaklar

[TODO]: add resources

## Hatalar

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

## Structure Related (Design Patterns, Class compositions, Inheritance, Page and App Structure)

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

## Navigation, Routing, Passing Data to Other Pages

### GlobalKeys in Flutter (used in Routing)

- *In Flutter GlobalKeys can be used to access the state of a StatefulWidget and that's what we'll use to access the NavigatorState outside of the build context. We'll create a NavigationService that contains the global key, we'll set that key on initialization and we'll expose a function on the service to navigate given a name. Let's start with the NavigationService.*

  ```dart
  final GlobalKey<NavigatorState> _searchScreenKey = GlobalKey<NavigatorState>();
  final GlobalKey<NavigatorState> _dietScreenKey = GlobalKey<NavigatorState>();
  final GlobalKey<NavigatorState> _savedRecipeScreenKey =
      GlobalKey<NavigatorState>();
  final GlobalKey<NavigatorState> _profileScreenKey = GlobalKey<NavigatorState>();
  DateTime? currentBackPressTime;

  class MainScreen extends StatefulWidget {
  const MainScreen({Key? key}) : super(key: key);
  @override
  _MainScreenState createState() => _MainScreenState();
  }

  class _MainScreenState extends State<MainScreen>{
  late final int _indexOfDietScreen;
  late GlobalKey<NavigatorState> _currentKey;

  final List<GlobalKey<NavigatorState>> _keys = <GlobalKey<NavigatorState>>[
    _searchScreenKey,
    _dietScreenKey,
    _savedRecipeScreenKey,
    _profileScreenKey,
  ];

  void _onItemTapped(int index) { //bottom navigation barlardaki onTap fonksiyonu
    setState(() {
      _currentKey = _keys.elementAt(_selectedIndex);
    });
  }

  @override
  void initState(){
    super.initState();
    _indexOfDietScreen = _keys.indexOf(_dietScreenKey);
    _currentKey = _keys.elementAt(_indexOfDietScreen);
    
  }
  }

  Future<bool> _onWillPop() {
    // print('current context: ${Navigator.of(_currentKey.currentContext!)}'); //* şu anki context bilgisi
    if (Navigator.of(_currentKey.currentContext!).canPop()) {
      Navigator.of(_currentKey.currentContext!).pop();
      return Future.value(false);
    } else {
      now = DateTime.now();
      if (currentBackPressTime == null ||
          now.difference(currentBackPressTime!) > Duration(seconds: 2)) {
        currentBackPressTime = now;
        ScaffoldMessenger.of(context)
            .showSnackBar(customSnackBar("Çıkmak için tekrar geri basın"));
        return Future.value(false);
      }
      return Future.value(true);
    }
  }

  @override
  Widget build (buildContext context){
    return WillPopScope(
      onWillPop: _onWillPop,
      child: Scaffold(
        //body: stack->offstage->tickermode->materialApp-><customPages>
        //bottomNavigationBar: bottomnavigationbar->onTap: _onItemTapped,
      ),
    );
  }
  ```

### Inherited Widgets and good practices while building Custom Widgets

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

- AlertDialog WillPopScope

  This example is used on multi page navigation app. Current Key is the key of a page that is currently on the screen. We set that key whenever user jumps around different navigation pages so we know where we at.

  If the context of the Current Key (page we're on) can be popped; then we pop this page. But if not; show alert dialog to users if they're sure to exit the app.
  
  ```dart
  Future<bool> _onWillPop() async {
    // print('current context: ${Navigator.of(_currentKey.currentContext!)}'); //* şu anki context bilgisi
    if (Navigator.of(_currentKey.currentContext!).canPop()) {
      Navigator.of(_currentKey.currentContext!).pop();
      return Future.value(false);
    } else {
      return (await showDialog(
            context: context,
            builder: (context) => new AlertDialog(
              title: new Text('Are you sure?'),
              content: new Text('Do you want to exit an App'),
              actions: <Widget>[
                TextButton(
                  onPressed: () => Navigator.of(context).pop(false),
                  child: new Text('No'),
                ),
                TextButton(
                  onPressed: () => Navigator.of(context).pop(true),
                  child: new Text('Yes'),
                ),
              ],
            ),
          )) ??
          false;
    }
  }
  ```

### Production and Uploading to a Store

- Creating SHA1 and SHA-256 keys for Firebase auth or some other things
  1. Add firebase_core package to your dependencies on your `pubspec.yaml`
  2. Install JDK 11 (JDK 16 is not working on Aug 2021, 23th)
  3. Set JAVA_HOME on path variables to JDK folder (not bin, the whole jdk folder)
  4. Close and reopen IDE
  5. Open shell terminal in the project root
  6. `cd android`
  7. `gradlew signingReport`
  8. Check terminal to see where your key files are located at (mine was in `C:\Users\cengi\.android\`)

## Database (NoSQL, Firebase) and Backend related

- özetin özeti:
  1. NoSQL implementasyonu
  2. Field'lar verilerin tutulduğu en küçük birim
     1. key: value
     2. key'ler string
     3. value'lar int, bool, string, array, georeference gibi birçok değeri veya subcollection yapılarını tutabiliyor
  3. Döküman: bir sürü field'a sahip belgeler
  4. Collection: dökümanların saklandığı yapı
  5. Veriler JSON olarak tutuluyor (key:value)
  6. Dökümanlar en fazla 1MiB yer tutabilir. (char, int vs. 1 byte. 1 milyon char veya int kombinasyonuna sahip olman lazım. biraz zor)
  7. Döküman boyutu hesaplanırken
     1. döküman path'inin string değeri
     2. dökümandaki tüm field'ların keylerinin byte değeri
     3. dökümandaki tüm field'ların value'larının byte değeri hesaplanıyor
  8. Dökümanlar alt üye olarak subcollection içerebiliyor
     1. Subcollection'lar döküman boyutundan yemiyorlar.
  9. Data structure için 3 yaklaşım var.
     1. Genel collection'lar aç (user, messages, chat_rooms). Dökümanların altındaki field'larla her şeyi hallet
     2. Collection'ların altındaki dökümanlara subcollection'lar açarak ağaçlı bir yapı sağla. Scalability'si yüksek bir NoSQL implementasyonun olsun. Ama complex query'ler çok ağaçlı yapıdan dolayı gerçeklemesi zor. (kod olarak zor olduğu kesin. docs'da arama gecikmesi olarak da sıkıntı olacağı da söyleniyor)
     3. Aynı değerleri farklı koleksiyonlara bölerek RDBMS'deki many-to-many yapısını gerçekle.

- Data duplication vs. data referencing while storing data in Firestore NoSQL schema [SO | @alex mamo](https://stackoverflow.com/questions/53053768/what-is-the-correct-way-to-structure-this-kind-of-data-in-firestore/53057707#:~:text=Data%20duplication%20is%20the%20key%20to%20faster%20reads)
- A good GitHub repo to summarize use cases, edge cases and caveats in Flutter [github | ecgan/firestore-considerations](https://link)

  <details>
    <summary>Firebase summary</summary>

  ### Introduction
  
  Firestore is [launched](https://firebase.googleblog.com/2017/10/introducing-cloud-firestore.html) in early October 2017 to the public as a  beta release.
  
  Google has been [recommending](https://firebase.googleblog.com/2017/10/cloud-firestore-for-rtdb-developers.html) "most new applications start with  Cloud Firestore".
  
  However, as a developer goes through the [docs](https://firebase.google.com/docs/firestore/), you may notice there are some caveats.  
  
  This page serves as a one-stop center to know about the things / "caveats" to   consider before really adopting Cloud Firestore in an app.
  
  Even though Firestore addresses a number of shortcomings in the Realtime  Database, it is important to note that it is not a magic silver bullet.
  
  ### Considerations
  
  #### Deleting a document does not delete its subcollections
  
  In Firestore, the data is organized in the collection -> document ->  subcollection -> document -> ... structure.
  
  When you delete a document that has associated subcollections, the  subcollections are not deleted. They are still accessible by reference.
  
  If you want to delete documents in subcollections when deleting a document,   you must do so manually, as shown in [Delete Collections](https://firebase.google.com/docs/firestore/manage-data/delete-data#collections)
  
  [Link](https://firebase.google.com/docs/firestore/data-model#subcollections)
  
  #### Delete collection by deleting individual documents
  
  "To delete an entire collection or subcollection in Cloud Firestore, retrieve   all the documents within the collection or subcollection and delete them. If  you have larger  collections, you may want to delete the documents in smaller  batches to avoid out-of-memory errors. Repeat the process until you've   deleted the entire collection or   subcollection."
  
  [Link](https://firebase.google.com/docs/firestore/manage-data/delete-data)
  
  ### Document deletion limit: max 20K/day on free Spark plan
  
  Something to be aware of, in case you need to delete many important documents.
  
  [Link](https://firebase.google.com/pricing/)
  
  ### Cannot query across subcollections
  
  If you need to query data across collections, use root-level collections.
  
  [Link](https://firebase.google.com/docs/firestore/data-model#subcollections)
  
  ### Data nesting can be up to 100 levels deep
  
  Self explanatory. This should be good enough for most cases.
  
  ### Choosing data structure - nested data in document vs. subcollections vs.  root-level collections
  
  The doc explains it quite well.
  
  [Link](https://firebase.google.com/docs/firestore/manage-data/structure-data)
  
  ### No automatic ordering by creation date
  
  "If you want to be able to order your documents by creation date, you should  store a timestamp as a field in the documents."
  
  [Link](https://firebase.google.com/docs/firestore/manage-data/add-data)
  
  ### Transactions
  
  - Read operations must come before write operations.
  - A function calling a transaction (transaction function) might run more than   once if a concurrent edit affects a document that the transaction reads.
  - Transaction functions should not directly modify application state.
  - Transactions will fail when the client is offline.
  
  </details>
  