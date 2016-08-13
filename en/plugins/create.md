We can easily create a plugin according to our requirements.

A plugin is just some codes implemented by javascript object literal.

Below is a simple plugin:
```
var Bold = {
  className: 'font-bold',
  eventType: 'click',
  icon: 'icon-bold',
  eventSelector: '.font-bold',
  eventCallback: function(editor) {
    return function(e) {
      alert('bold');
    };
  },
  svg: [
  '<symbol id="icon-bold" viewBox="0 0 32 32">',
    '<path class="path1" d="M22.121 15.145c1.172-1.392 1.879-3.188 1.879-5.145 0-4.411-3.589-8-8-8h-10v28h12c4.411 0 8-3.589 8-8 0-2.905-1.556-5.453-3.879-6.855zM12 6h3.172c1.749 0 3.172 1.794 3.172 4s-1.423 4-3.172 4h-3.172v-8zM16.969 26h-4.969v-8h4.969c1.827 0 3.313 1.794 3.313 4s-1.486 4-3.313 4z"></path>',
  '</symbol>'
  ].join()
};
```
After definition the object literal, we only need add the object into plugin manager
```
beeji.PluginManager.addPlugin('bold', Bold);
```

Now let's have a look on those all plugin options we can use.

# 1. Icon related

> option icon and option svg are used for defining icon

Currently BeeJi recommend svg as the plugin icon.

You can go to [IconMoon](https://icomoon.io/) select the svg icon and then copy out the svg `defs`.

Then put the `symbol` part into `svg` option and set the symbol's id into `icon` option.

Below is an example:

```
var Bold = {
  icon: 'icon-bold',
  svg: `
    <symbol id="icon-bold" viewBox="0 0 32 32">
      <path class="path1" d="M22.121 15.145c1.172 don't copy just for doc"></path>
    </symbol>
  `,
  ...
};
```

## 1.1 icon
The svg symbol's id

```
icon: 'icon-bold'
```

## 1.2 svg
svg symbol body
```
svg: `
  <symbol id="icon-bold" viewBox="0 0 32 32">
    <path class="path1" d="M22.121 15.145c1.172 don't copy just for doc"></path>
  </symbol>
`
```
If we didn't use es6, we can also did it like this
```
svg: [
  '<symbol id="icon-bold" viewBox="0 0 32 32">',
    '<path class="path1" d="M22.121 15.145c1.172 don't copy just for doc"></path>',
  '</symbol>'
].join()
```

# 2. Event related
## 2.1 eventSelector
Event listener will be added to this dom selector.
Definition:
```
eventSelector: '.font-bold',
```
If there is existed this key-value, in the BeeJi code, will init a event listener, like below:
```
xxx.querySelector('.font-bold').addEventListener....
```

If we didn't set this option, we will use the other option `className` as the default event selector.

## 2.2 eventType
A name used to refer to the specific event, like `click` or `change`
```
eventType: 'click'
```
Then in BeeJi code will do below stuff
```
xxx.addEventListener('click', function() {})
```

## 2.3 eventCallback
This is an import option for event as all the logic code will be put here.
```
eventCallback: function(editor) {
  return function(e) {
    alert('bold');
  };
}
```
The parameter `editor` has some methods and properties we can used to implement our logic.

### 2.3.1 methods
* `getRange`  
This method will return the javascript Range object, 
it means we can get all javascript standard methods or properties in Range object, 
like commonAncestorContainer or startContainer and so on

# 3. Other options
## 3.1 className
Each plugin will be inserted into a `li` element.

And the value of option `className` will be set as the className of the `li` element.

Then we can use the `className` to do some thing, like customize the style.

If we didn't set the eventSelector, the `className` will be used as the default eventSelector.

## 3.2 stylesheet
The value of the option, we be set into the `<style></style>` in the html header.

It means we can customize our plugin style by css like below:
```
stylesheet: '.className {font-size: 20px;}'
```

If we use CommonJS, we can just require a stylesheet path, like 
```
stylesheet: require('./path/style.css')
```

# 4. Fully example
```
var style = `
  label.upload-label input[type="file"] {
    position: fixed;
    top: -1000px;
  }
  .upload-label {
    display: inline-block;
    height: 44px;
    line-height: 44px;
    padding: 0;
    margin: 0;
    width:100%;
    cursor: pointer;
  }
  .upload-label:hover {
    background: #CCC;
  }
  .upload-label:active {
    background: #CCF;
    background: #DDD;
  }
  .upload-label :invalid + span {
    color: #A44;
  }
  .upload-label :valid + span {
    color: #333333;
  }
`;
var insertImage = function(files, range) {
  var reader = new FileReader();
  var image = new Image();
  image.style.maxWidth = '100%';
  reader.readAsDataURL(files[0]);
  reader.onload = function(_file) {
    image.src = _file.target.result;
    if (range !== undefined) {
      range.insertNode(image);
    }
  };
};
var InsetImage = {
  className: 'upload-image',
  stylesheet: style,
  eventSelector: 'input.upload-image',
  eventType: 'change',
  svg: `
    <symbol id="icon-image" viewBox="0 0 32 32">
      <path class="path1" d="M29.996 4c0.001 0.001 0.003 0.002 0.004 0.004v23.993c-0.001 0.001-0.002 0.003-0.004 0.004h-27.993c-0.001-0.001-0.003-0.002-0.004-0.004v-23.993c0.001-0.001 0.002-0.003 0.004-0.004h27.993zM30 2h-28c-1.1 0-2 0.9-2 2v24c0 1.1 0.9 2 2 2h28c1.1 0 2-0.9 2-2v-24c0-1.1-0.9-2-2-2v0z"></path>
      <path class="path2" d="M26 9c0 1.657-1.343 3-3 3s-3-1.343-3-3 1.343-3 3-3 3 1.343 3 3z"></path>
      <path class="path3" d="M28 26h-24v-4l7-12 8 10h2l7-6z"></path>
    </symbol>
  `,
  template: `
    <label class="upload-label">
      <input class="upload-image" type="file"/>
      <span>
        <svg class="icon icon-image"><use xlink:href="#icon-image"></use></svg>
      </span>
    </label>
  `,
  eventCallback: function(editor) {
    return function(e) {
      let files = e.srcElement.files;
      let currentRange = editor.getRange();
      insertImage(files, currentRange);
    };
  }
};
```





