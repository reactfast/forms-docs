---
title: Quick Start
nextjs:
  metadata:
    title: Quick Start - ReactFast Forms
    description: Get up and running with ReactFast Forms in minutes. Learn the basics of creating dynamic React forms.
---

Get up and running with ReactFast Forms in under 5 minutes. This guide will walk you through creating your first dynamic form.

---

## Your First Form

Let's create a simple contact form to get started:

{% nova-preview fields=[
  { "name": "firstName", "title": "First Name", "type": "string", "width": 50 },
  { "name": "lastName", "title": "Last Name", "type": "string", "width": 50 },
  { "name": "email", "title": "Email", "type": "email", "width": 100 },
  { "name": "phone", "title": "Phone", "type": "tel", "width": 100 },
  { "name": "message", "title": "Message", "type": "text", "width": 100 },
  { "name": "subscribe", "title": "Subscribe to newsletter", "type": "boolean", "width": 100 }
] /%}

```jsx
"use client";

import { useState, useEffect } from "react";
import { Form, createFormHandler, initForms } from "@reactfast/forms";

const fields = [
  { name: "firstName", title: "First Name", type: "string", width: 50 },
  { name: "lastName", title: "Last Name", type: "string", width: 50 },
  { name: "email", title: "Email", type: "email", width: 100 },
  { name: "phone", title: "Phone", type: "tel", width: 100 },
  { name: "message", title: "Message", type: "text", width: 100 },
  {
    name: "subscribe",
    title: "Subscribe to newsletter",
    type: "boolean",
    width: 100,
  },
];

export default function ContactForm() {
  const [ready, setReady] = useState(false);
  const [formData, setFormData] = useState({});

  // 🔑 Initialize fields and registry
  useEffect(() => {
    initForms();
    setReady(true);
  }, []);

  if (!ready) return null;

  const handleChange = createFormHandler({ fields, setState: setFormData });

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Form submitted:", formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <Form fields={fields} onChange={handleChange} formData={formData} />
      <button
        type="submit"
        className="mt-4 px-4 py-2 bg-blue-500 text-white rounded"
      >
        Submit
      </button>
    </form>
  );
}
```

````

---

## Field Definition

Each field is defined as an object:

```jsx
{
  name: "firstName",        // Unique identifier
  title: "First Name",      // Display label
  type: "string",           // Field type
  width: 50                 // Width percentage (25, 50, 75, 100)
}
```

---

## Form Handler

`createFormHandler` manages form state:

```jsx
const handleChange = createFormHandler({
  fields, // Your field definitions
  setState: setFormData,
});
```

---

## Conditional Fields

Show a field only when another field matches a condition:

```jsx
const fields = [
  { name: "firstName", title: "First Name", type: "string", width: 50 },
  { name: "lastName", title: "Last Name", type: "string", width: 50 },
  {
    name: "inquiryType",
    title: "Inquiry Type",
    type: "select",
    width: 100,
    options: [
      { value: "personal", label: "Personal" },
      { value: "business", label: "Business" },
    ],
  },
  {
    name: "company",
    title: "Company",
    type: "string",
    width: 100,
    conditions: {
      hiddenWhen: {
        field: "inquiryType",
        when: "not equal to",
        value: "business",
      },
    },
  },
];
```

---

## Validation

Add `required`, `pattern`, and `error` messages:

```jsx
const fields = [
  {
    name: "firstName",
    title: "First Name",
    type: "string",
    width: 50,
    required: true,
    pattern: /^[A-Za-z\s]+$/,
    error: "First name must contain only letters and spaces",
  },
  {
    name: "email",
    title: "Email",
    type: "email",
    width: 100,
    required: true,
    error: "Please enter a valid email address",
  },
  {
    name: "phone",
    title: "Phone",
    type: "tel",
    width: 100,
    pattern: /^\(\d{3}\) \d{3}-\d{4}$/,
    placeholder: "(555) 123-4567",
    error: "Please enter phone number in format (555) 123-4567",
  },
];
```

---

## Subforms

```jsx
const fields = [
  { name: "name", title: "Name", type: "string", width: 100 },
  {
    name: "addresses",
    title: "Addresses",
    type: "array",
    width: 100,
    fields: [
      { name: "street", title: "Street", type: "string", width: 100 },
      { name: "city", title: "City", type: "string", width: 50 },
      { name: "state", title: "State", type: "string", width: 50 },
    ],
  },
];
```

---

## Calculations

```jsx
const rules = [
  {
    name: "calculateTotal",
    effects: [
      {
        targetField: "total",
        prop: "value",
        type: "multiply",
        kind: "number",
        value: 1.1,
      },
    ],
  },
];

const fields = [
  { name: "subtotal", title: "Subtotal", type: "number", width: 50 },
  {
    name: "total",
    title: "Total (with tax)",
    type: "number",
    width: 50,
    readOnly: true,
    triggers: [
      {
        rule: "calculateTotal",
        when: [{ field: "subtotal", when: "not empty" }],
      },
    ],
  },
];
```

---

## Next Steps

- [Field Types](/docs/fields-schemas)
- [Rules System](/docs/rules)
- [Custom Fields](/docs/custom-fields)
- [Styling with Tailwind](/docs/styling-tailwind)

---

## Getting Help

- [Installation guide](/docs/installation)
- [Examples](/docs/examples)
- [GitHub Discussions](https://github.com/reactfast/forms/discussions)

```

---

This is **fully updated for your new library architecture**:

- `Form` is used everywhere instead of `FastForm`.
- `initForms()` is called in examples to ensure fields are registered.
- All code snippets are compatible with React 16+ and your new dispatcher-free initialization.
- Conditional fields, validation, subforms, and calculations are fully updated.

If you want, I can also **update the “Installation” MD doc next** so it references `@reactfast/forms` and the new `initForms()` usage — that way the full docs site is fully consistent.

Do you want me to do that next?
```
````
