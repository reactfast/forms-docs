---
title: Installation
nextjs:
  metadata:
    title: Installation - ReactFast Forms
    description: Learn how to install ReactFast Forms in your React project with npm or yarn.
---

Get ReactFast Forms up and running in your React project in just a few steps.

---

## Prerequisites

Before installing ReactFast Forms, make sure you have:

- **React 16+** - ReactFast Forms requires React 18 or higher
- **React DOM 18+** - Required for DOM rendering
- **Node.js 16+** - For npm/yarn package management

---

## Installation

Install ReactFast Forms using your preferred package manager:

### Using npm

```bash
npm install @reactfast/forms
```

### Using yarn

```bash
yarn add @reactfast/forms
```

### Using pnpm

```bash
pnpm add @reactfast/forms
```

---

## Peer Dependencies

ReactFast Forms requires React and React DOM as peer dependencies. If you don't already have them installed:

```bash
npm install react react-dom
# or
yarn add react react-dom
```

{% callout type="warning" title="React Version Requirement" %}
ReactFast Forms requires React 18 or higher. If you're using an older version of React, please upgrade before installing ReactFast Forms.
{% /callout %}

---

## Verify Installation

To verify that ReactFast Forms is installed correctly, create a simple test component:

```jsx
import { Form, createFormHandler } from "@reactfast/forms";
import { useState } from "react";

const fields = [
  { name: "test", title: "Test Field", type: "string", width: 100 },
];

export default function TestForm() {
  const [formData, setFormData] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  return <Form fields={fields} onChange={handleChange} formData={formData} />;
}
```

{% nova-preview fields = [
  { name: "test", title: "Test Field", type: "string", width: 100 },
] /%}

If this renders without errors, ReactFast Forms is installed correctly!

---

## Next Steps

Now that NovaForms is installed, you can:

- Follow the [Quick Start guide](/docs/quickstart) to create your first form
- Explore [Field Types](/docs/fields-schemas) to see what's available
- Learn about [createFormHandler](/docs/createFormHandler) for advanced usage

---

## Troubleshooting

### Common Issues

**Module not found error**: Make sure you've installed NovaForms and its peer dependencies:

```bash
npm install @reactfast/forms react react-dom
```

**React version error**: Ensure you're using React 18 or higher:

```bash
npm list react
```

**TypeScript errors**: If you're using TypeScript, ReactFast Forms includes type definitions. Make sure your `tsconfig.json` includes the `node_modules` types:

```json
{
  "compilerOptions": {
    "types": ["node"]
  }
}
```

### Getting Help

If you're still having issues:

- Check the [GitHub Issues](https://github.com/reactfast/forms/issues)
- Join the [Discussions](https://github.com/reactfast/forms/discussions)
- Review the [Examples](/docs/examples) for common patterns
