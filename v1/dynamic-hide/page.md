---
title: Dynamic Hide
nextjs:
  metadata:
    title: Dynamic Hide - NovaForms
    description: Learn how to dynamically show and hide fields based on other field values in NovaForms.
---

Dynamic field hiding allows you to show or hide fields based on the values of other fields. NovaForms provides two ways to implement dynamic hiding: field-level conditions and rule-based attribute effects.

---

## Overview

There are two approaches to dynamic field hiding in NovaForms:

1. **Field Conditions**: Direct field-level logic using `conditions.hiddenWhen`
2. **Rule Effects**: Top-level rules that modify field attributes using `prop: "hidden"`

---

## Method 1: Field Conditions

Field conditions provide a direct way to hide fields based on other field values.

### Basic Syntax

```jsx
{
  name: "fieldName",
  type: "string",
  conditions: {
    hiddenWhen: [
      { field: "otherField", when: "equal", value: "hide" }
    ],
    hiddenMode: "any"  // or "all"
  }
}
```

### Simple Examples

#### Hide field when another field equals a value
```jsx
const fields = [
  {
    name: "contactMethod",
    type: "select",
    title: "Preferred Contact Method",
    options: [
      { value: "email", label: "Email" },
      { value: "phone", label: "Phone" }
    ]
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

#### Hide field when another field is empty
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
  }
];
```

#### Hide field when another field is not empty
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
      hiddenWhen: [
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
      hiddenWhen: [
        { field: "orderTotal", when: "less than", value: 100 },
        { field: "customerType", when: "equal", value: "regular" }
      ],
      hiddenMode: "all"  // Hide if BOTH conditions are true
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
    name: "parentalConsent",
    type: "boolean",
    title: "Parental Consent Required",
    conditions: {
      hiddenWhen: [
        { field: "age", when: "greater than", value: 18 },
        { field: "age", when: "less than", value: 13 }
      ],
      hiddenMode: "any"  // Hide if EITHER condition is true
    }
  }
];
```

### Range-based hiding
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
      hiddenWhen: [
        { field: "score", when: "between", value: [70, 100] }
      ]
    }
  }
];
```

### Pattern-based hiding
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
  }
];
```

---

## Method 2: Rule Effects

Rule effects provide a more powerful way to hide fields using the rules system.

### Basic Syntax

```jsx
const rules = [
  {
    name: "hideFieldRule",
    effects: [
      {
        targetField: "fieldToHide",
        prop: "hidden",
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
        rule: "hideFieldRule",
        when: "equal",
        value: "hide"
      }
    ]
  },
  {
    name: "fieldToHide",
    type: "string",
    title: "Field to Hide"
  }
];
```

### Rule-Based Examples

#### Hide field based on complex conditions
```jsx
const rules = [
  {
    name: "hideDiscountField",
    effects: [
      {
        targetField: "discount",
        prop: "hidden",
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
        rule: "hideDiscountField",
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

#### Toggle field visibility
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
  },
  {
    name: "showField",
    effects: [
      {
        targetField: "optionalField",
        prop: "hidden",
        value: false
      }
    ]
  }
];

const fields = [
  {
    name: "showOptional",
    type: "boolean",
    title: "Show Optional Field",
    triggers: [
      {
        rule: "showField",
        when: "equal",
        value: true
      },
      {
        rule: "toggleFieldVisibility",
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

### Contact Form with Dynamic Fields

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
  },
  {
    name: "preferredTime",
    type: "select",
    title: "Preferred Contact Time",
    options: [
      { value: "morning", label: "Morning" },
      { value: "afternoon", label: "Afternoon" },
      { value: "evening", label: "Evening" }
    ],
    conditions: {
      hiddenWhen: [
        { field: "contactMethod", when: "equal", value: "email" }
      ]
    }
  }
];
```

### User Registration with Age-Based Fields

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
      hiddenWhen: [
        { field: "age", when: "greater than", value: 18 }
      ]
    }
  },
  {
    name: "guardianName",
    type: "string",
    title: "Guardian Name",
    conditions: {
      hiddenWhen: [
        { field: "age", when: "greater than", value: 18 }
      ]
    }
  },
  {
    name: "guardianPhone",
    type: "tel",
    title: "Guardian Phone",
    conditions: {
      hiddenWhen: [
        { field: "age", when: "greater than", value: 18 }
      ]
    }
  },
  {
    name: "seniorDiscount",
    type: "boolean",
    title: "Senior Discount (65+)",
    conditions: {
      hiddenWhen: [
        { field: "age", when: "less than", value: 65 }
      ]
    }
  }
];
```

### Order Form with Conditional Fields

```jsx
const rules = [
  {
    name: "hideShippingFields",
    effects: [
      {
        targetField: "shippingAddress",
        prop: "hidden",
        value: true
      },
      {
        targetField: "shippingMethod",
        prop: "hidden",
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
        rule: "hideShippingFields",
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
        rule: "hideShippingFields",
        when: "equal",
        value: "pickup"
      }
    ]
  }
];
```

---

## Advanced Patterns

### Cascading Field Visibility

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
        { field: "hasAddress", when: "equal", value: false },
        { field: "street", when: "empty" }
      ],
      hiddenMode: "any"
    }
  },
  {
    name: "zip",
    type: "string",
    title: "ZIP Code",
    conditions: {
      hiddenWhen: [
        { field: "hasAddress", when: "equal", value: false },
        { field: "street", when: "empty" },
        { field: "city", when: "empty" }
      ],
      hiddenMode: "any"
    }
  }
];
```

### Multi-Step Form with Progress

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
      hiddenWhen: [
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
      hiddenWhen: [
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
      hiddenWhen: [
        { field: "step", when: "not equal", value: 3 }
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
    hiddenWhen: [
      { field: "age", when: "less than", value: 0 },
      { field: "age", when: "greater than", value: 120 }
    ],
    hiddenMode: "any"
  }
}
```

---

## Troubleshooting

### Common Issues

1. **Fields not hiding**: Check field names match exactly
2. **Logic not behaving as expected**: Verify `hiddenMode` setting
3. **Performance issues**: Avoid too many complex conditions

### Debug Tips

```jsx
// Add console logging to debug hiding logic
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

*Dynamic field hiding is essential for creating intuitive, user-friendly forms. Use it to reduce cognitive load and guide users through complex form flows.*
