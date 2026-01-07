---
title: Styling with Tailwind
nextjs:
  metadata:
    title: Styling with Tailwind - NovaForms
    description: Learn how to customize NovaForms appearance using Tailwind CSS classes and responsive design patterns.
---

NovaForms is built with Tailwind CSS and provides a responsive, utility-first approach to styling forms. This guide covers how to customize the appearance of NovaForms using Tailwind classes and CSS.

---

## Overview

NovaForms uses Tailwind CSS for:

- **Responsive Layout**: Automatic width handling with responsive classes
- **Component Styling**: Consistent styling across all field types
- **Theme Integration**: CSS custom properties for easy customization
- **Utility Classes**: Flexible styling with Tailwind utilities

---

## Responsive Layout System

NovaForms automatically applies responsive width classes based on the `width` property:

### Width Classes

| Width Value | Tailwind Classes | Description |
|-------------|------------------|-------------|
| `25` | `w-full sm:w-1/4` | Full width on mobile, 25% on desktop |
| `50` | `w-full sm:w-1/2` | Full width on mobile, 50% on desktop |
| `75` | `w-full sm:w-3/4` | Full width on mobile, 75% on desktop |
| `100` | `w-full` | Full width on all screen sizes |

### Layout Structure

```jsx
<div className="w-full">
  <div className="-mx-2 flex flex-wrap">
    {fields.map((field, index) => (
      <div
        key={field.name || index}
        className={`${getWidthClass(field.width || 100, isMobileView)} mb-4 px-2`}
      >
        {/* Field component */}
      </div>
    ))}
  </div>
</div>
```

---

## Built-in Field Styling

### Input Fields

All input fields use consistent Tailwind classes:

```jsx
<input
  className="block w-full py-1.5 text-base sm:text-sm/6"
  style={{
    color: theme.inputText,
    backgroundColor: theme.inputBackground,
    borderColor: borderColor,
    borderWidth: '1px',
    borderStyle: 'solid',
    borderRadius: '0.375rem', // rounded-md
    paddingLeft: LeadingIcon ? '2.5rem' : '0.75rem',
    paddingRight: TrailingIcon || hasError ? '2.5rem' : '0.75rem',
    outline: 'none'
  }}
/>
```

### Labels

Labels use consistent typography:

```jsx
<label
  className="block text-sm/6 font-medium"
  style={{ color: theme.label }}
>
  {title}
  {required && (
    <span style={{ color: theme.requiredAsterisk }}> *</span>
  )}
</label>
```

### Error Messages

Error messages use consistent styling:

```jsx
<p
  className="mt-1 text-sm"
  style={{ color: theme.error }}
>
  {error}
</p>
```

---

## Customizing with Tailwind Classes

### Overriding Field Styles

You can override field styles by wrapping NovaForm in a container with custom classes:

```jsx
<div className="max-w-4xl mx-auto p-6 bg-gray-50 rounded-lg shadow-lg">
  <NovaForm
    fields={fields}
    onChange={handleChange}
    formData={formData}
  />
</div>
```

### Custom Field Container Styles

```jsx
<div className="space-y-6">
  <NovaForm
    fields={fields}
    onChange={handleChange}
    formData={formData}
  />
</div>
```

### Form Section Styling

```jsx
<div className="bg-white rounded-lg shadow-sm border border-gray-200 p-6">
  <h2 className="text-lg font-semibold text-gray-900 mb-4">
    Personal Information
  </h2>
  <NovaForm
    fields={personalFields}
    onChange={handleChange}
    formData={formData}
  />
</div>
```

---

## Custom CSS Classes

### Adding Custom Classes

You can add custom CSS classes to enhance NovaForms:

```css
/* Custom form styles */
.nova-form-container {
  @apply bg-white rounded-lg shadow-sm border border-gray-200 p-6;
}

.nova-form-section {
  @apply mb-8;
}

.nova-form-section-title {
  @apply text-lg font-semibold text-gray-900 mb-4 border-b border-gray-200 pb-2;
}

.nova-form-field-group {
  @apply bg-gray-50 rounded-md p-4 mb-4;
}

.nova-form-field-group-title {
  @apply text-sm font-medium text-gray-700 mb-2;
}

/* Custom input styles */
.nova-form-input {
  @apply transition-colors duration-200 focus:ring-2 focus:ring-blue-500 focus:border-blue-500;
}

.nova-form-input-error {
  @apply border-red-500 focus:ring-red-500 focus:border-red-500;
}

/* Custom button styles */
.nova-form-button {
  @apply bg-blue-600 text-white px-4 py-2 rounded-md hover:bg-blue-700 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition-colors duration-200;
}

.nova-form-button-secondary {
  @apply bg-gray-200 text-gray-900 px-4 py-2 rounded-md hover:bg-gray-300 focus:ring-2 focus:ring-gray-500 focus:ring-offset-2 transition-colors duration-200;
}
```

### Using Custom Classes

```jsx
<div className="nova-form-container">
  <div className="nova-form-section">
    <h3 className="nova-form-section-title">Contact Information</h3>
    <div className="nova-form-field-group">
      <h4 className="nova-form-field-group-title">Primary Contact</h4>
      <NovaForm
        fields={contactFields}
        onChange={handleChange}
        formData={formData}
      />
    </div>
  </div>
</div>
```

---

## Responsive Design Patterns

### Mobile-First Approach

NovaForms uses a mobile-first approach with responsive breakpoints:

```jsx
// Mobile-first responsive design
<div className="w-full sm:w-1/2 lg:w-1/3 xl:w-1/4">
  {/* Field content */}
</div>
```

### Responsive Form Layouts

```jsx
// Responsive form container
<div className="max-w-sm mx-auto sm:max-w-md md:max-w-lg lg:max-w-xl xl:max-w-2xl">
  <NovaForm
    fields={fields}
    onChange={handleChange}
    formData={formData}
  />
</div>
```

### Responsive Field Groups

```jsx
// Responsive field grouping
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
  <div className="nova-form-field-group">
    <NovaForm
      fields={personalFields}
      onChange={handleChange}
      formData={formData}
    />
  </div>
  <div className="nova-form-field-group">
    <NovaForm
      fields={contactFields}
      onChange={handleChange}
      formData={formData}
    />
  </div>
  <div className="nova-form-field-group">
    <NovaForm
      fields={preferenceFields}
      onChange={handleChange}
      formData={formData}
    />
  </div>
</div>
```

---

## Dark Mode Support

### Dark Mode Classes

```css
/* Dark mode styles */
@media (prefers-color-scheme: dark) {
  .nova-form-container {
    @apply bg-gray-800 border-gray-700;
  }
  
  .nova-form-section-title {
    @apply text-gray-100 border-gray-700;
  }
  
  .nova-form-field-group {
    @apply bg-gray-700;
  }
  
  .nova-form-field-group-title {
    @apply text-gray-300;
  }
}
```

### Dark Mode Theme

```jsx
const darkTheme = {
  title: "#f9fafb",
  label: "#e5e7eb",
  inputText: "#f9fafb",
  inputBackground: "#374151",
  inputBorder: "#4b5563",
  inputPlaceholder: "#9ca3af",
  inputFocusBorder: "#3b82f6",
  description: "#d1d5db",
  error: "#f87171",
  requiredAsterisk: "#3b82f6"
};

<NovaForm
  fields={fields}
  onChange={handleChange}
  formData={formData}
  theme={darkTheme}
/>
```

---

## Custom Component Styling

### Styling Custom Fields

When creating custom fields, use Tailwind classes for consistent styling:

```jsx
function CustomField({ field, value, onChange, theme }) {
  return (
    <div className="space-y-2">
      {/* Label */}
      {field.title && (
        <label className="block text-sm font-medium text-gray-700">
          {field.title}
          {field.required && (
            <span className="text-red-500 ml-1">*</span>
          )}
        </label>
      )}
      
      {/* Input */}
      <div className="relative">
        <input
          type="text"
          value={value || ""}
          onChange={onChange}
          placeholder={field.placeholder}
          className="block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
          style={{
            color: theme.inputText,
            backgroundColor: theme.inputBackground,
            borderColor: theme.inputBorder
          }}
        />
      </div>
      
      {/* Description */}
      {field.description && (
        <p className="text-sm text-gray-500">
          {field.description}
        </p>
      )}
    </div>
  );
}
```

### Styling Subforms

```jsx
function CustomSubForm({ field, value, onChange, theme }) {
  return (
    <div className="space-y-4">
      <div className="border border-gray-200 rounded-lg p-4 bg-gray-50">
        <h3 className="text-lg font-medium text-gray-900 mb-4">
          {field.title}
        </h3>
        
        <div className="space-y-4">
          {field.fields.map((subField, index) => (
            <div key={index} className="grid grid-cols-1 sm:grid-cols-2 gap-4">
              <ReturnFieldsV2
                field={subField}
                value={value?.[subField.name] || ""}
                onChange={(e) => handleSubFieldChange(subField.name, e)}
                theme={theme}
              />
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}
```

---

## Animation and Transitions

### Smooth Transitions

```css
/* Smooth transitions for form interactions */
.nova-form-field {
  @apply transition-all duration-200 ease-in-out;
}

.nova-form-input {
  @apply transition-colors duration-200 focus:ring-2 focus:ring-blue-500 focus:border-blue-500;
}

.nova-form-button {
  @apply transition-colors duration-200 hover:bg-blue-700 focus:ring-2 focus:ring-blue-500 focus:ring-offset-2;
}
```

### Loading States

```jsx
function LoadingField({ field, value, onChange, theme }) {
  const [loading, setLoading] = useState(false);
  
  return (
    <div className="relative">
      <input
        type="text"
        value={value || ""}
        onChange={onChange}
        className="block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-blue-500 focus:border-blue-500 sm:text-sm"
        disabled={loading}
      />
      
      {loading && (
        <div className="absolute inset-y-0 right-0 flex items-center pr-3">
          <div className="animate-spin rounded-full h-4 w-4 border-b-2 border-blue-600"></div>
        </div>
      )}
    </div>
  );
}
```

---

## Best Practices

### 1. Use Consistent Spacing
```jsx
// ✅ Good: Consistent spacing
<div className="space-y-4">
  <NovaForm fields={fields} onChange={handleChange} formData={formData} />
</div>

// ❌ Avoid: Inconsistent spacing
<div>
  <NovaForm fields={fields} onChange={handleChange} formData={formData} />
</div>
```

### 2. Use Semantic Color Classes
```jsx
// ✅ Good: Semantic colors
<div className="bg-white text-gray-900 border-gray-200">
  <NovaForm fields={fields} onChange={handleChange} formData={formData} />
</div>

// ❌ Avoid: Hard-coded colors
<div className="bg-white text-black border-gray-200">
  <NovaForm fields={fields} onChange={handleChange} formData={formData} />
</div>
```

### 3. Use Responsive Design
```jsx
// ✅ Good: Responsive design
<div className="max-w-sm mx-auto sm:max-w-md md:max-w-lg">
  <NovaForm fields={fields} onChange={handleChange} formData={formData} />
</div>

// ❌ Avoid: Fixed widths
<div className="w-96 mx-auto">
  <NovaForm fields={fields} onChange={handleChange} formData={formData} />
</div>
```

### 4. Use Consistent Typography
```jsx
// ✅ Good: Consistent typography
<h2 className="text-lg font-semibold text-gray-900 mb-4">
  Form Title
</h2>

// ❌ Avoid: Inconsistent typography
<h2 className="text-xl font-bold text-black mb-2">
  Form Title
</h2>
```

---

## Troubleshooting

### Common Issues

1. **Styles not applying**: Check Tailwind CSS is properly configured
2. **Responsive issues**: Verify responsive classes are working
3. **Theme conflicts**: Ensure theme properties are properly set

### Debug Tips

```jsx
// Add debug classes to see layout
<div className="border-2 border-red-500 p-2">
  <NovaForm
    fields={fields}
    onChange={handleChange}
    formData={formData}
  />
</div>
```

---

*Tailwind CSS provides a powerful, utility-first approach to styling NovaForms. Use it to create beautiful, responsive forms that match your design system.*
