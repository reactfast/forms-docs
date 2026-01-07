---
title: API Reference
nextjs:
  metadata:
    title: API Reference - NovaForms
    description: Internal API reference for NovaForms contributors and developers who want to understand the library's architecture.
---

This is the internal API reference for NovaForms contributors and developers who want to understand the library's architecture, internal functions, and implementation details.

---

## Architecture Overview

NovaForms is built on a modular architecture with several key components:

- **Field Registry System** - Manages field type definitions and components
- **Rules Engine** - Executes conditional logic and calculations
- **State Management** - Handles form state updates and synchronization
- **Validation System** - Provides client-side validation capabilities
- **Theme System** - Manages styling and appearance customization

---

## Internal Components

### Field Registry (`src/registry/fieldRegistry.js`)

The field registry is the central system for managing field types and their components.

#### `FieldRegistry` Class

```javascript
class FieldRegistry {
  constructor() {
    this.fields = new Map();
    this.defaultFields = new Map();
  }
  
  register(type, component, isDefault = false) {
    // Internal implementation
  }
  
  get(type) {
    // Internal implementation
  }
  
  getAll() {
    // Internal implementation
  }
  
  has(type) {
    // Internal implementation
  }
}
```

#### Internal Methods

| Method | Description | Parameters | Returns |
|--------|-------------|------------|---------|
| `register(type, component, isDefault)` | Register field type | `string`, `Component`, `boolean` | `void` |
| `get(type)` | Get field component | `string` | `Component \| undefined` |
| `getAll()` | Get all registered fields | - | `Map<string, Component>` |
| `has(type)` | Check if field exists | `string` | `boolean` |
| `unregister(type)` | Remove field type | `string` | `boolean` |
| `clear()` | Clear all fields | - | `void` |

### Rules Engine (`src/engine/rulesEngine.js`)

The rules engine processes and executes rule definitions.

#### `RulesEngine` Class

```javascript
class RulesEngine {
  constructor() {
    this.rules = new Map();
    this.executionQueue = [];
    this.isExecuting = false;
  }
  
  addRule(rule) {
    // Internal implementation
  }
  
  executeRule(ruleName, context) {
    // Internal implementation
  }
  
  processEffects(effects, context) {
    // Internal implementation
  }
}
```

#### Internal Methods

| Method | Description | Parameters | Returns |
|--------|-------------|------------|---------|
| `addRule(rule)` | Add rule to engine | `RuleDefinition` | `void` |
| `executeRule(ruleName, context)` | Execute specific rule | `string`, `ExecutionContext` | `Promise<void>` |
| `processEffects(effects, context)` | Process rule effects | `Effect[]`, `ExecutionContext` | `Promise<void>` |
| `validateRule(rule)` | Validate rule structure | `RuleDefinition` | `boolean` |
| `clearRules()` | Clear all rules | - | `void` |

### State Manager (`src/state/stateManager.js`)

Handles form state updates and synchronization.

#### `StateManager` Class

```javascript
class StateManager {
  constructor(initialState = {}) {
    this.state = initialState;
    this.listeners = new Set();
    this.history = [];
    this.maxHistorySize = 50;
  }
  
  setState(updater) {
    // Internal implementation
  }
  
  getState() {
    // Internal implementation
  }
  
  subscribe(listener) {
    // Internal implementation
  }
  
  unsubscribe(listener) {
    // Internal implementation
  }
}
```

#### Internal Methods

| Method | Description | Parameters | Returns |
|--------|-------------|------------|---------|
| `setState(updater)` | Update state | `Function \| Object` | `void` |
| `getState()` | Get current state | - | `Object` |
| `subscribe(listener)` | Subscribe to changes | `Function` | `Function` (unsubscribe) |
| `unsubscribe(listener)` | Unsubscribe from changes | `Function` | `void` |
| `undo()` | Undo last change | - | `boolean` |
| `redo()` | Redo last change | - | `boolean` |

---

## Internal Functions

### `createFormHandler` Implementation (`src/handlers/createFormHandler.js`)

The core function that creates form change handlers.

#### Internal Structure

```javascript
function createFormHandler(options) {
  const {
    fields,
    setState,
    rules = [],
    validate = false,
    onError = null,
    debug = false
  } = options;
  
  // Internal validation
  validateOptions(options);
  
  // Create internal state manager
  const stateManager = new StateManager();
  
  // Create rules engine
  const rulesEngine = new RulesEngine();
  
  // Register rules
  rules.forEach(rule => rulesEngine.addRule(rule));
  
  // Return handler function
  return function handleChange(event) {
    // Internal implementation
  };
}
```

#### Internal Helper Functions

| Function | Description | Parameters | Returns |
|----------|-------------|------------|---------|
| `validateOptions(options)` | Validate handler options | `Object` | `void` |
| `processFieldChange(field, value, context)` | Process field change | `FieldDefinition`, `any`, `ExecutionContext` | `Promise<void>` |
| `executeTriggers(field, value, context)` | Execute field triggers | `FieldDefinition`, `any`, `ExecutionContext` | `Promise<void>` |
| `updateFieldState(field, updates)` | Update field state | `FieldDefinition`, `Object` | `void` |
| `validateField(field, value)` | Validate field value | `FieldDefinition`, `any` | `ValidationResult` |

### Field Processing (`src/processing/fieldProcessor.js`)

Handles field rendering and processing.

#### `FieldProcessor` Class

```javascript
class FieldProcessor {
  constructor(registry, theme) {
    this.registry = registry;
    this.theme = theme;
    this.processedFields = new Map();
  }
  
  processField(field, context) {
    // Internal implementation
  }
  
  renderField(field, props) {
    // Internal implementation
  }
  
  validateField(field, value) {
    // Internal implementation
  }
}
```

#### Internal Methods

| Method | Description | Parameters | Returns |
|--------|-------------|------------|---------|
| `processField(field, context)` | Process field definition | `FieldDefinition`, `ProcessingContext` | `ProcessedField` |
| `renderField(field, props)` | Render field component | `FieldDefinition`, `FieldProps` | `ReactElement` |
| `validateField(field, value)` | Validate field value | `FieldDefinition`, `any` | `ValidationResult` |
| `applyConditions(field, context)` | Apply field conditions | `FieldDefinition`, `ExecutionContext` | `FieldDefinition` |
| `applyModifiers(field, context)` | Apply field modifiers | `FieldDefinition`, `ExecutionContext` | `FieldDefinition` |

---

## Internal Data Structures

### Field Definition (`src/types/FieldDefinition.js`)

```javascript
interface FieldDefinition {
  // Core properties
  name: string;
  type: string;
  title?: string;
  width?: number;
  default?: any;
  
  // State properties
  required?: boolean;
  readOnly?: boolean;
  disabled?: boolean;
  hidden?: boolean;
  
  // UI properties
  placeholder?: string;
  description?: string;
  helper?: string;
  error?: string;
  
  // Validation properties
  pattern?: PatternDefinition[];
  min?: number;
  max?: number;
  step?: number;
  minLength?: number;
  maxLength?: number;
  
  // Conditional logic
  conditions?: ConditionDefinition;
  triggers?: TriggerDefinition[];
  
  // Type-specific properties
  options?: OptionDefinition[];
  fields?: FieldDefinition[]; // For subforms/arrays
  
  // Internal properties (not user-facing)
  _id?: string;
  _processed?: boolean;
  _errors?: string[];
  _warnings?: string[];
}
```

### Rule Definition (`src/types/RuleDefinition.js`)

```javascript
interface RuleDefinition {
  name: string;
  effects: EffectDefinition[];
  priority?: number;
  condition?: ConditionDefinition;
  metadata?: {
    description?: string;
    version?: string;
    author?: string;
  };
}
```

### Effect Definition (`src/types/EffectDefinition.js`)

```javascript
interface EffectDefinition {
  targetField: string;
  prop: string;
  type: EffectType;
  kind?: DataType;
  value?: any;
  strictString?: boolean;
  sourceFields?: SourceFieldDefinition[];
  condition?: ConditionDefinition;
}
```

### Execution Context (`src/types/ExecutionContext.js`)

```javascript
interface ExecutionContext {
  formData: Record<string, any>;
  fieldData: Record<string, any>;
  previousData: Record<string, any>;
  triggerField?: string;
  triggerValue?: any;
  timestamp: number;
  executionId: string;
  metadata: {
    userAgent?: string;
    sessionId?: string;
    requestId?: string;
  };
}
```

---

## Internal Utilities

### Validation Utilities (`src/utils/validation.js`)

```javascript
// Internal validation functions
export function validateFieldDefinition(field) {
  // Implementation
}

export function validateRuleDefinition(rule) {
  // Implementation
}

export function validateEffectDefinition(effect) {
  // Implementation
}

export function validatePattern(pattern) {
  // Implementation
}

export function validateCondition(condition) {
  // Implementation
}
```

### State Utilities (`src/utils/state.js`)

```javascript
// Internal state management utilities
export function deepClone(obj) {
  // Implementation
}

export function deepMerge(target, source) {
  // Implementation
}

export function getNestedValue(obj, path) {
  // Implementation
}

export function setNestedValue(obj, path, value) {
  // Implementation
}

export function createStateSnapshot(state) {
  // Implementation
}
```

### Event Utilities (`src/utils/events.js`)

```javascript
// Internal event handling utilities
export function createSyntheticEvent(nativeEvent) {
  // Implementation
}

export function normalizeEvent(event) {
  // Implementation
}

export function extractFieldData(event) {
  // Implementation
}

export function createChangeEvent(field, value) {
  // Implementation
}
```

---

## Internal Hooks

### `useFieldRegistry` (`src/hooks/useFieldRegistry.js`)

```javascript
function useFieldRegistry() {
  const registry = useContext(FieldRegistryContext);
  
  const register = useCallback((type, component) => {
    registry.register(type, component);
  }, [registry]);
  
  const get = useCallback((type) => {
    return registry.get(type);
  }, [registry]);
  
  return { register, get, getAll: registry.getAll.bind(registry) };
}
```

### `useRulesEngine` (`src/hooks/useRulesEngine.js`)

```javascript
function useRulesEngine() {
  const engine = useContext(RulesEngineContext);
  
  const addRule = useCallback((rule) => {
    engine.addRule(rule);
  }, [engine]);
  
  const executeRule = useCallback((ruleName, context) => {
    return engine.executeRule(ruleName, context);
  }, [engine]);
  
  return { addRule, executeRule, clearRules: engine.clearRules.bind(engine) };
}
```

### `useFormState` (`src/hooks/useFormState.js`)

```javascript
function useFormState(initialState = {}) {
  const [state, setState] = useState(initialState);
  const stateManager = useRef(new StateManager(initialState));
  
  const updateState = useCallback((updater) => {
    stateManager.current.setState(updater);
    setState(stateManager.current.getState());
  }, []);
  
  return [state, updateState, stateManager.current];
}
```

---

## Internal Constants

### Field Types (`src/constants/fieldTypes.js`)

```javascript
export const FIELD_TYPES = {
  // Text fields
  STRING: 'string',
  TEXT: 'text',
  EMAIL: 'email',
  TEL: 'tel',
  URL: 'url',
  
  // Numeric fields
  NUMBER: 'number',
  CURRENCY: 'currency',
  
  // Boolean fields
  BOOLEAN: 'boolean',
  TOGGLE: 'toggle',
  
  // Date fields
  DATE: 'date',
  DATETIME: 'datetime',
  TIME: 'time',
  
  // Selection fields
  SELECT: 'select',
  MULTISELECT: 'multiselect',
  RADIO: 'radio',
  
  // File fields
  FILE: 'file',
  FILE_V2: 'fileV2',
  UPLOAD_TO_BASE: 'uploadToBase',
  
  // Specialized fields
  COLOR: 'color',
  SIGNATURE: 'signature',
  RATING: 'rating',
  SCALE: 'scale',
  CAPTCHA: 'captcha',
  
  // Layout fields
  HEADER: 'header',
  PARAGRAPH: 'paragraph',
  IMAGE: 'image',
  
  // Complex fields
  ARRAY: 'array',
  SUBFORM: 'subForm'
};
```

### Effect Types (`src/constants/effectTypes.js`)

```javascript
export const EFFECT_TYPES = {
  // Math operations
  ADD: 'add',
  SUBTRACT: 'subtract',
  MULTIPLY: 'multiply',
  DIVIDE: 'divide',
  REPLACE: 'replace',
  
  // String operations
  CONCAT: 'concat',
  
  // Attribute operations
  READ_ONLY: 'readOnly',
  HIDDEN: 'hidden',
  TITLE: 'title',
  REQUIRED: 'required',
  DISABLED: 'disabled'
};
```

### Condition Types (`src/constants/conditionTypes.js`)

```javascript
export const CONDITION_TYPES = {
  // Basic conditions
  TRUE: 'true',
  FALSE: 'false',
  EMPTY: 'empty',
  NOT_EMPTY: 'not empty',
  NULL: 'null',
  NOT_NULL: 'not null',
  
  // Comparison conditions
  EQUAL: 'equal',
  NOT_EQUAL: 'not equal',
  LESS_THAN: 'less than',
  GREATER_THAN: 'greater than',
  BETWEEN: 'between',
  
  // Pattern conditions
  MATCHES: 'matches',
  NOT_MATCHES: 'not matches'
};
```

---

## Internal Error Handling

### Error Types (`src/errors/ErrorTypes.js`)

```javascript
export const ERROR_TYPES = {
  VALIDATION_ERROR: 'VALIDATION_ERROR',
  RULE_EXECUTION_ERROR: 'RULE_EXECUTION_ERROR',
  FIELD_RENDER_ERROR: 'FIELD_RENDER_ERROR',
  STATE_UPDATE_ERROR: 'STATE_UPDATE_ERROR',
  REGISTRY_ERROR: 'REGISTRY_ERROR'
};
```

### Error Classes (`src/errors/Errors.js`)

```javascript
export class NovaFormsError extends Error {
  constructor(message, type, context = {}) {
    super(message);
    this.name = 'NovaFormsError';
    this.type = type;
    this.context = context;
    this.timestamp = Date.now();
  }
}

export class ValidationError extends NovaFormsError {
  constructor(message, field, value) {
    super(message, ERROR_TYPES.VALIDATION_ERROR, { field, value });
    this.name = 'ValidationError';
  }
}

export class RuleExecutionError extends NovaFormsError {
  constructor(message, rule, context) {
    super(message, ERROR_TYPES.RULE_EXECUTION_ERROR, { rule, context });
    this.name = 'RuleExecutionError';
  }
}
```

---

## Performance Considerations

### Optimization Strategies

1. **Field Memoization** - Memoize field components to prevent unnecessary re-renders
2. **Rule Batching** - Batch rule executions to reduce state updates
3. **Lazy Loading** - Load field components on demand
4. **State Normalization** - Normalize form state for efficient updates
5. **Event Debouncing** - Debounce rapid field changes

### Memory Management

1. **Cleanup Listeners** - Properly cleanup event listeners
2. **State History Limits** - Limit state history size
3. **Rule Cache** - Cache processed rules
4. **Field Registry Cleanup** - Cleanup unused field types

---

## Testing Utilities

### Test Helpers (`src/testing/helpers.js`)

```javascript
export function createMockField(overrides = {}) {
  return {
    name: 'testField',
    type: 'string',
    title: 'Test Field',
    ...overrides
  };
}

export function createMockRule(overrides = {}) {
  return {
    name: 'testRule',
    effects: [{
      targetField: 'testField',
      prop: 'value',
      type: 'replace',
      value: 'test'
    }],
    ...overrides
  };
}

export function createMockFormData(overrides = {}) {
  return {
    testField: 'test value',
    ...overrides
  };
}
```

---

*This API reference is intended for NovaForms contributors and developers who need to understand the internal architecture and implementation details of the library.*