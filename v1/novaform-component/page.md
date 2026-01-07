---
title: NovaForm Component
nextjs:
  metadata:
    title: NovaForm Component - NovaForms
    description: Complete reference for the NovaForm component, including props, usage examples, and advanced features.
---

The `NovaForm` component is the main component in NovaForms that renders dynamic forms based on field schemas. It handles form layout, field rendering, and integrates with the form state management system.

---

## Basic Usage

```jsx
import { NovaForm, createFormHandler } from "nova-forms";
import { useState } from "react";

const fields = [
  { name: "firstName", title: "First Name", type: "string", width: 50 },
  { name: "lastName", title: "Last Name", type: "string", width: 50 },
  { name: "email", title: "Email", type: "email", width: 100 },
];

export default function MyForm() {
  const [formData, setFormData] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  return (
    <NovaForm
      fields={fields}
      onChange={handleChange}
      formData={formData}
    />
  );
}
```

---

## Props Reference

### Required Props

| Prop | Type | Description |
|------|------|-------------|
| `fields` | `Array` | Array of field definition objects |
| `onChange` | `Function` | Change handler function (usually from `createFormHandler`) |
| `formData` | `Object` | Current form data object |

### Optional Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `theme` | `Object` | `defaultTheme` | Theme object for styling |
| `className` | `string` | `""` | Additional CSS classes |
| `style` | `Object` | `{}` | Inline styles |
| `id` | `string` | `undefined` | Form ID attribute |
| `name` | `string` | `undefined` | Form name attribute |
| `onSubmit` | `Function` | `undefined` | Form submission handler |
| `onReset` | `Function` | `undefined` | Form reset handler |
| `disabled` | `boolean` | `false` | Disable entire form |
| `readOnly` | `boolean` | `false` | Make entire form read-only |
| `showLabels` | `boolean` | `true` | Show/hide field labels |
| `showDescriptions` | `boolean` | `true` | Show/hide field descriptions |
| `showErrors` | `boolean` | `true` | Show/hide validation errors |
| `spacing` | `string` | `"normal"` | Field spacing (`"compact"`, `"normal"`, `"loose"`) |
| `layout` | `string` | `"responsive"` | Layout mode (`"responsive"`, `"fixed"`) |

---

## Field Definition Object

Each field in the `fields` array is an object with the following structure:

```jsx
{
  name: "fieldName",           // Required: Unique identifier
  type: "fieldType",           // Required: Field type
  title: "Display Label",      // Optional: Human-readable label
  width: 100,                  // Optional: Width percentage (25, 50, 75, 100)
  default: "defaultValue",      // Optional: Default value
  required: false,             // Optional: Mark field as required
  readOnly: false,             // Optional: Make field read-only
  disabled: false,             // Optional: Disable field
  placeholder: "Placeholder",  // Optional: Placeholder text
  description: "Description", // Optional: Help text below field
  helper: "Helper text",       // Optional: Additional help text
  error: "Error message",      // Optional: Error message
  pattern: "regex",            // Optional: Validation pattern
  patternMessage: "Invalid",   // Optional: Custom validation message
  min: 0,                      // Optional: Minimum value (numbers)
  max: 100,                    // Optional: Maximum value (numbers)
  step: 1,                     // Optional: Step value (numbers)
  options: [...],               // Optional: Options for select/radio fields
  // ... type-specific properties
}
```

---

## Advanced Usage Examples

### Basic Form with Validation

```jsx
const fields = [
  {
    name: "email",
    type: "email",
    title: "Email Address",
    required: true,
    pattern: "^[\\w\\.-]+@[\\w\\.-]+\\.[a-zA-Z]{2,}$",
    patternMessage: "Please enter a valid email address"
  },
  {
    name: "age",
    type: "number",
    title: "Age",
    min: 18,
    max: 120,
    required: true
  }
];

function ValidatedForm() {
  const [formData, setFormData] = useState({});
  const [errors, setErrors] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    // Validate and submit form
    console.log("Form data:", formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <NovaForm
        fields={fields}
        onChange={handleChange}
        formData={formData}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Form with Custom Theme

```jsx
const customTheme = {
  title: "#1f2937",
  label: "#374151",
  inputText: "#111827",
  inputBackground: "#ffffff",
  inputBorder: "#d1d5db",
  inputPlaceholder: "#6b7280",
  inputFocusBorder: "#3b82f6",
  description: "#6b7280",
  error: "#dc2626",
  requiredAsterisk: "#3b82f6"
};

function ThemedForm() {
  const [formData, setFormData] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  return (
    <NovaForm
      fields={fields}
      onChange={handleChange}
      formData={formData}
      theme={customTheme}
      className="max-w-2xl mx-auto p-6 bg-white rounded-lg shadow-lg"
    />
  );
}
```

### Form with Conditional Logic

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
    name: "adminCode",
    type: "string",
    title: "Admin Code",
    conditions: {
      hiddenWhen: [
        { field: "userType", when: "not equal", value: "admin" }
      ]
    }
  }
];

const rules = [
  {
    name: "disableAdminFields",
    effects: [
      {
        targetField: "adminCode",
        prop: "readOnly",
        value: true
      }
    ]
  }
];

function ConditionalForm() {
  const [formData, setFormData] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
    rules
  });

  return (
    <NovaForm
      fields={fields}
      onChange={handleChange}
      formData={formData}
    />
  );
}
```

### Form with Subforms

```jsx
const fields = [
  {
    name: "personalInfo",
    type: "subForm",
    title: "Personal Information",
    fields: [
      { name: "firstName", type: "string", title: "First Name", width: 50 },
      { name: "lastName", type: "string", title: "Last Name", width: 50 },
      { name: "email", type: "email", title: "Email", width: 100 }
    ]
  },
  {
    name: "address",
    type: "subForm",
    title: "Address",
    fields: [
      { name: "street", type: "string", title: "Street", width: 100 },
      { name: "city", type: "string", title: "City", width: 50 },
      { name: "state", type: "string", title: "State", width: 25 },
      { name: "zip", type: "string", title: "ZIP", width: 25 }
    ]
  }
];

function SubFormExample() {
  const [formData, setFormData] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  return (
    <NovaForm
      fields={fields}
      onChange={handleChange}
      formData={formData}
      spacing="loose"
    />
  );
}
```

### Form with Custom Styling

```jsx
function StyledForm() {
  const [formData, setFormData] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  return (
    <div className="max-w-4xl mx-auto">
      <div className="bg-gradient-to-r from-blue-50 to-indigo-50 rounded-lg p-8">
        <h2 className="text-2xl font-bold text-gray-900 mb-6">
          Contact Information
        </h2>
        
        <NovaForm
          fields={fields}
          onChange={handleChange}
          formData={formData}
          className="space-y-6"
          style={{
            backgroundColor: "rgba(255, 255, 255, 0.8)",
            borderRadius: "0.5rem",
            padding: "1.5rem"
          }}
        />
        
        <div className="mt-8 flex justify-end space-x-4">
          <button
            type="button"
            className="px-6 py-2 border border-gray-300 rounded-md text-gray-700 hover:bg-gray-50"
          >
            Cancel
          </button>
          <button
            type="button"
            className="px-6 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700"
          >
            Save
          </button>
        </div>
      </div>
    </div>
  );
}
```

---

## Layout and Spacing

### Spacing Options

```jsx
// Compact spacing
<NovaForm
  fields={fields}
  onChange={handleChange}
  formData={formData}
  spacing="compact"
/>

// Normal spacing (default)
<NovaForm
  fields={fields}
  onChange={handleChange}
  formData={formData}
  spacing="normal"
/>

// Loose spacing
<NovaForm
  fields={fields}
  onChange={handleChange}
  formData={formData}
  spacing="loose"
/>
```

### Layout Modes

```jsx
// Responsive layout (default)
<NovaForm
  fields={fields}
  onChange={handleChange}
  formData={formData}
  layout="responsive"
/>

// Fixed layout
<NovaForm
  fields={fields}
  onChange={handleChange}
  formData={formData}
  layout="fixed"
/>
```

---

## Form State Management

### Using createFormHandler

The `createFormHandler` function is the recommended way to handle form state:

```jsx
const handleChange = createFormHandler({
  fields,        // Field definitions
  setState,      // React setState function
  rules          // Optional: Array of rules
});
```

### Manual State Management

You can also handle state manually:

```jsx
function ManualForm() {
  const [formData, setFormData] = useState({});

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };

  return (
    <NovaForm
      fields={fields}
      onChange={handleChange}
      formData={formData}
    />
  );
}
```

---

## Event Handlers

### Form Submission

```jsx
function FormWithSubmission() {
  const [formData, setFormData] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log("Submitting:", formData);
    // Handle form submission
  };

  return (
    <form onSubmit={handleSubmit}>
      <NovaForm
        fields={fields}
        onChange={handleChange}
        formData={formData}
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Form Reset

```jsx
function FormWithReset() {
  const [formData, setFormData] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  const handleReset = () => {
    setFormData({});
  };

  return (
    <div>
      <NovaForm
        fields={fields}
        onChange={handleChange}
        formData={formData}
        onReset={handleReset}
      />
      <button onClick={handleReset}>Reset Form</button>
    </div>
  );
}
```

---

## Accessibility Features

NovaForm automatically provides accessibility features:

- **Proper labeling**: All fields are properly labeled
- **Required indicators**: Required fields are marked with asterisks
- **Error announcements**: Screen readers announce validation errors
- **Keyboard navigation**: Full keyboard support
- **Focus management**: Proper focus handling

### Custom Accessibility

```jsx
<NovaForm
  fields={fields}
  onChange={handleChange}
  formData={formData}
  id="contact-form"
  name="contactForm"
  aria-label="Contact Information Form"
/>
```

---

## Performance Considerations

### Optimizing Large Forms

```jsx
// Use React.memo for expensive forms
const MemoizedForm = React.memo(({ fields, onChange, formData }) => (
  <NovaForm
    fields={fields}
    onChange={onChange}
    formData={formData}
  />
));

// Lazy load form sections
const LazyForm = lazy(() => import('./LazyForm'));
```

### Field Optimization

```jsx
// Use width properties for responsive layout
const optimizedFields = [
  { name: "firstName", type: "string", width: 50 },
  { name: "lastName", type: "string", width: 50 },
  { name: "email", type: "email", width: 100 }
];
```

---

## Troubleshooting

### Common Issues

1. **Form not updating**: Ensure `onChange` handler is properly connected
2. **Fields not rendering**: Check field definitions and types
3. **Styling issues**: Verify theme object structure
4. **Validation not working**: Check pattern and validation properties

### Debug Tips

```jsx
// Add debugging to form
<NovaForm
  fields={fields}
  onChange={(e) => {
    console.log("Form change:", e);
    handleChange(e);
  }}
  formData={formData}
/>

// Log form data
useEffect(() => {
  console.log("Form data updated:", formData);
}, [formData]);
```

---

*The NovaForm component is the heart of NovaForms, providing a powerful and flexible way to create dynamic forms with minimal code. Use it with `createFormHandler` for the best experience.*
