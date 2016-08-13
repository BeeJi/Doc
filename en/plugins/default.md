# Default supported plugins
We can use default plugin directly, the way is putting the key into the array option `plugins` when we init BeeJi Editor.

Like below:
```
plugins: ['unorderedlist', 'insertimage', ...]
```

Fully code
```
var objInit = {
  className: 'editor',
  plugins: ['font', 'bold', 'orderedlist', 'unorderedlist', 'indentdecrease', 'indentincrease', 'insertimage'],
  type: 'click',
  cancelText: 'Cancel',
  okText: 'OK',
  okCallback: function(data, idx) {
    var divs = document.getElementsByClassName('editor');
    divs[idx].innerHTML = data;
  }
};
beeji.fly(objInit);
```

## unorderedlist
Unordered list

## orderedlist
Ordered list

## insertimage
Insert a image as base64 format

## indentdecrease
Paragraph style decrease indent

## indentincrease
Paragraph style increase indent

## bold
Bold font style

## italic
Italic font style

## strikethrough
Strikethrough font style

## underline
Underline font style

## paragraphcenter
Paragraph style Align Center

## paragraphleft
Paragraph style Align Left

## paragraphright
Paragraph style Align Right

## font
