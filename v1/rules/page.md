---
title: Rules System
nextjs:
  metadata:
    title: Rules System - NovaForms
    description: Learn how to use NovaForms rules system for complex form logic, calculations, and dynamic behaviors.
---

The NovaForms rules system is a powerful feature that allows you to create complex, dynamic form behaviors. Rules define reusable logic that can be triggered by field changes, enabling calculations, field modifications, and conditional logic.

---

## Overview

The rules system consists of three main components:

1. **Rules**: Named collections of effects that define what should happen
2. **Triggers**: Field-level conditions that determine when rules should execute
3. **Effects**: Specific actions that modify field values or attributes

---

## Rule Structure

A rule is a JavaScript object with a unique name and an array of effects:

```jsx
const rules = [
  {
    name: "ruleName",           // Unique identifier
    effects: [                  // Array of effects to apply
      {
        targetField: "fieldName", // Field to modify
        prop: "value",           // Property to modify
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

---

## Effect Properties

| Property | Type | Description |
|----------|------|-------------|
| `targetField` | `string` | Field name to modify |
| `prop` | `string` | Property to modify (`value`, `readOnly`, `hidden`, `title`, etc.) |
| `type` | `string` | Operation type (see operations below) |
| `kind` | `string` | Data type (`number`, `string`) |
| `value` | `any` | Value for operation |
| `strictString` | `boolean` | Force string operations |
| `sourceFields` | `Array` | For concat operations with multiple fields |

---

## Operation Types

### Math Operations

#### `add` - Addition
Add a value to the target field.

```jsx
{
  targetField: "total",
  type: "add",
  kind: "number",
  value: 10
}
```

#### `subtract` - Subtraction
Subtract a value from the target field.

```jsx
{
  targetField: "discount",
  type: "subtract",
  kind: "number",
  value: 5
}
```

#### `multiply` - Multiplication
Multiply the target field by a value.

```jsx
{
  targetField: "totalWithTax",
  type: "multiply",
  kind: "number",
  value: 1.08  // 8% tax
}
```

#### `divide` - Division
Divide the target field by a value.

```jsx
{
  targetField: "average",
  type: "divide",
  kind: "number",
  value: 2
}
```

#### `replace` - Replace Value
Replace the target field with a new value.

```jsx
{
  targetField: "status",
  type: "replace",
  kind: "string",
  value: "completed"
}
```

### String Operations

#### `concat` - Concatenation
Concatenate strings with custom separators.

```jsx
{
  targetField: "fullName",
  type: "concat",
  sourceFields: [
    { field: "firstName", charBefore: "", charAfter: " " },
    { field: "lastName", charBefore: "", charAfter: "" }
  ]
}
```

---

## Triggers

Triggers define when rules should execute. They can be simple or complex:

### Simple Triggers

```jsx
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
```

### Complex Triggers

```jsx
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
```

### Trigger Conditions

| Condition | Description | Example |
|-----------|-------------|---------|
| `equal` | Field equals value | `{ field: "status", when: "equal", value: "active" }` |
| `not equal` | Field doesn't equal value | `{ field: "status", when: "not equal", value: "inactive" }` |
| `greater than` | Field is greater than value | `{ field: "age", when: "greater than", value: 18 }` |
| `less than` | Field is less than value | `{ field: "age", when: "less than", value: 65 }` |
| `not empty` | Field has a value | `{ field: "email", when: "not empty" }` |
| `empty` | Field is empty | `{ field: "optional", when: "empty" }` |
| `matches` | Field matches pattern | `{ field: "code", when: "matches", value: "ABC123" }` |

---

## Examples

### Simple Calculation

```jsx
const rules = [
  {
    name: "calculateTax",
    effects: [
      {
        targetField: "tax",
        type: "multiply",
        kind: "number",
        value: 0.08  // 8% tax
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
    name: "tax",
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

### Complex Multi-Step Calculation

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

### Conditional Field Modification

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

const fields = [
  {
    name: "isMinor",
    type: "boolean",
    triggers: [
      {
        rule: "lockAgeField",
        when: [
          { field: "isMinor", when: "equal", value: true }
        ]
      }
    ]
  },
  {
    name: "age",
    type: "number"
  }
];
```

---

## Best Practices

### 1. Use Descriptive Rule Names
```jsx
// ✅ Good
const rules = [
  {
    name: "calculateOrderTotalWithTax",
    effects: [...]
  }
];

// ❌ Avoid
const rules = [
  {
    name: "calc1",
    effects: [...]
  }
];
```

### 2. Keep Rules Focused
Each rule should have a single responsibility:

```jsx
// ✅ Good: Separate rules
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

### 3. Use Appropriate Data Types
```jsx
// ✅ Good: Specify data types
{
  targetField: "total",
  type: "add",
  kind: "number",
  value: 10
}

// ❌ Avoid: Missing data types
{
  targetField: "total",
  type: "add",
  value: 10
}
```

---

## Advanced Features

### Multiple Effects per Rule
Rules can have multiple effects that execute together:

```jsx
const rules = [
  {
    name: "processOrder",
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
      },
      {
        targetField: "status",
        prop: "value",
        type: "replace",
        kind: "string",
        value: "processed"
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
    name: "toggleFieldVisibility",
    effects: [
      {
        targetField: "optionalField",
        prop: "hidden",
        value: true
      }
    ]
  }
];
```

---

## Troubleshooting

### Common Issues

1. **Rules not executing**: Check that triggers reference the correct rule name
2. **Calculations not working**: Ensure `kind: "number"` for numeric operations
3. **String concatenation issues**: Use `sourceFields` for complex string operations

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

*The rules system is designed to handle complex form logic while keeping your components simple. Use it to create dynamic, interactive forms that respond intelligently to user input.*
