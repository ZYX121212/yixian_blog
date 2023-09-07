

- [Redux Tookit的使用](#redux-tookit的使用)
    - [安装redux tookit依赖](#安装redux-tookit依赖)
    - [创建store](#创建store)

# Redux Tookit的使用

-- 本文默认大家都创建好了react-app项目
因为个人使用的是npm语法，所以以下不会涉及到yarn语法

<!-- omit from toc -->
## 使用步骤()，
### 安装redux tookit依赖
```
npm install @reduxjs/tookit

```

-------

### 创建store
1. 目录格式 `src/store/index.js`，这里的index.js与全局的Index.js是同样功能，都是作为入口，当然，你也可以使用其他的来作为入口，但一般来说没有必要。下一步在index.js中码代码(鉴于通用性，所以先开始创建结构，不会码完所有的代码)


```jsx 
import {  configureStore } from '@reduxjs/toolkit'
const store configureStore =({

  reducer:{

  }
})

export default store
```


2. 目前我们创建了store仓库，仓库是要在全局使用的所以，我们要在全局的入口处，作为参数传递下去，全局入口为`src/index.js`

```jsx
<!-- 因为我们在store中设置了index.js为store的入口，所以不用from './store/index' -->

import store from './store';
import { Provider } from 'react-redux';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Provider store={store}>
    <App />
  </Provider>

);
```
3. 现在我们的store中没有任何数据，这自然是不行的，但是由于store中的数据来自于切片中，所以我们要先建立slice
```jsx
import { createSlice } from "@reduxjs/toolkit";

export const postSlice = createSlice({
  name: 'posts',
  initialState: initialState,
  reducers: {
    postAdded(state, action) {
      state.push(action.payload);
    }
  }
})

export const { postAdded } = postSlice.actions;

export default postSlice.reducer;
```
|每一个函数都有参数，都有一个规定，而createSlice的基本参数是
`name` `initialState` `reducers`
* `name`  
  是这个切片的名字(好像是句废话)
* `initialState`
  是这个切片中的state的初始值
* `reducers`
  是与此切片state值的变化相关的函数集合，因为是函数集合，并且与state的变化相关，所以reducer中可以创建多个函数来修改更新state，那么就要有state的入口，所以参数state就是这个name切片的state，action是一个辅助参数，他当中有个paload用于辅助改变state，比如说赋值给state中的value，那么可以这样写
  ```
  state.value=actoion.payload
  ```
4. 现在创建好了切片，那么我们要将这些(捏可以根据需要创建不同的切片，步骤和第三步中一样)切片，为了便于管理我们将这些切片放到store中，那么就要回到`src/store/index.js`中了。  
    还记得store中，有一个`reducer:{}`吧。这个就是将切片同意管的容器，比如我们想把刚刚创建的posts切片放进去。

    ```jsx
    <!-- 导入切片 -->
    import postsReducer from './postSlice'
    const store = configureStore({

      reducer: {
        posts: postsReducer,
      }

    });

    ```
    posts就是我们之前起的 `name`，postReducer就是这个切片。 


**到目前为止，我们就创建好了一个redux tookit的store了，是不是感觉步骤很简单，**  
1. 那现在开始我们使用store吧

