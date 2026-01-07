---
title: Fields & Schemas
nextjs:
  metadata:
    title: Fields & Schemas - NovaForms
    description: Complete reference for NovaForms field types, schemas, and configuration options.
---

NovaForms uses a JSON schema approach to define form fields. Each field is a JavaScript object that describes the input type, behavior, and appearance. This guide covers all available field types and their configuration options.

---

## Field Schema Structure

Every field object has a basic structure:

```jsx
{
  name: "fieldName",        // Required: Unique identifier
  type: "fieldType",        // Required: Field type
  title: "Display Label",   // Optional: Human-readable label
  width: 100,               // Optional: Width percentage (25, 50, 75, 100)
  default: "defaultValue",   // Optional: Default value
  // ... type-specific options
}
```

---

## Core Field Properties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `name` | `string` | Yes | Unique field identifier |
| `type` | `string` | Yes | Field type (see types below) |
| `title` | `string` | No | Display label |
| `label` | `string` | No | Legacy display label (use `title`) |
| `width` | `number` | No | Width percentage (25, 50, 75, 100) |
| `default` | `any` | No | Default value |
| `required` | `boolean` | No | Mark field as required |
| `readOnly` | `boolean` | No | Make field read-only |
| `placeholder` | `string` | No | Placeholder text |
| `description` | `string` | No | Help text below field |
| `helper` | `string` | No | Additional help text |
| `error` | `string` | No | Error message to display |
| `leadingIcon` | `Component` | No | Icon component before input |
| `trailingIcon` | `Component` | No | Icon component after input |

---

## Built-in Field Types

### Text Input Fields

#### `string` - Text Input
Basic text input field.

{% nova-preview fields=[
  { "name": "firstName", "type": "string", "title": "First Name", "placeholder": "Enter your first name", "required": true, "width": 50 },
  { "name": "lastName", "type": "string", "title": "Last Name", "placeholder": "Enter your last name", "required": true, "width": 50 }
] /%}

```jsx
{
  name: "firstName",
  type: "string",
  title: "First Name",
  placeholder: "Enter your first name",
  required: true
}
```

#### `text` - Textarea
Multi-line text input.

{% nova-preview fields=[
  { "name": "message", "type": "text", "title": "Message", "placeholder": "Enter your message here...", "width": 100 }
] /%}

```jsx
{
  name: "message",
  type: "text",
  title: "Message",
  placeholder: "Enter your message here...",
  width: 100
}
```

#### `email` - Email Input
Email input with built-in validation.

{% nova-preview fields=[
  { "name": "email", "type": "email", "title": "Email Address", "required": true, "width": 100 }
] /%}

```jsx
{
  name: "email",
  type: "email",
  title: "Email Address",
  required: true,
  pattern: [
    {
      regex: "^[^\\s@]+@[^\\s@]+\\.[^\\s@]+$",
      message: "Please enter a valid email address"
    }
  ]
}
```

#### `tel` - Phone Input
Phone number input.

{% nova-preview fields=[
  { "name": "phone", "type": "tel", "title": "Phone Number", "placeholder": "(555) 123-4567", "width": 100 }
] /%}

```jsx
{
  name: "phone",
  type: "tel",
  title: "Phone Number",
  placeholder: "(555) 123-4567"
}
```

#### `url` - URL Input
URL input with validation.

```jsx
{
  name: "website",
  type: "url",
  title: "Website",
  placeholder: "https://example.com"
}
```

### Numeric Fields

#### `number` - Number Input
Numeric input field.

{% nova-preview fields=[
  { "name": "age", "type": "number", "title": "Age", "default": 0, "min": 0, "max": 120, "width": 100 }
] /%}

```jsx
{
  name: "age",
  type: "number",
  title: "Age",
  default: 0,
  min: 0,
  max: 120
}
```

### Boolean Fields

#### `boolean` - Checkbox
Single checkbox field.

{% nova-preview fields=[
  { "name": "subscribe", "type": "boolean", "title": "Subscribe to newsletter", "default": false, "width": 100 }
] /%}

```jsx
{
  name: "subscribe",
  type: "boolean",
  title: "Subscribe to newsletter",
  default: false
}
```

#### `toggle` - Toggle Switch
Toggle switch component.

```jsx
{
  name: "notifications",
  type: "toggle",
  title: "Enable notifications",
  default: true
}
```

### Date & Time Fields

#### `date` - Date Picker
Date selection input.

```jsx
{
  name: "birthDate",
  type: "date",
  title: "Birth Date",
  required: true
}
```

#### `datetime` - Date & Time Picker
Date and time selection input.

```jsx
{
  name: "eventTime",
  type: "datetime",
  title: "Event Date & Time",
  required: true
}
```

#### `time` - Time Picker
Time selection input.

```jsx
{
  name: "startTime",
  type: "time",
  title: "Start Time",
  default: "09:00"
}
```

### Selection Fields

#### `select` - Single Select Dropdown
Single selection dropdown.

{% nova-preview fields=[
  { "name": "country", "type": "select", "title": "Country", "options": [
    { "value": "us", "label": "United States" },
    { "value": "ca", "label": "Canada" },
    { "value": "mx", "label": "Mexico" }
  ], "required": true, "width": 100 }
] /%}

```jsx
{
  name: "country",
  type: "select",
  title: "Country",
  options: [
    { value: "us", label: "United States" },
    { value: "ca", label: "Canada" },
    { value: "mx", label: "Mexico" }
  ],
  required: true
}
```

#### `multiselect` - Multi-Select Dropdown
Multiple selection dropdown.

{% nova-preview fields=[
  { "name": "interests", "type": "multiselect", "title": "Areas of Interest", "options": [
    { "value": "tech", "label": "Technology" },
    { "value": "design", "label": "Design" },
    { "value": "business", "label": "Business" },
    { "value": "marketing", "label": "Marketing" }
  ], "width": 100 }
] /%}

```jsx
{
  name: "interests",
  type: "multiselect",
  title: "Areas of Interest",
  options: [
    { value: "tech", label: "Technology" },
    { value: "design", label: "Design" },
    { value: "business", label: "Business" },
    { value: "marketing", label: "Marketing" }
  ]
}
```

#### `radio` - Radio Button Group
Radio button group for single selection.

{% nova-preview fields=[
  { "name": "contactMethod", "type": "radio", "title": "Preferred Contact Method", "options": [
    { "value": "email", "label": "Email" },
    { "value": "phone", "label": "Phone" },
    { "value": "sms", "label": "SMS" }
  ], "required": true, "width": 100 }
] /%}

```jsx
{
  name: "contactMethod",
  type: "radio",
  title: "Preferred Contact Method",
  options: [
    { value: "email", label: "Email" },
    { value: "phone", label: "Phone" },
    { value: "sms", label: "SMS" }
  ],
  required: true
}
```

### File Upload Fields

#### `file` - Basic File Upload
Simple file upload input.

```jsx
{
  name: "resume",
  type: "file",
  title: "Resume",
  accept: ".pdf,.doc,.docx"
}
```

#### `fileV2` - Enhanced File Upload
Advanced file upload with preview.

```jsx
{
  name: "profilePhoto",
  type: "fileV2",
  title: "Profile Photo",
  accept: "image/*"
}
```

#### `uploadToBase` - Base64 Image Upload
Image upload that converts to base64.

```jsx
{
  name: "avatar",
  type: "uploadToBase",
  title: "Avatar",
  maxSize: 1024 * 1024  // 1MB
}
```

### Specialized Fields

#### `color` - Color Picker
Color selection input.

{% nova-preview fields=[
  { "name": "themeColor", "type": "color", "title": "Theme Color", "default": "#3b82f6", "width": 100 }
] /%}

```jsx
{
  name: "themeColor",
  type: "color",
  title: "Theme Color",
  default: "#3b82f6"
}
```

#### `signature` - Signature Pad
Digital signature input.

```jsx
{
  name: "signature",
  type: "signature",
  title: "Digital Signature",
  required: true
}
```

#### `rating` - Star Rating
Star rating component.

{% nova-preview fields=[
  { "name": "satisfaction", "type": "rating", "title": "Rate your experience", "maxRating": 5, "default": 0, "width": 100 }
] /%}

```jsx
{
  name: "satisfaction",
  type: "rating",
  title: "Rate your experience",
  maxRating: 5,
  default: 0
}
```

#### `scale` - Likert Scale
Likert scale rating.

```jsx
{
  name: "agreement",
  type: "scale",
  title: "I agree with the terms",
  min: 1,
  max: 5,
  labels: ["Strongly Disagree", "Disagree", "Neutral", "Agree", "Strongly Agree"]
}
```

#### `captcha` - reCAPTCHA
reCAPTCHA verification.

```jsx
{
  name: "captcha",
  type: "captcha",
  siteKey: "your-recaptcha-site-key"
}
```

### Layout Fields

#### `header` - Section Header
Section header for form organization.

```jsx
{
  name: "personalInfoHeader",
  type: "header",
  title: "Personal Information",
  width: 100
}
```

#### `paragraph` - Static Text
Static text content.

```jsx
{
  name: "instructions",
  type: "paragraph",
  content: "Please fill out all required fields marked with an asterisk (*).",
  width: 100
}
```

#### `image` - Static Image
Static image display.

```jsx
{
  name: "logo",
  type: "image",
  image: {
    src: "/images/logo.png",
    alt: "Company Logo"
  },
  width: 100
}
```

### Complex Fields

#### `array` - Dynamic Array/Subform
Dynamic array of objects with add/remove functionality.

{% nova-preview fields=[
  { "name": "addresses", "type": "array", "title": "Addresses", "width": 100, "fields": [
    { "name": "street", "type": "string", "title": "Street", "width": 100 },
    { "name": "city", "type": "string", "title": "City", "width": 50 },
    { "name": "state", "type": "string", "title": "State", "width": 25 },
    { "name": "zip", "type": "string", "title": "ZIP", "width": 25 }
  ] }
] /%}

```jsx
{
  name: "addresses",
  type: "array",
  title: "Addresses",
  width: 100,
  fields: [
    { name: "street", type: "string", title: "Street", width: 100 },
    { name: "city", type: "string", title: "City", width: 50 },
    { name: "state", type: "string", title: "State", width: 25 },
    { name: "zip", type: "string", title: "ZIP", width: 25 }
  ]
}
```

#### `subForm` - Nested Form Group
Nested form group (similar to array but for single objects).

```jsx
{
  name: "emergencyContact",
  type: "subForm",
  title: "Emergency Contact",
  width: 100,
  fields: [
    { name: "name", type: "string", title: "Name", width: 100 },
    { name: "relationship", type: "string", title: "Relationship", width: 50 },
    { name: "phone", type: "tel", title: "Phone", width: 50 }
  ]
}
```

---

## Field Options

### Select Field Options
For `select`, `multiselect`, and `radio` fields:

```jsx
{
  name: "category",
  type: "select",
  options: [
    { value: "option1", label: "Option 1" },
    { value: "option2", label: "Option 2" },
    { value: "option3", label: "Option 3" }
  ]
}
```

### Pattern Validation
Client-side validation with regex patterns:

```jsx
{
  name: "phone",
  type: "tel",
  pattern: [
    {
      regex: "^\\d{3}-\\d{3}-\\d{4}$",
      message: "Please enter phone number as XXX-XXX-XXXX"
    }
  ]
}
```

Multiple patterns:

```jsx
{
  name: "email",
  type: "email",
  pattern: [
    {
      regex: "^[^\\s@]+@[^\\s@]+\\.[^\\s@]+$",
      message: "Please enter a valid email address"
    },
    {
      regex: ".*@company\\.com$",
      message: "Please use your company email"
    }
  ]
}
```

---

## Conditional Logic

### Field Conditions
Control field visibility and state:

```jsx
{
  name: "phone",
  type: "tel",
  title: "Phone Number",
  conditions: {
    hiddenWhen: [
      { field: "contactMethod", when: "equal", value: "email" }
    ],
    hiddenMode: "any"  // or "all"
  }
}
```

### Read-Only Conditions
Make fields read-only based on conditions:

```jsx
{
  name: "age",
  type: "number",
  title: "Age",
  conditions: {
    readOnlyWhen: [
      { field: "age", when: "less than", value: 0 },
      { field: "age", when: "greater than", value: 120 }
    ],
    readOnlyMode: "any"
  }
}
```

---

## Triggers and Rules

### Field Triggers
Trigger rules when field values change:

```jsx
{
  name: "quantity",
  type: "number",
  triggers: [
    {
      rule: "calculateTotal",
      when: "not empty"
    }
  ]
}
```

Complex triggers:

```jsx
{
  name: "discountCode",
  type: "string",
  triggers: [
    {
      rule: "applyDiscount",
      when: [
        { field: "discountCode", when: "matches", value: "SAVE10" },
        { field: "subtotal", when: "greater than", value: 50 }
      ],
      mode: "all"
    }
  ]
}
```

---

## Layout and Styling

### Width Classes
Fields automatically get responsive width classes:

- `width: 25` → `w-full sm:w-1/4`
- `width: 50` → `w-full sm:w-1/2`
- `width: 75` → `w-full sm:w-3/4`
- `width: 100` → `w-full`

### Icon Support
Add icons to fields:

```jsx
import { UserIcon, MailIcon } from "@heroicons/react/24/outline";

const fields = [
  {
    name: "username",
    type: "string",
    title: "Username",
    leadingIcon: UserIcon
  },
  {
    name: "email",
    type: "email",
    title: "Email",
    leadingIcon: MailIcon
  }
];
```

---

## Best Practices

### 1. Use Descriptive Names
```jsx
// ✅ Good
{ name: "userEmailAddress", type: "email", title: "Email Address" }

// ❌ Avoid
{ name: "email", type: "email", title: "Email" }
```

### 2. Provide Helpful Placeholders
```jsx
// ✅ Good
{ name: "phone", type: "tel", placeholder: "(555) 123-4567" }

// ❌ Avoid
{ name: "phone", type: "tel", placeholder: "Phone" }
```

### 3. Use Appropriate Field Types
```jsx
// ✅ Good
{ name: "age", type: "number", min: 0, max: 120 }

// ❌ Avoid
{ name: "age", type: "string", pattern: [{ regex: "^\\d+$" }] }
```

### 4. Group Related Fields
```jsx
const fields = [
  // Personal Information
  { name: "firstName", type: "string", title: "First Name", width: 50 },
  { name: "lastName", type: "string", title: "Last Name", width: 50 },
  { name: "email", type: "email", title: "Email", width: 100 },
  
  // Contact Information
  { name: "phone", type: "tel", title: "Phone", width: 50 },
  { name: "address", type: "text", title: "Address", width: 100 }
];
```

### 5. Use Default Values Wisely
```jsx
// ✅ Good: Sensible defaults
{ name: "country", type: "select", default: "us" }
{ name: "notifications", type: "boolean", default: true }

// ❌ Avoid: Confusing defaults
{ name: "firstName", type: "string", default: "John" }
```

---

## Common Patterns

### Form with Validation

{% nova-preview fields=[
  { "name": "email", "type": "email", "title": "Email Address", "required": true, "width": 100 },
  { "name": "password", "type": "string", "title": "Password", "required": true, "width": 100 }
] /%}

```jsx
const fields = [
  {
    name: "email",
    type: "email",
    title: "Email Address",
    required: true,
    pattern: [
      {
        regex: "^[^\\s@]+@[^\\s@]+\\.[^\\s@]+$",
        message: "Please enter a valid email address"
      }
    ]
  },
  {
    name: "password",
    type: "string",
    title: "Password",
    required: true,
    pattern: [
      {
        regex: "^.{8,}$",
        message: "Password must be at least 8 characters"
      }
    ]
  }
];
```

### Conditional Fields
```jsx
const fields = [
  {
    name: "hasAddress",
    type: "boolean",
    title: "Do you have a mailing address?"
  },
  {
    name: "street",
    type: "string",
    title: "Street Address",
    conditions: {
      hiddenWhen: [
        { field: "hasAddress", when: "equal", value: false }
      ]
    }
  }
];
```

### Dynamic Arrays
```jsx
const fields = [
  {
    name: "skills",
    type: "array",
    title: "Skills",
    fields: [
      { name: "name", type: "string", title: "Skill Name", width: 50 },
      { name: "level", type: "select", title: "Level", width: 50, options: [
        { value: "beginner", label: "Beginner" },
        { value: "intermediate", label: "Intermediate" },
        { value: "advanced", label: "Advanced" }
      ]}
    ]
  }
];
```

---

*This comprehensive field reference covers all built-in field types and their configuration options. Use it as a reference when building your NovaForms schemas.*
