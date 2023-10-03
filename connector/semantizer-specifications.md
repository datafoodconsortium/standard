---
description: >-
  Semantizer is a library to manipulate RDF data in an object oriented
  environment.
---

# Semantizer specifications

## Interfaces

### Semanticable

#### addSemanticPropertyReference

description

#### addSemanticPropertyLiteral

#### getSemanticProperty

#### getSemanticPropertyAll

#### removeSemanticProperty

remove one property + its resource (if blank node).

Throws `Error`.

#### removeSemanticPropertyAll

remove all properties + its resource (if blank node).

Throws `Error`.

#### setSemanticProperty

### IStore

### IFactory

## Implementation notes

### The SemanticObjecAnonymous class

TBD

### The Store Class

## Error codes

## Appendixes

### List of interfaces

| Interface name | Description |
| -------------- | ----------- |
| `Semanticable` |             |
| `IStore`       |             |
| `IFactory`     |             |
|                |             |
|                |             |

### List of classes

| Class name                | Description |
| ------------------------- | ----------- |
| `SemanticObject`          |             |
| `SemanticObjectAnonymous` |             |
| `Semantizer`              |             |
| `Store`                   |             |
|                           |             |
|                           |             |

