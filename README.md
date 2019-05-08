# simpleTree - A jQuery Plugin for Searchable Tree Views

This plugin renders a tree view into any div container.

**Example Screenshot**

The screenshot displays an example tree with a search box. The user is searching for `kera`. All occurrences of the search term in the tree are highlighted. The user has selected the node labeled _"Mittelalterliche Glasurkeramik"_.

<p align="center"><img style="border: 5px solid black" width="500" title="simpleTree Screenshot" alt="simpleTree Screenshot" src="https://github.com/eScienceCenter/eScienceCenter.github.io/blob/master/assets/simpleTree/screenshot1.png?raw=true"></p>

## Features 

* Rendering tree nodes, each with a label, a value and child nodes into any given jQuery div element
* Collapsing and expanding of non-leaf nodes
* Custom indenting and styling
* Toggling visibilty of nodes
* Filtering tree view by providing a search input box; search term is highlighted in matching node labels
* Retrieving, setting and clearing the selected node
* Programmatic scrolling and expanding to any given node 

## Installation

Add the files `simpleTree.js` and `simpleTree.css` to your project

simpleTree is available via NPM:

```npm install @esciencecenter/simple-tree```

## Requirements

* jQuery v3 (might also work with earlier versions, not tested)
* A container element in the DOM

## Limitations

* All node data must be provided upon initialization
* Nodes cannot be added or removed at runtime
* Only single nodes can be selected

## Initialization

To render the tree, simply invoke the `simpleTree()` function on the jQuery element.

```$('#mytree').simpleTree(options, data);```

**Arguments:**

`options` is an object that can be used to override the default simpleTree options. The defaults are as follows:

```JavaScript
var _defaults = {
        // Optionally provide here the jQuery element that you use as the search box for filtering the tree. simpleTree then takes control over the provided box, handling user input
        searchBox: undefined,

        // Search starts after at least 3 characters are entered in the search box
        searchMinInputLength: 3,

        // Number of pixels to indent each additional nesting level
        indentSize: 25,

        // Show child count badges?
        childCountShow: true,

        // Symbols for expanded and collapsed nodes that have child nodes
        symbols: {
            collapsed: '▶',
            expanded: '▼'
        },

        // these are the CSS class names used on various occasions. If you change these names, you also need to provide the corresponding CSS class
        css: {
            childrenContainer: 'simpleTree-childrenContainer',
            childCountBadge: 'simpleTree-childCountBadge badge badge-pill badge-secondary',
            highlight: 'simpleTree-highlight',
            indent: 'simpleTree-indent',
            label: 'simpleTree-label',
            mainContainer: 'simpleTree-mainContainer',
            nodeContainer: 'simpleTree-nodeContainer',
            selected: 'simpleTree-selected',
            toggle: 'simpleTree-toggle'
        }
    };
```

`data` is an array of top-level nodes that will always be visible. Each node has the following properties:

* `label` (string): The node label
* `value` (any): The app's value to identify the node
* `children` (array): An array of child nodes. Can be empty or omitted if this is a leaf node
* `expanded` (boolean, optional): Determines whether the node, if it has children, will be initially displayed as expanded. (default: `false`)

An example `data` array:

```Javascript
let data = [
   {
       label: 'Animals',
       value: 'animals',
       children: [
           {
               label: 'Puppy the dog',
               value: 'puppy'
           }, {
               label: 'Pussy the cat',
               value: 'pussy'
           }
       ]
   }, {
       label: 'People',
       value: 'people',
       children: [
           {
               label: 'Tom',
               value: 'tom',
               children: [
                   {
                       label: 'Sue',
                       value: 'sue'
                   }
               ]
           }, {
               label: 'Cathy',
               value: 'cathy'
           }
       ]
   }
];
```

## Methods

The tree object's public method API can be retrieved by calling `$('#mytree').data('simpleTree');`

The ojbect exposes the following methods, which can be chained unless a return value is mentioned explicitly:

* `getSelectedNode()`: returns the currently selected node object, or `undefined` if no node is selected

* `setSelectedNode(node)`: makes `node` the selected node

* `clearSelection()`: Clears any selection

* `getNodeCount()`: Get total count of nodes in the tree

* `expandAll()`: Expand all non-leaf nodes

* `collapseAll()`: Collapse all non-leaf nodes

* `toggleSubtree(node)`: expands or collapses the descenandants of `node`

* `getNodeFromValue(value)`: Returns the node with he corresponding `value` property

* `scrollTo(node)`: Scrolls the tree container to the given `node`

* `expandTo(node)`: Makes sure that all ancestor nodes of `node` are expanded

* `isNodeVisible(node)`: Determines whether the ancestry of `node` is expanded and not hidden via `hideNode()`, i.e. the node is rendered and visible somewhere in the container

* `showNode(node)`: Shows a previously hidden `node` in the DOM. This only works on nodes that are visible in the tree (i.e. `isNodeVisible(node)` would evaluate to `true`)

* `hideNode(node)`: Hides a visible `node` in the DOM. This only works on nodes that are visible in the tree (i.e. `isNodeVisible(node)` would evaluate to `true`)
 
* `toggleNodeVisibility(node)`: Toggles the visibility of the `node` by invoking either showNode(node) or hideNode(node)

* `traverseTree(callback, [startNode])`: Traverses the tree data using a depth-first search and invokes the `callback` function with the current node as a parameter. The parameter `startNode` can be provided as the starting node for traversal; if omitted, the this method will traverse the whole tree

## Events

The simpleTree element fires the following events:

* `simpleTree:change` is fired when the node selection changes based on user input. The currently selected node is passed along with the event. Note this event is also fired when the node selection is cleared (e.g. by `clearSelection()`); the selected node is then `undefined`.

## Changelog

See the [changelog](CHANGELOG.md).

## License

simpleTree is licensed under the [MIT License](LICENSE).
