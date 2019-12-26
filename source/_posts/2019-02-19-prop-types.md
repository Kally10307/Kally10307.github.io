---
title: prop-types
date: 2019-02-19 09:44:28
categories: React
---

# prop-types 的基本使用

`prop-types`是`React`类型检测的一种方式。`React`不像`Vue`那样可以直接在`props`对象上定义其类型及默认值，需要引入`prop-types`依赖进行定义。

## 基本类型

```
import React, { Component, PureComponent } from 'React';
import PropTypes from 'prop-types';

class myComponent extends Component {
    
}

myComponent.propTypes = {
    optionalString: PropTypes.string;
    optionalNumber: PropTypes.number;
    optionalBoolean: PropTypes.bool;
    optionalFunction: PropTypes.func;
    optionalObject: PropTypes.object;
    optionalArray: PropTypes.array;
    optionalSymbol: PropTypes.symbol; 
}

// or

class myContainer extends PureComponent {
    
}

myContainer.contextTypes = {
    value: PropTypes.number;
}

myContainer.propTypes = {
    name: PropTypes.string;
    age: PropTypes.number;
}

myContainer.defaultProps = {
    name: '';
    value: 0
}
```



## 使用方法

1. `arrayOf`方法和`objectOf`方法

   ```
   import React, { Component } from 'React';
   import PropTypes from 'prop-types';
   
   class myComponent extends Component {
       
   }
   
   myComponent.propTypes = {
       name: PropTypes.string.isRequired;
       age: PropTypes.number;
       favorite: PropTypes.arrayOf(PropTypes.string);
       others: PropTypes.objectOf(PropTypes.string);
   }
   ```

   

2. `shape`方法

   ```
   import React, { Component } from 'React';
   import PropTypes from 'prop-types';
   
   class myComponent extends Component {
       
   }
   
   myComponent.propTypes = {
       name: PropTypes.string.isRequired;
       age: PropTypes.number;
       favorite: PropTypes.arrayOf(PropTypes.shape({
           name: PropTypes.string;
           star: PropTypes.number;
       }))
   }
   ```

   

3. `oneOfType`方法

   ```
   import React, { Component } from 'React';
   import PropTypes from 'prop-types';
   
   class myComponent extends Component {
       
   }
   
   myComponent.propTypes = {
       name: PropTypes.string.isRequired;
       age: PropTypes.number;
       other: PropTypes.oneOfType([
           PropTypes.string;
           PropTypes.number;
       ])
   }
   ```

