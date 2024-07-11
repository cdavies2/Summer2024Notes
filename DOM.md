# Introduction to the Document Object Model
*  The Document Object Model is what allows web pages to render, response to user events, and change.
## HTML vs DOM
* HTML is a way to create a document, and the DOM is a different kind of representation of the document.
* HTML is a string of characters
* The DOM is a different kind of structure, it's a tree.
* The objects on the DOM tree are called nodes, and edges (arrows) connect parents to children.
* HTML has tags (p, h1, body), and tags can have attributes and the attributes can have values. HTML doesn't have a text tag, but DOM has text nodes.
### DOM Example
```txt
Body
|->h1
|->p
|  |->text
|  |->a
|  |->text
|->p
```
### HTML Example
```html
 <body>
  <h1>Hello</h1>
  <p>
    Check out my
    <a href="/page>Page!</a>
      It's the best page out there
  </p>
</body>
```
## The DOM is a Tree
* A tree is an abstract data type.
* There is a Node that branches into other Nodes (its children Nodes)
  * Each Node can 0 to many children Nodes
  * Nodes can have 0 or 1 parent
  * Nodes can have 0 or many sibling nodes.
## What does the Browser Take
* The Browser takes HTML from the server, converts it into a DOM and converts it to something else before presenting it to us.
* What you see in Chrome is not necessarily what you're given by the server. You receive it from the server, get a DOM, then you manipulate it in your browser.
* The DOM makes it possible to use JavaScript to manipulate the document content and structure.
## Nodes have Lots of Attributes
* Nodes are JavaScript objects.
* Nodes have Attributes that are JavaScript properties
* Attributes define how the Node looks and responds to User activity.
* One node has hundreds of properties
## The document Object
* Global reference to the DOM entry point
* Provides methods for:
  * Navigating the DOM
  * Manipulating the DOM
* The document object is the important connection between the DOM and JavaScript code.
## Searching the DOM
* getElementById (find nodes with a certain ID attribute)
  * document.getElementByID("will");
* getElementsByClassName (find nodes with a certain CLASS ATTRIBUTE)
  * document.getElementsByClassName("will");
* getElementsByTagName (find nodes with a certain HTML tag)
  * document.getElementsByTagName("div");
* querySelector, querySelectorAll (search using CSS selectors)
  * document.querySelector("#will .will:first-child");
## Array-Like Objects?
* An HTMLCollection (the result of running document.getElementsByTagName) is not technically an array
* Array-like objects are semantically different from arrays, but similar operations are meant to be performed on them, they can be easily converted to arrays.
* HTMLCollections have far fewer methods than Arrays.
* To convert array-like objects to arrays, you have several options
  * const realArr =[].prototype.slice.call(arrayLike)
  * const realArr = Array.from(arrayLike)
  * const realArr = [..arrayLike]
## Traversing the DOM
* Tree structures are easy to navigate
  * At any point in the DOM you're at a Node.
  * No matter where you go, you're still at a Node
    * Child
    * Parent
    * Sibling
* All Nodes share similar DOM navigation method
* The browser determines an order for the siblings, and it's typically the same order that they had in HTML.
* Access children
  * element.children, element.lastChild, element.firstChild 
*  Access siblings
  *  element.nextElementSibling, element.previousElementSibling
*  Access parent
  * element.parentElement
## Changing Style Attributes
* element.style.backgroundColor="blue"
* CSS                                            JavaScript
  * background-color                ->            backgroundColor
  * border-radius                   ->            borderRadius
  * font-size                       ->            fontSize
  * list-style-type                 ->            listStyleType
  * word-spacing                    ->            wordSpacing
  * z-index                         ->            zIndex
* CSS properties can include a hyphen character, JavaScript ones cannot
* CSS properties are converted to camelcase for JavaScript
## Changing CSS Classes
* className attribute is a string of all of a Node's classes
* classList is HTML5 way to modify which classes are on a node.
  * document.getElementById("MyElement").classList.add('class');
  * document.getElementById("MyElement").classList.remove('class');
  * if (document.getElementById("MyElement").classList.contains('class')) document.getElementById("MyElement").classList.toggle('class');
## Creating Elements
* Create an element
  * document.createElement(tagName)
* Duplicate an existing node
  * node.cloneNode()  
*  Nodes are just free floating, not connected to the document itself until you link them to the node.
## Adding Elements to the DOM
* Insert newNode at end of current node
  * node.appendChild(newNode); 
*  Insert newNode at start of current node
  * node.prependChild(newNode)  
*  Insert newNode before a certain childNode
  * node.insertBefore(newNode, sibling);
## Removing Elements
* Removes the oldNode child
  * node.removeChild(oldNode); 
*  Quick hack
  * oldNode.parentNode.removeChild(oldNode);
    
