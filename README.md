# qnipp:treeview

This is a Blaze template to be used to display a tree (using jsTree) right from a collection.

## Demo

See treeview-demo for a demo.

## Usage

### Installation

    meteor add qnipp:treeview

### Mount the tree in your Blaze template

```
{{> TreeView treeArgs }}
```

### Configure the tree in a template helper

This is a short configuration to show a tree, e. g. for navigation purposes. It contains no events to alter the tree.

```javascript
Template.templatename.helpers({
  treeArgs: {
    collection: collection,
    subscription: subscription,
    events: {
      changed: function(e, item) {
        console.log("Item " + item + "selected.");
      }
    }
  }
});
```

## Configuration options

See the demo to see different configurations in action.

### Data source

- **collection** sets the collection to be used.
- **subscription** sets the subscription for the template.
- **getNodes** is a `function(parent)`, which returns the nodes for a given parent as cursor or array.
- **mapping** contains a mapping from the fields from the data source to the tree fields. If a string is set, this field is taken from the orginal data. If a function is given, the function is called with the signature `function(item, field)`. `item` is the row in the database.
 - **parent**: The field containing the id of the parent node.
 - **text**: The text to be used at the nodes.
 - **icon**: The icon to be used for this node.
 - **state**: The state for the given node as object. It contains three attributes:
   - **selected**: The node is selected.
   - **opened**: The node is opened, the children are shown.
   - **disabled**: The node is rendered disabled.
 - **liClass**: The class to be used on the LI element.
 - **aAttr**: Attributes for the A element.

### Tree configuration

- **parent** shows only nodes below this parent. If not set, the parent node is identified by parent `null`.
- **select** selects a node on rendering the tree.
- **jstree** can be used to set the jsTree configuration options.
 - **plugins** activates one or more of the available plugins.

### Events

Events are configured in the `events` property. It allows the following keys:

- **changed** is called, when the selection changes.
- **create** is called, when a new node is to be created. It is created with a default name, which is renamed afterwards.
- **rename** is called, when an existing node is renamed.
- **delete** is called, when a node is deleted.
- **copy** is called to copy the selected nodes.
- **move** is called to move the selected nodes.

The function signature is `function(e, item, data)`.

- **e** is the original event.
- **item** contains the id of the respective item, or a list of ids in the case of the **changed** event. The root node has the id *#*.
- **data** is an array with additional data, dependent on the event:
 - **text** contains the new name of a renamed node.
 - **parent** contains the parent for a new or copied or moved node.
 - **position** contains the position of a copied or moved node.
