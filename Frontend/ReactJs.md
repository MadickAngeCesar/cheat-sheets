# React.js Cheat Sheet - Beginner to Pro

## Table of Contents
- [Setup & Basics](#setup--basics)
- [JSX](#jsx)
- [Components](#components)
- [Props](#props)
- [State](#state)
- [Hooks](#hooks)
- [Events](#events)
- [Conditional Rendering](#conditional-rendering)
- [Lists & Keys](#lists--keys)
- [Forms](#forms)
- [Refs](#refs)
- [Context API](#context-api)
- [Performance Optimization](#performance-optimization)
- [Custom Hooks](#custom-hooks)
- [Error Boundaries](#error-boundaries)
- [Portals](#portals)
- [Code Splitting](#code-splitting)
- [Testing](#testing)
- [Best Practices](#best-practices)

---

## Setup & Basics

### Create React App
```bash
# Create new app
npx create-react-app my-app
cd my-app
npm start

# With TypeScript
npx create-react-app my-app --template typescript

# With Vite (faster alternative)
npm create vite@latest my-app -- --template react
npm create vite@latest my-app -- --template react-ts
```

### Basic Component
```jsx
import React from 'react';

function App() {
    return (
        <div className="App">
            <h1>Hello, React!</h1>
        </div>
    );
}

export default App;
```

---

## JSX

### JSX Syntax
```jsx
// JavaScript expressions in JSX
const name = 'John';
const element = <h1>Hello, {name}!</h1>;

// Attributes
const element = <div className="container" id="main"></div>;

// Inline styles (camelCase)
const element = <div style={{ color: 'red', fontSize: '16px' }}>Text</div>;

// Children
const element = (
    <div>
        <h1>Title</h1>
        <p>Paragraph</p>
    </div>
);

// Self-closing tags
const element = <img src="image.jpg" alt="Description" />;

// Fragments (return multiple elements)
const element = (
    <>
        <h1>Title</h1>
        <p>Paragraph</p>
    </>
);

// Or with React.Fragment
const element = (
    <React.Fragment>
        <h1>Title</h1>
        <p>Paragraph</p>
    </React.Fragment>
);
```

### JavaScript in JSX
```jsx
// Expressions
const element = <h1>{2 + 2}</h1>; // 4

// Function calls
const getName = () => 'John';
const element = <h1>Hello, {getName()}!</h1>;

// Ternary operator
const isLoggedIn = true;
const element = <div>{isLoggedIn ? 'Welcome' : 'Please login'}</div>;

// Logical AND
const messages = ['msg1', 'msg2'];
const element = <div>{messages.length > 0 && <p>You have messages</p>}</div>;

// Comments
const element = (
    <div>
        {/* This is a comment */}
        <h1>Title</h1>
    </div>
);
```

---

## Components

### Function Components
```jsx
// Basic function component
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}

// Arrow function component
const Welcome = (props) => {
    return <h1>Hello, {props.name}</h1>;
};

// Implicit return
const Welcome = (props) => <h1>Hello, {props.name}</h1>;

// With destructuring
const Welcome = ({ name, age }) => {
    return (
        <div>
            <h1>Hello, {name}</h1>
            <p>Age: {age}</p>
        </div>
    );
};
```

### Class Components (Legacy)
```jsx
import React, { Component } from 'react';

class Welcome extends Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0
        };
    }
    
    componentDidMount() {
        // Runs after component mounts
    }
    
    componentDidUpdate(prevProps, prevState) {
        // Runs after component updates
    }
    
    componentWillUnmount() {
        // Cleanup before component unmounts
    }
    
    handleClick = () => {
        this.setState({ count: this.state.count + 1 });
    }
    
    render() {
        return (
            <div>
                <h1>Hello, {this.props.name}</h1>
                <p>Count: {this.state.count}</p>
                <button onClick={this.handleClick}>Increment</button>
            </div>
        );
    }
}
```

### Component Composition
```jsx
function App() {
    return (
        <div>
            <Header />
            <Main />
            <Footer />
        </div>
    );
}

function Header() {
    return <header><h1>My App</h1></header>;
}

function Main() {
    return <main><p>Content</p></main>;
}

function Footer() {
    return <footer><p>Footer</p></footer>;
}
```

---

## Props

### Passing Props
```jsx
// Pass props to component
<Welcome name="John" age={30} />

// Spread props
const user = { name: 'John', age: 30 };
<Welcome {...user} />

// Children prop
<Container>
    <h1>Title</h1>
    <p>Content</p>
</Container>

function Container({ children }) {
    return <div className="container">{children}</div>;
}
```

### Props Destructuring
```jsx
// Without destructuring
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}

// With destructuring
function Welcome({ name, age }) {
    return (
        <div>
            <h1>Hello, {name}</h1>
            <p>Age: {age}</p>
        </div>
    );
}

// With default values
function Welcome({ name = 'Guest', age = 0 }) {
    return <h1>Hello, {name}</h1>;
}

// Rest props
function Welcome({ name, ...rest }) {
    return <div {...rest}>Hello, {name}</div>;
}
```

### PropTypes (Validation)
```jsx
import PropTypes from 'prop-types';

function Welcome({ name, age, email, children }) {
    return <div>Hello, {name}</div>;
}

Welcome.propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number,
    email: PropTypes.string,
    children: PropTypes.node,
    onClick: PropTypes.func,
    user: PropTypes.shape({
        id: PropTypes.number,
        name: PropTypes.string
    }),
    items: PropTypes.arrayOf(PropTypes.string),
    status: PropTypes.oneOf(['active', 'inactive']),
    value: PropTypes.oneOfType([
        PropTypes.string,
        PropTypes.number
    ])
};

Welcome.defaultProps = {
    age: 0,
    email: 'noemail@example.com'
};
```

---

## State

### useState Hook
```jsx
import { useState } from 'react';

function Counter() {
    // Declare state variable
    const [count, setCount] = useState(0);
    
    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>Increment</button>
            <button onClick={() => setCount(count - 1)}>Decrement</button>
            <button onClick={() => setCount(0)}>Reset</button>
        </div>
    );
}
```

### Multiple State Variables
```jsx
function Form() {
    const [name, setName] = useState('');
    const [email, setEmail] = useState('');
    const [age, setAge] = useState(0);
    
    return (
        <form>
            <input 
                value={name} 
                onChange={(e) => setName(e.target.value)} 
            />
            <input 
                value={email} 
                onChange={(e) => setEmail(e.target.value)} 
            />
            <input 
                type="number"
                value={age} 
                onChange={(e) => setAge(Number(e.target.value))} 
            />
        </form>
    );
}
```

### Object State
```jsx
function UserForm() {
    const [user, setUser] = useState({
        name: '',
        email: '',
        age: 0
    });
    
    const handleChange = (e) => {
        setUser({
            ...user,
            [e.target.name]: e.target.value
        });
    };
    
    return (
        <form>
            <input 
                name="name"
                value={user.name} 
                onChange={handleChange} 
            />
            <input 
                name="email"
                value={user.email} 
                onChange={handleChange} 
            />
        </form>
    );
}
```

### Array State
```jsx
function TodoList() {
    const [todos, setTodos] = useState([]);
    
    const addTodo = (text) => {
        setTodos([...todos, { id: Date.now(), text }]);
    };
    
    const removeTodo = (id) => {
        setTodos(todos.filter(todo => todo.id !== id));
    };
    
    const updateTodo = (id, newText) => {
        setTodos(todos.map(todo => 
            todo.id === id ? { ...todo, text: newText } : todo
        ));
    };
    
    return (
        <div>
            {todos.map(todo => (
                <div key={todo.id}>
                    {todo.text}
                    <button onClick={() => removeTodo(todo.id)}>Delete</button>
                </div>
            ))}
        </div>
    );
}
```

### Lazy Initial State
```jsx
// Expensive computation only runs once
const [state, setState] = useState(() => {
    const initialState = computeExpensiveValue();
    return initialState;
});
```

---

## Hooks

### useEffect Hook
```jsx
import { useEffect } from 'react';

function Component() {
    // Run on every render
    useEffect(() => {
        console.log('Component rendered');
    });
    
    // Run once on mount
    useEffect(() => {
        console.log('Component mounted');
    }, []);
    
    // Run when dependencies change
    useEffect(() => {
        console.log('Count changed');
    }, [count]);
    
    // Cleanup function
    useEffect(() => {
        const timer = setInterval(() => {
            console.log('Tick');
        }, 1000);
        
        return () => {
            clearInterval(timer);
        };
    }, []);
    
    return <div>Component</div>;
}
```

### useEffect Examples
```jsx
// Fetch data
function UserProfile({ userId }) {
    const [user, setUser] = useState(null);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
        setLoading(true);
        fetch(`/api/users/${userId}`)
            .then(res => res.json())
            .then(data => {
                setUser(data);
                setLoading(false);
            });
    }, [userId]);
    
    if (loading) return <div>Loading...</div>;
    return <div>{user.name}</div>;
}

// Event listeners
function WindowSize() {
    const [size, setSize] = useState({
        width: window.innerWidth,
        height: window.innerHeight
    });
    
    useEffect(() => {
        const handleResize = () => {
            setSize({
                width: window.innerWidth,
                height: window.innerHeight
            });
        };
        
        window.addEventListener('resize', handleResize);
        
        return () => {
            window.removeEventListener('resize', handleResize);
        };
    }, []);
    
    return <div>{size.width} x {size.height}</div>;
}
```

### useContext Hook
```jsx
import { createContext, useContext } from 'react';

// Create context
const ThemeContext = createContext('light');

// Provider
function App() {
    return (
        <ThemeContext.Provider value="dark">
            <Toolbar />
        </ThemeContext.Provider>
    );
}

// Consumer
function ThemedButton() {
    const theme = useContext(ThemeContext);
    return <button className={theme}>Button</button>;
}
```

### useRef Hook
```jsx
import { useRef } from 'react';

function TextInput() {
    const inputRef = useRef(null);
    
    const focusInput = () => {
        inputRef.current.focus();
    };
    
    return (
        <div>
            <input ref={inputRef} type="text" />
            <button onClick={focusInput}>Focus Input</button>
        </div>
    );
}

// Store mutable value
function Timer() {
    const intervalRef = useRef(null);
    const [count, setCount] = useState(0);
    
    useEffect(() => {
        intervalRef.current = setInterval(() => {
            setCount(c => c + 1);
        }, 1000);
        
        return () => clearInterval(intervalRef.current);
    }, []);
    
    return <div>{count}</div>;
}
```

### useReducer Hook
```jsx
import { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
    switch (action.type) {
        case 'increment':
            return { count: state.count + 1 };
        case 'decrement':
            return { count: state.count - 1 };
        case 'reset':
            return initialState;
        default:
            throw new Error('Unknown action type');
    }
}

function Counter() {
    const [state, dispatch] = useReducer(reducer, initialState);
    
    return (
        <div>
            <p>Count: {state.count}</p>
            <button onClick={() => dispatch({ type: 'increment' })}>+</button>
            <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
            <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
        </div>
    );
}
```

### useMemo Hook
```jsx
import { useMemo } from 'react';

function ExpensiveComponent({ items, filterText }) {
    // Only recalculate when items or filterText changes
    const filteredItems = useMemo(() => {
        return items.filter(item => 
            item.name.toLowerCase().includes(filterText.toLowerCase())
        );
    }, [items, filterText]);
    
    return (
        <ul>
            {filteredItems.map(item => (
                <li key={item.id}>{item.name}</li>
            ))}
        </ul>
    );
}
```

### useCallback Hook
```jsx
import { useCallback } from 'react';

function Parent() {
    const [count, setCount] = useState(0);
    
    // Memoize callback to prevent child re-renders
    const handleClick = useCallback(() => {
        console.log('Clicked');
    }, []); // Only create once
    
    return (
        <div>
            <Child onClick={handleClick} />
            <button onClick={() => setCount(count + 1)}>
                Count: {count}
            </button>
        </div>
    );
}

const Child = React.memo(({ onClick }) => {
    console.log('Child rendered');
    return <button onClick={onClick}>Click</button>;
});
```

### useLayoutEffect Hook
```jsx
import { useLayoutEffect, useRef } from 'react';

// Runs synchronously after DOM mutations
function Component() {
    const ref = useRef(null);
    
    useLayoutEffect(() => {
        const { height } = ref.current.getBoundingClientRect();
        console.log('Height:', height);
    }, []);
    
    return <div ref={ref}>Content</div>;
}
```

### useImperativeHandle Hook
```jsx
import { forwardRef, useImperativeHandle, useRef } from 'react';

const CustomInput = forwardRef((props, ref) => {
    const inputRef = useRef();
    
    useImperativeHandle(ref, () => ({
        focus: () => {
            inputRef.current.focus();
        },
        clear: () => {
            inputRef.current.value = '';
        }
    }));
    
    return <input ref={inputRef} />;
});

function Parent() {
    const inputRef = useRef();
    
    return (
        <div>
            <CustomInput ref={inputRef} />
            <button onClick={() => inputRef.current.focus()}>Focus</button>
            <button onClick={() => inputRef.current.clear()}>Clear</button>
        </div>
    );
}
```

---

## Events

### Event Handling
```jsx
function Button() {
    const handleClick = (e) => {
        e.preventDefault();
        console.log('Button clicked');
    };
    
    return <button onClick={handleClick}>Click Me</button>;
}

// Inline handler
<button onClick={() => console.log('Clicked')}>Click</button>

// Pass parameters
<button onClick={() => handleClick(id)}>Click</button>

// Prevent default
<form onSubmit={(e) => {
    e.preventDefault();
    handleSubmit();
}}>
```

### Common Events
```jsx
// Mouse events
<div
    onClick={handleClick}
    onDoubleClick={handleDoubleClick}
    onMouseEnter={handleMouseEnter}
    onMouseLeave={handleMouseLeave}
    onMouseMove={handleMouseMove}
/>

// Form events
<input
    onChange={handleChange}
    onFocus={handleFocus}
    onBlur={handleBlur}
    onSubmit={handleSubmit}
/>

// Keyboard events
<input
    onKeyDown={handleKeyDown}
    onKeyUp={handleKeyUp}
    onKeyPress={handleKeyPress}
/>

// Event object properties
function handleEvent(e) {
    e.target;          // Element that triggered event
    e.currentTarget;   // Element with event listener
    e.preventDefault();
    e.stopPropagation();
    e.type;
    e.key;             // For keyboard events
    e.clientX;         // For mouse events
}
```

---

## Conditional Rendering

### If/Else
```jsx
function Greeting({ isLoggedIn }) {
    if (isLoggedIn) {
        return <h1>Welcome back!</h1>;
    }
    return <h1>Please sign in.</h1>;
}
```

### Ternary Operator
```jsx
function Greeting({ isLoggedIn }) {
    return (
        <div>
            {isLoggedIn ? (
                <h1>Welcome back!</h1>
            ) : (
                <h1>Please sign in.</h1>
            )}
        </div>
    );
}
```

### Logical AND (&&)
```jsx
function Mailbox({ unreadMessages }) {
    return (
        <div>
            <h1>Hello!</h1>
            {unreadMessages.length > 0 && (
                <h2>You have {unreadMessages.length} unread messages.</h2>
            )}
        </div>
    );
}
```

### Switch Statement
```jsx
function StatusMessage({ status }) {
    switch (status) {
        case 'loading':
            return <div>Loading...</div>;
        case 'error':
            return <div>Error occurred</div>;
        case 'success':
            return <div>Success!</div>;
        default:
            return null;
    }
}
```

### Prevent Rendering
```jsx
function Warning({ warn }) {
    if (!warn) {
        return null; // Don't render anything
    }
    
    return <div className="warning">Warning!</div>;
}
```

---

## Lists & Keys

### Rendering Lists
```jsx
function TodoList({ todos }) {
    return (
        <ul>
            {todos.map(todo => (
                <li key={todo.id}>{todo.text}</li>
            ))}
        </ul>
    );
}
```

### Keys
```jsx
// Use unique IDs
<li key={item.id}>{item.name}</li>

// Index as key (only if items don't reorder)
<li key={index}>{item.name}</li>

// Don't use random values
// Bad: <li key={Math.random()}>{item.name}</li>
```

### Lists with Components
```jsx
function TodoItem({ todo }) {
    return <li>{todo.text}</li>;
}

function TodoList({ todos }) {
    return (
        <ul>
            {todos.map(todo => (
                <TodoItem key={todo.id} todo={todo} />
            ))}
        </ul>
    );
}
```

### Filtering Lists
```jsx
function FilteredList({ items, filter }) {
    const filteredItems = items.filter(item => 
        item.name.toLowerCase().includes(filter.toLowerCase())
    );
    
    return (
        <ul>
            {filteredItems.map(item => (
                <li key={item.id}>{item.name}</li>
            ))}
        </ul>
    );
}
```

---

## Forms

### Controlled Components
```jsx
function Form() {
    const [name, setName] = useState('');
    const [email, setEmail] = useState('');
    
    const handleSubmit = (e) => {
        e.preventDefault();
        console.log({ name, email });
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <input
                type="text"
                value={name}
                onChange={(e) => setName(e.target.value)}
                placeholder="Name"
            />
            <input
                type="email"
                value={email}
                onChange={(e) => setEmail(e.target.value)}
                placeholder="Email"
            />
            <button type="submit">Submit</button>
        </form>
    );
}
```

### Multiple Inputs
```jsx
function Form() {
    const [formData, setFormData] = useState({
        name: '',
        email: '',
        age: ''
    });
    
    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData(prev => ({
            ...prev,
            [name]: value
        }));
    };
    
    return (
        <form>
            <input
                name="name"
                value={formData.name}
                onChange={handleChange}
            />
            <input
                name="email"
                value={formData.email}
                onChange={handleChange}
            />
            <input
                name="age"
                type="number"
                value={formData.age}
                onChange={handleChange}
            />
        </form>
    );
}
```

### Checkboxes & Radio Buttons
```jsx
function Form() {
    const [agreed, setAgreed] = useState(false);
    const [gender, setGender] = useState('');
    
    return (
        <form>
            {/* Checkbox */}
            <label>
                <input
                    type="checkbox"
                    checked={agreed}
                    onChange={(e) => setAgreed(e.target.checked)}
                />
                I agree
            </label>
            
            {/* Radio buttons */}
            <label>
                <input
                    type="radio"
                    value="male"
                    checked={gender === 'male'}
                    onChange={(e) => setGender(e.target.value)}
                />
                Male
            </label>
            <label>
                <input
                    type="radio"
                    value="female"
                    checked={gender === 'female'}
                    onChange={(e) => setGender(e.target.value)}
                />
                Female
            </label>
        </form>
    );
}
```

### Select Dropdown
```jsx
function Form() {
    const [country, setCountry] = useState('');
    
    return (
        <select value={country} onChange={(e) => setCountry(e.target.value)}>
            <option value="">Select country</option>
            <option value="us">United States</option>
            <option value="uk">United Kingdom</option>
            <option value="ca">Canada</option>
        </select>
    );
}
```

### Uncontrolled Components
```jsx
function Form() {
    const nameRef = useRef();
    const emailRef = useRef();
    
    const handleSubmit = (e) => {
        e.preventDefault();
        console.log({
            name: nameRef.current.value,
            email: emailRef.current.value
        });
    };
    
    return (
        <form onSubmit={handleSubmit}>
            <input ref={nameRef} type="text" defaultValue="John" />
            <input ref={emailRef} type="email" />
            <button type="submit">Submit</button>
        </form>
    );
}
```

---

## Refs

### Creating Refs
```jsx
import { useRef } from 'react';

function Component() {
    const myRef = useRef(null);
    
    return <div ref={myRef}>Content</div>;
}
```

### Accessing DOM Elements
```jsx
function TextInput() {
    const inputRef = useRef(null);
    
    const focusInput = () => {
        inputRef.current.focus();
    };
    
    const getValue = () => {
        console.log(inputRef.current.value);
    };
    
    return (
        <>
            <input ref={inputRef} type="text" />
            <button onClick={focusInput}>Focus</button>
            <button onClick={getValue}>Get Value</button>
        </>
    );
}
```

### Forwarding Refs
```jsx
const FancyButton = React.forwardRef((props, ref) => (
    <button ref={ref} className="fancy-button">
        {props.children}
    </button>
));

function App() {
    const buttonRef = useRef();
    
    return <FancyButton ref={buttonRef}>Click me</FancyButton>;
}
```

### Callback Refs
```jsx
function Component() {
    const [height, setHeight] = useState(0);
    
    const measureRef = useCallback(node => {
        if (node !== null) {
            setHeight(node.getBoundingClientRect().height);
        }
    }, []);
    
    return (
        <>
            <div ref={measureRef}>Measure me</div>
            <p>Height: {height}px</p>
        </>
    );
}
```

---

## Context API

### Creating Context
```jsx
import { createContext, useContext, useState } from 'react';

// Create context
const UserContext = createContext(null);

// Provider component
function UserProvider({ children }) {
    const [user, setUser] = useState(null);
    
    return (
        <UserContext.Provider value={{ user, setUser }}>
            {children}
        </UserContext.Provider>
    );
}

// Custom hook to use context
function useUser() {
    const context = useContext(UserContext);
    if (!context) {
        throw new Error('useUser must be used within UserProvider');
    }
    return context;
}

// Usage
function App() {
    return (
        <UserProvider>
            <Dashboard />
        </UserProvider>
    );
}

function Dashboard() {
    const { user, setUser } = useUser();
    
    return (
        <div>
            {user ? (
                <p>Welcome, {user.name}</p>
            ) : (
                <button onClick={() => setUser({ name: 'John' })}>
                    Login
                </button>
            )}
        </div>
    );
}
```

### Multiple Contexts
```jsx
const ThemeContext = createContext('light');
const UserContext = createContext(null);

function App() {
    return (
        <ThemeContext.Provider value="dark">
            <UserContext.Provider value={{ name: 'John' }}>
                <Dashboard />
            </UserContext.Provider>
        </ThemeContext.Provider>
    );
}

function Dashboard() {
    const theme = useContext(ThemeContext);
    const user = useContext(UserContext);
    
    return <div className={theme}>Welcome, {user.name}</div>;
}
```

---

## Performance Optimization

### React.memo
```jsx
// Prevents re-render if props haven't changed
const ExpensiveComponent = React.memo(function ExpensiveComponent({ data }) {
    console.log('Rendering ExpensiveComponent');
    return <div>{data}</div>;
});

// Custom comparison
const Component = React.memo(
    function Component({ user }) {
        return <div>{user.name}</div>;
    },
    (prevProps, nextProps) => {
        return prevProps.user.id === nextProps.user.id;
    }
);
```

### useMemo
```jsx
function Component({ items }) {
    // Memoize expensive calculation
    const total = useMemo(() => {
        return items.reduce((sum, item) => sum + item.price, 0);
    }, [items]);
    
    return <div>Total: ${total}</div>;
}
```

### useCallback
```jsx
function Parent() {
    const [count, setCount] = useState(0);
    
    // Memoize callback
    const handleClick = useCallback(() => {
        console.log('Clicked');
    }, []);
    
    return (
        <>
            <Child onClick={handleClick} />
            <button onClick={() => setCount(c => c + 1)}>
                Count: {count}
            </button>
        </>
    );
}

const Child = React.memo(({ onClick }) => {
    return <button onClick={onClick}>Click</button>;
});
```

### Lazy Loading
```jsx
import { lazy, Suspense } from 'react';

// Lazy load component
const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
    return (
        <Suspense fallback={<div>Loading...</div>}>
            <HeavyComponent />
        </Suspense>
    );
}
```

### Virtualization (react-window)
```jsx
import { FixedSizeList } from 'react-window';

function VirtualList({ items }) {
    const Row = ({ index, style }) => (
        <div style={style}>
            {items[index]}
        </div>
    );
    
    return (
        <FixedSizeList
            height={600}
            itemCount={items.length}
            itemSize={35}
            width="100%"
        >
            {Row}
        </FixedSizeList>
    );
}
```

---

## Custom Hooks

### Creating Custom Hooks
```jsx
// useFetch hook
function useFetch(url) {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);
    
    useEffect(() => {
        setLoading(true);
        fetch(url)
            .then(res => res.json())
            .then(data => {
                setData(data);
                setLoading(false);
            })
            .catch(err => {
                setError(err);
                setLoading(false);
            });
    }, [url]);
    
    return { data, loading, error };
}

// Usage
function Component() {
    const { data, loading, error } = useFetch('/api/users');
    
    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error.message}</div>;
    return <div>{data.name}</div>;
}
```

### More Custom Hooks Examples
```jsx
// useLocalStorage hook
function useLocalStorage(key, initialValue) {
    const [value, setValue] = useState(() => {
        const item = localStorage.getItem(key);
        return item ? JSON.parse(item) : initialValue;
    });
    
    const setStoredValue = (newValue) => {
        setValue(newValue);
        localStorage.setItem(key, JSON.stringify(newValue));
    };
    
    return [value, setStoredValue];
}

// useToggle hook
function useToggle(initialValue = false) {
    const [value, setValue] = useState(initialValue);
    const toggle = useCallback(() => setValue(v => !v), []);
    return [value, toggle];
}

// usePrevious hook
function usePrevious(value) {
    const ref = useRef();
    useEffect(() => {
        ref.current = value;
    }, [value]);
    return ref.current;
}

// useDebounce hook
function useDebounce(value, delay) {
    const [debouncedValue, setDebouncedValue] = useState(value);
    
    useEffect(() => {
        const handler = setTimeout(() => {
            setDebouncedValue(value);
        }, delay);
        
        return () => {
            clearTimeout(handler);
        };
    }, [value, delay]);
    
    return debouncedValue;
}
```

---

## Error Boundaries

### Creating Error Boundary
```jsx
class ErrorBoundary extends React.Component {
    constructor(props) {
        super(props);
        this.state = { hasError: false, error: null };
    }
    
    static getDerivedStateFromError(error) {
        return { hasError: true, error };
    }
    
    componentDidCatch(error, errorInfo) {
        console.error('Error caught:', error, errorInfo);
    }
    
    render() {
        if (this.state.hasError) {
            return (
                <div>
                    <h1>Something went wrong</h1>
                    <p>{this.state.error.message}</p>
                </div>
            );
        }
        
        return this.props.children;
    }
}

// Usage
function App() {
    return (
        <ErrorBoundary>
            <BuggyComponent />
        </ErrorBoundary>
    );
}
```

---

## Portals

### Creating Portals
```jsx
import { createPortal } from 'react-dom';

function Modal({ children, isOpen }) {
    if (!isOpen) return null;
    
    return createPortal(
        <div className="modal-backdrop">
            <div className="modal">
                {children}
            </div>
        </div>,
        document.getElementById('modal-root')
    );
}

// Usage
function App() {
    const [isOpen, setIsOpen] = useState(false);
    
    return (
        <>
            <button onClick={() => setIsOpen(true)}>Open Modal</button>
            <Modal isOpen={isOpen}>
                <h2>Modal Content</h2>
                <button onClick={() => setIsOpen(false)}>Close</button>
            </Modal>
        </>
    );
}
```

---

## Code Splitting

### Dynamic Import
```jsx
import { lazy, Suspense } from 'react';

const OtherComponent = lazy(() => import('./OtherComponent'));

function App() {
    return (
        <Suspense fallback={<div>Loading...</div>}>
            <OtherComponent />
        </Suspense>
    );
}
```

### Route-based Code Splitting
```jsx
import { lazy, Suspense } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Contact = lazy(() => import('./pages/Contact'));

function App() {
    return (
        <BrowserRouter>
            <Suspense fallback={<div>Loading...</div>}>
                <Routes>
                    <Route path="/" element={<Home />} />
                    <Route path="/about" element={<About />} />
                    <Route path="/contact" element={<Contact />} />
                </Routes>
            </Suspense>
        </BrowserRouter>
    );
}
```

---

## Testing

### Jest & React Testing Library
```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import '@testing-library/jest-dom';
import Button from './Button';

test('renders button with text', () => {
    render(<Button>Click me</Button>);
    const button = screen.getByText(/click me/i);
    expect(button).toBeInTheDocument();
});

test('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    const button = screen.getByText(/click me/i);
    fireEvent.click(button);
    
    expect(handleClick).toHaveBeenCalledTimes(1);
});

test('input changes value', () => {
    render(<input />);
    const input = screen.getByRole('textbox');
    
    fireEvent.change(input, { target: { value: 'Hello' } });
    
    expect(input.value).toBe('Hello');
});
```

---

## Best Practices

### Component Structure
```jsx
// 1. Imports
import { useState, useEffect } from 'react';
import PropTypes from 'prop-types';
import { helperFunction } from './utils';
import './Component.css';

// 2. Component definition
function Component({ prop1, prop2 }) {
    // 3. State
    const [state, setState] = useState(initialValue);
    
    // 4. Effects
    useEffect(() => {
        // Side effects
    }, [dependencies]);
    
    // 5. Event handlers
    const handleClick = () => {
        // Handle event
    };
    
    // 6. Helper functions
    const formatData = (data) => {
        return data.toUpperCase();
    };
    
    // 7. Early returns
    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error</div>;
    
    // 8. Main render
    return (
        <div>
            <h1>{prop1}</h1>
            <button onClick={handleClick}>{prop2}</button>
        </div>
    );
}

// 9. PropTypes
Component.propTypes = {
    prop1: PropTypes.string.isRequired,
    prop2: PropTypes.string
};

// 10. Default props
Component.defaultProps = {
    prop2: 'Default value'
};

// 11. Export
export default Component;
```

### Naming Conventions
```jsx
// Components: PascalCase
function UserProfile() {}

// Hooks: useCamelCase
function useCustomHook() {}

// Event handlers: handleEventName
const handleClick = () => {};
const handleSubmit = () => {};

// Boolean props: is/has prefix
<Component isVisible hasError />

// Array/Object state: plural
const [users, setUsers] = useState([]);
const [user, setUser] = useState({});
```

### State Management Tips
```jsx
// Keep state as close to where it's used as possible
// Lift state up only when multiple components need it

// Use reducer for complex state logic
const [state, dispatch] = useReducer(reducer, initialState);

// Separate UI state from server state
const [loading, setLoading] = useState(false); // UI state
const [users, setUsers] = useState([]); // Server state

// Avoid redundant state
// Bad
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const [fullName, setFullName] = useState('');

// Good
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const fullName = `${firstName} ${lastName}`;
```

### Performance Tips
```jsx
// Use React.memo for expensive components
const ExpensiveComponent = React.memo(Component);

// Use useCallback for event handlers passed to children
const handleClick = useCallback(() => {}, []);

// Use useMemo for expensive calculations
const value = useMemo(() => expensiveCalculation(), [dep]);

// Lazy load heavy components
const HeavyComponent = lazy(() => import('./Heavy'));

// Virtualize long lists
// Use react-window or react-virtualized

// Avoid inline object/array creation in render
// Bad: <Component style={{ margin: 10 }} />
// Good: const style = { margin: 10 }; <Component style={style} />
```

---

**Pro Tip:** Master hooks, embrace component composition, optimize performance with memoization, and always keep components small and focused!