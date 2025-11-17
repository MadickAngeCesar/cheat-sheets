# React Native Cheat Sheet - Beginner to Pro

## Table of Contents
- [Setup & Installation](#setup--installation)
- [Project Structure](#project-structure)
- [Core Components](#core-components)
- [Styling](#styling)
- [Props & State](#props--state)
- [Hooks](#hooks)
- [Navigation](#navigation)
- [Lists & ScrollViews](#lists--scrollviews)
- [Forms & Input](#forms--input)
- [Images & Media](#images--media)
- [Networking](#networking)
- [AsyncStorage](#asyncstorage)
- [Platform Specific Code](#platform-specific-code)
- [Animations](#animations)
- [Native Modules](#native-modules)
- [Performance](#performance)
- [Testing](#testing)
- [Deployment](#deployment)
- [Best Practices](#best-practices)

---

## Setup & Installation

### Install React Native CLI
```bash
# Install Node.js (required)
# https://nodejs.org/

# Install React Native CLI
npm install -g react-native-cli

# Or use npx (recommended)
npx react-native init MyApp

# With TypeScript
npx react-native init MyApp --template react-native-template-typescript
```

### Expo (Managed Workflow)
```bash
# Install Expo CLI
npm install -g expo-cli

# Create new Expo project
expx create-app MyApp
cd MyApp

# Start development server
expo start
npm start
```

### Run Project
```bash
# iOS (macOS only)
npx react-native run-ios
npx react-native run-ios --simulator="iPhone 14 Pro"

# Android
npx react-native run-android

# Start Metro bundler
npx react-native start
```

---

## Project Structure

### Basic Structure
```
MyApp/
├── android/
├── ios/
├── node_modules/
├── src/
│   ├── components/
│   ├── screens/
│   ├── navigation/
│   ├── services/
│   ├── utils/
│   └── assets/
├── App.js
├── package.json
└── metro.config.js
```

---

## Core Components

### View
```jsx
import { View, StyleSheet } from 'react-native';

const App = () => (
  <View style={styles.container}>
    <View style={styles.box} />
  </View>
);

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
  },
  box: {
    width: 100,
    height: 100,
    backgroundColor: 'blue',
  },
});
```

### Text
```jsx
import { Text, StyleSheet } from 'react-native';

const App = () => (
  <View>
    <Text style={styles.title}>Hello World</Text>
    <Text style={styles.subtitle}>Welcome to React Native</Text>
    <Text numberOfLines={2} ellipsizeMode="tail">
      Long text that will be truncated...
    </Text>
  </View>
);

const styles = StyleSheet.create({
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
  },
  subtitle: {
    fontSize: 16,
    color: '#666',
  },
});
```

### Button & Touchable
```jsx
import { Button, TouchableOpacity, TouchableHighlight, Pressable } from 'react-native';

// Basic Button
<Button
  title="Press Me"
  onPress={() => console.log('Pressed')}
  color="#841584"
  disabled={false}
/>

// TouchableOpacity (most common)
<TouchableOpacity
  onPress={() => console.log('Pressed')}
  activeOpacity={0.7}
  style={styles.button}
>
  <Text>Press Me</Text>
</TouchableOpacity>

// TouchableHighlight
<TouchableHighlight
  onPress={() => console.log('Pressed')}
  underlayColor="#DDDDDD"
>
  <Text>Press Me</Text>
</TouchableHighlight>

// Pressable (modern, recommended)
<Pressable
  onPress={() => console.log('Pressed')}
  onLongPress={() => console.log('Long press')}
  style={({ pressed }) => [
    styles.button,
    pressed && styles.pressed
  ]}
>
  {({ pressed }) => (
    <Text style={{ color: pressed ? 'white' : 'black' }}>
      Press Me
    </Text>
  )}
</Pressable>
```

### Image
```jsx
import { Image } from 'react-native';

// Local image
<Image
  source={require('./assets/logo.png')}
  style={{ width: 100, height: 100 }}
  resizeMode="cover" // cover, contain, stretch, repeat, center
/>

// Remote image
<Image
  source={{ uri: 'https://example.com/image.jpg' }}
  style={{ width: 200, height: 200 }}
/>

// With loading indicator
<Image
  source={{ uri: 'https://example.com/image.jpg' }}
  style={{ width: 200, height: 200 }}
  loadingIndicatorSource={require('./assets/loading.gif')}
/>
```

---

## Styling

### StyleSheet
```jsx
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    padding: 20,
  },
  text: {
    fontSize: 16,
    color: '#333',
    fontWeight: 'bold',
  },
});
```

### Flexbox
```jsx
const styles = StyleSheet.create({
  container: {
    flex: 1,
    flexDirection: 'row', // row, column, row-reverse, column-reverse
    justifyContent: 'center', // flex-start, flex-end, center, space-between, space-around, space-evenly
    alignItems: 'center', // flex-start, flex-end, center, stretch, baseline
    flexWrap: 'wrap', // wrap, nowrap
  },
  item: {
    flex: 1,
    alignSelf: 'flex-start', // auto, flex-start, flex-end, center, stretch
  },
});
```

### Dimensions & Layout
```jsx
import { Dimensions, useWindowDimensions } from 'react-native';

// Get screen dimensions
const { width, height } = Dimensions.get('window');
const screenWidth = Dimensions.get('screen').width;

// Hook (recommended - updates on rotation)
const MyComponent = () => {
  const { width, height } = useWindowDimensions();
  
  return (
    <View style={{ width: width * 0.9, height: height * 0.5 }} />
  );
};
```

### Platform Specific Styles
```jsx
import { Platform, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: {
    ...Platform.select({
      ios: {
        backgroundColor: 'red',
      },
      android: {
        backgroundColor: 'blue',
      },
    }),
  },
  text: {
    fontSize: Platform.OS === 'ios' ? 16 : 14,
  },
});
```

---

## Props & State

### Props
```jsx
// Parent component
const App = () => (
  <Greeting name="John" age={25} />
);

// Child component
const Greeting = ({ name, age }) => (
  <View>
    <Text>Hello {name}!</Text>
    <Text>Age: {age}</Text>
  </View>
);

// With default props
const Greeting = ({ name = 'Guest', age = 0 }) => (
  <Text>Hello {name}, {age}</Text>
);

// PropTypes
import PropTypes from 'prop-types';

Greeting.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number,
};
```

### State (Class Components)
```jsx
import React, { Component } from 'react';
import { View, Text, Button } from 'react-native';

class Counter extends Component {
  state = {
    count: 0,
  };

  increment = () => {
    this.setState({ count: this.state.count + 1 });
  };

  render() {
    return (
      <View>
        <Text>Count: {this.state.count}</Text>
        <Button title="Increment" onPress={this.increment} />
      </View>
    );
  }
}
```

---

## Hooks

### useState
```jsx
import { useState } from 'react';
import { View, Text, Button } from 'react-native';

const Counter = () => {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('John');

  return (
    <View>
      <Text>Count: {count}</Text>
      <Button title="+" onPress={() => setCount(count + 1)} />
      <Button title="-" onPress={() => setCount(prev => prev - 1)} />
    </View>
  );
};
```

### useEffect
```jsx
import { useEffect } from 'react';

const MyComponent = () => {
  // Run once on mount
  useEffect(() => {
    console.log('Component mounted');
    
    // Cleanup on unmount
    return () => {
      console.log('Component unmounted');
    };
  }, []);

  // Run when dependency changes
  useEffect(() => {
    console.log('Count changed');
  }, [count]);

  // Run on every render
  useEffect(() => {
    console.log('Component rendered');
  });
};
```

### useCallback & useMemo
```jsx
import { useCallback, useMemo } from 'react';

const MyComponent = () => {
  const [count, setCount] = useState(0);

  // Memoize callback
  const handlePress = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  // Memoize computed value
  const expensiveValue = useMemo(() => {
    return count * 2;
  }, [count]);

  return <View />;
};
```

### useRef
```jsx
import { useRef } from 'react';
import { TextInput } from 'react-native';

const MyComponent = () => {
  const inputRef = useRef(null);
  const countRef = useRef(0);

  const focusInput = () => {
    inputRef.current?.focus();
  };

  return (
    <TextInput ref={inputRef} />
  );
};
```

### useContext
```jsx
import { createContext, useContext } from 'react';

// Create context
const ThemeContext = createContext('light');

// Provider
const App = () => (
  <ThemeContext.Provider value="dark">
    <ThemedComponent />
  </ThemeContext.Provider>
);

// Consumer
const ThemedComponent = () => {
  const theme = useContext(ThemeContext);
  return <Text>Theme: {theme}</Text>;
};
```

---

## Navigation

### React Navigation (Most Popular)
```bash
npm install @react-navigation/native
npm install react-native-screens react-native-safe-area-context
npm install @react-navigation/native-stack
```

### Stack Navigation
```jsx
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

const Stack = createNativeStackNavigator();

const App = () => (
  <NavigationContainer>
    <Stack.Navigator initialRouteName="Home">
      <Stack.Screen 
        name="Home" 
        component={HomeScreen}
        options={{ title: 'My Home' }}
      />
      <Stack.Screen name="Details" component={DetailsScreen} />
    </Stack.Navigator>
  </NavigationContainer>
);

// Navigate between screens
const HomeScreen = ({ navigation }) => (
  <View>
    <Button
      title="Go to Details"
      onPress={() => navigation.navigate('Details', { id: 123 })}
    />
  </View>
);

// Access params
const DetailsScreen = ({ route, navigation }) => {
  const { id } = route.params;
  
  return (
    <View>
      <Text>Details ID: {id}</Text>
      <Button title="Go Back" onPress={() => navigation.goBack()} />
    </View>
  );
};
```

### Tab Navigation
```bash
npm install @react-navigation/bottom-tabs
```

```jsx
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import Icon from 'react-native-vector-icons/Ionicons';

const Tab = createBottomTabNavigator();

const App = () => (
  <NavigationContainer>
    <Tab.Navigator
      screenOptions={({ route }) => ({
        tabBarIcon: ({ focused, color, size }) => {
          let iconName;
          if (route.name === 'Home') {
            iconName = focused ? 'home' : 'home-outline';
          } else if (route.name === 'Settings') {
            iconName = focused ? 'settings' : 'settings-outline';
          }
          return <Icon name={iconName} size={size} color={color} />;
        },
        tabBarActiveTintColor: 'tomato',
        tabBarInactiveTintColor: 'gray',
      })}
    >
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  </NavigationContainer>
);
```

### Drawer Navigation
```bash
npm install @react-navigation/drawer react-native-gesture-handler react-native-reanimated
```

```jsx
import { createDrawerNavigator } from '@react-navigation/drawer';

const Drawer = createDrawerNavigator();

const App = () => (
  <NavigationContainer>
    <Drawer.Navigator>
      <Drawer.Screen name="Home" component={HomeScreen} />
      <Drawer.Screen name="Profile" component={ProfileScreen} />
    </Drawer.Navigator>
  </NavigationContainer>
);
```

---

## Lists & ScrollViews

### ScrollView
```jsx
import { ScrollView } from 'react-native';

<ScrollView
  horizontal={false}
  showsVerticalScrollIndicator={true}
  contentContainerStyle={{ padding: 20 }}
>
  <Text>Content here...</Text>
</ScrollView>
```

### FlatList
```jsx
import { FlatList } from 'react-native';

const DATA = [
  { id: '1', title: 'Item 1' },
  { id: '2', title: 'Item 2' },
  { id: '3', title: 'Item 3' },
];

const MyList = () => (
  <FlatList
    data={DATA}
    keyExtractor={item => item.id}
    renderItem={({ item }) => (
      <View style={styles.item}>
        <Text>{item.title}</Text>
      </View>
    )}
    ItemSeparatorComponent={() => <View style={styles.separator} />}
    ListHeaderComponent={() => <Text>Header</Text>}
    ListFooterComponent={() => <Text>Footer</Text>}
    ListEmptyComponent={() => <Text>No data</Text>}
    refreshing={false}
    onRefresh={() => console.log('Refresh')}
    onEndReached={() => console.log('Load more')}
    onEndReachedThreshold={0.5}
  />
);
```

### SectionList
```jsx
import { SectionList } from 'react-native';

const DATA = [
  {
    title: 'Main dishes',
    data: ['Pizza', 'Burger', 'Risotto'],
  },
  {
    title: 'Sides',
    data: ['French Fries', 'Onion Rings', 'Fried Shrimps'],
  },
];

const MyList = () => (
  <SectionList
    sections={DATA}
    keyExtractor={(item, index) => item + index}
    renderItem={({ item }) => <Text>{item}</Text>}
    renderSectionHeader={({ section: { title } }) => (
      <Text style={styles.header}>{title}</Text>
    )}
  />
);
```

---

## Forms & Input

### TextInput
```jsx
import { TextInput } from 'react-native';

const MyForm = () => {
  const [text, setText] = useState('');
  const [password, setPassword] = useState('');

  return (
    <View>
      <TextInput
        style={styles.input}
        value={text}
        onChangeText={setText}
        placeholder="Enter text"
        placeholderTextColor="#999"
        autoCapitalize="none"
        autoCorrect={false}
      />

      <TextInput
        style={styles.input}
        value={password}
        onChangeText={setPassword}
        placeholder="Password"
        secureTextEntry
        keyboardType="default" // default, numeric, email-address, phone-pad
        returnKeyType="done" // done, go, next, search, send
        onSubmitEditing={() => console.log('Submit')}
      />
    </View>
  );
};
```

### Switch & Checkbox
```jsx
import { Switch } from 'react-native';

const MySwitch = () => {
  const [isEnabled, setIsEnabled] = useState(false);

  return (
    <Switch
      trackColor={{ false: '#767577', true: '#81b0ff' }}
      thumbColor={isEnabled ? '#f5dd4b' : '#f4f3f4'}
      ios_backgroundColor="#3e3e3e"
      onValueChange={setIsEnabled}
      value={isEnabled}
    />
  );
};
```

---

## Images & Media

### ImageBackground
```jsx
import { ImageBackground } from 'react-native';

<ImageBackground
  source={require('./assets/background.jpg')}
  style={{ flex: 1 }}
  resizeMode="cover"
>
  <Text>Content here</Text>
</ImageBackground>
```

### Camera (expo-camera)
```bash
npx expo install expo-camera
```

```jsx
import { Camera } from 'expo-camera';
import { useState, useEffect } from 'react';

const CameraScreen = () => {
  const [hasPermission, setHasPermission] = useState(null);
  const [camera, setCamera] = useState(null);

  useEffect(() => {
    (async () => {
      const { status } = await Camera.requestCameraPermissionsAsync();
      setHasPermission(status === 'granted');
    })();
  }, []);

  const takePicture = async () => {
    if (camera) {
      const photo = await camera.takePictureAsync();
      console.log(photo.uri);
    }
  };

  if (hasPermission === null) return <View />;
  if (hasPermission === false) return <Text>No camera access</Text>;

  return (
    <Camera
      style={{ flex: 1 }}
      type={Camera.Constants.Type.back}
      ref={ref => setCamera(ref)}
    >
      <Button title="Take Picture" onPress={takePicture} />
    </Camera>
  );
};
```

---

## Networking

### Fetch API
```jsx
// GET request
const fetchData = async () => {
  try {
    const response = await fetch('https://api.example.com/data');
    const json = await response.json();
    console.log(json);
  } catch (error) {
    console.error(error);
  }
};

// POST request
const postData = async () => {
  try {
    const response = await fetch('https://api.example.com/data', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        name: 'John',
        email: 'john@example.com',
      }),
    });
    const json = await response.json();
    console.log(json);
  } catch (error) {
    console.error(error);
  }
};
```

### Axios
```bash
npm install axios
```

```jsx
import axios from 'axios';

// GET
const fetchData = async () => {
  try {
    const response = await axios.get('https://api.example.com/data');
    console.log(response.data);
  } catch (error) {
    console.error(error);
  }
};

// POST
const postData = async () => {
  try {
    const response = await axios.post('https://api.example.com/data', {
      name: 'John',
      email: 'john@example.com',
    });
    console.log(response.data);
  } catch (error) {
    console.error(error);
  }
};
```

---

## AsyncStorage

### Installation
```bash
npm install @react-native-async-storage/async-storage
```

### Usage
```jsx
import AsyncStorage from '@react-native-async-storage/async-storage';

// Store data
const storeData = async (value) => {
  try {
    await AsyncStorage.setItem('@storage_key', value);
  } catch (e) {
    console.error(e);
  }
};

// Store object
const storeObject = async (value) => {
  try {
    const jsonValue = JSON.stringify(value);
    await AsyncStorage.setItem('@storage_key', jsonValue);
  } catch (e) {
    console.error(e);
  }
};

// Read data
const getData = async () => {
  try {
    const value = await AsyncStorage.getItem('@storage_key');
    if (value !== null) {
      return value;
    }
  } catch (e) {
    console.error(e);
  }
};

// Read object
const getObject = async () => {
  try {
    const jsonValue = await AsyncStorage.getItem('@storage_key');
    return jsonValue != null ? JSON.parse(jsonValue) : null;
  } catch (e) {
    console.error(e);
  }
};

// Remove data
const removeValue = async () => {
  try {
    await AsyncStorage.removeItem('@storage_key');
  } catch (e) {
    console.error(e);
  }
};

// Clear all
const clearAll = async () => {
  try {
    await AsyncStorage.clear();
  } catch (e) {
    console.error(e);
  }
};
```

---

## Platform Specific Code

### Platform Module
```jsx
import { Platform } from 'react-native';

// Check platform
if (Platform.OS === 'ios') {
  // iOS specific code
} else if (Platform.OS === 'android') {
  // Android specific code
}

// Platform version
if (Platform.Version > 21) {
  // Android API level > 21
}

// Platform select
const styles = StyleSheet.create({
  container: {
    ...Platform.select({
      ios: {
        backgroundColor: 'red',
      },
      android: {
        backgroundColor: 'blue',
      },
      default: {
        backgroundColor: 'gray',
      },
    }),
  },
});
```

---

## Animations

### Animated API
```jsx
import { Animated } from 'react-native';

const FadeInView = ({ children }) => {
  const fadeAnim = useRef(new Animated.Value(0)).current;

  useEffect(() => {
    Animated.timing(fadeAnim, {
      toValue: 1,
      duration: 1000,
      useNativeDriver: true,
    }).start();
  }, [fadeAnim]);

  return (
    <Animated.View style={{ opacity: fadeAnim }}>
      {children}
    </Animated.View>
  );
};

// Spring animation
const SpringBox = () => {
  const springValue = useRef(new Animated.Value(0.3)).current;

  const spring = () => {
    Animated.spring(springValue, {
      toValue: 1,
      friction: 3,
      tension: 40,
      useNativeDriver: true,
    }).start();
  };

  return (
    <Animated.View style={{ transform: [{ scale: springValue }] }}>
      <Button title="Spring" onPress={spring} />
    </Animated.View>
  );
};
```

---

## Native Modules

### Linking Native Libraries
```bash
# Auto-linking (React Native 0.60+)
npm install react-native-package
cd ios && pod install

# Manual linking (if needed)
react-native link react-native-package
```

---

## Performance

### Optimization Tips
```jsx
// Use React.memo for pure components
const MyComponent = React.memo(({ data }) => {
  return <Text>{data}</Text>;
});

// Use useCallback for event handlers
const handlePress = useCallback(() => {
  console.log('Pressed');
}, []);

// Use useMemo for expensive calculations
const expensiveValue = useMemo(() => {
  return calculateExpensiveValue(data);
}, [data]);

// Use FlatList for long lists
<FlatList
  data={data}
  renderItem={renderItem}
  keyExtractor={item => item.id}
  maxToRenderPerBatch={10}
  windowSize={10}
  removeClippedSubviews={true}
/>
```

---

## Testing

### Jest
```bash
npm install --save-dev jest @testing-library/react-native
```

```jsx
import { render, fireEvent } from '@testing-library/react-native';
import MyComponent from './MyComponent';

test('renders correctly', () => {
  const { getByText } = render(<MyComponent />);
  expect(getByText('Hello')).toBeTruthy();
});

test('handles press', () => {
  const onPress = jest.fn();
  const { getByText } = render(<Button onPress={onPress} title="Press" />);
  fireEvent.press(getByText('Press'));
  expect(onPress).toHaveBeenCalled();
});
```

---

## Deployment

### iOS
```bash
# Build release
cd ios
xcodebuild -workspace MyApp.xcworkspace -scheme MyApp -configuration Release

# Archive and upload to App Store
# Use Xcode or Fastlane
```

### Android
```bash
# Generate release APK
cd android
./gradlew assembleRelease

# Generate AAB for Play Store
./gradlew bundleRelease

# Output: android/app/build/outputs/
```

---

## Best Practices

### Component Structure
```jsx
// ✅ Good: Functional component with hooks
import { useState, useEffect } from 'react';
import { View, Text } from 'react-native';

const MyComponent = ({ title }) => {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetchData();
  }, []);

  return (
    <View>
      <Text>{title}</Text>
    </View>
  );
};

export default MyComponent;
```

### Performance
```jsx
// ✅ Good: Use FlatList for lists
<FlatList data={data} renderItem={renderItem} />

// ❌ Bad: Use map in ScrollView
<ScrollView>
  {data.map(item => <Item key={item.id} />)}
</ScrollView>

// ✅ Good: Memoize expensive operations
const value = useMemo(() => expensiveCalculation(data), [data]);

// ✅ Good: Use useCallback for callbacks
const handlePress = useCallback(() => {
  doSomething();
}, []);
```

### Styling
```jsx
// ✅ Good: Use StyleSheet.create
const styles = StyleSheet.create({
  container: { flex: 1 },
});

// ❌ Bad: Inline styles
<View style={{ flex: 1 }} />

// ✅ Good: Separate styles by component
// styles.js
export const styles = StyleSheet.create({...});
```

---

**Pro Tip:** Use TypeScript for better type safety, implement proper error boundaries, optimize FlatList with proper keys and memoization, use React Navigation for routing, and always test on both iOS and Android devices!