---
title: Validation
nextjs:
  metadata:
    title: Validation - NovaForms
    description: Comprehensive guide to implementing robust form validation in NovaForms, including regex patterns and custom rules.
---

NovaForms provides a flexible and powerful validation system to ensure data integrity and guide users through your forms. You can implement validation using built-in features, regex patterns, constraints, and custom functions.

## Overview

NovaForms supports several layers of validation:

1.  **Built-in Validation**: Automatic validation for specific field types (e.g., `email`, `url`, `number`).
2.  **Pattern Validation**: Using regular expressions (`regex`) to enforce specific input formats.
3.  **Constraint Validation**: Defining `min`, `max`, `minLength`, `maxLength`, `maxSize` for various field types.
4.  **Required Field Validation**: Marking fields as mandatory.
5.  **Custom Validation**: Implementing complex, dynamic, or cross-field validation logic.

## Built-in Validation

Several field types come with automatic, built-in validation to ensure common data formats are correct.

### Email Fields
`type: "email"` fields automatically validate for a basic email format.

```jsx
{
  name: "userEmail",
  type: "email",
  title: "Your Email",
  required: true
}
```

### URL Fields
`type: "url"` fields automatically validate for a basic URL format.

```jsx
{
  name: "websiteUrl",
  type: "url",
  title: "Company Website"
}
```

### Number Fields
`type: "number"` fields respect `min`, `max`, and `step` properties for numeric range validation.

```jsx
{
  name: "age",
  type: "number",
  title: "Age",
  min: 18,
  max: 99,
  step: 1,
  required: true
}
```

### Date Fields
`type: "date"` and `type: "datetime"` fields respect `minDate` and `maxDate` properties.

```jsx
{
  name: "eventDate",
  type: "date",
  title: "Event Date",
  minDate: "2023-01-01", // ISO 8601 format
  maxDate: "2024-12-31"
}
```

## Pattern Validation (Regex)

For more specific format requirements, you can use regular expressions (`regex`) with the `pattern` property on any text-based field (`string`, `text`, `email`, `tel`, `url`).

The `pattern` property accepts an array of objects, allowing you to define multiple patterns, each with its own error message.

### Basic Pattern

```jsx
{
  name: "productCode",
  type: "string",
  title: "Product Code",
  pattern: [
    {
      regex: "^[A-Z]{3}-\\d{4}$", // e.g., ABC-1234
      message: "Product code must be 3 uppercase letters followed by a dash and 4 digits."
    }
  ],
  required: true
}
```

### Multiple Patterns

When multiple patterns are provided, the field is valid only if *all* patterns match.

```jsx
{
  name: "strongPassword",
  type: "string",
  title: "Password",
  pattern: [
    { regex: ".{8,}", message: "Must be at least 8 characters long." },
    { regex: "[0-9]", message: "Must contain at least one digit." },
    { regex: "[a-z]", message: "Must contain at least one lowercase letter." },
    { regex: "[A-Z]", message: "Must contain at least one uppercase letter." },
    { regex: "[^a-zA-Z0-9]", message: "Must contain at least one special character." }
  ],
  required: true
}
```

### Common Regex Patterns

Here are some common regex patterns you might find useful:

#### Phone Numbers

**US Phone Number (XXX-XXX-XXXX)**
```jsx
{
  name: "usPhone",
  type: "tel",
  title: "US Phone Number",
  pattern: [{ regex: "^\\d{3}-\\d{3}-\\d{4}$", message: "Format: XXX-XXX-XXXX" }]
}
```

**International Phone Number (simple)**
```jsx
{
  name: "intlPhone",
  type: "tel",
  title: "International Phone",
  pattern: [{ regex: "^\\+?[0-9]{6,14}$", message: "Enter a valid international phone number." }]
}
```

#### Passwords

**Strong Password (8+ chars, upper, lower, digit, special)**
```jsx
{
  name: "password",
  type: "string",
  title: "Password",
  pattern: [
    { regex: ".{8,}", message: "Min 8 characters" },
    { regex: "[0-9]", message: "At least one digit" },
    { regex: "[a-z]", message: "At least one lowercase" },
    { regex: "[A-Z]", message: "At least one uppercase" },
    { regex: "[^a-zA-Z0-9]", message: "At least one special character" }
  ],
  required: true
}
```

#### Credit Cards

**Visa Card (13 or 16 digits, starts with 4)**
```jsx
{
  name: "visaCard",
  type: "string",
  title: "Visa Card Number",
  pattern: [{ regex: "^4[0-9]{12}(?:[0-9]{3})?$", message: "Invalid Visa card number." }]
}
```

**Mastercard (16 digits, starts with 51-55)**
```jsx
{
  name: "masterCard",
  type: "string",
  title: "Mastercard Number",
  pattern: [{ regex: "^5[1-5][0-9]{14}$", message: "Invalid Mastercard number." }]
}
```

**American Express (15 digits, starts with 34 or 37)**
```jsx
{
  name: "amexCard",
  type: "string",
  title: "Amex Card Number",
  pattern: [{ regex: "^3[47][0-9]{13}$", message: "Invalid American Express card number." }]
}
```

#### Postal Codes

**US ZIP Code (5 digits or 5+4)**
```jsx
{
  name: "usZip",
  type: "string",
  title: "US ZIP Code",
  pattern: [{ regex: "^\\d{5}(?:[-\\s]\\d{4})?$", message: "Invalid US ZIP code." }]
}
```

**Canadian Postal Code (A1A 1A1)**
```jsx
{
  name: "caPostal",
  type: "string",
  title: "Canadian Postal Code",
  pattern: [{ regex: "^[A-Za-z]\\d[A-Za-z][ -]?\\d[A-Za-z]\\d$", message: "Invalid Canadian postal code." }]
}
```

**UK Postcode (various formats)**
```jsx
{
  name: "ukPostcode",
  type: "string",
  title: "UK Postcode",
  pattern: [{ regex: "^([Gg][Ii][Rr] 0[Aa]{2})|((([A-Za-z][0-9]{1,2})|(([A-Za-z][A-Ha-hJ-Yj-y][0-9]{1,2})|(([A-Za-z][0-9][A-Za-z])|([A-Za-z][A-Ha-hJ-Yj-y][0-9][A-Za-z]?))))\\s?[0-9][A-Za-z]{2})$", message: "Invalid UK postcode." }]
}
```

#### Social Security Number (SSN)

**US SSN (XXX-XX-XXXX)**
```jsx
{
  name: "ssn",
  type: "string",
  title: "Social Security Number",
  pattern: [{ regex: "^(?!000|666)[0-8][0-9]{2}-(?!00)[0-9]{2}-(?!0000)[0-9]{4}$", message: "Invalid SSN format." }]
}
```

#### Product Codes

**ISBN-10 or ISBN-13**
```jsx
{
  name: "isbn",
  type: "string",
  title: "ISBN",
  pattern: [{ regex: "^(?:ISBN(?:-13)?:?)(?=[0-9]{13}$)([0-9]{3}-){2}[0-9]{3}[0-9X]$|^[0-9]{9}[0-9X]$", message: "Invalid ISBN format." }]
}
```

## Constraint Validation

Beyond regex patterns, NovaForms supports various constraints to validate field values.

### Numeric Constraints (`number`, `currency`, `rating`, `scale`)
-   `min`: Minimum allowed value.
-   `max`: Maximum allowed value.
-   `step`: Valid step interval for the value.

```jsx
{
  name: "quantity",
  type: "number",
  title: "Quantity",
  min: 1,
  max: 100,
  step: 1
}
```

### String Length Constraints (`string`, `text`)
-   `minLength`: Minimum number of characters.
-   `maxLength`: Maximum number of characters.

```jsx
{
  name: "username",
  type: "string",
  title: "Username",
  minLength: 3,
  maxLength: 20
}
```

### File Constraints (`file`, `fileV2`, `uploadToBase`)
-   `maxSize`: Maximum file size in bytes.
-   `accept`: Comma-separated list of accepted file types (e.g., `"image/*"`, `".pdf,.doc"`).

```jsx
{
  name: "documentUpload",
  type: "file",
  title: "Upload Document",
  maxSize: 5 * 1024 * 1024, // 5 MB
  accept: ".pdf,.doc,.docx"
}
```

### Date Constraints (`date`, `datetime`)
-   `minDate`: Earliest allowed date (ISO 8601 string).
-   `maxDate`: Latest allowed date (ISO 8601 string).

```jsx
{
  name: "bookingDate",
  type: "date",
  title: "Booking Date",
  minDate: "2024-01-01",
  maxDate: "2024-12-31"
}
```

## Required Field Validation

The `required: true` property ensures that a field must have a non-empty value.

```jsx
{
  name: "fullName",
  type: "string",
  title: "Full Name",
  required: true
}
```

### Conditional Required Fields

You can make a field required based on conditions using the rules system (see [Rules & Effects](/docs/rules-effects)).

```jsx
const rules = [
  {
    name: "makeShippingRequired",
    effects: [
      {
        targetField: "shippingAddress",
        prop: "required",
        type: "replace",
        value: true
      }
    ]
  },
  {
    name: "makeShippingOptional",
    effects: [
      {
        targetField: "shippingAddress",
        prop: "required",
        type: "replace",
        value: false
      }
    ]
  }
];

const fields = [
  {
    name: "deliveryOption",
    type: "radio",
    title: "Delivery Option",
    options: [
      { value: "pickup", label: "Pickup" },
      { value: "delivery", label: "Delivery" }
    ],
    triggers: [
      { rule: "makeShippingRequired", when: "equal", value: "delivery" },
      { rule: "makeShippingOptional", when: "equal", value: "pickup" }
    ]
  },
  {
    name: "shippingAddress",
    type: "string",
    title: "Shipping Address"
    // 'required' will be set by the rule
  }
];
```

## Custom Validation Functions

For validation logic that goes beyond patterns and constraints, you can implement custom validation. This typically involves using custom field components or integrating with a form-level validation library.

### Field-Level Custom Validation (via Custom Fields)

When creating a [Custom Field](/docs/custom-fields), you have full control over its validation logic.

```jsx
// In your custom field component (e.g., src/components/CustomEmailField.jsx)
import React, { useState } from "react";
import { registerField } from "nova-forms";

function CustomEmailField({ field, value, onChange, theme }) {
  const [error, setError] = useState("");

  const validateEmail = (email) => {
    if (field.required && !email) {
      return "Email is required.";
    }
    if (email && !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
      return "Please enter a valid email address.";
    }
    if (email && field.domainRestriction && !email.endsWith(field.domainRestriction)) {
      return `Email must be from ${field.domainRestriction}.`;
    }
    return "";
  };

  const handleChange = (e) => {
    const newValue = e.target.value;
    const validationError = validateEmail(newValue);
    setError(validationError);
    onChange(e); // Pass the event up to NovaForm
  };

  return (
    <div>
      <label style={{ color: theme.label }}>{field.title}</label>
      <input
        type="email"
        value={value || ""}
        onChange={handleChange}
        onBlur={handleChange} // Validate on blur too
        placeholder={field.placeholder}
        style={{ borderColor: error ? theme.error : theme.inputBorder }}
      />
      {error && <p style={{ color: theme.error }}>{error}</p>}
      {field.description && <p style={{ color: theme.description }}>{field.description}</p>}
    </div>
  );
}

registerField("customEmail", CustomEmailField);

// In your form schema
const fields = [
  {
    name: "workEmail",
    type: "customEmail",
    title: "Work Email",
    required: true,
    domainRestriction: "@mycompany.com", // Custom prop for validation
    description: "Please use your company email address."
  }
];
```

### Form-Level Validation

For validation that involves the entire form data or complex inter-field dependencies, you can perform validation in your `onSubmit` handler.

```jsx
import { useState } from "react";
import { NovaForm, createFormHandler } from "nova-forms";

const fields = [
  { name: "startDate", type: "date", title: "Start Date", required: true },
  { name: "endDate", type: "date", title: "End Date", required: true },
  { name: "reason", type: "string", title: "Reason for Leave", required: true }
];

export default function LeaveRequestForm() {
  const [formData, setFormData] = useState({});
  const [formErrors, setFormErrors] = useState({});

  const handleChange = createFormHandler({
    fields,
    setState: setFormData,
  });

  const validateForm = (data) => {
    const errors = {};
    if (data.startDate && data.endDate) {
      const start = new Date(data.startDate);
      const end = new Date(data.endDate);
      if (start > end) {
        errors.endDate = "End Date cannot be before Start Date.";
      }
    }
    // Add more form-level validation rules here
    return errors;
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const errors = validateForm(formData);
    if (Object.keys(errors).length > 0) {
      setFormErrors(errors);
      alert("Please fix the form errors.");
    } else {
      setFormErrors({});
      console.log("Form submitted successfully:", formData);
      alert("Form submitted successfully!");
    }
  };

  // Merge field-level errors with form-level errors for display
  const fieldsWithErrors = fields.map(field => ({
    ...field,
    error: formErrors[field.name] || field.error // Prioritize formErrors
  }));

  return (
    <form onSubmit={handleSubmit} className="max-w-2xl mx-auto p-4">
      <h1 className="text-2xl font-bold mb-6">Leave Request</h1>
      <NovaForm
        fields={fieldsWithErrors}
        onChange={handleChange}
        formData={formData}
      />
      <button
        type="submit"
        className="mt-6 px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700"
      >
        Submit Request
      </button>
    </form>
  );
}
```

### Cross-Field Validation

Cross-field validation can be handled either within custom field components (if the fields are closely related) or more commonly, at the form level in the `onSubmit` handler, as shown in the `LeaveRequestForm` example above.

## Advanced Validation Patterns

### Dynamic Error Messages

Error messages can be made dynamic based on the input value or other form state. This is best handled within [Custom Fields](/docs/custom-fields).

```jsx
// Example: Password strength message
// (See Custom Fields documentation for full component implementation)
{
  name: "password",
  type: "customPassword", // A custom field type
  title: "Password",
  minLength: 8,
  // Custom field would dynamically update its error message based on strength
}
```

### Conditional Validation

You might want to apply validation rules only when certain conditions are met. This can be achieved by combining the rules system with custom validation.

```jsx
const rules = [
  {
    name: "requireReasonIfOther",
    effects: [
      {
        targetField: "otherReasonText",
        prop: "required",
        type: "replace",
        value: true
      }
    ]
  },
  {
    name: "makeReasonOptional",
    effects: [
      {
        targetField: "otherReasonText",
        prop: "required",
        type: "replace",
        value: false
      }
    ]
  }
];

const fields = [
  {
    name: "issueType",
    type: "select",
    title: "Type of Issue",
    options: [
      { value: "bug", label: "Bug Report" },
      { value: "feature", label: "Feature Request" },
      { value: "other", label: "Other" }
    ],
    triggers: [
      { rule: "requireReasonIfOther", when: "equal", value: "other" },
      { rule: "makeReasonOptional", when: "not equal", value: "other" }
    ]
  },
  {
    name: "otherReasonText",
    type: "string",
    title: "Please specify",
    // 'required' will be set by the rule
  }
];
```

## Error Handling and Display

NovaForms automatically displays error messages provided via the `error` prop on a field.

```jsx
{
  name: "username",
  type: "string",
  title: "Username",
  required: true,
  error: "Username is required." // This error will be displayed
}
```

When using `pattern` validation, the `message` property of the pattern object will be displayed as the error.

### Custom Error Styling

You can customize the appearance of error messages using the `theme.error` property (see [Theme Styling](/docs/styling-theme)) or by overriding Tailwind classes (see [Styling with Tailwind](/docs/styling-tailwind)).

## Best Practices

### 1. Use Appropriate Validation Methods
-   **Built-in**: For standard formats (email, URL, numbers).
-   **Pattern**: For specific string formats (phone, password, product codes).
-   **Constraints**: For numeric ranges, string lengths, file sizes.
-   **Required**: For mandatory fields.
-   **Custom**: For complex logic, cross-field validation, or dynamic rules.

### 2. Provide Clear and Helpful Error Messages
Error messages should tell the user *what* is wrong and *how* to fix it.

```jsx
// ✅ Good
{ regex: ".{8,}", message: "Password must be at least 8 characters long." }

// ❌ Avoid
{ regex: ".{8,}", message: "Invalid password." }
```

### 3. Validate Early and Often (but not too often)
-   Validate on `blur` for immediate feedback.
-   Validate on `change` for patterns (but be careful not to annoy users).
-   Validate on `submit` for final checks and cross-field logic.

### 4. Handle Edge Cases
Consider what happens with empty inputs, invalid characters, or boundary values.

### 5. Keep Validation Logic Separate
For complex validation, consider separating validation functions from your component logic for better maintainability.

## Troubleshooting

### Common Issues

1.  **Validation not firing**:
    -   Ensure `required: true` is set for mandatory fields.
    -   Check `pattern` regex for syntax errors.
    -   Verify `min`/`max` values are correctly set for numeric fields.
    -   For custom validation, ensure your `onChange` handler is correctly calling validation logic and updating the `error` prop.
2.  **Incorrect error messages**:
    -   Verify the `message` property in your `pattern` objects.
    -   Ensure custom validation functions return the correct error string.
3.  **Cross-field validation issues**:
    -   Make sure your form-level validation function has access to all necessary `formData`.
    -   Ensure `formErrors` are correctly mapped back to the `fields` for display.

### Debug Tips

-   **Console Log `formData`**: In your `onSubmit` or `onChange` handlers, log the `formData` to see its current state.
-   **Test Regex**: Use online regex testers to verify your patterns.
-   **Isolate Validation**: Temporarily simplify validation rules to pinpoint the problematic one.
-   **Inspect Field Props**: In your React DevTools, inspect the `field` prop passed to individual components to see if `error` or other validation-related props are being set as expected.

---

*Robust validation is crucial for creating user-friendly and reliable forms. NovaForms provides a comprehensive set of tools to implement validation, from simple required fields to complex regex patterns and custom logic.*
