# Accountable Client v0.5

## Features

- uses:
  - only React (create-react-app)
  - firebase
  - react-router
- features:
  - Sign In
  - Sign Up
  - Sign Out
  - Password Forget
  - Password Change
  - Verification Email
  - Protected Routes with Authorization
  - Roles-based Authorization
  - Social Logins with Google, Facebook and Twitter
  - Linking of Social Logins on Account dashboard
  - Auth Persistence with Local Storage
  - Database with Users and Messages

## Installation

- `git clone https://github.com/CariHQ/scam.network.git`
- `cd scam.network`
- `npm install`
- `npm start`
- visit http://localhost:3000

### Firebase Configuration

- copy/paste your configuration from your Firebase project's dashboard into one of these files
  - _src/components/Firebase/firebase.js_ file
  - _.env_ file
  - _.env.development_ and _.env.production_ files

The _.env_ or _.env.development_ and _.env.production_ files could look like the following then:

```
REACT_APP_API_KEY=AIzaSyBtxZ3phPeXcsZsRTySIXa7n33NtQ
REACT_APP_AUTH_DOMAIN=react-firebase-s2233d64f8.firebaseapp.com
REACT_APP_DATABASE_URL=https://react-firebase-s2233d64f8.firebaseio.com
REACT_APP_PROJECT_ID=react-firebase-s2233d64f8
REACT_APP_STORAGE_BUCKET=react-firebase-s2233d64f8.appspot.com
REACT_APP_MESSAGING_SENDER_ID=701928454501
```

### Activate Sign-In Methods

![firebase-enable-google-social-login_640](https://user-images.githubusercontent.com/2479967/49687774-e0a31e80-fb42-11e8-9d8a-4b4c794134e6.jpg)

- Email/Password
- Google
- Facebook
- Twitter

### Activate Verification E-Mail

- add a redirect URL for redirecting a user after an email verification into one of these files
  - _src/components/Firebase/firebase.js_ file
  - _.env_ file
  - _.env.development_ and _.env.production_ files

The _.env_ or _.env.development_ and _.env.production_ files could look like the following then (excl. the Firebase configuration).

**Development:**

```
REACT_APP_CONFIRMATION_EMAIL_REDIRECT=http://localhost:3000
```

**Production:**

```
REACT_APP_CONFIRMATION_EMAIL_REDIRECT=https://mydomain.com
```

### Security Rules

```
{
  "rules": {
    ".read": false,
    ".write": false,
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid || root.child('users/'+auth.uid).child('roles').hasChildren(['ADMIN'])",
        ".write": "$uid === auth.uid || root.child('users/'+auth.uid).child('roles').hasChildren(['ADMIN'])"
      },
      ".read": "root.child('users/'+auth.uid).child('roles').hasChildren(['ADMIN'])",
      ".write": "root.child('users/'+auth.uid).child('roles').hasChildren(['ADMIN'])"
    },
    "messages": {
      ".indexOn": ["createdAt"],
      "$uid": {
        ".write": "data.exists() ? data.child('userId').val() === auth.uid : newData.child('userId').val() === auth.uid"
      },
      ".read": "auth != null",
      ".write": "auth != null",
    },
  }
}
```
