---
title: Rules & Effects
nextjs:
  metadata:
    title: Rules & Effects - NovaForms
    description: Learn how NovaForms rules and effects work together to create dynamic form behaviors and calculations.
---

The NovaForms rules system uses **effects** to define what happens when a rule is triggered. Think of effects as the "actions" that get executed - they're the specific things that change in your form when conditions are met.

---

## What Are Effects?

**Effects** are specific actions that modify field values or properties when a rule is triggered. They're like instructions that tell NovaForms:

- "When this happens, do this to that field"
- "Calculate this value and put it here"
- "Hide this field when this condition is true"
- "Make this field read-only when that happens"

### Simple Analogy

Think of effects like a recipe:
- **Rule** = The recipe name ("Make Chocolate Cake")
- **Trigger** = When to start cooking ("When guests arrive")
- **Effect** = The actual cooking steps ("Mix ingredients", "Bake for 30 minutes", "Add frosting")

---

## Effect Structure

Every effect is an object that specifies:

```jsx
{
  targetField: "fieldName",  // Which field to modify
  prop: "value",             // What property to change
  type: "add",               // What operation to perform
  kind: "number",            // What data type to work with
  value: 10,                 // The value to use
  // ... additional properties
}
```

### Effect Properties

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `targetField` | `string` | Field name to modify | `"total"` |
| `prop` | `string` | Property to modify | `"value"`, `"readOnly"`, `"hidden"` |
| `type` | `string` | Operation to perform | `"add"`, `"multiply"`, `"replace"` |
| `kind` | `string` | Data type | `"number"`, `"string"` |
| `value` | `any` | Value for operation | `10`, `"Hello"`, `true` |
| `sourceFields` | `Array` | Fields to use for operations | `[{field: "firstName"}]` |

---

## Types of Effects

### 1. Value Effects (Math Operations)

These effects modify field **values** using mathematical operations:

#### `add` - Addition
```jsx
{
  targetField: "total",
  prop: "value",
  type: "add",
  kind: "number",
  value: 10
}
// Result: total = total + 10
```

#### `subtract` - Subtraction
```jsx
{
  targetField: "discount",
  prop: "value", 
  type: "subtract",
  kind: "number",
  value: 5
}
// Result: discount = discount - 5
```

#### `multiply` - Multiplication
```jsx
{
  targetField: "totalWithTax",
  prop: "value",
  type: "multiply", 
  kind: "number",
  value: 1.08  // 8% tax
}
// Result: totalWithTax = totalWithTax * 1.08
```

#### `divide` - Division
```jsx
{
  targetField: "average",
  prop: "value",
  type: "divide",
  kind: "number", 
  value: 2
}
// Result: average = average / 2
```

#### `replace` - Replace Value
```jsx
{
  targetField: "status",
  prop: "value",
  type: "replace",
  value: "completed"
}
// Result: status = "completed"
```

### 2. String Effects

These effects work with text/string values:

#### `concat` - Concatenation
```jsx
{
  targetField: "fullName",
  prop: "value",
  type: "concat",
  sourceFields: [
    { field: "firstName", charBefore: "", charAfter: " " },
    { field: "lastName", charBefore: "", charAfter: "" }
  ]
}
// Result: fullName = firstName + " " + lastName
```

Simple concatenation:
```jsx
{
  targetField: "greeting",
  prop: "value", 
  type: "concat",
  value: "Hello, "
}
// Result: greeting = "Hello, " + greeting
```

### 3. Attribute Effects

These effects modify field **properties** (not values):

#### `readOnly` - Make Field Read-Only
```jsx
{
  targetField: "calculatedField",
  prop: "readOnly",
  value: true
}
// Result: calculatedField becomes read-only
```

#### `hidden` - Hide Field
```jsx
{
  targetField: "optionalField",
  prop: "hidden", 
  value: true
}
// Result: optionalField becomes hidden
```

#### `title` - Change Field Title
```jsx
{
  targetField: "email",
  prop: "title",
  value: "Primary Contact Email"
}
// Result: email field title changes to "Primary Contact Email"
```

#### `required` - Make Field Required
```jsx
{
  targetField: "phone",
  prop: "required",
  value: true
}
// Result: phone field becomes required
```

---

## Complete Examples

### Example 1: Order Calculation System

```jsx
const rules = [
  {
    name: "calculateSubtotal",
    effects: [
      {
        targetField: "subtotal",
        prop: "value",
        type: "multiply",
        kind: "number",
        value: 1  // This will be calculated from quantity * price
      }
    ]
  },
  {
    name: "calculateTax", 
    effects: [
      {
        targetField: "tax",
        prop: "value",
        type: "multiply",
        kind: "number",
        value: 0.08  // 8% tax
      }
    ]
  },
  {
    name: "calculateTotal",
    effects: [
      {
        targetField: "total",
        prop: "value", 
        type: "add",
        kind: "number",
        value: 0  // This will be subtotal + tax
      }
    ]
  }
];

const fields = [
  {
    name: "quantity",
    type: "number",
    title: "Quantity",
    default: 1,
    triggers: [
      { rule: "calculateSubtotal", when: "not empty" },
      { rule: "calculateTax", when: "not empty" },
      { rule: "calculateTotal", when: "not empty" }
    ]
  },
  {
    name: "price", 
    type: "number",
    title: "Price per Item",
    default: 0,
    triggers: [
      { rule: "calculateSubtotal", when: "not empty" },
      { rule: "calculateTax", when: "not empty" },
      { rule: "calculateTotal", when: "not empty" }
    ]
  },
  {
    name: "subtotal",
    type: "number", 
    title: "Subtotal",
    readOnly: true  // This field shows the calculated result
  },
  {
    name: "tax",
    type: "number",
    title: "Tax (8%)", 
    readOnly: true  // This field shows the calculated tax
  },
  {
    name: "total",
    type: "number",
    title: "Total",
    readOnly: true  // This field shows the final total
  }
];
```

**What happens:**
1. User enters quantity and price
2. Rules trigger and effects execute:
   - `subtotal` = quantity × price
   - `tax` = subtotal × 0.08
   - `total` = subtotal + tax

### Example 2: Dynamic Field Behavior

```jsx
const rules = [
  {
    name: "lockAgeField",
    effects: [
      {
        targetField: "age",
        prop: "readOnly",  // Changing the readOnly property
        value: true
      }
    ]
  },
  {
    name: "hideDiscountField", 
    effects: [
      {
        targetField: "discount",
        prop: "hidden",  // Changing the hidden property
        value: true
      }
    ]
  },
  {
    name: "changeEmailTitle",
    effects: [
      {
        targetField: "email",
        prop: "title",  // Changing the title property
        value: "Primary Contact Email"
      }
    ]
  }
];

const fields = [
  {
    name: "userType",
    type: "select",
    title: "User Type",
    options: [
      { value: "admin", label: "Administrator" },
      { value: "user", label: "Regular User" }
    ],
    triggers: [
      {
        rule: "lockAgeField",
        when: "equal", 
        value: "admin"
      }
    ]
  },
  {
    name: "age",
    type: "number",
    title: "Age"
  },
  {
    name: "orderTotal",
    type: "number", 
    title: "Order Total",
    triggers: [
      {
        rule: "hideDiscountField",
        when: "less than",
        value: 100
      }
    ]
  },
  {
    name: "discount",
    type: "number",
    title: "Discount Amount"
  },
  {
    name: "email",
    type: "email",
    title: "Email",
    triggers: [
      {
        rule: "changeEmailTitle", 
        when: "not empty"
      }
    ]
  }
];
```

**What happens:**
1. When user selects "admin" → `age` field becomes read-only
2. When order total < 100 → `discount` field becomes hidden
3. When email has a value → email field title changes to "Primary Contact Email"

### Example 3: String Concatenation

```jsx
const rules = [
  {
    name: "createFullName",
    effects: [
      {
        targetField: "fullName",
        prop: "value",
        type: "concat",
        sourceFields: [
          { field: "firstName", charBefore: "", charAfter: " " },
          { field: "lastName", charBefore: "", charAfter: "" }
        ]
      }
    ]
  },
  {
    name: "createGreeting",
    effects: [
      {
        targetField: "greeting", 
        prop: "value",
        type: "concat",
        value: "Hello, "
      }
    ]
  }
];

const fields = [
  {
    name: "firstName",
    type: "string",
    title: "First Name",
    triggers: [
      { rule: "createFullName", when: "not empty" },
      { rule: "createGreeting", when: "not empty" }
    ]
  },
  {
    name: "lastName",
    type: "string", 
    title: "Last Name",
    triggers: [
      { rule: "createFullName", when: "not empty" }
    ]
  },
  {
    name: "fullName",
    type: "string",
    title: "Full Name",
    readOnly: true
  },
  {
    name: "greeting",
    type: "string",
    title: "Greeting", 
    readOnly: true
  }
];
```

**What happens:**
1. User enters "John" in firstName → `greeting` becomes "Hello, John"
2. User enters "Doe" in lastName → `fullName` becomes "John Doe"

---

## Effect vs Field Conditions

It's important to understand the difference between **effects** and **field conditions**:

### Effects (Rule-based)
- **When**: Triggered by rules when conditions are met
- **Where**: Defined in the `rules` array
- **What**: Can modify any field property or value
- **How**: Executed by the rules system

```jsx
// Effect in rules array
{
  name: "hideField",
  effects: [
    {
      targetField: "optionalField",
      prop: "hidden",
      value: true
    }
  ]
}
```

### Field Conditions (Direct)
- **When**: Evaluated directly on the field
- **Where**: Defined in the field's `conditions` property
- **What**: Can only hide or make read-only
- **How**: Evaluated immediately when field values change

```jsx
// Condition in field definition
{
  name: "optionalField",
  type: "string",
  conditions: {
    hiddenWhen: [
      { field: "otherField", when: "equal", value: "hide" }
    ]
  }
}
```

---

## Best Practices

### 1. Use Effects for Complex Logic
```jsx
// ✅ Good: Use effects for calculations
{
  name: "calculateTotal",
  effects: [
    {
      targetField: "total",
      prop: "value",
      type: "add",
      kind: "number",
      value: 0
    }
  ]
}

// ❌ Avoid: Using field conditions for calculations
// (Field conditions can't do math)
```

### 2. Use Field Conditions for Simple Show/Hide
```jsx
// ✅ Good: Use field conditions for simple visibility
{
  name: "phone",
  conditions: {
    hiddenWhen: [
      { field: "contactMethod", when: "equal", value: "email" }
    ]
  }
}

// ❌ Avoid: Using effects for simple show/hide
// (Effects are overkill for this)
```

### 3. Name Effects Clearly
```jsx
// ✅ Good: Clear effect names
{
  name: "calculateOrderTotal",
  effects: [...]
}

// ❌ Avoid: Unclear names
{
  name: "rule1",
  effects: [...]
}
```

### 4. Group Related Effects
```jsx
// ✅ Good: Group related effects in one rule
{
  name: "processOrder",
  effects: [
    { targetField: "subtotal", prop: "value", type: "multiply", kind: "number", value: 1 },
    { targetField: "tax", prop: "value", type: "multiply", kind: "number", value: 0.08 },
    { targetField: "total", prop: "value", type: "add", kind: "number", value: 0 }
  ]
}
```

---

## Troubleshooting Effects

### Common Issues

1. **Effect not executing**: Check trigger conditions
2. **Wrong calculation**: Verify `kind` and `type` properties
3. **Field not updating**: Ensure `targetField` name matches exactly

### Debug Tips

```jsx
// Add logging to see effects in action
const handleChange = createFormHandler({
  fields,
  setState: (newData) => {
    console.log("Form data after effects:", newData);
    setFormData(newData);
  },
  rules
});
```

---

*Effects are the "actions" in NovaForms rules - they define exactly what happens when conditions are met. Use them to create dynamic calculations, field modifications, and complex form behaviors.*
