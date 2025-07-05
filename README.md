# Adaptive Observer Model (AOM)

**Adaptive Observer Model** (AOM) is a design pattern for turning any JSON-like data into a form builder or reactive structure. It is language-agnostic and implementation-independent, designed to handle dynamic and evolving data.

> In AOM, **everything is a field**—a primitive value, a nested object, or a recursive tree node. Forms, settings panels, routing definitions—all become structured, reactive data.

## Design Goals

* Automatically transform any JSON-like input into editable, observable fields
* Treat flat and deeply nested structures uniformly
* Enable full observation, mutation, and traversal at any level

## Why Use AOM?

* **Zero config**: Works with any structure without schema or setup
* **Unified model**: Consistent handling of arrays, objects, and trees
* **Fine-grained reactivity**: Observe and respond to changes at any depth
* **Composable & extensible**: Ideal for forms, editors, query builders, etc.

## Data Classification

AOM distinguishes two types of structures:

### Generic Data

Objects or arrays without recursive `children` keys.

Examples:

* `{ name: "Alice", age: 30 }`
* `[ { id: 1 }, { id: 2 } ]`
* `{ search: "term", page: 1 }`
* Tabular or flat records

### Tree Data

Recursive data with nodes containing a consistent `children` key.

Examples:

* Category trees
* Navigation menus
* File systems
* Comment threads
* ASTs
* Nested routing definitions

## Core Concepts

### FieldNode (Generic Data)
**Constructor**

  `let fieldNode = new FieldNode(defaultValue)`

**Parsing Logic:**

* `object` → `FieldNode` with child nodes for each key
* `array` → `FieldNode` with child nodes for each item
* Primitive → `FieldNode` of type `single`

**Structure:**

* `type`: single | array | object
* `defaultValue`: original input
* `children`: `FieldList` if `value` is object or array
* `states`: transient UI state (label, collapsed, selected, etc.)
* `validator`: function to validate current value

**Reconstruction:**

* `single` → returns `_value`
* `array` → maps child values
* `object` → rebuilds object from child names and values

### FieldList (Generic Data)
* `let filedList = new FieldList(defaultValue)`
* Exposed for Usage when input data is array
* Use as internal auto parsing as fieldNode.children
* Extends from `ResusiveList`


### TreeField
**Constructor**

  `let treeField = new TreeField(defaultValue,treeKey="children")`

**Parsing Logic:**

* Uses `treeKey` (default `'children'`) to detect recursion
* Other properties stored as `entries`

**Structure:**

* `treeKey`: key used for recursion
* `children`: `TreeList` present array of `TreeField` nodes
* `entries`: key-value pairs excluding `treeKey`
* `states`: transient UI state
* `validator`: validates `entries`

**Reconstruction:**

* `object` Merges `entries` and recursive `children` under `treeKey`

### TreeList
* `let treeField = new TreeList(defaultValue,treeKey="children")`
* Exposed for Usage when input data is array like categories
* Use as internal auto parsing as treeField.children
* Extends from `ResusiveList`

## Implementation

### JavaScript Library

📦 [`domphy/struct`](https://docs.domphy.com/struct)

## License

MIT © 2025 [Khánh Nguyễn](https://github.com/huukhanhnguyen)
