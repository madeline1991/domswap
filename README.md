# Domswap

Domswap is a JavaScript library that can be used to transverse and manipulate the DOM.  For a given argument, the library returns an instance of the DOMNodeCollection class. The DOMNodeCollection can contain a single element or multiple elements and methods are chainable.

## API

### Wrapper
  * $l
  * $l.ajax

### DOM Manipulation
  * html
  * append
  * empty
  * attr
  * addClass
  * removeClass
  
### DOM Transversing
  * child
  * parent
  * find
  * remove

### Wrapper
  #### $l
  $l is a wrapper for all methods in Domswap. It can take in an HTML element, a string, or a function. If the argument passed in is a function, $l will wait until the document is laoded and execute the function as a callback.

  #### $l.ajax
  Ajax requests can be sent using the $l wrapper. The ajax method takes in a hash with the type of request and url. Success and error callbacks maybe given, however, if none are supplied, default callbacks will console the returned data.

```js
$l.ajax = function(object) {
  const defaults = {data: {}};
  const xhr = new XMLHttpRequest();

  defaults.success = function(returnData) {
    console.log("Success");
    console.log(returnData);
  };

  defaults.error = function(returnData) {
    console.log("Error");
    console.log(returnData);
  };

  const data = Object.assign({}, defaults, object);
  xhr.open(data.type, data.url);
  xhr.onload = function() {
    const returnData = JSON.parse(xhr.response);
    if(xhr.status === 200) {
      data.success(returnData);
    } else {
      data.error(returnData);
    }
  };

  xhr.send(data.data);
};
```

### DOM Manipulation
  #### html
  If a string is passed to the function, it sets the inner HTML of every element in the DOMNodeCollection. If no argument is passed, it returns the inner html of the first node in the array.

  #### append
  The append method excepts an wrapped collection, an HTML element, or a string. It the argument is an instance of the DOMNodeCollection or an HTML element, it adds the outer html to each element's inner html, otherwise, it adds the argument to each element's inner html.

  #### empty
  Clears the inner html of each element in the DOMNodeCollection.

  #### addClass and removeClass
    These two methods both take in a class name as an argument and either adds a class or removes a class from each element in the DOMNodeCollection. If an element already has a class name, the new class name will be added onto the old class name, giving the element multiple class names.

  ### DOM Transversing

  #### children
    Recursively transverses the tree and returns a DOMNodeCollection containing all children of every direct child of the elements in the original collection.

  ```js
  children() {

    const allChildren = [];

    this.elements.forEach((el) => {
      let children = el.children;
      let childrenArray = Array.from(children);
      childrenArray.forEach((child) => {
        allChildren.push(child);
      });
    });
    return new DOMNodeCollection(allChildren);
  }
  ```
  #### parent
  Returns a new DOMNodeCollection containing the parents of every element in the original DOMNodeCollection. If two nodes have the same parent, the parent is only added once to the new collection.

  #### find
  Takes in a selector as an argument and returns a new collection of all nodes that are decedents of the elements in the original collection and that match the selector.
