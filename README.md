import 'package:flutter/material.dart'; import 'package:firebase_core/firebase_core.dart'; import 'package:cloud_firestore/cloud_firestore.dart'; import 'package:firebase_auth/firebase_auth.dart';

void main() async { WidgetsFlutterBinding.ensureInitialized(); await Firebase.initializeApp(); runApp(YouthConnectApp()); }

class YouthConnectApp extends StatelessWidget { @override Widget build(BuildContext context) { return MaterialApp( title: 'YouthConnect', theme: ThemeData( primarySwatch: Colors.blue, ), home: AuthGate(), ); } }

class AuthGate extends StatelessWidget { @override Widget build(BuildContext context) { return StreamBuilder<User?>( stream: FirebaseAuth.instance.authStateChanges(), builder: (context, snapshot) { if (snapshot.connectionState == ConnectionState.active) { User? user = snapshot.data; if (user == null) { return LoginPage(); } else { return HomePage(); } } return Scaffold( body: Center(child: CircularProgressIndicator()), ); }, ); } }

class LoginPage extends StatelessWidget { final emailController = TextEditingController(); final passwordController = TextEditingController();

@override Widget build(BuildContext context) { return Scaffold( appBar: AppBar(title: Text("Login")), body: Padding( padding: const EdgeInsets.all(16.0), child: Column( children: [ TextField(controller: emailController, decoration: InputDecoration(labelText: 'Email')), TextField(controller: passwordController, decoration: InputDecoration(labelText: 'Password'), obscureText: true), ElevatedButton( onPressed: () async { await FirebaseAuth.instance.signInWithEmailAndPassword( email: emailController.text, password: passwordController.text, ); }, child: Text("Login"), ), ], ), ), ); } }

class HomePage extends StatelessWidget { final postController = TextEditingController();

@override Widget build(BuildContext context) { return Scaffold( appBar: AppBar( title: Text("YouthConnect Home"), actions: [ IconButton( icon: Icon(Icons.logout), onPressed: () => FirebaseAuth.instance.signOut(), ) ], ), body: Column( children: [ Padding( padding: const EdgeInsets.all(8.0), child: TextField( controller: postController, decoration: InputDecoration( labelText: 'What's on your mind?', ), ), ), ElevatedButton( onPressed: () { FirebaseFirestore.instance.collection('posts').add({ 'text': postController.text, 'timestamp': FieldValue.serverTimestamp(), }); postController.clear(); }, child: Text("Post"), ), Expanded( child: StreamBuilder<QuerySnapshot>( stream: FirebaseFirestore.instance.collection('posts').orderBy('timestamp', descending: true).snapshots(), builder: (context, snapshot) { if (!snapshot.hasData) return Center(child: CircularProgressIndicator()); final docs = snapshot.data!.docs; return ListView.builder( itemCount: docs.length, itemBuilder: (context, index) { return ListTile( title: Text(docs[index]['text'] ?? 'No Content'), ); }, ); }, ), ), ], ), ); } }
import 'package:flutter/material.dart'; import 'package:firebase_core/firebase_core.dart'; import 'package:cloud_firestore/cloud_firestore.dart'; import 'package:firebase_auth/firebase_auth.dart';

void main() async { WidgetsFlutterBinding.ensureInitialized(); await Firebase.initializeApp(); runApp(YouthConnectApp()); }

class YouthConnectApp extends StatelessWidget { @override Widget build(BuildContext context) { return MaterialApp( title: 'YouthConnect', theme: ThemeData( primarySwatch: Colors.blue, ), home: AuthGate(), ); } }

class AuthGate extends StatelessWidget { @override Widget build(BuildContext context) { return StreamBuilder<User?>( stream: FirebaseAuth.instance.authStateChanges(), builder: (context, snapshot) { if (snapshot.connectionState == ConnectionState.active) { User? user = snapshot.data; if (user == null) { return LoginPage(); } else { return HomePage(); } } return Scaffold( body: Center(child: CircularProgressIndicator()), ); }, ); } }

class LoginPage extends StatelessWidget { final emailController = TextEditingController(); final passwordController = TextEditingController();

@override Widget build(BuildContext context) { return Scaffold( appBar: AppBar(title: Text("Login")), body: Padding( padding: const EdgeInsets.all(16.0), child: Column( children: [ TextField(controller: emailController, decoration: InputDecoration(labelText: 'Email')), TextField(controller: passwordController, decoration: InputDecoration(labelText: 'Password'), obscureText: true), ElevatedButton( onPressed: () async { await FirebaseAuth.instance.signInWithEmailAndPassword( email: emailController.text, password: passwordController.text, ); }, child: Text("Login"), ), ], ), ), ); } }

class HomePage extends StatelessWidget { final postController = TextEditingController();

@override Widget build(BuildContext context) { return Scaffold( appBar: AppBar( title: Text("YouthConnect Home"), actions: [ IconButton( icon: Icon(Icons.logout), onPressed: () => FirebaseAuth.instance.signOut(), ) ], ), body: Column( children: [ Padding( padding: const EdgeInsets.all(8.0), child: TextField( controller: postController, decoration: InputDecoration( labelText: 'What's on your mind?', ), ), ), ElevatedButton( onPressed: () { FirebaseFirestore.instance.collection('posts').add({ 'text': postController.text, 'timestamp': FieldValue.serverTimestamp(), }); postController.clear(); }, child: Text("Post"), ), Expanded( child: StreamBuilder<QuerySnapshot>( stream: FirebaseFirestore.instance.collection('posts').orderBy('timestamp', descending: true).snapshots(), builder: (context, snapshot) { if (!snapshot.hasData) return Center(child: CircularProgressIndicator()); final docs = snapshot.data!.docs; return ListView.builder( itemCount: docs.length, itemBuilder: (context, index) { return ListTile( title: Text(docs[index]['text'] ?? 'No Content'), ); }, ); }, ), ), ], ), ); } }


