## Authentication Flow with React Navigation v5

<sub><sup>cover image by [Darius Foroux](https://dariusforoux.com)</sup></sub>

Due to the recent update in react navigation some major changes have taken place and with that some notable changes can be made in the authentication flow.

> **“ Your best teacher is your last mistake ” — Ralph Nader**

## Prerequisite

In this tutorial I am going to assume that you have a basic understanding of react native, react navigation and expo. In case of any doubt or if you need an in depth tutorial do ask in the comments.😎

## Overview

In this tutorial I will be building two screens i.e. a signup screen and a signin screen. I will only focus on the design aspect and the navigation and won’t go into detail of how to connect it to the database and authenticate, if you guys need to know just ask in the comments. So enough chit chat and lets start.🏎

![signin-signup-screen-demo](https://miro.medium.com/max/722/1*9rTef9t7XRGzqNDvpJ2fmw.gif)

## Approach

> **Note :**  I am using react native elements to speed up the design process.

Install the required files using npm

1.  Use expo to create a project

```
Expo init "_appName"_
```

2. Installing react navigation and its dependencies

```
npm install @react-navigation/native

expo install react-native-gesture-handler react-native-reanimated react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

3. I am using a stack navigator for the signin and signup screens and a bottom tab navigator for the main part of the app. We need to separately install stack and tab navigator before using them.

```
npm install @react-navigation/bottom-tabs  
npm install @react-navigation/stack
```

> **Note:**  In the old version of react navigation a switch navigator was usually used to switch between signin and signup screens. React navigation now recommends using stack navigator in place of that. For more info read this 👉  [upgrading from v4](https://reactnavigation.org/docs/upgrading-from-4.x/#switch-navigator).

4. Install react native elements

```
npm install react-**native**-elements
```

## Project structure

![react-navigation-vscode-project-structure](https://miro.medium.com/max/592/0*0b4ZbC09OMK14Tjh)

As react native is all about components and reusing them let’s create two components  **AuthForm**  and  **NavLink**. AuthForm is helpful for giving that common UI as both signin and signup screen are pretty much identical. NavLink helps to give the bottom link to either of the pages. The spacer component is used for providing a uniform space between each view or text elements.

### Spacer.js

```
import React from "react";

import { View, StyleSheet } from "react-native";

const Spacer = ({ children }) => {

return <View style={styles.spacer}>{children}</View>;

};

const styles = StyleSheet.create({

spacer: {

margin: 15,

},

});

export default Spacer;
```
### AuthForm.js

**Things to note:**  AuthForm is called with 4 properties, these properties help to modify this component to the screen (i.e signin or signup). What each of these properties do is pretty much evident from their names.

- headerText => shows Signup page or Sigin page according to the context.

- errorMessage => shows error and will become more useful when using the api and database requests.

- onSubmit => uses the email and password to do the submit action.

- submitButtonText => changes the button name accordingly (i.e signin or signup).

```
import React, { useState } from "react";

import { StyleSheet } from "react-native";

import { Text, Button, Input } from "react-native-elements";

import Spacer from "./Spacer";

const AuthForm = ({ headerText, errorMessage, onSubmit, submitButtonText }) => {

const [email, setEmail] = useState("");

const [password, setPassword] = useState("");

return (

<>

<Spacer>

<Text h3>{headerText}</Text>

</Spacer>

<Input

label="Email"

value={email}

onChangeText={setEmail}

autoCapitalize="none"

autoCorrect={false}

/>

<Spacer />

<Input

secureTextEntry

label="Password"

value={password}

onChangeText={setPassword}

autoCapitalize="none"

autoCorrect={false}

/>

{errorMessage ? (

<Text style={styles.errorMessage}>{errorMessage}</Text>

) : null}

<Spacer>

<Button

title={submitButtonText}

onPress={() => onSubmit({ email, password })}

/>

</Spacer>

</>

);

};

const styles = StyleSheet.create({

errorMessage: {

fontSize: 16,

color: "red",

marginLeft: 15,

marginTop: 15,

},

});

export default AuthForm;
```

### NavLink.js

This is used to change screen that is between signup screen or signin Screen

![navlink-react-navigation-demo](https://miro.medium.com/max/704/1*wcxGIULq0y_0H5XI3VA4Aw.gif)

**Things to note:** Here I used useNavigation from react navigation to switch between the screen. Note that this method only works in a deeply nested child which is satisfied here. NavLink requires two parameters : text and routeName.

text => used to show the specific text.

routeName => it is the name of the route as specified in the App.js file

```
import React from "react";

import { Text, StyleSheet, TouchableOpacity } from "react-native";

import Spacer from "./Spacer";

import { useNavigation } from "@react-navigation/native";

const NavLink = ({ text, routeName }) => {

const navigation = useNavigation();

return (

<TouchableOpacity onPress={() => navigation.navigate(routeName)}>

<Spacer>

<Text style={styles.link}>{text}</Text>

</Spacer>

</TouchableOpacity>

);

};

const styles = StyleSheet.create({

link: {

color: "blue",

},

});

export default NavLink;
```

### SigninScreen.js

**Things to note:** Don’t forget to import AuthForm and NavLink  and to provide all the parameters.

```
import React from "react";

import { View, StyleSheet, Text } from "react-native";

import AuthForm from "../components/AuthForm";

import NavLink from "../components/NavLink";

const SigninScreen = () => {

return (

<View style={styles.container}>

<AuthForm

headerText="Sign In to your Acoount"

errorMessage=""

onSubmit={() => {}}

submitButtonText="Sign In"

/>

<NavLink

text="Dont have an account? Sign up instead"

routeName="Signup"

/>

</View>

);

};

const styles = StyleSheet.create({

container: {

flex: 1,

justifyContent: "center",

marginBottom: 200,

},

});

export default SigninScreen;
```

### SignupScreen.js

**Things to note:** Don’t forget to import AuthForm and NavLink  and to provide all the parameters.

```
import React, { useContext } from "react";

import { View, StyleSheet } from "react-native";

import { Context as AuthContext } from "../context/AuthContext";

import AuthForm from "../components/AuthForm";

import NavLink from "../components/NavLink";

const SignupScreen = ({ navigation }) => {

const { state, signup } = useContext(AuthContext);

return (

<View style={styles.container}>

<AuthForm

headerText="Sign Up for Tracker"

errorMessage={state.errorMessage}

submitButtonText="Sign Up"

onSubmit={signup}

/>

<NavLink

routeName="Signin"

text="Already Have an account? Sign in instead!"

/>

</View>

);

};

const styles = StyleSheet.create({

container: {

flex: 1,

justifyContent: "center",

marginBottom: 200,

},

});

export default SignupScreen;
```

### App.js

**Things to note:** If you look in the App function you will see a isLoggedIn flag set to true, react navigation recommends this approach also keep in mind that this flag is only for the development phase and in the final build you can use JWT( json web token) for ensuring the user is logged in.

```
import React, { useContext } from "react";

import { NavigationContainer } from "@react-navigation/native";

import { createStackNavigator } from "@react-navigation/stack";

import { createBottomTabNavigator } from "@react-navigation/bottom-tabs";

import SigninScreen from "./src/screens/SigninScreen";

import SignupScreen from "./src/screens/SignupScreen";

const Stack = createStackNavigator();

const Tab = createBottomTabNavigator();

function mainFlow() {

return (

<Tab.Navigator>

<Tab.Screen name="tab1" component={tab1} />

<Tab.Screen name="tab2" component={tab2} />

<Tab.Screen name="tab3" component={tab2} />

</Tab.Navigator>

);

}

function App() {

const isLoggedIn = true;

return (

<NavigationContainer>

<Stack.Navigator>

{isLoggedIn ? (

<>

<Stack.Screen

name="Signup"

component={SignupScreen}

options={{ headerShown: false }}

/>

<Stack.Screen

name="Signin"

component={SigninScreen}

options={{ headerShown: false }}

/>

</>

) : (

<Stack.Screen

name="mainFlow"

component={mainFlow}

options={{ headerShown: false }}

/>

)}

</Stack.Navigator>

</NavigationContainer>

);

}

export default App;
```

Thank you very much for reading, liking and commenting on my articles. please consider following me. cheers 😊

👉🏼 checkout my website,  [**milindsoorya.com**](https://www.milindsoorya.com/)  for more updates and getting in touch. cheers.

## You might also like:-

-   [Mushroom dataset analysis and classification in python](https://milindsoorya.com/blog/mushroom-dataset-analysis-and-classification-python)
-   [How To Set Up Jupyter Notebook with Python 3 on Ubuntu 20.04](https://milindsoorya.com/blog/how-to-Set-up-jupyter-notebook-with-python-3-on-ubuntu-20.04)
-   [How to use python virtual environment with conda](https://www.milindsoorya.com/blog/how-to-use-virtual-environment-with-conda)
-   [Build a spam classifier in python](https://www.milindsoorya.com/blog/build-a-spam-classifier-in-python)
-   [Connecting Rasa Chatbot to a website — A step by step tutorial](https://www.milindsoorya.com/blog/connecting-rasa-to-a-website-a-step-by-step-tutorial)

 