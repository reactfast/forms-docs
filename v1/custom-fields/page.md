---
title: Custom Fields
nextjs:
  metadata:
    title: Custom Fields - NovaForms
    description: Learn how to create and register custom field types to extend NovaForms with specialized inputs and third-party components.
---

NovaForms provides a powerful field registry system that allows you to create and register custom field types. This enables you to extend NovaForms with specialized inputs, third-party components, or domain-specific form elements.

---

## Overview

The field registry system consists of:

1. **Field Registry**: Central storage for field type definitions
2. **Field Components**: React components that implement field behavior
3. **Registration API**: Functions to register and retrieve field types

---

## Field Registry API

### `registerField(type, component)`
Registers a new field type with the registry.

```jsx
import { registerField } from "nova-forms";

registerField("customType", CustomFieldComponent);
```

### `getField(type)`
Retrieves a field component by type.

```jsx
import { getField } from "nova-forms";

const CustomField = getField("customType");
```

### `getAllFields()`
Returns all registered field types.

```jsx
import { getAllFields } from "nova-forms";

const allFields = getAllFields();
console.log(Object.keys(allFields)); // ["string", "email", "customType", ...]
```

---

## Creating Custom Field Components

### Basic Field Component Structure

Every custom field component receives these props:

```jsx
function CustomField({ field, value, onChange, theme }) {
  // field: Field definition object
  // value: Current field value
  // onChange: Change handler function
  // theme: Theme object with styling properties
  
  return (
    <div>
      {/* Your custom field implementation */}
    </div>
  );
}
```

### Field Props

| Prop | Type | Description |
|------|------|-------------|
| `field` | `object` | Field definition object |
| `value` | `any` | Current field value |
| `onChange` | `function` | Change handler function |
| `theme` | `object` | Theme object with styling properties |

### Field Definition Object

The `field` prop contains the field definition:

```jsx
{
  name: "fieldName",        // Field name
  type: "customType",       // Field type
  title: "Field Title",      // Display label
  width: 100,               // Width percentage
  default: "defaultValue",   // Default value
  required: true,           // Required flag
  readOnly: false,          // Read-only flag
  placeholder: "Placeholder", // Placeholder text
  description: "Description", // Help text
  helper: "Helper text",    // Additional help
  error: "Error message",   // Error message
  // ... custom properties
}
```

---

## Example: Simple Custom Field

Let's create a simple color picker field:

```jsx
import React, { useState } from "react";
import { registerField } from "nova-forms";

function ColorPickerField({ field, value, onChange, theme }) {
  const [isOpen, setIsOpen] = useState(false);
  
  const colors = [
    "#ff0000", "#00ff00", "#0000ff", "#ffff00",
    "#ff00ff", "#00ffff", "#000000", "#ffffff"
  ];
  
  const handleColorSelect = (color) => {
    onChange({
      target: {
        name: field.name,
        value: color
      }
    });
    setIsOpen(false);
  };
  
  return (
    <div>
      {/* Label */}
      {field.title && (
        <label
          htmlFor={field.name}
          style={{ color: theme.label }}
          className="block text-sm font-medium mb-1"
        >
          {field.title}
          {field.required && (
            <span style={{ color: theme.requiredAsterisk }}> *</span>
          )}
        </label>
      )}
      
      {/* Color picker */}
      <div className="relative">
        <button
          type="button"
          onClick={() => setIsOpen(!isOpen)}
          className="w-full h-10 border rounded-md flex items-center justify-between px-3"
          style={{
            backgroundColor: value || "#ffffff",
            borderColor: theme.inputBorder
          }}
        >
          <span style={{ color: theme.inputText }}>
            {value || "Select color"}
          </span>
          <span>▼</span>
        </button>
        
        {isOpen && (
          <div className="absolute top-full left-0 right-0 mt-1 p-2 bg-white border rounded-md shadow-lg z-10">
            <div className="grid grid-cols-4 gap-2">
              {colors.map((color) => (
                <button
                  key={color}
                  type="button"
                  onClick={() => handleColorSelect(color)}
                  className="w-8 h-8 rounded border"
                  style={{ backgroundColor: color }}
                />
              ))}
            </div>
          </div>
        )}
      </div>
      
      {/* Description */}
      {field.description && (
        <p style={{ color: theme.description }} className="mt-1 text-sm">
          {field.description}
        </p>
      )}
    </div>
  );
}

// Register the field
registerField("colorPicker", ColorPickerField);
```

---

## Example: Advanced Custom Field

Let's create a more complex field - a file upload with drag and drop:

```jsx
import React, { useState, useRef } from "react";
import { registerField } from "nova-forms";

function FileUploadField({ field, value, onChange, theme }) {
  const [isDragging, setIsDragging] = useState(false);
  const [uploading, setUploading] = useState(false);
  const fileInputRef = useRef(null);
  
  const handleFileSelect = async (file) => {
    if (!file) return;
    
    setUploading(true);
    
    try {
      // Simulate file upload
      const formData = new FormData();
      formData.append("file", file);
      
      // Replace with actual upload logic
      const response = await fetch("/api/upload", {
        method: "POST",
        body: formData
      });
      
      const result = await response.json();
      
      onChange({
        target: {
          name: field.name,
          value: result.url
        }
      });
    } catch (error) {
      console.error("Upload failed:", error);
    } finally {
      setUploading(false);
    }
  };
  
  const handleDrop = (e) => {
    e.preventDefault();
    setIsDragging(false);
    
    const files = e.dataTransfer.files;
    if (files.length > 0) {
      handleFileSelect(files[0]);
    }
  };
  
  const handleDragOver = (e) => {
    e.preventDefault();
    setIsDragging(true);
  };
  
  const handleDragLeave = (e) => {
    e.preventDefault();
    setIsDragging(false);
  };
  
  return (
    <div>
      {/* Label */}
      {field.title && (
        <label
          htmlFor={field.name}
          style={{ color: theme.label }}
          className="block text-sm font-medium mb-1"
        >
          {field.title}
          {field.required && (
            <span style={{ color: theme.requiredAsterisk }}> *</span>
          )}
        </label>
      )}
      
      {/* File upload area */}
      <div
        onDrop={handleDrop}
        onDragOver={handleDragOver}
        onDragLeave={handleDragLeave}
        className={`border-2 border-dashed rounded-lg p-6 text-center transition-colors ${
          isDragging ? "border-blue-500 bg-blue-50" : "border-gray-300"
        }`}
        style={{
          borderColor: isDragging ? theme.inputFocusBorder : theme.inputBorder
        }}
      >
        {uploading ? (
          <div className="text-gray-500">Uploading...</div>
        ) : value ? (
          <div>
            <div className="text-green-600 mb-2">✓ File uploaded</div>
            <div className="text-sm text-gray-600">{value}</div>
            <button
              type="button"
              onClick={() => onChange({ target: { name: field.name, value: "" } })}
              className="mt-2 text-red-600 text-sm"
            >
              Remove file
            </button>
          </div>
        ) : (
          <div>
            <div className="text-gray-500 mb-2">
              Drag and drop a file here, or{" "}
              <button
                type="button"
                onClick={() => fileInputRef.current?.click()}
                className="text-blue-600 underline"
              >
                browse
              </button>
            </div>
            <div className="text-sm text-gray-400">
              {field.accept && `Accepted formats: ${field.accept}`}
            </div>
          </div>
        )}
      </div>
      
      {/* Hidden file input */}
      <input
        ref={fileInputRef}
        type="file"
        accept={field.accept}
        onChange={(e) => handleFileSelect(e.target.files[0])}
        className="hidden"
      />
      
      {/* Description */}
      {field.description && (
        <p style={{ color: theme.description }} className="mt-1 text-sm">
          {field.description}
        </p>
      )}
    </div>
  );
}

// Register the field
registerField("fileUpload", FileUploadField);
```

---

## Example: Third-Party Integration

Let's create a field that integrates with a third-party component:

```jsx
import React from "react";
import { registerField } from "nova-forms";
import { DatePicker } from "react-datepicker";
import "react-datepicker/dist/react-datepicker.css";

function CustomDatePickerField({ field, value, onChange, theme }) {
  const handleDateChange = (date) => {
    onChange({
      target: {
        name: field.name,
        value: date ? date.toISOString() : ""
      }
    });
  };
  
  return (
    <div>
      {/* Label */}
      {field.title && (
        <label
          htmlFor={field.name}
          style={{ color: theme.label }}
          className="block text-sm font-medium mb-1"
        >
          {field.title}
          {field.required && (
            <span style={{ color: theme.requiredAsterisk }}> *</span>
          )}
        </label>
      )}
      
      {/* Date picker */}
      <DatePicker
        selected={value ? new Date(value) : null}
        onChange={handleDateChange}
        placeholderText={field.placeholder || "Select date"}
        className="w-full px-3 py-2 border rounded-md"
        style={{
          color: theme.inputText,
          backgroundColor: theme.inputBackground,
          borderColor: theme.inputBorder
        }}
        readOnly={field.readOnly}
        required={field.required}
      />
      
      {/* Description */}
      {field.description && (
        <p style={{ color: theme.description }} className="mt-1 text-sm">
          {field.description}
        </p>
      )}
    </div>
  );
}

// Register the field
registerField("customDate", CustomDatePickerField);
```

---

## Example: Domain-Specific Field

Let's create a field specific to a particular domain - a product SKU field:

```jsx
import React, { useState } from "react";
import { registerField } from "nova-forms";

function SKUField({ field, value, onChange, theme }) {
  const [isValid, setIsValid] = useState(true);
  const [suggestions, setSuggestions] = useState([]);
  
  const validateSKU = (sku) => {
    // SKU validation logic
    const pattern = /^[A-Z]{2}[0-9]{4}$/;
    return pattern.test(sku);
  };
  
  const generateSuggestions = (partial) => {
    // Generate SKU suggestions based on partial input
    const categories = ["EL", "CL", "BK", "FD"];
    const numbers = ["0001", "0002", "0003", "0004"];
    
    return categories
      .filter(cat => cat.startsWith(partial.toUpperCase()))
      .flatMap(cat => numbers.map(num => cat + num))
      .slice(0, 5);
  };
  
  const handleChange = (e) => {
    const newValue = e.target.value.toUpperCase();
    const valid = validateSKU(newValue);
    
    setIsValid(valid);
    setSuggestions(generateSuggestions(newValue));
    
    onChange({
      target: {
        name: field.name,
        value: newValue
      }
    });
  };
  
  const handleSuggestionClick = (suggestion) => {
    onChange({
      target: {
        name: field.name,
        value: suggestion
      }
    });
    setSuggestions([]);
    setIsValid(true);
  };
  
  return (
    <div>
      {/* Label */}
      {field.title && (
        <label
          htmlFor={field.name}
          style={{ color: theme.label }}
          className="block text-sm font-medium mb-1"
        >
          {field.title}
          {field.required && (
            <span style={{ color: theme.requiredAsterisk }}> *</span>
          )}
        </label>
      )}
      
      {/* SKU input */}
      <div className="relative">
        <input
          type="text"
          value={value || ""}
          onChange={handleChange}
          placeholder={field.placeholder || "EL0001"}
          maxLength={6}
          className="w-full px-3 py-2 border rounded-md"
          style={{
            color: theme.inputText,
            backgroundColor: theme.inputBackground,
            borderColor: isValid ? theme.inputBorder : theme.error
          }}
          readOnly={field.readOnly}
          required={field.required}
        />
        
        {/* Suggestions dropdown */}
        {suggestions.length > 0 && (
          <div className="absolute top-full left-0 right-0 mt-1 bg-white border rounded-md shadow-lg z-10">
            {suggestions.map((suggestion) => (
              <button
                key={suggestion}
                type="button"
                onClick={() => handleSuggestionClick(suggestion)}
                className="w-full px-3 py-2 text-left hover:bg-gray-100"
              >
                {suggestion}
              </button>
            ))}
          </div>
        )}
      </div>
      
      {/* Validation message */}
      {!isValid && value && (
        <p style={{ color: theme.error }} className="mt-1 text-sm">
          SKU must be in format: XX0000 (2 letters, 4 numbers)
        </p>
      )}
      
      {/* Description */}
      {field.description && (
        <p style={{ color: theme.description }} className="mt-1 text-sm">
          {field.description}
        </p>
      )}
    </div>
  );
}

// Register the field
registerField("sku", SKUField);
```

---

## Using Custom Fields

Once registered, custom fields can be used in your form schemas:

```jsx
const fields = [
  {
    name: "primaryColor",
    type: "colorPicker",
    title: "Primary Color",
    description: "Choose your brand's primary color"
  },
  {
    name: "logo",
    type: "fileUpload",
    title: "Logo",
    accept: "image/*",
    description: "Upload your company logo"
  },
  {
    name: "launchDate",
    type: "customDate",
    title: "Launch Date",
    description: "When will you launch?"
  },
  {
    name: "productSKU",
    type: "sku",
    title: "Product SKU",
    required: true,
    description: "Unique product identifier"
  }
];
```

---

## Best Practices

### 1. Follow Field Component Conventions
```jsx
// ✅ Good: Standard field component structure
function CustomField({ field, value, onChange, theme }) {
  return (
    <div>
      {/* Label */}
      {field.title && (
        <label style={{ color: theme.label }}>
          {field.title}
          {field.required && <span style={{ color: theme.requiredAsterisk }}> *</span>}
        </label>
      )}
      
      {/* Field input */}
      <input
        value={value || ""}
        onChange={onChange}
        placeholder={field.placeholder}
        readOnly={field.readOnly}
        required={field.required}
      />
      
      {/* Description */}
      {field.description && (
        <p style={{ color: theme.description }}>{field.description}</p>
      )}
    </div>
  );
}
```

### 2. Handle All Field Properties
```jsx
// ✅ Good: Handle all relevant field properties
function CustomField({ field, value, onChange, theme }) {
  const {
    name,
    title,
    placeholder,
    description,
    required,
    readOnly,
    error,
    helper
  } = field;
  
  // Use all properties appropriately
}
```

### 3. Use Theme Properties
```jsx
// ✅ Good: Use theme for consistent styling
const styles = {
  color: theme.inputText,
  backgroundColor: theme.inputBackground,
  borderColor: theme.inputBorder
};
```

### 4. Provide Proper Change Events
```jsx
// ✅ Good: Provide proper change event structure
onChange({
  target: {
    name: field.name,
    value: newValue
  }
});
```

### 5. Handle Edge Cases
```jsx
// ✅ Good: Handle edge cases
function CustomField({ field, value, onChange, theme }) {
  const safeValue = value || "";
  const isReadOnly = field.readOnly === true;
  
  // Handle empty values, read-only state, etc.
}
```

---

## Advanced Patterns

### Field with Sub-fields
```jsx
function AddressField({ field, value, onChange, theme }) {
  const handleSubFieldChange = (subFieldName, subValue) => {
    const newValue = {
      ...value,
      [subFieldName]: subValue
    };
    
    onChange({
      target: {
        name: field.name,
        value: newValue
      }
    });
  };
  
  return (
    <div>
      <label style={{ color: theme.label }}>{field.title}</label>
      
      <div className="grid grid-cols-2 gap-2">
        <input
          placeholder="Street"
          value={value?.street || ""}
          onChange={(e) => handleSubFieldChange("street", e.target.value)}
        />
        <input
          placeholder="City"
          value={value?.city || ""}
          onChange={(e) => handleSubFieldChange("city", e.target.value)}
        />
      </div>
    </div>
  );
}
```

### Field with Validation
```jsx
function ValidatedField({ field, value, onChange, theme }) {
  const [error, setError] = useState("");
  
  const validate = (val) => {
    if (field.pattern) {
      const regex = new RegExp(field.pattern);
      if (!regex.test(val)) {
        setError(field.patternMessage || "Invalid format");
        return false;
      }
    }
    setError("");
    return true;
  };
  
  const handleChange = (e) => {
    const newValue = e.target.value;
    validate(newValue);
    onChange(e);
  };
  
  return (
    <div>
      <label style={{ color: theme.label }}>{field.title}</label>
      <input
        value={value || ""}
        onChange={handleChange}
        style={{
          borderColor: error ? theme.error : theme.inputBorder
        }}
      />
      {error && (
        <p style={{ color: theme.error }}>{error}</p>
      )}
    </div>
  );
}
```

---

## Troubleshooting

### Common Issues

1. **Field not rendering**: Check that the field type is registered correctly
2. **Change events not working**: Ensure proper event structure
3. **Styling issues**: Use theme properties for consistent styling

### Debug Tips

```jsx
// Add console logging to debug custom fields
function CustomField({ field, value, onChange, theme }) {
  console.log("Custom field props:", { field, value, theme });
  
  // Your field implementation
}
```

---

*Custom fields are the key to extending NovaForms for your specific needs. Use them to create specialized inputs, integrate third-party components, or implement domain-specific form elements.*
