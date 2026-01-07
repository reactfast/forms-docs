---
title: Field Types
nextjs:
  metadata:
    title: Field Types - NovaForms
    description: Complete reference for all built-in field types in NovaForms, including examples and configuration options.
---

NovaForms provides 20+ built-in field types to handle various input scenarios. Each field type has specific properties and behaviors optimized for different use cases.

---

## Text Input Fields

### `string` - Text Input
Basic single-line text input field.

```jsx
{
  name: "firstName",
  type: "string",
  title: "First Name",
  placeholder: "Enter your first name",
  required: true,
  maxLength: 50
}
```

**Properties:**
- `maxLength` - Maximum character length
- `minLength` - Minimum character length
- `pattern` - Validation regex pattern

### `text` - Textarea
Multi-line text input for longer content.

```jsx
{
  name: "message",
  type: "text",
  title: "Message",
  placeholder: "Enter your message here...",
  width: 100,
  rows: 4
}
```

**Properties:**
- `rows` - Number of visible text lines
- `cols` - Number of visible character columns
- `maxLength` - Maximum character length

### `email` - Email Input
Email input with built-in validation.

```jsx
{
  name: "email",
  type: "email",
  title: "Email Address",
  required: true,
  placeholder: "user@example.com"
}
```

**Features:**
- Built-in email format validation
- Mobile keyboard optimization
- Pattern validation support

### `tel` - Phone Input
Phone number input with mobile optimization.

```jsx
{
  name: "phone",
  type: "tel",
  title: "Phone Number",
  placeholder: "(555) 123-4567",
  pattern: "^\\d{3}-\\d{3}-\\d{4}$"
}
```

**Features:**
- Mobile numeric keypad
- Pattern validation for phone formats
- International number support

### `url` - URL Input
URL input with validation.

```jsx
{
  name: "website",
  type: "url",
  title: "Website",
  placeholder: "https://example.com"
}
```

**Features:**
- Built-in URL format validation
- Protocol validation (http/https)
- Domain format checking

---

## Numeric Fields

### `number` - Number Input
Numeric input with validation constraints.

```jsx
{
  name: "age",
  type: "number",
  title: "Age",
  min: 0,
  max: 120,
  step: 1,
  default: 0
}
```

**Properties:**
- `min` - Minimum allowed value
- `max` - Maximum allowed value
- `step` - Increment/decrement step
- `precision` - Decimal places

### `currency` - Currency Input
Formatted currency input.

```jsx
{
  name: "price",
  type: "currency",
  title: "Price",
  currency: "USD",
  min: 0,
  step: 0.01
}
```

**Properties:**
- `currency` - Currency code (USD, EUR, etc.)
- `locale` - Number formatting locale
- `symbol` - Currency symbol

---

## Boolean Fields

### `boolean` - Checkbox
Single checkbox for true/false values.

```jsx
{
  name: "subscribe",
  type: "boolean",
  title: "Subscribe to newsletter",
  default: false
}
```

**Features:**
- Simple true/false toggle
- Custom styling support
- Accessibility compliant

### `toggle` - Toggle Switch
Modern toggle switch component.

```jsx
{
  name: "notifications",
  type: "toggle",
  title: "Enable notifications",
  default: true
}
```

**Features:**
- Smooth animation
- Custom colors
- Mobile-friendly touch targets

---

## Date & Time Fields

### `date` - Date Picker
Date selection with calendar interface.

```jsx
{
  name: "birthDate",
  type: "date",
  title: "Birth Date",
  required: true,
  min: "1900-01-01",
  max: "2024-12-31"
}
```

**Properties:**
- `min` - Minimum selectable date
- `max` - Maximum selectable date
- `format` - Date display format
- `locale` - Date formatting locale

### `datetime` - Date & Time Picker
Combined date and time selection.

```jsx
{
  name: "eventTime",
  type: "datetime",
  title: "Event Date & Time",
  required: true,
  timezone: "UTC"
}
```

**Properties:**
- `timezone` - Timezone handling
- `format` - DateTime display format
- `timeStep` - Time increment (minutes)

### `time` - Time Picker
Time-only selection input.

```jsx
{
  name: "startTime",
  type: "time",
  title: "Start Time",
  default: "09:00",
  step: 15
}
```

**Properties:**
- `step` - Time increment in minutes
- `format` - Time display format (12h/24h)

---

## Selection Fields

### `select` - Single Select Dropdown
Single selection from a list of options.

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
  required: true,
  placeholder: "Select a country"
}
```

**Properties:**
- `options` - Array of option objects
- `placeholder` - Placeholder text
- `searchable` - Enable search functionality
- `clearable` - Allow clearing selection

### `multiselect` - Multi-Select Dropdown
Multiple selection from a list of options.

```jsx
{
  name: "interests",
  type: "multiselect",
  title: "Areas of Interest",
  options: [
    { value: "tech", label: "Technology" },
    { value: "design", label: "Design" },
    { value: "business", label: "Business" }
  ],
  maxSelections: 3
}
```

**Properties:**
- `maxSelections` - Maximum number of selections
- `minSelections` - Minimum number of selections
- `searchable` - Enable search functionality

### `radio` - Radio Button Group
Single selection using radio buttons.

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
  required: true,
  layout: "vertical"  // or "horizontal"
}
```

**Properties:**
- `layout` - Button arrangement (vertical/horizontal)
- `inline` - Display options inline

---

## File Upload Fields

### `file` - Basic File Upload
Simple file upload input.

```jsx
{
  name: "resume",
  type: "file",
  title: "Resume",
  accept: ".pdf,.doc,.docx",
  maxSize: 5 * 1024 * 1024  // 5MB
}
```

**Properties:**
- `accept` - Accepted file types
- `maxSize` - Maximum file size in bytes
- `multiple` - Allow multiple file selection

### `fileV2` - Enhanced File Upload
Advanced file upload with preview and progress.

```jsx
{
  name: "profilePhoto",
  type: "fileV2",
  title: "Profile Photo",
  accept: "image/*",
  maxSize: 2 * 1024 * 1024,  // 2MB
  preview: true
}
```

**Features:**
- File preview
- Upload progress
- Drag and drop support
- File validation

### `uploadToBase` - Base64 Image Upload
Image upload that converts to base64 string.

```jsx
{
  name: "avatar",
  type: "uploadToBase",
  title: "Avatar",
  maxSize: 1024 * 1024,  // 1MB
  dimensions: { width: 200, height: 200 }
}
```

**Properties:**
- `dimensions` - Image size constraints
- `quality` - Image compression quality
- `format` - Output format (jpeg, png, webp)

---

## Specialized Fields

### `color` - Color Picker
Color selection with visual picker.

```jsx
{
  name: "themeColor",
  type: "color",
  title: "Theme Color",
  default: "#3b82f6",
  format: "hex"  // hex, rgb, hsl
}
```

**Properties:**
- `format` - Color format (hex, rgb, hsl)
- `presets` - Predefined color options
- `alpha` - Include alpha channel

### `signature` - Signature Pad
Digital signature capture.

```jsx
{
  name: "signature",
  type: "signature",
  title: "Digital Signature",
  required: true,
  width: 400,
  height: 200
}
```

**Properties:**
- `width` - Signature pad width
- `height` - Signature pad height
- `backgroundColor` - Pad background color
- `penColor` - Signature pen color

### `rating` - Star Rating
Star-based rating system.

```jsx
{
  name: "satisfaction",
  type: "rating",
  title: "Rate your experience",
  maxRating: 5,
  default: 0,
  allowHalf: true
}
```

**Properties:**
- `maxRating` - Maximum number of stars
- `allowHalf` - Allow half-star ratings
- `size` - Star size (small, medium, large)
- `color` - Star color

### `scale` - Likert Scale
Likert scale rating system.

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

**Properties:**
- `min` - Minimum scale value
- `max` - Maximum scale value
- `labels` - Array of label text
- `orientation` - Scale orientation (horizontal/vertical)

### `captcha` - reCAPTCHA
reCAPTCHA verification field.

```jsx
{
  name: "captcha",
  type: "captcha",
  siteKey: "your-recaptcha-site-key",
  theme: "light"  // light or dark
}
```

**Properties:**
- `siteKey` - reCAPTCHA site key
- `theme` - reCAPTCHA theme
- `size` - reCAPTCHA size (normal, compact)

---

## Layout Fields

### `header` - Section Header
Section header for form organization.

```jsx
{
  name: "personalInfoHeader",
  type: "header",
  title: "Personal Information",
  width: 100,
  level: 2  // h1, h2, h3, h4, h5, h6
}
```

**Properties:**
- `level` - Header level (1-6)
- `align` - Text alignment (left, center, right)

### `paragraph` - Static Text
Static text content for instructions or information.

```jsx
{
  name: "instructions",
  type: "paragraph",
  content: "Please fill out all required fields marked with an asterisk (*).",
  width: 100,
  align: "left"
}
```

**Properties:**
- `content` - Text content (supports HTML)
- `align` - Text alignment
- `style` - Custom CSS styles

### `image` - Static Image
Static image display.

```jsx
{
  name: "logo",
  type: "image",
  image: {
    src: "/images/logo.png",
    alt: "Company Logo",
    width: 200,
    height: 100
  },
  width: 100
}
```

**Properties:**
- `image.src` - Image source URL
- `image.alt` - Alt text
- `image.width` - Image width
- `image.height` - Image height

---

## Complex Fields

### `array` - Dynamic Array/Subform
Dynamic array of objects with add/remove functionality.

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
  ],
  minItems: 1,
  maxItems: 5
}
```

**Properties:**
- `fields` - Array of field definitions
- `minItems` - Minimum number of items
- `maxItems` - Maximum number of items
- `addLabel` - Add button text
- `removeLabel` - Remove button text

### `subForm` - Nested Form Group
Nested form group for single objects.

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

**Properties:**
- `fields` - Array of field definitions
- `collapsible` - Make subform collapsible
- `defaultCollapsed` - Start collapsed

---

## Field Validation

### Pattern Validation
Client-side validation with regex patterns:

```jsx
{
  name: "phone",
  type: "tel",
  pattern: "^\\d{3}-\\d{3}-\\d{4}$",
  patternMessage: "Please enter phone number as XXX-XXX-XXXX"
}
```

### Multiple Patterns
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

### Numeric Validation
```jsx
{
  name: "age",
  type: "number",
  min: 18,
  max: 65,
  step: 1,
  required: true
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
    { value: "option1", label: "Option 1", disabled: false },
    { value: "option2", label: "Option 2", disabled: false },
    { value: "option3", label: "Option 3", disabled: true }
  ]
}
```

**Option Properties:**
- `value` - Option value
- `label` - Display text
- `disabled` - Disable option
- `group` - Group options together

---

## Best Practices

### 1. Use Appropriate Field Types
```jsx
// ✅ Good: Use specific field types
{ name: "email", type: "email" }
{ name: "phone", type: "tel" }
{ name: "website", type: "url" }

// ❌ Avoid: Using generic string for everything
{ name: "email", type: "string" }
{ name: "phone", type: "string" }
```

### 2. Provide Clear Labels and Placeholders
```jsx
// ✅ Good: Clear and descriptive
{
  name: "phone",
  type: "tel",
  title: "Phone Number",
  placeholder: "(555) 123-4567"
}

// ❌ Avoid: Unclear labels
{
  name: "phone",
  type: "tel",
  title: "Phone"
}
```

### 3. Use Validation Appropriately
```jsx
// ✅ Good: Appropriate validation
{
  name: "age",
  type: "number",
  min: 18,
  max: 120,
  required: true
}

// ❌ Avoid: Over-validation
{
  name: "firstName",
  type: "string",
  pattern: "^[A-Za-z]+$",
  minLength: 2,
  maxLength: 50,
  required: true
}
```

### 4. Consider User Experience
```jsx
// ✅ Good: User-friendly options
{
  name: "country",
  type: "select",
  options: countries,
  searchable: true,
  placeholder: "Search countries..."
}

// ❌ Avoid: Poor UX
{
  name: "country",
  type: "select",
  options: countries  // No search, no placeholder
}
```

---

## Troubleshooting

### Common Issues

1. **Field not rendering**: Check field type is valid
2. **Validation not working**: Verify pattern syntax
3. **Options not showing**: Check options array format
4. **File upload failing**: Verify file size and type constraints

### Debug Tips

```jsx
// Log field definitions
console.log("Field definition:", field);

// Test field types
const testField = {
  name: "test",
  type: "string",
  title: "Test Field"
};
```

---

*NovaForms provides a comprehensive set of field types to handle virtually any form input scenario. Choose the appropriate field type for your use case to ensure the best user experience.*
