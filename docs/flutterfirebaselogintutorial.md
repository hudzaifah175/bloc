# Flutter Login Tutorial

![advanced](https://img.shields.io/badge/level-advanced-red.svg)

> In the following tutorial, we're going to build a Firebase Login Flow in Flutter using the Bloc library.

![demo](./assets/gifs/flutter_firebase_login.gif)

## Setup

We'll start off by creating a brand new Flutter project

```bash
flutter create flutter_firebase_login
```

We can then go ahead and replace the contents of `pubspec.yaml` with

```yaml
name: flutter_firebase_login
description: A new Flutter project.

version: 1.0.0+1

environment:
  sdk: ">=2.0.0-dev.68.0 <3.0.0"

dependencies:
  flutter:
    sdk: flutter
  cloud_firestore: ^0.9.7
  firebase_auth: ^0.8.1+4
  google_sign_in: ^4.0.1+1
  flutter_bloc: ^0.11.0
  equatable: ^0.2.0
  meta: ^1.1.6
  font_awesome_flutter: ^8.4.0

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  uses-material-design: true
  assets:
    - assets/
```

and then install all of our dependencies

```bash
flutter packages get
```

The last thing we need to do is follow the [firebase_auth usage instructions](https://pub.dartlang.org/packages/firebase_auth#usage) in order to hook up our application to firebase and enable [google_signin](https://pub.dartlang.org/packages/google_sign_in).

## User Repository

Just like in the [flutter login tutorial](./flutterlogintutorial.md), we're going to need to create our `UserRepository` which will be responsible for abstracting the underlying implementation for how we authenticate and retreive user information.

Let's create `user_repository.dart` and get started.

We can start by defining our `UserRepository` class and implementing the constructor. You can immediately see that the `UserRepository` will have a dependency on both `FirebaseAuth` and `GoogleSignIn`.

```dart
import 'package:firebase_auth/firebase_auth.dart';
import 'package:google_sign_in/google_sign_in.dart';

class UserRepository {
  final FirebaseAuth _firebaseAuth;
  final GoogleSignIn _googleSignIn;

  UserRepository({FirebaseAuth firebaseAuth, GoogleSignIn googleSignin})
      : _firebaseAuth = firebaseAuth ?? FirebaseAuth.instance,
        _googleSignIn = googleSignin ?? GoogleSignIn();
}
```

?> **Note:** If `FirebaseAuth` and/or `GoogleSignIn` are not injected into the `UserRepository`, then we instantiate them internally. This allows us to be able to inject mock instances so that we can easily test the `UserRepository`.
