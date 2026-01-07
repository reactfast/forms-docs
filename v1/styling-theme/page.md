---
title: Theme Styling
nextjs:
  metadata:
    title: Theme Styling - NovaForms
    description: Learn how to customize NovaForms appearance using the theme system, which overrides Tailwind styling for consistent branding.
---

NovaForms provides a comprehensive theming system that allows you to customize the appearance of all form elements using a theme object. **Theme styling overrides Tailwind styling**, ensuring consistent branding across your application.

---

## Overview

The NovaForms theming system:

- **Centralized Configuration**: Define all colors and styles in one place
- **Consistent Application**: All field types use the same theme properties
- **Easy Customization**: Override specific properties without affecting others
- **CSS Custom Properties**: Uses CSS variables for dynamic theming
- **Overrides Tailwind**: Theme properties take precedence over Tailwind classes

---

## Default Theme

NovaForms comes with a built-in default theme:

```jsx
const defaultTheme = {
  title: "#000",
  label: "#111",
  inputText: "#000",
  inputBackground: "#fff",
  inputBorder: "#ebebeb",
  inputPlaceholder: "#888",
  inputFocusBorder: "#020DF9",
  description: "#555",
  error: "#ff0000",
  requiredAsterisk: "#020DF9",

  // Rating-specific
  ratingActive: "#020DF9",
  ratingInactive: "#8e8e8eff",
  ratingHover: "#5555ff",
};
```

---

## Theme Properties

### Core Properties

| Property | Description | Usage |
|----------|-------------|-------|
| `title` | Main title color | Section headers, field titles |
| `label` | Field label color | Field labels |
| `inputText` | Input text color | Text inside inputs |
| `inputBackground` | Input background color | Background of input fields |
| `inputBorder` | Input border color | Default border color |
| `inputPlaceholder` | Placeholder text color | Placeholder text |
| `inputFocusBorder` | Focus border color | Border when field is focused |
| `description` | Description text color | Help text below fields |
| `error` | Error message color | Error messages |
| `requiredAsterisk` | Required asterisk color | Required field indicators |

### Specialized Properties

| Property | Description | Usage |
|----------|-------------|-------|
| `ratingActive` | Active rating color | Star ratings when active |
| `ratingInactive` | Inactive rating color | Star ratings when inactive |
| `ratingHover` | Rating hover color | Star ratings on hover |

---

## Using Themes

### Basic Theme Usage

```jsx
import { NovaForm, createFormHandler } from "nova-forms";

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

function MyForm() {
  const [formData, setFormData] = useState({});
  const handleChange = createFormHandler({
    fields,
    setState: setFormData
  });

  return (
    <NovaForm
      fields={fields}
      onChange={handleChange}
      formData={formData}
      theme={customTheme}
    />
  );
}
```

### Partial Theme Override

You can override only specific theme properties:

```jsx
const partialTheme = {
  inputFocusBorder: "#10b981", // Green focus border
  error: "#ef4444",            // Red error messages
  requiredAsterisk: "#f59e0b" // Orange required asterisk
};

<NovaForm
  fields={fields}
  onChange={handleChange}
  formData={formData}
  theme={partialTheme}
/>
```

---

## Predefined Themes

### Light Theme

```jsx
const lightTheme = {
  title: "#1f2937",
  label: "#374151",
  inputText: "#111827",
  inputBackground: "#ffffff",
  inputBorder: "#d1d5db",
  inputPlaceholder: "#6b7280",
  inputFocusBorder: "#3b82f6",
  description: "#6b7280",
  error: "#dc2626",
  requiredAsterisk: "#3b82f6",
  ratingActive: "#3b82f6",
  ratingInactive: "#d1d5db",
  ratingHover: "#2563eb"
};
```

### Dark Theme

```jsx
const darkTheme = {
  title: "#f9fafb",
  label: "#e5e7eb",
  inputText: "#f9fafb",
  inputBackground: "#374151",
  inputBorder: "#4b5563",
  inputPlaceholder: "#9ca3af",
  inputFocusBorder: "#60a5fa",
  description: "#d1d5db",
  error: "#f87171",
  requiredAsterisk: "#60a5fa",
  ratingActive: "#60a5fa",
  ratingInactive: "#4b5563",
  ratingHover: "#93c5fd"
};
```

### High Contrast Theme

```jsx
const highContrastTheme = {
  title: "#000000",
  label: "#000000",
  inputText: "#000000",
  inputBackground: "#ffffff",
  inputBorder: "#000000",
  inputPlaceholder: "#666666",
  inputFocusBorder: "#0000ff",
  description: "#000000",
  error: "#ff0000",
  requiredAsterisk: "#ff0000",
  ratingActive: "#0000ff",
  ratingInactive: "#cccccc",
  ratingHover: "#0000ff"
};
```

### Brand Theme

```jsx
const brandTheme = {
  title: "#1e40af",
  label: "#1e40af",
  inputText: "#1e40af",
  inputBackground: "#ffffff",
  inputBorder: "#e5e7eb",
  inputPlaceholder: "#9ca3af",
  inputFocusBorder: "#1e40af",
  description: "#6b7280",
  error: "#dc2626",
  requiredAsterisk: "#1e40af",
  ratingActive: "#1e40af",
  ratingInactive: "#e5e7eb",
  ratingHover: "#3b82f6"
};
```

---

## Dynamic Theming

### Theme Switching

```jsx
function ThemedForm() {
  const [theme, setTheme] = useState(lightTheme);
  const [formData, setFormData] = useState({});
  
  const handleChange = createFormHandler({
    fields,
    setState: setFormData
  });

  const toggleTheme = () => {
    setTheme(theme === lightTheme ? darkTheme : lightTheme);
  };

  return (
    <div>
      <button onClick={toggleTheme} className="mb-4 px-4 py-2 bg-blue-500 text-white rounded">
        Toggle Theme
      </button>
      
      <NovaForm
        fields={fields}
        onChange={handleChange}
        formData={formData}
        theme={theme}
      />
    </div>
  );
}
```

### User Preference Theme

```jsx
function UserPreferenceForm() {
  const [theme, setTheme] = useState(() => {
    const savedTheme = localStorage.getItem('formTheme');
    return savedTheme ? JSON.parse(savedTheme) : lightTheme;
  });
  
  const [formData, setFormData] = useState({});
  
  const handleChange = createFormHandler({
    fields,
    setState: setFormData
  });

  const updateTheme = (newTheme) => {
    setTheme(newTheme);
    localStorage.setItem('formTheme', JSON.stringify(newTheme));
  };

  return (
    <div>
      <div className="mb-4 space-x-2">
        <button onClick={() => updateTheme(lightTheme)}>Light</button>
        <button onClick={() => updateTheme(darkTheme)}>Dark</button>
        <button onClick={() => updateTheme(highContrastTheme)}>High Contrast</button>
      </div>
      
      <NovaForm
        fields={fields}
        onChange={handleChange}
        formData={formData}
        theme={theme}
      />
    </div>
  );
}
```

---

## Custom Field Theming

### Using Theme in Custom Fields

```jsx
function CustomField({ field, value, onChange, theme }) {
  return (
    <div>
      {/* Label */}
      {field.title && (
        <label
          style={{ color: theme.label }}
          className="block text-sm font-medium mb-1"
        >
          {field.title}
          {field.required && (
            <span style={{ color: theme.requiredAsterisk }}> *</span>
          )}
        </label>
      )}
      
      {/* Input */}
      <input
        type="text"
        value={value || ""}
        onChange={onChange}
        placeholder={field.placeholder}
        className="block w-full px-3 py-2 border rounded-md"
        style={{
          color: theme.inputText,
          backgroundColor: theme.inputBackground,
          borderColor: theme.inputBorder
        }}
      />
      
      {/* Description */}
      {field.description && (
        <p
          style={{ color: theme.description }}
          className="mt-1 text-sm"
        >
          {field.description}
        </p>
      )}
    </div>
  );
}
```

### Advanced Custom Field Theming

```jsx
function AdvancedCustomField({ field, value, onChange, theme }) {
  const [isFocused, setIsFocused] = useState(false);
  
  const borderColor = isFocused ? theme.inputFocusBorder : theme.inputBorder;
  
  return (
    <div>
      {/* Label */}
      {field.title && (
        <label
          style={{ color: theme.label }}
          className="block text-sm font-medium mb-1"
        >
          {field.title}
          {field.required && (
            <span style={{ color: theme.requiredAsterisk }}> *</span>
          )}
        </label>
      )}
      
      {/* Input with focus states */}
      <div className="relative">
        <input
          type="text"
          value={value || ""}
          onChange={onChange}
          onFocus={() => setIsFocused(true)}
          onBlur={() => setIsFocused(false)}
          placeholder={field.placeholder}
          className="block w-full px-3 py-2 border rounded-md transition-colors"
          style={{
            color: theme.inputText,
            backgroundColor: theme.inputBackground,
            borderColor: borderColor,
            outline: 'none'
          }}
        />
        
        {/* Focus indicator */}
        {isFocused && (
          <div
            className="absolute inset-0 border-2 rounded-md pointer-events-none"
            style={{ borderColor: theme.inputFocusBorder }}
          />
        )}
      </div>
      
      {/* Description */}
      {field.description && (
        <p
          style={{ color: theme.description }}
          className="mt-1 text-sm"
        >
          {field.description}
        </p>
      )}
    </div>
  );
}
```

---

## CSS Custom Properties

### Using CSS Variables

You can also use CSS custom properties for theming:

```css
:root {
  --nova-title-color: #1f2937;
  --nova-label-color: #374151;
  --nova-input-text-color: #111827;
  --nova-input-background-color: #ffffff;
  --nova-input-border-color: #d1d5db;
  --nova-input-placeholder-color: #6b7280;
  --nova-input-focus-border-color: #3b82f6;
  --nova-description-color: #6b7280;
  --nova-error-color: #dc2626;
  --nova-required-asterisk-color: #3b82f6;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root {
    --nova-title-color: #f9fafb;
    --nova-label-color: #e5e7eb;
    --nova-input-text-color: #f9fafb;
    --nova-input-background-color: #374151;
    --nova-input-border-color: #4b5563;
    --nova-input-placeholder-color: #9ca3af;
    --nova-input-focus-border-color: #60a5fa;
    --nova-description-color: #d1d5db;
    --nova-error-color: #f87171;
    --nova-required-asterisk-color: #60a5fa;
  }
}
```

### Theme with CSS Variables

```jsx
const cssVariableTheme = {
  title: "var(--nova-title-color)",
  label: "var(--nova-label-color)",
  inputText: "var(--nova-input-text-color)",
  inputBackground: "var(--nova-input-background-color)",
  inputBorder: "var(--nova-input-border-color)",
  inputPlaceholder: "var(--nova-input-placeholder-color)",
  inputFocusBorder: "var(--nova-input-focus-border-color)",
  description: "var(--nova-description-color)",
  error: "var(--nova-error-color)",
  requiredAsterisk: "var(--nova-required-asterisk-color)"
};
```

---

## Theme Validation

### Theme Validation Function

```jsx
function validateTheme(theme) {
  const requiredProperties = [
    'title', 'label', 'inputText', 'inputBackground',
    'inputBorder', 'inputPlaceholder', 'inputFocusBorder',
    'description', 'error', 'requiredAsterisk'
  ];
  
  const missingProperties = requiredProperties.filter(
    prop => !(prop in theme)
  );
  
  if (missingProperties.length > 0) {
    console.warn(`Missing theme properties: ${missingProperties.join(', ')}`);
  }
  
  return theme;
}

// Usage
const validatedTheme = validateTheme(customTheme);
```

---

## Best Practices

### 1. Use Semantic Color Names
```jsx
// ✅ Good: Semantic names
const theme = {
  title: "#1f2937",
  label: "#374151",
  error: "#dc2626"
};

// ❌ Avoid: Non-semantic names
const theme = {
  color1: "#1f2937",
  color2: "#374151",
  color3: "#dc2626"
};
```

### 2. Maintain Color Contrast
```jsx
// ✅ Good: Good contrast
const theme = {
  inputText: "#111827",      // Dark text
  inputBackground: "#ffffff" // Light background
};

// ❌ Avoid: Poor contrast
const theme = {
  inputText: "#666666",      // Gray text
  inputBackground: "#f0f0f0" // Light gray background
};
```

### 3. Use Consistent Color Palette
```jsx
// ✅ Good: Consistent palette
const theme = {
  inputFocusBorder: "#3b82f6",
  requiredAsterisk: "#3b82f6",
  ratingActive: "#3b82f6"
};

// ❌ Avoid: Inconsistent colors
const theme = {
  inputFocusBorder: "#3b82f6",
  requiredAsterisk: "#ef4444",
  ratingActive: "#10b981"
};
```

### 4. Test Accessibility
```jsx
// ✅ Good: Accessible colors
const accessibleTheme = {
  inputText: "#000000",      // High contrast
  inputBackground: "#ffffff",
  error: "#dc2626"           // Clear error indication
};
```

---

## Troubleshooting

### Common Issues

1. **Theme not applying**: Check theme object structure
2. **Colors not visible**: Verify color contrast
3. **Inconsistent styling**: Ensure all theme properties are defined

### Debug Tips

```jsx
// Log theme to debug
console.log("Current theme:", theme);

// Test theme changes
const testTheme = {
  ...theme,
  inputFocusBorder: "#ff0000" // Red focus border for testing
};
```

---

*The NovaForms theming system provides a powerful way to customize form appearance while maintaining consistency. **Theme properties override Tailwind styling**, ensuring your brand colors and design system take precedence.*
