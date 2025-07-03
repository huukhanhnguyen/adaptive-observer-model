# Adaptive Observer Model (AOM)

**Adaptive Observer Model** is a design pattern for representing structured data as editable, observable entities. It is language-agnostic and implementation-independent, designed to handle dynamic and evolving data structures.

## Design Goals

* Automatically transform any JSON-like data into `editable` and `reactive` form

## Why Use Adaptive Observer Model?

* **Zero configuration**: Automatically adapts to any JSON-like structure — no schema or manual setup required.
* **Unified abstraction**: Handles both flat and nested (tree-like) data consistently.
* **Fine-grained observation**: Track and respond to changes at any depth or node.
* **Composable & adaptable**: Ideal for form engines, configuration editors, interactive tools, or any system that involves structured data with change propagation.

## Data Classification

AOM categorizes data structures into two primary forms:

### Generic Structure

A flat structure refers to any non-recursive object or array. These are shallow and lack self-referencing elements. Examples include:

* **Configuration objects**: `{ theme: 'dark', language: 'en', autosave: true }`
* **Form data**: `{ name: 'Alice', age: 30, agreeToTerms: true }`
* **API payloads**: `{ userId: 42, status: 'active' }`
* **Flat tables or lists**: `[{ id: 1, name: 'Item A' }, { id: 2, name: 'Item B' }]`
* **Query parameters**: `{ search: 'keyword', limit: 10 }`
* **Spreadsheet-like records**
* **Flat preferences or feature flags**

### Tree Structure

A tree structure is a recursive object where nodes can contain child nodes, typically under a key like `children`. Examples include:

* **Category hierarchies**: product categories, taxonomy trees
* **Navigation menus**: with submenus and nested links
* **Comment threads**: replies nested under parent comments
* **File systems**: folders with nested folders/files
* **Abstract syntax trees (ASTs)**: parsed code structures
* **Nested routing definitions**
* **Scene graphs** in graphics engines

## Parsing Rules

### Generic Data

* **Primitive values** (string, number, boolean, null) become leaf nodes.
* **Object or array** values become nested structures.

### Tree Data

* Recognize the recursive key (default: `'children'`) as branch data.
* All other properties are treated as node-level data and tracked reactively.

## Class Summary

| Class        | Extends     | Description                                                                                                     |
| ------------ | ----------- | --------------------------------------------------------------------------------------------------------------- |
| `Notifier`   | –           | Event-driven system with `addListener(event, listener)`, `removeListener(event)`, `notify(event, ...args)`      |
| `State`      | –           | Reactive key-value store. Provides `getEntry(key)`, `setEntry(key, value)`, `onChange(listener)`, `notify(key)` |
| `FieldList`  | `Notifier`  | Reactive list of `items`. Emits events like `append`, `remove`, and supports full list manipulation.            |
| `FieldNode`  | `State`     | Manages hierarchy: `parent`, `root`, `depth`, `ancestors`, `descendants`. Core unit for structured data.        |
| `FieldState` | `FieldNode` | Extends `FieldNode` with validation: `entries: { value, errors, warnings, successes }`                          |
| `FieldGroup` | `FieldNode` | Provides reactive `entries` and a `children: FieldList`. Suitable for object/array structures.                  |
| `TreeField`  | `FieldNode` | Designed for tree-shaped data like `{ label: ..., children: [...] }`. Works with `TreeList`.                    |
| `TreeList`   | `FieldList` | Specialized for arrays of tree nodes, e.g., `[{ label, children }]`. Supports hierarchical operations.          |

## Implementation

### JavaScript Libraries

* [domphy/form](https://docs.domphy.com/form)
* [domphy/tree](https://docs.domphy.com/form)

License: MIT Copyright: 2025 Author: [Khánh Nguyễn](https://github.com/huukhanhnguyen)
