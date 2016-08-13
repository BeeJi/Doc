# Installation

```
npm install beeji --save
```

# Usage
* import js, css files

* init
```javascript
var objInit = {
  className: 'editor',
  type: 'click',
  cancelText: 'Cancel',
  okText: 'Ok',
  okCallback: function(data, idx) {
    let divs = document.getElementsByClassName('editor');
    divs[idx].innerHTML = data;
  }
};
bee.fly(objInit);
```

# Config
## plugins
* unordered-list

