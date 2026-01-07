---
title: createFormHandler
nextjs:
  metadata:
    title: createFormHandler - NovaForms
    description: Learn how to use createFormHandler for state management, rules execution, and form logic in NovaForms.
---

The `createFormHandler` function is the heart of NovaForms' state management system. It creates a change handler that automatically manages form state, applies field modifiers, and executes rules when field values change.

---

## Overview

`createFormHandler` returns a function that:
- Manages form state updates
- Applies legacy field-level modifiers
- Executes top-level rules triggered by field changes
- Handles both React synthetic events and direct value calls
- Supports complex calculations and string operations

---

## Basic Usage

```jsx
import { createFormHandler } from "nova-forms";

const handleChange = createFormHandler({
  fields,        // Array of field definitions
  setState,      // React setState function
  rules          // Optional array of rules
});
```

---

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `fields` | `Array` | Yes | Array of field definitions |
| `setState` | `Function` | Yes | React setState function |
| `rules` | `Array` | No | Array of top-level rules |

---

## How It Works

### 1. Event Handling
The handler accepts both React synthetic events and direct value calls:

```jsx
// React synthetic event (from input onChange)
handleChange(event);

// Direct value call (from custom components)
handleChange("new value", "fieldName");
```

### 2. State Management
The handler automatically updates form state:

```jsx
const [formData, setFormData] = useState({});

const handleChange = createFormHandler({
  fields,
  setState: setFormData
});

// When a field changes, formData is automatically updated
// formData = { firstName: "John", lastName: "Doe", ... }
```

### 3. Rule Execution
When a field changes, the handler checks for triggers and executes matching rules:

```jsx
const rules = [
  {
    name: "calculateTotal",
    effects: [
      {
        targetField: "total",
        type: "add",
        value: 10
      }
    ]
  }
];

const fields = [
  {
    name: "quantity",
    type: "number",
    triggers: [
      {
        rule: "calculateTotal",
        when: "not empty"
      }
    ]
  }
];
```

---

## Legacy Modifiers

For backward compatibility, NovaForms still supports field-level modifiers:

```jsx
const fields = [
  {
    name: "firstName",
    type: "string",
    modifiers: [
      {
        target: "fullName",
        type: "concat",
        when: "true",
        value: " ",
        strictString: true
      }
    ]
  }
];
```

### Modifier Properties

| Property | Type | Description |
|----------|------|-------------|
| `target` | `string` | Field name to modify |
| `type` | `string` | Operation type (add, subtract, multiply, divide, concat, replace) |
| `when` | `string` | Condition when to apply |
| `value` | `any` | Value to use in operation |
| `kind` | `string` | Data type (number, string) |
| `strictString` | `boolean` | Force string operations |

---

## Rules System

The modern approach uses top-level rules with field-level triggers:

### Rule Structure

```jsx
const rules = [
  {
    name: "ruleName",           // Unique identifier
    effects: [                  // Array of effects to apply
      {
        targetField: "fieldName", // Field to modify
        prop: "value",           // Property to modify (value, readOnly, hidden, etc.)
        type: "add",             // Operation type
        kind: "number",          // Data type
        value: 10,               // Value for operation
        strictString: false,     // Force string operations
        sourceFields: []         // For concat operations
      }
    ]
  }
];
```

### Effect Properties

| Property | Type | Description |
|----------|------|-------------|
| `targetField` | `string` | Field name to modify |
| `prop` | `string` | Property to modify (`value`, `readOnly`, `hidden`, `title`, etc.) |
| `type` | `string` | Operation type |
| `kind` | `string` | Data type (`number`, `string`) |
| `value` | `any` | Value for operation |
| `strictString` | `boolean` | Force string operations |
| `sourceFields` | `Array` | For concat operations with multiple fields |

---

## Operation Types

### Math Operations
- `add`: Addition
- `subtract`: Subtraction
- `multiply`: Multiplication
- `divide`: Division
- `replace`: Replace with new value

### String Operations
- `concat`: Concatenation
- `replace`: Replace with new value

### Special Operations
- `concat` with `sourceFields`: Concatenate multiple fields with custom separators

---

## Examples

### Simple Calculation

```jsx
const rules = [
  {
    name: "calculateTax",
    effects: [
      {
        targetField: "totalWithTax",
        type: "multiply",
        kind: "number",
        value: 1.08  // 8% tax
      }
    ]
  }
];

const fields = [
  {
    name: "subtotal",
    type: "number",
    triggers: [
      {
        rule: "calculateTax",
        when: "not empty"
      }
    ]
  },
  {
    name: "totalWithTax",
    type: "number",
    readOnly: true
  }
];
```

### String Concatenation

```jsx
const rules = [
  {
    name: "buildFullName",
    effects: [
      {
        targetField: "fullName",
        type: "concat",
        sourceFields: [
          { field: "firstName", charBefore: "", charAfter: " " },
          { field: "lastName", charBefore: "", charAfter: "" }
        ]
      }
    ]
  }
];

const fields = [
  {
    name: "firstName",
    type: "string",
    triggers: [
      {
        rule: "buildFullName",
        when: "not empty"
      }
    ]
  },
  {
    name: "lastName",
    type: "string",
    triggers: [
      {
        rule: "buildFullName",
        when: "not empty"
      }
    ]
  },
  {
    name: "fullName",
    type: "string",
    readOnly: true
  }
];
```

### Complex Multi-Field Calculation

```jsx
const rules = [
  {
    name: "calculateOrderTotal",
    effects: [
      {
        targetField: "subtotal",
        type: "multiply",
        kind: "number",
        value: 1
      },
      {
        targetField: "tax",
        type: "multiply",
        kind: "number",
        value: 0.08
      },
      {
        targetField: "total",
        type: "add",
        kind: "number",
        value: 0
      }
    ]
  }
];

const fields = [
  {
    name: "quantity",
    type: "number",
    triggers: [
      {
        rule: "calculateOrderTotal",
        when: "not empty"
      }
    ]
  },
  {
    name: "price",
    type: "number",
    triggers: [
      {
        rule: "calculateOrderTotal",
        when: "not empty"
      }
    ]
  },
  {
    name: "subtotal",
    type: "number",
    readOnly: true
  },
  {
    name: "tax",
    type: "number",
    readOnly: true
  },
  {
    name: "total",
    type: "number",
    readOnly: true
  }
];
```

---

## Advanced Features

### Conditional Rule Execution

Rules can have complex trigger conditions:

```jsx
const fields = [
  {
    name: "discountCode",
    type: "string",
    triggers: [
      {
        rule: "applyDiscount",
        when: [
          { field: "discountCode", when: "matches", value: "SAVE10" },
          { field: "subtotal", when: "greater than", value: 50 }
        ],
        mode: "all"  // Both conditions must be true
      }
    ]
  }
];
```

### Attribute Effects

Rules can modify field attributes, not just values:

```jsx
const rules = [
  {
    name: "lockAgeField",
    effects: [
      {
        targetField: "age",
        prop: "readOnly",
        value: true
      }
    ]
  }
];
```

---

## Best Practices

### 1. Use Rules Over Modifiers
Prefer the rules system over legacy modifiers for new forms:

```jsx
// ✅ Good: Use rules
const rules = [
  {
    name: "calculateTotal",
    effects: [
      {
        targetField: "total",
        type: "add",
        value: 10
      }
    ]
  }
];

// ❌ Avoid: Legacy modifiers
const fields = [
  {
    name: "quantity",
    modifiers: [
      {
        target: "total",
        type: "add",
        value: 10
      }
    ]
  }
];
```

### 2. Keep Rules Focused
Each rule should have a single responsibility:

```jsx
// ✅ Good: Focused rules
const rules = [
  {
    name: "calculateSubtotal",
    effects: [
      { targetField: "subtotal", type: "multiply", value: 1 }
    ]
  },
  {
    name: "calculateTax",
    effects: [
      { targetField: "tax", type: "multiply", value: 0.08 }
    ]
  }
];

// ❌ Avoid: Monolithic rules
const rules = [
  {
    name: "calculateEverything",
    effects: [
      { targetField: "subtotal", type: "multiply", value: 1 },
      { targetField: "tax", type: "multiply", value: 0.08 },
      { targetField: "total", type: "add", value: 0 }
    ]
  }
];
```

### 3. Use Descriptive Names
Make rule and field names descriptive:

```jsx
// ✅ Good: Descriptive names
const rules = [
  {
    name: "calculateOrderTotalWithTax",
    effects: [...]
  }
];

// ❌ Avoid: Unclear names
const rules = [
  {
    name: "calc1",
    effects: [...]
  }
];
```

---

## Troubleshooting

### Common Issues

1. **Rules not executing**: Check that triggers reference the correct rule name
2. **Calculations not working**: Ensure `strictString: false` for numeric operations
3. **State not updating**: Verify `setState` is the correct React setState function

### Debug Tips

```jsx
const handleChange = createFormHandler({
  fields,
  setState: (newData) => {
    console.log("Form data updated:", newData);
    setFormData(newData);
  },
  rules
});
```

---

*The `createFormHandler` function is designed to handle complex form logic while keeping your components simple. Use it as the foundation for all your NovaForms implementations.*
