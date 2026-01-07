---
title: Dynamic Disable
nextjs:
  metadata:
    title: Dynamic Disable - NovaForms
    description: Learn how to dynamically enable and disable fields based on other field values in NovaForms.
---

Dynamic field disabling allows you to make fields read-only or disabled based on the values of other fields. NovaForms provides two ways to implement dynamic disabling: field-level conditions and rule-based attribute effects.

---

## Overview

There are two approaches to dynamic field disabling in NovaForms:

1. **Field Conditions**: Direct field-level logic using `conditions.readOnlyWhen`
2. **Rule Effects**: Top-level rules that modify field attributes using `prop: "readOnly"`

---

## Method 1: Field Conditions

Field conditions provide a direct way to disable fields based on other field values.

### Basic Syntax

```jsx
{
  name: "fieldName",
  type: "string",
  conditions: {
    readOnlyWhen: [
      { field: "otherField", when: "equal", value: "disable" }
    ],
    readOnlyMode: "any"  // or "all"
  }
}
```

### Simple Examples

#### Disable field when another field equals a value
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
  }
];
```

#### Disable field when another field is empty
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
      readOnlyWhen: [
        { field: "hasAddress", when: "equal", value: false }
      ]
    }
  }
];
```

#### Disable field when another field is not empty
```jsx
const fields = [
  {
    name: "email",
    type: "email",
    title: "Email Address"
  },
  {
    name: "alternativeEmail",
    type: "email",
    title: "Alternative Email",
    conditions: {
      readOnlyWhen: [
        { field: "email", when: "not empty" }
      ]
    }
  }
];
```

---

## Complex Examples

### Multiple conditions with AND logic
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
      { value: "premium", label: "Premium" }
    ]
  },
  {
    name: "discount",
    type: "number",
    title: "Discount Amount",
    conditions: {
      readOnlyWhen: [
        { field: "orderTotal", when: "less than", value: 100 },
        { field: "customerType", when: "equal", value: "regular" }
      ],
      readOnlyMode: "all"  // Disable if BOTH conditions are true
    }
  }
];
```

### Multiple conditions with OR logic
```jsx
const fields = [
  {
    name: "age",
    type: "number",
    title: "Age"
  },
  {
    name: "adultContent",
    type: "boolean",
    title: "Access to Adult Content",
    conditions: {
      readOnlyWhen: [
        { field: "age", when: "less than", value: 18 },
        { field: "age", when: "greater than", value: 65 }
      ],
      readOnlyMode: "any"  // Disable if EITHER condition is true
    }
  }
];
```

### Range-based disabling
```jsx
const fields = [
  {
    name: "score",
    type: "number",
    title: "Test Score (0-100)"
  },
  {
    name: "retakeOption",
    type: "boolean",
    title: "Retake Test",
    conditions: {
      readOnlyWhen: [
        { field: "score", when: "between", value: [70, 100] }
      ]
    }
  }
];
```

### Pattern-based disabling
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
      readOnlyWhen: [
        { field: "email", when: "matches", value: ".*@company\\.com$" }
      ]
    }
  }
];
```

---

## Method 2: Rule Effects

Rule effects provide a more powerful way to disable fields using the rules system.

### Basic Syntax

```jsx
const rules = [
  {
    name: "disableFieldRule",
    effects: [
      {
        targetField: "fieldToDisable",
        prop: "readOnly",
        value: true
      }
    ]
  }
];

const fields = [
  {
    name: "triggerField",
    type: "string",
    triggers: [
      {
        rule: "disableFieldRule",
        when: "equal",
        value: "disable"
      }
    ]
  },
  {
    name: "fieldToDisable",
    type: "string",
    title: "Field to Disable"
  }
];
```

### Rule-Based Examples

#### Disable field based on complex conditions
```jsx
const rules = [
  {
    name: "disableDiscountField",
    effects: [
      {
        targetField: "discount",
        prop: "readOnly",
        value: true
      }
    ]
  }
];

const fields = [
  {
    name: "orderTotal",
    type: "number",
    title: "Order Total",
    triggers: [
      {
        rule: "disableDiscountField",
        when: "less than",
        value: 50
      }
    ]
  },
  {
    name: "discount",
    type: "number",
    title: "Discount Amount"
  }
];
```

#### Toggle field state
```jsx
const rules = [
  {
    name: "disableField",
    effects: [
      {
        targetField: "optionalField",
        prop: "readOnly",
        value: true
      }
    ]
  },
  {
    name: "enableField",
    effects: [
      {
        targetField: "optionalField",
        prop: "readOnly",
        value: false
      }
    ]
  }
];

const fields = [
  {
    name: "enableOptional",
    type: "boolean",
    title: "Enable Optional Field",
    triggers: [
      {
        rule: "enableField",
        when: "equal",
        value: true
      },
      {
        rule: "disableField",
        when: "equal",
        value: false
      }
    ]
  },
  {
    name: "optionalField",
    type: "string",
    title: "Optional Field"
  }
];
```

---

## Complete Examples

### User Registration with Age-Based Restrictions

```jsx
const fields = [
  {
    name: "age",
    type: "number",
    title: "Age",
    required: true
  },
  {
    name: "parentalConsent",
    type: "boolean",
    title: "Parental Consent Required",
    conditions: {
      readOnlyWhen: [
        { field: "age", when: "greater than", value: 18 }
      ]
    }
  },
  {
    name: "guardianName",
    type: "string",
    title: "Guardian Name",
    conditions: {
      readOnlyWhen: [
        { field: "age", when: "greater than", value: 18 }
      ]
    }
  },
  {
    name: "guardianPhone",
    type: "tel",
    title: "Guardian Phone",
    conditions: {
      readOnlyWhen: [
        { field: "age", when: "greater than", value: 18 }
      ]
    }
  },
  {
    name: "seniorDiscount",
    type: "boolean",
    title: "Senior Discount (65+)",
    conditions: {
      readOnlyWhen: [
        { field: "age", when: "less than", value: 65 }
      ]
    }
  }
];
```

### Order Form with Conditional Restrictions

```jsx
const rules = [
  {
    name: "disableShippingFields",
    effects: [
      {
        targetField: "shippingAddress",
        prop: "readOnly",
        value: true
      },
      {
        targetField: "shippingMethod",
        prop: "readOnly",
        value: true
      }
    ]
  }
];

const fields = [
  {
    name: "orderType",
    type: "select",
    title: "Order Type",
    options: [
      { value: "pickup", label: "Pickup" },
      { value: "delivery", label: "Delivery" }
    ]
  },
  {
    name: "shippingAddress",
    type: "subForm",
    title: "Shipping Address",
    fields: [
      { name: "street", type: "string", title: "Street", width: 100 },
      { name: "city", type: "string", title: "City", width: 50 },
      { name: "state", type: "string", title: "State", width: 25 },
      { name: "zip", type: "string", title: "ZIP", width: 25 }
    ],
    triggers: [
      {
        rule: "disableShippingFields",
        when: "equal",
        value: "pickup"
      }
    ]
  },
  {
    name: "shippingMethod",
    type: "select",
    title: "Shipping Method",
    options: [
      { value: "standard", label: "Standard (5-7 days)" },
      { value: "express", label: "Express (2-3 days)" },
      { value: "overnight", label: "Overnight" }
    ],
    triggers: [
      {
        rule: "disableShippingFields",
        when: "equal",
        value: "pickup"
      }
    ]
  }
];
```

### Form with Validation-Based Restrictions

```jsx
const fields = [
  {
    name: "email",
    type: "email",
    title: "Email Address",
    required: true
  },
  {
    name: "alternativeEmail",
    type: "email",
    title: "Alternative Email",
    conditions: {
      readOnlyWhen: [
        { field: "email", when: "not empty" }
      ]
    }
  },
  {
    name: "phone",
    type: "tel",
    title: "Phone Number",
    conditions: {
      readOnlyWhen: [
        { field: "email", when: "not empty" },
        { field: "alternativeEmail", when: "not empty" }
      ],
      readOnlyMode: "all"
    }
  }
];
```

### Multi-Step Form with Progress Restrictions

```jsx
const fields = [
  {
    name: "step",
    type: "number",
    title: "Current Step",
    default: 1,
    readOnly: true
  },
  {
    name: "personalInfo",
    type: "subForm",
    title: "Personal Information",
    fields: [
      { name: "firstName", type: "string", title: "First Name", width: 50 },
      { name: "lastName", type: "string", title: "Last Name", width: 50 },
      { name: "email", type: "email", title: "Email", width: 100 }
    ],
    conditions: {
      readOnlyWhen: [
        { field: "step", when: "not equal", value: 1 }
      ]
    }
  },
  {
    name: "contactInfo",
    type: "subForm",
    title: "Contact Information",
    fields: [
      { name: "phone", type: "tel", title: "Phone", width: 50 },
      { name: "address", type: "text", title: "Address", width: 100 }
    ],
    conditions: {
      readOnlyWhen: [
        { field: "step", when: "not equal", value: 2 }
      ]
    }
  },
  {
    name: "preferences",
    type: "subForm",
    title: "Preferences",
    fields: [
      { name: "newsletter", type: "boolean", title: "Newsletter", width: 100 },
      { name: "notifications", type: "boolean", title: "Notifications", width: 100 }
    ],
    conditions: {
      readOnlyWhen: [
        { field: "step", when: "not equal", value: 3 }
      ]
    }
  }
];
```

---

## Advanced Patterns

### Cascading Field Restrictions

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
      readOnlyWhen: [
        { field: "hasAddress", when: "equal", value: false }
      ]
    }
  },
  {
    name: "city",
    type: "string",
    title: "City",
    conditions: {
      readOnlyWhen: [
        { field: "hasAddress", when: "equal", value: false },
        { field: "street", when: "empty" }
      ],
      readOnlyMode: "any"
    }
  },
  {
    name: "zip",
    type: "string",
    title: "ZIP Code",
    conditions: {
      readOnlyWhen: [
        { field: "hasAddress", when: "equal", value: false },
        { field: "street", when: "empty" },
        { field: "city", when: "empty" }
      ],
      readOnlyMode: "any"
    }
  }
];
```

### Conditional Field Dependencies

```jsx
const fields = [
  {
    name: "userType",
    type: "select",
    title: "User Type",
    options: [
      { value: "admin", label: "Administrator" },
      { value: "user", label: "Regular User" },
      { value: "guest", label: "Guest" }
    ]
  },
  {
    name: "permissions",
    type: "multiselect",
    title: "Permissions",
    options: [
      { value: "read", label: "Read" },
      { value: "write", label: "Write" },
      { value: "delete", label: "Delete" },
      { value: "admin", label: "Admin" }
    ],
    conditions: {
      readOnlyWhen: [
        { field: "userType", when: "equal", value: "guest" }
      ]
    }
  },
  {
    name: "adminSettings",
    type: "subForm",
    title: "Admin Settings",
    fields: [
      { name: "canDelete", type: "boolean", title: "Can Delete Users" },
      { name: "canModify", type: "boolean", title: "Can Modify Settings" }
    ],
    conditions: {
      readOnlyWhen: [
        { field: "userType", when: "not equal", value: "admin" }
      ]
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

### 2. Group Related Fields
```jsx
// ✅ Good: Group related fields
const fields = [
  {
    name: "contactInfo",
    type: "subForm",
    title: "Contact Information",
    fields: [
      { name: "email", type: "email", title: "Email" },
      { name: "phone", type: "tel", title: "Phone" }
    ]
  }
];
```

### 3. Use Appropriate Logic Modes
```jsx
// ✅ Good: Use "all" when both conditions must be true
{
  readOnlyWhen: [
    { field: "age", when: "less than", value: 18 },
    { field: "parentalConsent", when: "equal", value: false }
  ],
  readOnlyMode: "all"
}

// ✅ Good: Use "any" when either condition can be true
{
  readOnlyWhen: [
    { field: "orderTotal", when: "less than", value: 50 },
    { field: "customerType", when: "equal", value: "regular" }
  ],
  readOnlyMode: "any"
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

1. **Fields not disabling**: Check field names match exactly
2. **Logic not behaving as expected**: Verify `readOnlyMode` setting
3. **Performance issues**: Avoid too many complex conditions

### Debug Tips

```jsx
// Add console logging to debug disabling logic
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

*Dynamic field disabling is essential for creating intuitive, user-friendly forms. Use it to guide users through complex form flows and prevent invalid data entry.*
