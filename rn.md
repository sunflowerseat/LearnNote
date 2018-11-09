# ReactNative 笔记



- jsonString 转jsonObject

  ```javascript
  var str = '{"name":"fancy"}'
  var jsonObj = eval('('+str+')')
  ```

- fetch请求

  ```javascript
      fetch('https://facebook.github.io/react-native/movies.json')
          .then(response => response.json())
          .then((result) => {
              // 网络请求成功，处理请求到的数据
              Alert.alert(JSON.stringify(result))
          })
          .catch((error) => {
              // 网络请求失败，处理错误信息
          });
  ```




## 导航栏的使用

1.底部导航栏

```javascript
import {
    createStackNavigator,createBottomTabNavigator
} from 'react-navigation';

import Dash from "./js/Dash"
import Profile3 from "./js/Profile3"

const App = createBottomTabNavigator({
    a : {
        screen: Dash,
        navigationOptions:{
            tabBarIcon: ({ tintColor }) => {
                return <Image source={require('./image/i1.png')}  style={{width:30, height:30, tintColor: tintColor}}/>
            }
        }
    },
    b : {
        screen: Profile3,
        navigationOptions:{
            tabBarIcon: ({ tintColor }) => {
                return <Image source={require('./image/i2.png')}  style={{width:30, height:30, tintColor: tintColor}}/>
            }
        }
    }
},{
    tabBarOptions: {
        activeTintColor: 'red'
    }
});

export default App;

```

2.顶部导航栏

```javascript
const Dash = createMaterialTopTabNavigator({
    p1 : {screen: Profile},
    p2 : {screen: Profile2}
},{
    style:{paddingTop: 30}
})

module.exports = Dash
```



## 图片

```javascript
<Image source={{uri : "http://fdafdafa"}}/>
<Image source={require('./image/i2.png')}/>
```



## 刘海屏适配

```javascript
export default class App extends Component {
    render() {
        return (
            <SafeAreaView style={{flex: 1}}>
                <BottomTab style={{flex:1}}/>
            </SafeAreaView>
        );
    }
}
```

