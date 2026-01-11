---

title: Quick Start
nextjs:
metadata:
title: Quick Start - ReactFast Forms
description: Get up and running with ReactFast Forms in minutes. Learn the basics of creating dynamic React forms.
------------------------------------------------------------------------------------------------------------------

Get up and running with ReactFast Forms in under 5 minutes. This guide walks you through creating your first dynamic form.

---

## Your First Form

We'll create a simple contact form:

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
import { Form, createFormHandler } from "@reactfast/forms";

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
  const [formData, setFormData] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  const handleSubmit() {
    //  Your submit logic
  };

  if (!ready) return null;

  return (
    <>
      <Form fields={fields} onChange={handleChange} formData={formData} />
      <div
        id="submitButton"
        onClick={handleSubmit}
        className="mt-4 px-4 py-2 bg-blue-500 text-white rounded"
      >
        Submit
      </div>
    </>
  );
}
```

---

## Field Definition

Each field is an object:

```jsx
{
  name: "firstName",   // unique identifier
  title: "First Name", // label
  type: "string",      // string, number, boolean, email, tel, select, etc.
  width: 50            // percentage width
}
```

---

## Form Handler

`createFormHandler` manages state and rules:

```jsx
const handleChange = createFormHandler({
  fields,
  setState: setFormData,
});
```

---

## Conditional Fields

Show a "Company" field only when "Business" is selected:

```jsx
const fields = [
  { name: "firstName", title: "First Name", type: "string", width: 50 },
  { name: "lastName", title: "Last Name", type: "string", width: 50 },
  { name: "email", title: "Email", type: "email", width: 100 },
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
  { name: "message", title: "Message", type: "text", width: 100 },
];
```

---

## Validation

Add required fields and patterns:

```jsx
const fields = [
  {
    name: "firstName",
    title: "First Name",
    type: "string",
    width: 50,
    required: true,
    pattern: /^[A-Za-z\s]+$/,
    error: "Only letters and spaces",
  },
  {
    name: "email",
    title: "Email",
    type: "email",
    width: 100,
    required: true,
    error: "Enter a valid email",
  },
  {
    name: "phone",
    title: "Phone",
    type: "tel",
    width: 100,
    pattern: /^\(\d{3}\) \d{3}-\d{4}$/,
    placeholder: "(555) 123-4567",
    error: "Format: (555) 123-4567",
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
      { name: "city", title: "City", type: 50 },
      { name: "state", title: "State", type: 50 },
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

- [Field Types](/docs/fields-schemas) – 20+ built-in types
- [Rules System](/docs/rules) – Conditional logic
- [Custom Fields](/docs/custom-fields) – Build your own
- [Styling](/docs/styling-tailwind) – Tailwind CSS customization

---

## Help

- [Installation guide](/docs/installation)
- [Examples](/docs/examples)
- [GitHub Discussions](https://github.com/reactfast/forms/discussions)
