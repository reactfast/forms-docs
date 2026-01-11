---
title: Getting started
---

Build dynamic React forms powered by JSON schemas, modifiers, and subforms. Create complex, adaptive form systems without boilerplate — designed for scale, simplicity, and composability. {% .lead %}

{% quick-links %}

{% quick-link title="Installation" icon="installation" href="/docs/installation" description="Step-by-step guides to setting up ReactFast Forms in your project." /%}

{% quick-link title="Quick Start" icon="presets" href="/docs/quickstart" description="Get up and running with your first ReactFast Form in minutes." /%}

{% quick-link title="Field Types" icon="plugins" href="/docs/fields-schemas" description="Explore 20+ built‑in field types from text to file uploads." /%}

{% quick-link title="Custom Fields" icon="theming" href="/docs/custom-fields" description="Extend ReactFast Forms with your own field components." /%}

{% /quick-links %}

ReactFast Forms makes building complex forms simple. Define your fields as JSON, add conditional logic with rules and triggers, and let ReactFast Forms handle the rest. Perfect for surveys, applications, data collection, and any form‑heavy application.

---

## Quick start

Get ReactFast Forms running in your project in under 5 minutes.

### Installing dependencies

Install ReactFast Forms and its peer dependencies:

```shell
npm install @reactfast/forms
```

ReactFast Forms requires **React 18+** and **React DOM 16+** as peer dependencies.

{% callout type="warning" title="React Version Requirement" %}
Make sure you're using React 16 or higher. ReactFast Forms uses modern React features that aren't available in older versions.
{% /callout %}

### Your first form

Create a simple form with just a few lines of code:

```jsx
import { useState } from "react";
import { Form, createFormHandler } from "@reactfast/forms";

const fields = [
  { name: "firstName", title: "First Name", type: "string", width: 50 },
  { name: "lastName", title: "Last Name", type: "string", width: 50 },
  { name: "email", title: "Email", type: "email", width: 100 },
  { name: "subscribe", title: "Subscribe?", type: "boolean", width: 100 },
];

export default function App() {
  const [formData, setFormData] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  return <Form fields={fields} onChange={handleChange} formData={formData} />;
}
```

That's it! You now have a fully functional form with validation, responsive layout, and state management.

{% callout title="Pro Tip" %}
The `createFormHandler` function automatically handles form state updates and applies any rules or modifiers you define. It's the heart of ReactFast Forms' power.
{% /callout %}

---

## Key features

ReactFast Forms comes packed with features that make form building effortless:

### ⚡ Controlled forms

Simple `value`/`onChange` pattern like React inputs, but with automatic state management.

### 🧩 Composable architecture

Each field is a reusable React component that you can customize or extend.

### 🔄 Advanced conditional logic

Dynamic show/hide, disable, and field dependencies with a powerful rules system.

### 📱 Responsive layout

Automatic width handling with Tailwind classes — no CSS required.

### 🧱 Subforms & arrays

Nested or repeated field groups are first-class citizens.

### 🎨 Theming-ready

Customize UI with Tailwind or your own design system.

### 🔌 Extensible

Register your own field components via `registerField()`.

---

## Built-in field types

ReactFast Forms includes 20+ field types ready to use:

| Type        | Description                 | Example                                     |
| ----------- | --------------------------- | ------------------------------------------- |
| `string`    | Text input                  | `{ type: "string", title: "Name" }`         |
| `email`     | Email input with validation | `{ type: "email", title: "Email" }`         |
| `boolean`   | Checkbox                    | `{ type: "boolean", title: "Subscribe" }`   |
| `select`    | Single select dropdown      | `{ type: "select", options: [...] }`        |
| `array`     | Dynamic subform/array       | `{ type: "array", fields: [...] }`          |
| `file`      | File upload                 | `{ type: "file", title: "Upload" }`         |
| `signature` | Signature pad               | `{ type: "signature", title: "Signature" }` |

And many more! See the [Field Types documentation](/docs/fields-schemas) for the complete list.

---

## Getting help

Need help getting started or have questions about ReactFast Forms?

### Submit an issue

Found a bug or have a feature request? We'd love to hear from you:

- 🐛 [Report a bug](https://github.com/reactfast/forms/issues)
- 💡 [Request a feature](https://github.com/reactfast/forms/issues)
- 📖 [View documentation](https://github.com/reactfast/forms)

### Join the community

Connect with other ReactFast Forms users and contributors:

- ⭐ [Star the repository](https://github.com/reactfast/forms)
- 💬 [Join discussions](https://github.com/reactfast/forms/discussions)
- 🤝 [Contribute to the project](https://github.com/reactfast/forms/blob/main/CONTRIBUTING.md)

ReactFast Forms is maintained by [Jonathon McClendon](https://github.com/jonathonmcclendon) and the open source community. We welcome contributions and feedback!
