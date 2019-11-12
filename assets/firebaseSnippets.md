# Firebase Queries and shit

## Authentication
<!-- from https://firebase.google.com/docs/auth/users -->
### User properties 
Firebase users have a fixed set of basic properties: a unique ID, a primary email adderss, a name and a photo URL -- stored in the project's user database, that can be updated by the user. You cannot add other prpoerties to the user object directly;instead you can store the additional properties in any storage service such as Firestore.
- if the user signed up with an email address and password, only the primary email address property ispopulated. 

The current user is the **Auth instance** which preserves information between restartnig the application. The auth istance is updated when a user signs up or signs in.

| |  |
| --- | --- | 
| firebase ID tokens | Created by Firebase when a user signs in to an app. These tokens are signed JWTs that securely identify a user in a Firebase project. These tokens contain basic profile information for a user, including the user's ID string, which is unique to the Firebase project. Because the integrity of ID tokens can be verified, you can send them to a backend server to identify the currently signed-in user. |
| Identity provider tokens | Created by federated identity providers, such as Google and Facebook. These tokens can have different formats, but are often OAuth 2.0 access tokens. Apps use these tokens to verify that users have successfully authenticated with the identity provider, and then convert them into credentials usable by Firebase services. |

Follow the steps at https://firebase.google.com/docs/auth/android/firebaseui to setup the authentication services. (Currently not working)

## Managing Users
https://firebase.google.com/docs/auth/android/manage-users

Getting the current user:
```kt
val user = FirebaseAuth.getInstance().currentUser
if (user != null) {
    // User is signed in
} else {
    // No user is signed in
}
```

Getting a users profile
```kt
val user = FirebaseAuth.getInstance().currentUser
user?.let {
    // Name, email address, and profile photo Url
    val name = user.displayName
    val email = user.email
    val photoUrl = user.photoUrl

    // Check if user's email is verified
    val emailVerified = user.isEmailVerified

    // The user's ID, unique to the Firebase project. Do NOT use this value to
    // authenticate with your backend server, if you have one. Use
    // FirebaseUser.getToken() instead.
    val uid = user.uid
}
```

Update a user's profile
```kt
val user = FirebaseAuth.getInstance().currentUser

val profileUpdates = UserProfileChangeRequest.Builder()
        .setDisplayName("Jane Q. User")
        .setPhotoUri(Uri.parse("https://example.com/jane-q-user/profile.jpg"))
        .build()

user?.updateProfile(profileUpdates)
        ?.addOnCompleteListener { task ->
            if (task.isSuccessful) {
                Log.d(TAG, "User profile updated.")
            }
        }
```

TODO: finish with a ton more snippets