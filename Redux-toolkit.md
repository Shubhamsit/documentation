# Redux toolkit

- ok , before learning redux-toolkit, i want to clear some confusion regarding the similar names like redux, redux-toolkit, react-redux

### redux

- redux is js library that is used to manage state and centralising the application state,
- In simple words we can say that if we, want to use certain variables and functions in any of our component in react, no matter where they are present , components can also be nested inside another component and then if we want to access those variables in that nested components . we can use redux to access and even modify them easily.

### redux-toolkit

- redux-toolkit is nothing fancy but a simplified version of redux, a better and easy way to implement redux , there is more abstraction in redux-toolkit, all things are managed internally. you can also use redux directly but if we use redux-toolkit then our effort will be saved and also there will be lesss setup code required in redux-toolkit.
- so in sort redux-toolkit is just implementing redux in easy manner without much effort

### react-redux

- as i have mentioned above that redux is a js library so redux can also be used with any js frontend libraries/framework like angular, vue, react
- so redux or redux-toolkit is independent library for any of these frontend framework/libraries
- but to implement or wiring-up in react application we use react-redux

```
Imp Point :

1. just like we use react (core library) and react-dom (wiring for web app) for web
and,
 react(core library) and react-native (wiring up for mobile) for mobile app

in a similar way we will use redux-toolkit(core library) and react-redux(to wiring up it in react)

```

#### So lets move ahead, but before that i want to introduce some of terminologies and what they meant

### 1. Store

- In redux-toolkit there is a concept of store , it is simply a common place for all variables and functions that you want to use anywhere inside your appplication,

### 2. reducers

- these are just functions that we create to update the values inside our store, simply we apply our logic in reducers that how data should update , for example

```
suppose we want to update a value to be increased by 2 then we will write these logic inside reducer and when we call these reducer then  reducer fn will run and upadte values and also updated value stored in our store and all component will get updated value

so we can say by using reducer we update the values inside store
```

### 3. useDispatch

- these are predefined fn/hooks from react-redux
- useDispatch work is to take value from user or any component from frontend and through reducer it will update value inside store. for example

```
suppose i have an array as a variable in my store and i want to push some values inside it then i will just call useDispatch and through a particular reducer (that we already defined) i will just pass my value
and my reducer fn will just push it inside my store and now any component can use this updated array

```

### 4. useSelector

- this is also prdefined fn/hooks in recat-redux.
- useSelector work is to take values from store

```
as we have seen how to send values from a component to my cetral store using useDispatch

now  if we want to acess that value inside any component in my application regardless of their nesting and where they are present,
we will use useSelector to take values from the store

Note :-  There should be only one store in an application (it is a good practice and recommanded)

```

### 5. slices

- slices in simple term is sub part or sub-store in our application

```
 we could have multiple slices in our application  like for userAuthentication , userPosts etc .

 simply it means like we are keeping the particular set of variables and functions  in a particular slice like post, comments, likes in userPosts slice and username, authtoken etc in our userAuthentication slice thats all  is slice

 slices is just for seperation

```

#### so with this much of basic knowledge about redux we can start setting up redux-toolkit in our application

##### so first install packages required

- open your react project and open terminal install

```
npm install @reduxjs/toolkit

npm install react-redux
```

- also i have explained above why we need to install these both packages, if dont understand please read once again .

```

This is the directory structure i am following in this article

src/
|-- store/
|   |-- userSlice.js
|   |-- store.js
|-- components/
|   |-- SomeComponent.js
|-- App.js
|-- index.js

```


### flow diagram of redux-toolkit

![Sample Image](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*shFCbLbQaFdPTmfrjf1wFg.png)


### now i will let you step by step

### 1. first we will create slice in our react application

- for here i am creating userSlice.js , name of file not necessary should have slice but it is good practice if put filename like that

#### userSlice.js

```javascript
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  userId: null,
  isAuthenticated: false,
  items: [],
};

const userSlice = createSlice({
  name: "user",
  initialState,

  reducers: {
    setUserId: (state, action) => {
      state.userId = action.payload;
    },

    setIsAuthenticated: (state, action) => {
      state.isAuthenticated = action.payload;
    },
    addItem: (state, action) => {
      state.items.push(action.payload);
    },

    // other reducers can be added here
  },
});

export const {setUserId, setIsAuthenticated, addItem} = userSlice.actions;
export default userSlice.reducer;
```

- createSlice is a method which is used to create slice 
- initial state -> it is the initial state of the variables ( these variables  will be avilable throghout application in any component )
- createSlice method takes an  object ans inside it there are three key pairs 
1. name: you can give any name it is just for developer console in browser
2.  initialState: initialState   (it sets the initial value of variables)
3. reducers: it is an object which contains different reduer fn and their declaration also 

#### now lets understand one of reducer function in detail
```javascript

setUserId:(state,action)=>{

     state.userId = action.payload;
}

here all the reducer fn will get acess to state and action .

1. state -> it contains the current state of our store .
state.userId -> it gives the acess to current value of userId from  our store .
similarly we can get acess to values of other variables  from our store like state.isAuthenticated ,state.items etc.

2. action -> action defines what actions need to take on these variables 

action.payload -> it is an object it contains the value that we passed through useDispatch and using this value we will update the  particular variable in our store , so that the updated value can be used by any component that is taking value from our store

so when we do  state.userId = action.payload;
it means we are updating the variable userId that is stored in our store by the value given by any component which is  using useDispatch and that value is taken through action.payload

similarly we have here different reducer fn for differnt uses and variables like adding value in array .

```
- export const {setUserId, setIsAuthenticated, addItem} = userSlice.actions;
above line exports different reducers we have defined so that they can be used in any of component by importing it 

- export default userSlice.reducer;    (this line exports all reducer and it will be used when we will configure our store )

### 2. Now  we will configure Store

```javascript
// src/store/store.js
import { configureStore } from '@reduxjs/toolkit';
import userReducer from './UserSlice.js';

const store = configureStore({
  reducer:userReducer
});

export default store;

```
- configureStore is method to create store 
- userReducer is the list of all reducer of a slice named UserSlice comes from (export default userSlice.reducer ) 

- reducer:userReducer (this gives information to store about the list of reducers so that store can easily identify them and can can make the changes in the store using them )

### 3. now we will have a component in which we will test these functionalities of useDispatch and useSelector


- someComponent.js
```javascript
// src/components/SomeComponent.js
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { setUserId, setIsAuthenticated, addItem } from '../store/userSlice';

const SomeComponent = () => {
  const dispatch = useDispatch();

  const userId = useSelector((state) => state.user.userId);

  const isAuthenticated = useSelector((state) => state.user.isAuthenticated);

  const items = useSelector((state) => state.user.items);

  const handleSetUserId = () => {
    dispatch(setUserId('newUserId123'));
  };

  const handleSetAuthentication = () => {
    dispatch(setIsAuthenticated(true));
  };

  const handleAddItem = () => {
    dispatch(addItem('New Item'));
  };

  return (
    <div>
      <h1>User Information</h1>
      <p>User ID: {userId}</p>
      <p>Authenticated: {isAuthenticated ? 'Yes' : 'No'}</p>
      <p>Items: {items.join(', ')}</p>
      <button onClick={handleSetUserId}>Set User ID</button>
      <button onClick={handleSetAuthentication}>Set Authentication</button>
      <button onClick={handleAddItem}>Add Item</button>
    </div>
  );
};

export default SomeComponent;



```

#### first we  will look into useSelector

```javascript

import { useSelector, useDispatch } from 'react-redux'; // useSelector  should be imported first

  const userId = useSelector((state) => state.user.userId);

useSelector is used to  take values from store 

useSelector takes a callback fn in which it has acess to state props 

 To take value from store we will take use of this state props which contains the  all variables inside our store 

state.user -> it refers to the userSlice which we have created  'user' here is the name which we have  given to our slice when we had created it.

state.user.userId ->  this will give the value of userId from store ( so by this method we can take acess to userId in any component throughout our application )

 Similarly we can access isAuthenticated, items etc. from store  easily 

```


#### Now we will look into useDispatch

```javascript

import { useSelector, useDispatch } from 'react-redux'; // first useDispatch should be imported

const dispatch = useDispatch(); // take refrence  of useDispatch() in dispatch

useDispatch is used to update value of a variable inside store by using a  particular reducer function which we have defined earlier.


  dispatch(setUserId('newUserId123'));

dipatch is the refrence that we have created above, using this we have called a dispatch function "setUserId" inside it  and passes the new value of userId "newUserId123" to update it 

so  when dispatch will run it will call setUserId and take "newUserId123"  as payload  and then the "setUserId" reducer function will execute it will have acess to this payload using action.payload (see in userSlice.js code )  and thus the value will  bw updated in the store 


```

### 4. Now we will import this component in App.js

```javascript
// src/App.js
import React from 'react';
import SomeComponent from './components/SomeComponent';

const App = () => {
  return (
    <div>
      <h1>Redux Toolkit Example</h1>
      <SomeComponent />
    </div>
  );
};

export default App;

```

### 5. Now in index.js  we have to wrap our whole application with Provider which contain the store so that our whole application can use store 

```javascript
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import store from './store/store';
import App from './App';

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);

```

- import { Provider } from 'react-redux';
(it is used to wrap the application with store)

```javascript
 <Provider store={store}>
    <App />
  </Provider>,


here we have wraped the whole application App inside a provier and which provide store access to all components in our application
```




#### thats all requied for setting up redux-toolkit  in our react application and you are ready to go 


#### Thankyou for Reading 
Shubham - kumar


