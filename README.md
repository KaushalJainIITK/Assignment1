import 'package:flutter/material.dart';
import 'dart:math';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Random Word Displayer',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: RandomWordDisplayer(),
    );
  }
}

class RandomWordDisplayer extends StatefulWidget {
  @override
  _RandomWordDisplayerState createState() => _RandomWordDisplayerState();
}

class _RandomWordDisplayerState extends State<RandomWordDisplayer> {
  final List<String> words = [
    'Flutter',
    'Dart',
    'Mobile',
    'App',
    'Development',
    'GDSC',
    'Kaushal'
  ];
  String currentWord = '';
  List<String> favorites = [];
  bool showProfile = false;

  void generateRandomWord() {
    final _random = Random();
    final index = _random.nextInt(words.length);
    setState(() {
      currentWord = words[index];
      showProfile = false; // Hide profile details when generating new word
    });
  }

  void addToFavorites() {
    setState(() {
      favorites.add(currentWord);
    });
  }

  void removeFromFavorites(String word) {
    setState(() {
      favorites.remove(word);
    });
  }

  void toggleProfileVisibility() {
    setState(() {
      showProfile = !showProfile;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Random Word Displayer'),
      ),
      drawer: Drawer(
        child: ListView(
          padding: EdgeInsets.zero,
          children: <Widget>[
            ListTile(
              title: Text('Home'),
              onTap: () {
                Navigator.pop(context);
                generateRandomWord(); // Return to random word generator
              },
            ),
            Divider(),
            ListTile(
              title: Text('Favorite Words'),
              onTap: () {
                Navigator.pop(context);
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => FavoritesScreen(
                      favorites: favorites,
                      removeFromFavorites: removeFromFavorites,
                    ),
                  ),
                );
              },
            ),
            ListTile(
              title: Text('Profile'),
              onTap: () {
                Navigator.pop(context);
                toggleProfileVisibility();
              },
            ),
          ],
        ),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            if (showProfile)
              Column(
                children: [
                  CircleAvatar(
                    radius: 50,
                    backgroundImage: AssetImage('IMG20230719105151.jpg'),
                  ),
                  SizedBox(height: 10),
                  Text(
                    'Kaushal Jain',
                    style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
                  ),
                  Text('kaushalj22@iitk.ac.in'),
                  Text('City: Ajmer'),
                  Text('Roll No.: 220510'),
                ],
              )
            else
              Text(
                currentWord,
                style: TextStyle(fontSize: 30, fontWeight: FontWeight.bold),
              ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: showProfile ? null : generateRandomWord,
              child: Text(showProfile ? 'Profile is displayed' : 'Generate Random Word'),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: showProfile ? null : addToFavorites,
              child: Text(showProfile ? 'Profile is displayed' : 'Add to Favorites'),
            ),
          ],
        ),
      ),
    );
  }
}

class FavoritesScreen extends StatelessWidget {
  final List<String> favorites;
  final Function(String) removeFromFavorites;

  FavoritesScreen({required this.favorites, required this.removeFromFavorites});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Favorite Words'),
      ),
      body: ListView.builder(
        itemCount: favorites.length,
        itemBuilder: (context, index) {
          return ListTile(
            title: Text(favorites[index]),
            trailing: IconButton(
              icon: Icon(Icons.delete),
              onPressed: () => removeFromFavorites(favorites[index]),
            ),
          );
        },
      ),
    );
  }
}

//Have written the code according to Dartpad. 


