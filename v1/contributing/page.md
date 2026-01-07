---
title: Contributing
nextjs:
  metadata:
    title: Contributing - NovaForms
    description: Guidelines and information for contributing to NovaForms development.
---

Thank you for your interest in contributing to NovaForms! This document provides guidelines and information for contributors.

---

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Contributing Guidelines](#contributing-guidelines)
- [Pull Request Process](#pull-request-process)
- [Issue Reporting](#issue-reporting)
- [Development Workflow](#development-workflow)
- [Testing](#testing)
- [Documentation](#documentation)
- [Release Process](#release-process)

---

## Code of Conduct

This project adheres to a code of conduct. By participating, you are expected to uphold this code. Please report unacceptable behavior to [contact information].

---

## Getting Started

### Prerequisites

- Node.js 16+ and npm
- Git
- Basic knowledge of React and JavaScript/TypeScript
- Familiarity with form libraries and validation concepts

### Fork and Clone

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/reactfast/forms.git
   cd reactfastforms
   ```
3. Add the upstream repository:
   ```bash
   git remote add upstream https://github.com/reactfast/forms.git
   ```

---

## Development Setup

### Installation

```bash
# Install dependencies
npm install

# Install peer dependencies for development
npm install react react-dom
```

### Development Scripts

```bash
# Start development server
npm run dev

# Run tests
npm test

# Run tests in watch mode
npm run test:watch

# Run linting
npm run lint

# Fix linting issues
npm run lint:fix

# Build the library
npm run build

# Build documentation
npm run build:docs
```

### Project Structure

```
NovaForms/
├── src/
│   ├── components/          # React components
│   ├── hooks/              # Custom React hooks
│   ├── utils/              # Utility functions
│   ├── types/              # TypeScript type definitions
│   ├── constants/          # Constants and enums
│   ├── registry/           # Field registry system
│   ├── engine/            # Rules engine
│   ├── state/             # State management
│   └── validation/        # Validation system
├── docs/                  # Documentation
├── examples/              # Example implementations
├── tests/                 # Test files
└── package.json
```

---

## Contributing Guidelines

### Types of Contributions

We welcome several types of contributions:

- **Bug fixes**: Fix issues and improve stability
- **New features**: Add new field types or functionality
- **Documentation**: Improve docs, add examples, fix typos
- **Tests**: Add test coverage for existing or new features
- **Performance**: Optimize existing code
- **Examples**: Add example implementations

### Before You Start

1. Check existing issues and pull requests to avoid duplicates
2. For significant changes, open an issue first to discuss the approach
3. Ensure your changes align with the project's goals and architecture

### Code Style

- Follow the existing code style and patterns
- Use TypeScript for type safety
- Write clear, self-documenting code
- Add comments for complex logic
- Follow React best practices

### Commit Messages

Use clear, descriptive commit messages:

```
feat: add new color picker field type
fix: resolve validation error in email field
docs: update installation instructions
test: add tests for array field validation
refactor: improve rules engine performance
```

---

## Pull Request Process

### Before Submitting

1. **Test your changes**: Ensure all tests pass
2. **Update documentation**: Update relevant docs if needed
3. **Add tests**: Include tests for new functionality
4. **Check linting**: Fix any linting issues
5. **Update examples**: Add examples for new features

### Pull Request Template

When creating a pull request, please include:

- **Description**: Clear description of changes
- **Type**: Bug fix, feature, documentation, etc.
- **Testing**: How you tested the changes
- **Breaking changes**: Any breaking changes and migration path
- **Related issues**: Link to related issues

### Review Process

1. Automated checks must pass (tests, linting, build)
2. Code review by maintainers
3. Discussion and feedback incorporation
4. Approval and merge

---

## Issue Reporting

### Bug Reports

When reporting bugs, please include:

- **Description**: Clear description of the bug
- **Steps to reproduce**: Detailed steps to reproduce
- **Expected behavior**: What should happen
- **Actual behavior**: What actually happens
- **Environment**: Node.js version, browser, OS
- **Code example**: Minimal code example if possible

### Feature Requests

For feature requests, include:

- **Description**: Clear description of the feature
- **Use case**: Why this feature would be useful
- **Proposed solution**: Your suggested implementation
- **Alternatives**: Other approaches you've considered

---

## Development Workflow

### Branch Naming

Use descriptive branch names:

- `feature/color-picker-field`
- `fix/email-validation-bug`
- `docs/update-installation-guide`
- `test/add-validation-tests`

### Development Process

1. Create a feature branch from `main`
2. Make your changes
3. Write/update tests
4. Update documentation
5. Test thoroughly
6. Submit pull request

### Testing Your Changes

```bash
# Run all tests
npm test

# Run specific test file
npm test -- --testNamePattern="color picker"

# Run tests with coverage
npm run test:coverage

# Test in different environments
npm run test:ci
```

---

## Testing

### Test Structure

- **Unit tests**: Test individual functions and components
- **Integration tests**: Test component interactions
- **E2E tests**: Test complete user workflows
- **Performance tests**: Test performance characteristics

### Writing Tests

```javascript
// Example test structure
describe('ColorPickerField', () => {
  it('should render color picker input', () => {
    // Test implementation
  });
  
  it('should handle color value changes', () => {
    // Test implementation
  });
  
  it('should validate color format', () => {
    // Test implementation
  });
});
```

### Test Coverage

- Aim for high test coverage (>80%)
- Test edge cases and error conditions
- Test accessibility features
- Test performance characteristics

---

## Documentation

### Documentation Standards

- Write clear, concise documentation
- Include code examples
- Update docs when adding features
- Follow the existing documentation style

### Documentation Types

- **API Documentation**: Function and component references
- **User Guides**: Step-by-step tutorials
- **Examples**: Working code examples
- **Architecture**: Internal system documentation

### Updating Documentation

When adding features:

1. Update relevant documentation files
2. Add code examples
3. Update the README if needed
4. Add migration guides for breaking changes

---

## Release Process

### Version Numbering

We follow [Semantic Versioning](https://semver.org/):

- **Major** (1.0.0): Breaking changes
- **Minor** (0.1.0): New features, backward compatible
- **Patch** (0.0.1): Bug fixes, backward compatible

### Release Checklist

- [ ] All tests pass
- [ ] Documentation updated
- [ ] Breaking changes documented
- [ ] Migration guide provided (if needed)
- [ ] Version number updated
- [ ] Changelog updated
- [ ] Release notes prepared

---

## Community

### Getting Help

- **GitHub Issues**: For bugs and feature requests
- **GitHub Discussions**: For questions and general discussion
- **Documentation**: Check existing docs first

### Contributing to Discussions

- Be respectful and constructive
- Search existing discussions before creating new ones
- Provide clear, detailed information
- Help others when you can

---

## Recognition

Contributors will be recognized in:

- CONTRIBUTORS.md file
- Release notes
- Project documentation

---

## License

By contributing to NovaForms, you agree that your contributions will be licensed under the same license as the project.

---

## Questions?

If you have questions about contributing, please:

1. Check existing documentation
2. Search GitHub issues and discussions
3. Open a new issue with the "question" label

Thank you for contributing to NovaForms! 🚀
