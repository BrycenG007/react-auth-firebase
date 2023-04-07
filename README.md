# react-with-firebase-auth

## Signature

```ts
type FirebaseAuthProps = {
  firebaseAppAuth: firebase.auth.Auth,
  providers?: {
    googleProvider?: firebase.auth.GoogleAuthProvider_Instance;
    facebookProvider?: firebase.auth.FacebookAuthProvider_Instance;
    twitterProvider?: firebase.auth.TwitterAuthProvider_Instance;
    githubProvider?:  firebase.auth.GithubAuthProvider_Instance;
  }
};

withFirebaseAuth<P>(authProps: FirebaseAuthProps) =>
  createComponentWithAuth(WrappedComponent: React.ComponentType<P>) =>
    React.ComponentType
```

## Props Provided

```ts
type WrappedComponentProps = {
  signInWithEmailAndPassword: (email: string, password: string) => void;
  createUserWithEmailAndPassword: (email: string, password: string) => void;
  signInWithGoogle: () => void;
  signInWithFacebook: () => void;
  signInWithGithub: () => void;
  signInWithTwitter: () => void;
  signInWithPhoneNumber: (
    phoneNumber: string,
    applicationVerifier: firebase.auth.ApplicationVerifier,
  ) => void;
  signInAnonymously: () => void;
  signOut: () => void;
  setError: (error: string) => void;
  user?: firebase.User | null;
  error?: string;
  loading: boolean;
};
```

## Usage

Install it:

```bash
npm install --save react-with-firebase-auth
```

Then use it in your components:

```tsx
import * as React from 'react';
import * as firebase from 'firebase/app';
import 'firebase/auth';

import withFirebaseAuth, { WrappedComponentProps } from 'react-with-firebase-auth';

import firebaseConfig from './firebaseConfig';

const firebaseApp = firebase.initializeApp(firebaseConfig);

const firebaseAppAuth = firebaseApp.auth();

/** See the signature above to find out the available providers */
const providers = {
  googleProvider: new firebase.auth.GoogleAuthProvider(),
};

/** providers can be customised as per the Firebase documentation on auth providers **/
providers.googleProvider.setCustomParameters({
  hd: 'mycompany.com',
});

/** Create the FirebaseAuth component wrapper */
const createComponentWithAuth = withFirebaseAuth({
  providers,
  firebaseAppAuth,
});

const App = ({
  /** These props are provided by withFirebaseAuth HOC */
  signInWithEmailAndPassword,
  createUserWithEmailAndPassword,
  signInWithGoogle,
  signInWithFacebook,
  signInWithGithub,
  signInWithTwitter,
  signInAnonymously,
  signOut,
  setError,
  user,
  error,
  loading,
}: WrappedComponentProps) => (
  <React.Fragment>
    {
      user
        ? <h1>Hello, {user.displayName}</h1>
        : <h1>Log in</h1>
    }

    {
      user
        ? <button onClick={signOut}>Sign out</button>
        : <button onClick={signInWithGoogle}>Sign in with Google</button>
    }

    {
      loading && <h2>Loading..</h2>
    }
  </React.Fragment>
);

/** Wrap it */
export default createComponentWithAuth(App);
```
## Articles

 - ["How to setup Firebase Authentication with React in 5 minutes (maybe 10)"](https://medium.com/firebase-developers/how-to-setup-firebase-authentication-with-react-in-5-minutes-maybe-10-bb8bb53e8834)

## Stargazers

[![Stargazers over time](https://starchart.cc/armand1m/react-with-firebase-auth.svg)](https://starchart.cc/armand1m/react-with-firebase-auth)

## License

MIT © [Armando Magalhaes](https://github.com/BrycenG007)
