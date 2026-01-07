---
title: Triggers & Conditions
nextjs:
  metadata:
    title: Triggers & Conditions - NovaForms
    description: Learn how to use triggers and conditions to create dynamic, interactive forms in NovaForms.
---

Triggers and conditions are the mechanisms that make NovaForms dynamic. They allow fields to respond to changes in other fields, creating interactive and intelligent forms.

---

## Overview

NovaForms provides two levels of conditional logic:

1. **Triggers**: Field-level conditions that activate rules when met
2. **Conditions**: Direct field-level logic for show/hide and enable/disable

---

## Triggers

Triggers are defined on fields and reference top-level rules. When a trigger's condition is met, the referenced rule executes.

### Basic Trigger Structure

```jsx
{
  name: "fieldName",
  type: "string",
  triggers: [
    {
      rule: "ruleName",        // Name of the rule to trigger
      when: "condition",       // Condition type
      value: "comparisonValue" // Value to compare against
    }
  ]
}
```

### Trigger Properties

| Property | Type | Description |
|----------|------|-------------|
| `rule` | `string` | Name of the rule to trigger |
| `when` | `string` | Condition type (see conditions below) |
| `value` | `any` | Value to compare against |
| `mode` | `string` | Logic mode for multiple conditions (`all` or `any`) |

### Simple Triggers

```jsx
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

### Complex Triggers

Triggers can have multiple conditions with AND/OR logic:

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

---

## Conditions

Conditions provide direct field-level logic without requiring rules. They control field visibility and state.

### Condition Structure

```jsx
{
  name: "fieldName",
  type: "string",
  conditions: {
    hiddenWhen: [              // Conditions to hide field
      { field: "otherField", when: "equal", value: "hide" }
    ],
    hiddenMode: "any",         // Logic mode (any/all)
    readOnlyWhen: [            // Conditions to make read-only
      { field: "otherField", when: "equal", value: "lock" }
    ],
    readOnlyMode: "any"        // Logic mode (any/all)
  }
}
```

### Condition Properties

| Property | Type | Description |
|----------|------|-------------|
| `field` | `string` | Field name to check |
| `when` | `string` | Condition type (see types below) |
| `value` | `any` | Value to compare against |
| `mode` | `string` | Logic mode for multiple conditions (`any` or `all`) |

---

## Condition Types

### Basic Conditions

#### `true` - Field has any value
```jsx
{ field: "email", when: "true" }
```

#### `false` - Field is empty/falsy
```jsx
{ field: "email", when: "false" }
```

#### `empty` - Field is empty
```jsx
{ field: "email", when: "empty" }
```

#### `not empty` - Field has a value
```jsx
{ field: "email", when: "not empty" }
```

#### `null` - Field is null/undefined
```jsx
{ field: "email", when: "null" }
```

#### `not null` - Field is not null/undefined
```jsx
{ field: "email", when: "not null" }
```

### Comparison Conditions

#### `equal` - Field equals value
```jsx
{ field: "status", when: "equal", value: "active" }
```

#### `not equal` - Field doesn't equal value
```jsx
{ field: "status", when: "not equal", value: "inactive" }
```

#### `less than` - Field is less than value
```jsx
{ field: "age", when: "less than", value: 18 }
```

#### `greater than` - Field is greater than value
```jsx
{ field: "age", when: "greater than", value: 65 }
```

#### `between` - Field is between two values
```jsx
{ field: "age", when: "between", value: [18, 65] }
```

### Pattern Conditions

#### `matches` - Field matches regex pattern
```jsx
{ field: "email", when: "matches", value: ".*@company\\.com$" }
```

---

## Complete Examples

### Dynamic Field Visibility

```jsx
const fields = [
  {
    name: "contactMethod",
    type: "select",
    title: "Preferred Contact Method",
    options: [
      { value: "email", label: "Email" },
      { value: "phone", label: "Phone" },
      { value: "either", label: "Either" }
    ]
  },
  {
    name: "email",
    type: "email",
    title: "Email Address",
    conditions: {
      hiddenWhen: [
        { field: "contactMethod", when: "equal", value: "phone" }
      ]
    }
  },
  {
    name: "phone",
    type: "tel",
    title: "Phone Number",
    conditions: {
      hiddenWhen: [
        { field: "contactMethod", when: "equal", value: "email" }
      ]
    }
  }
];
```

### Conditional Field State

```jsx
const fields = [
  {
    name: "userType",
    type: "select",
    title: "User Type",
    options: [
      { value: "admin", label: "Administrator" },
      { value: "user", label: "Regular User" }
    ]
  },
  {
    name: "age",
    type: "number",
    title: "Age",
    conditions: {
      readOnlyWhen: [
        { field: "userType", when: "equal", value: "admin" }
      ]
    }
  },
  {
    name: "salary",
    type: "number",
    title: "Salary",
    conditions: {
      hiddenWhen: [
        { field: "userType", when: "not equal", value: "admin" }
      ]
    }
  }
];
```

### Complex Multi-Condition Logic

```jsx
const fields = [
  {
    name: "orderTotal",
    type: "number",
    title: "Order Total"
  },
  {
    name: "customerType",
    type: "select",
    title: "Customer Type",
    options: [
      { value: "regular", label: "Regular" },
      { value: "premium", label: "Premium" },
      { value: "vip", label: "VIP" }
    ]
  },
  {
    name: "discount",
    type: "number",
    title: "Discount Amount",
    conditions: {
      hiddenWhen: [
        { field: "orderTotal", when: "less than", value: 100 },
        { field: "customerType", when: "equal", value: "regular" }
      ],
      hiddenMode: "any"  // Hide if EITHER condition is true
    }
  },
  {
    name: "freeShipping",
    type: "boolean",
    title: "Free Shipping",
    conditions: {
      readOnlyWhen: [
        { field: "orderTotal", when: "greater than", value: 50 },
        { field: "customerType", when: "not equal", value: "regular" }
      ],
      readOnlyMode: "all"  // Read-only if BOTH conditions are true
    }
  }
];
```

### Pattern-Based Conditions

```jsx
const fields = [
  {
    name: "email",
    type: "email",
    title: "Email Address"
  },
  {
    name: "companyEmail",
    type: "email",
    title: "Company Email",
    conditions: {
      hiddenWhen: [
        { field: "email", when: "matches", value: ".*@company\\.com$" }
      ]
    }
  },
  {
    name: "personalEmail",
    type: "email",
    title: "Personal Email",
    conditions: {
      hiddenWhen: [
        { field: "email", when: "not matches", value: ".*@company\\.com$" }
      ]
    }
  }
];
```

### Age-Based Conditions

```jsx
const fields = [
  {
    name: "age",
    type: "number",
    title: "Age"
  },
  {
    name: "parentalConsent",
    type: "boolean",
    title: "Parental Consent Required",
    conditions: {
      hiddenWhen: [
        { field: "age", when: "greater than", value: 18 }
      ]
    }
  },
  {
    name: "seniorDiscount",
    type: "boolean",
    title: "Senior Discount",
    conditions: {
      hiddenWhen: [
        { field: "age", when: "less than", value: 65 }
      ]
    }
  },
  {
    name: "adultContent",
    type: "boolean",
    title: "Access to Adult Content",
    conditions: {
      readOnlyWhen: [
        { field: "age", when: "less than", value: 18 }
      ]
    }
  }
];
```

### Range-Based Conditions

```jsx
const fields = [
  {
    name: "score",
    type: "number",
    title: "Test Score (0-100)"
  },
  {
    name: "grade",
    type: "select",
    title: "Grade",
    options: [
      { value: "A", label: "A (90-100)" },
      { value: "B", label: "B (80-89)" },
      { value: "C", label: "C (70-79)" },
      { value: "D", label: "D (60-69)" },
      { value: "F", label: "F (0-59)" }
    ],
    conditions: {
      hiddenWhen: [
        { field: "score", when: "between", value: [90, 100] }
      ]
    }
  }
];
```

---

## Advanced Patterns

### Cascading Conditions

```jsx
const fields = [
  {
    name: "country",
    type: "select",
    title: "Country",
    options: [
      { value: "us", label: "United States" },
      { value: "ca", label: "Canada" },
      { value: "mx", label: "Mexico" }
    ]
  },
  {
    name: "state",
    type: "select",
    title: "State/Province",
    conditions: {
      hiddenWhen: [
        { field: "country", when: "not equal", value: "us" }
      ]
    }
  },
  {
    name: "province",
    type: "select",
    title: "Province",
    conditions: {
      hiddenWhen: [
        { field: "country", when: "not equal", value: "ca" }
      ]
    }
  },
  {
    name: "municipality",
    type: "select",
    title: "Municipality",
    conditions: {
      hiddenWhen: [
        { field: "country", when: "not equal", value: "mx" }
      ]
    }
  }
];
```

### Multi-Field Dependencies

```jsx
const fields = [
  {
    name: "hasAddress",
    type: "boolean",
    title: "Do you have a mailing address?"
  },
  {
    name: "street",
    type: "string",
    title: "Street Address",
    conditions: {
      hiddenWhen: [
        { field: "hasAddress", when: "equal", value: false }
      ]
    }
  },
  {
    name: "city",
    type: "string",
    title: "City",
    conditions: {
      hiddenWhen: [
        { field: "hasAddress", when: "equal", value: false }
      ]
    }
  },
  {
    name: "zip",
    type: "string",
    title: "ZIP Code",
    conditions: {
      hiddenWhen: [
        { field: "hasAddress", when: "equal", value: false },
        { field: "street", when: "empty" }
      ],
      hiddenMode: "any"
    }
  }
];
```

---

## Best Practices

### 1. Use Clear Condition Logic
```jsx
// ✅ Good: Clear and readable
{ field: "age", when: "greater than", value: 18 }

// ❌ Avoid: Unclear conditions
{ field: "age", when: "gt", value: 18 }
```

### 2. Group Related Conditions
```jsx
// ✅ Good: Grouped conditions
const fields = [
  {
    name: "personalInfo",
    type: "subForm",
    fields: [
      { name: "firstName", type: "string", title: "First Name" },
      { name: "lastName", type: "string", title: "Last Name" }
    ]
  }
];
```

### 3. Use Appropriate Logic Modes
```jsx
// ✅ Good: Use "all" when both conditions must be true
{
  hiddenWhen: [
    { field: "age", when: "less than", value: 18 },
    { field: "parentalConsent", when: "equal", value: false }
  ],
  hiddenMode: "all"
}

// ✅ Good: Use "any" when either condition can be true
{
  hiddenWhen: [
    { field: "orderTotal", when: "less than", value: 50 },
    { field: "customerType", when: "equal", value: "regular" }
  ],
  hiddenMode: "any"
}
```

### 4. Test Edge Cases
```jsx
// ✅ Good: Handle edge cases
{
  name: "age",
  type: "number",
  conditions: {
    readOnlyWhen: [
      { field: "age", when: "less than", value: 0 },
      { field: "age", when: "greater than", value: 120 }
    ],
    readOnlyMode: "any"
  }
}
```

---

## Troubleshooting

### Common Issues

1. **Conditions not working**: Check field names match exactly
2. **Logic not behaving as expected**: Verify `mode` setting (`any` vs `all`)
3. **Pattern matching issues**: Escape special regex characters

### Debug Tips

```jsx
// Add console logging to debug conditions
const handleChange = createFormHandler({
  fields,
  setState: (newData) => {
    console.log("Form data:", newData);
    setFormData(newData);
  },
  rules
});
```

---

*Triggers and conditions are the foundation of dynamic forms in NovaForms. Use them to create intelligent, responsive forms that adapt to user input.*
