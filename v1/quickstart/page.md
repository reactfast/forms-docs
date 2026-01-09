---

title: Quick Start
nextjs:
metadata:
title: Quick Start - ReactFast Forms
description: Get up and running with ReactFast Forms in minutes. Learn the basics of creating dynamic React forms.
------------------------------------------------------------------------------------------------------------------

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
import { useState } from "react";
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

---

## Understanding the Code

### Field Definition

Each field is defined as an object with these key properties:

```jsx
{
  name: "firstName",        // Unique identifier
  title: "First Name",      // Display label
  type: "string",           // Field type
  width: 50                  // Width percentage (25, 50, 75, 100)
}
```

### Form Handler

The `createFormHandler` function manages form state:

```jsx
const handleChange = createFormHandler({
  fields,
  setState: setFormData,
});
```

### Form Component

The `Form` component renders your fields:

```jsx
<Form fields={fields} onChange={handleChange} formData={formData} />
```

---

## Conditional Logic

Show a "Company" field only if "Business" is selected:

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

## Validation Rules

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

## Subforms Example

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

## Calculations Example

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

## Getting Help

- Check the [Installation guide](/docs/installation) for setup problems
- Browse the [Examples](/docs/examples) for common patterns
- Join the [GitHub Discussions](https://github.com/reactfast/forms/discussions) for community help
