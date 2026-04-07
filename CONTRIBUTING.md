# Contributing to Pinion OS

Thank you for your interest in contributing to Pinion OS! This document provides guidelines and instructions for contributing to this x402 micropayments SDK for the Base network.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [How to Contribute](#how-to-contribute)
- [Development Setup](#development-setup)
- [Project Structure](#project-structure)
- [Adding New Skills](#adding-new-skills)
- [Testing](#testing)
- [Pull Request Process](#pull-request-process)
- [Reporting Issues](#reporting-issues)
- [Code Style](#code-style)

## Code of Conduct

This project and everyone participating in it is governed by our commitment to:

- Be respectful and inclusive in all interactions
- Welcome newcomers and help them get started
- Focus on constructive feedback
- Respect different viewpoints and experiences

## Getting Started

### Prerequisites

- **Node.js** v14 or higher
- **npm** or **yarn**
- **Git**
- A **Base network wallet** (for testing micropayments)
- Some **Base Sepolia ETH** (for testnet transactions)

### Quick Start

1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/YOUR_USERNAME/pinion-os.git
   cd pinion-os
   ```
3. Install dependencies:
   ```bash
   npm install
   ```
4. Copy the environment template:
   ```bash
   cp .env.example .env
   ```
5. Run tests to ensure everything works:
   ```bash
   npm test
   ```

## How to Contribute

### Types of Contributions Welcome

- 🐛 **Bug fixes** - Fix issues in the SDK or skill framework
- ✨ **New skills** - Add new on-chain skill integrations
- 📚 **Documentation** - Improve README, code comments, or examples
- 🧪 **Tests** - Add test coverage for existing features
- ⚡ **Performance** - Optimize micropayment processing
- 🔒 **Security** - Report or fix security vulnerabilities

### Areas for Contribution

#### High Priority

- Additional payment provider integrations
- More skill examples
- Better error handling
- Test coverage improvements

#### Nice to Have

- CLI tool improvements
- Additional language bindings
- Plugin system enhancements
- Developer tooling

## Development Setup

### Local Development

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Set up your environment:**
   ```bash
   cp .env.example .env
   # Edit .env with your test wallet details
   ```

3. **Build the project:**
   ```bash
   npm run build
   ```

4. **Run in development mode:**
   ```bash
   npm run dev
   ```

### Testing with Real Micropayments

To test with actual x402 micropayments:

1. Get Base Sepolia ETH from a faucet
2. Configure your `.env` file with test wallet private key
3. Use the examples in the `/examples` directory
4. Start with small amounts (0.001 USDC or less)

⚠️ **Never commit real private keys or mainnet credentials!**

## Project Structure

```
pinion-os/
├── src/              # Core SDK source code
│   ├── client/       # Client SDK for payments
│   ├── server/       # Skill server framework
│   └── utils/        # Utility functions
├── skills/           # Built-in skill implementations
├── examples/         # Usage examples
├── tests/            # Test suite
├── assets/           # Static assets
└── docs/             # Additional documentation
```

### Key Files

- `src/client/` - SDK for integrating payments into apps
- `src/server/` - Framework for creating payment-enabled skills
- `skills/` - Example skill implementations
- `examples/` - Working code examples
- `openclaw.plugin.json` - Claude plugin configuration

## Adding New Skills

Skills are the core of Pinion OS - they define what actions can be triggered with micropayments.

### Skill Structure

A skill consists of:

1. **Skill definition** - Metadata and configuration
2. **Handler function** - The code that executes
3. **Payment config** - x402 payment requirements
4. **Tests** - Unit and integration tests

### Step-by-Step: Creating a New Skill

1. **Create a new skill file** in `src/skills/`:
   ```typescript
   import { Skill, PaymentConfig } from '../types';
   
   export const mySkill: Skill = {
     name: 'my-skill',
     description: 'What this skill does',
     parameters: {
       input: 'string',
       // other parameters
     },
     payment: {
       amount: '0.001',
       currency: 'USDC',
       network: 'base'
     },
     handler: async (params, context) => {
       // Skill implementation
       return { success: true, result: 'done' };
     }
   };
   ```

2. **Register the skill** in `src/skills/index.ts`:
   ```typescript
   export { mySkill } from './my-skill';
   ```

3. **Add tests** in `tests/skills/my-skill.test.ts`

4. **Create an example** in `examples/my-skill-example.ts`

5. **Document the skill** in the README

### Skill Best Practices

- Keep skills focused on a single responsibility
- Validate all input parameters
- Return clear error messages
- Include proper TypeScript types
- Document expected payment amounts
- Handle edge cases gracefully

## Testing

### Running Tests

```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage

# Run specific test file
npm test -- skills/my-skill.test.ts
```

### Test Structure

Tests are organized by component:

```
tests/
├── client/           # Client SDK tests
├── server/           # Server framework tests
├── skills/           # Skill-specific tests
└── integration/      # End-to-end tests
```

### Writing Tests

Use the existing test patterns:

```typescript
import { describe, it, expect } from 'vitest';
import { mySkill } from '../../src/skills/my-skill';

describe('mySkill', () => {
  it('should process payment and execute', async () => {
    const result = await mySkill.handler(
      { input: 'test' },
      { payment: { verified: true } }
    );
    expect(result.success).toBe(true);
  });
  
  it('should handle invalid input', async () => {
    await expect(
      mySkill.handler({ input: null }, {})
    ).rejects.toThrow('Invalid input');
  });
});
```

### Integration Testing

For integration tests that require actual micropayments:

1. Use Base Sepolia testnet
2. Keep payment amounts minimal
3. Mock external services when possible
4. Clean up test transactions

## Pull Request Process

### Before Submitting

1. **Ensure tests pass:**
   ```bash
   npm test
   ```

2. **Check code style:**
   ```bash
   npm run lint
   npm run format:check
   ```

3. **Build succeeds:**
   ```bash
   npm run build
   ```

4. **Update documentation** if needed

### PR Guidelines

1. **Create a feature branch:**
   ```bash
   git checkout -b feature/my-new-feature
   ```

2. **Make focused commits** with clear messages:
   ```bash
   git commit -m "feat: add weather API skill
   
   - Implements weather lookup skill
   - Uses OpenWeatherMap API
   - Includes tests and example"
   ```

3. **Push to your fork:**
   ```bash
   git push origin feature/my-new-feature
   ```

4. **Open a Pull Request** with:
   - Clear title and description
   - Link to related issues
   - Screenshots/examples if applicable
   - Test results

### PR Checklist

- [ ] Tests added/updated and passing
- [ ] Code follows style guidelines
- [ ] Documentation updated
- [ ] Example added for new features
- [ ] No breaking changes (or clearly documented)
- [ ] Commit messages are clear

## Reporting Issues

### Bug Reports

Include:
- **Description** - What happened vs. what you expected
- **Steps to reproduce** - Minimal example
- **Environment** - Node version, OS, wallet used
- **Error messages** - Full stack traces
- **Transaction hashes** - If applicable

### Feature Requests

Include:
- **Use case** - What problem does this solve?
- **Proposed solution** - How should it work?
- **Alternatives** - What else did you consider?
- **Willingness to contribute** - Can you help implement it?

## Code Style

### TypeScript

- Use TypeScript for all new code
- Enable strict mode
- Define interfaces for all public APIs
- Use explicit return types on exported functions

### Formatting

- 2 spaces for indentation
- Single quotes for strings
- Semicolons required
- Max line length: 100 characters

### Naming Conventions

- `camelCase` for variables and functions
- `PascalCase` for classes and interfaces
- `SCREAMING_SNAKE_CASE` for constants
- Descriptive names over abbreviations

### Example

```typescript
// Good
interface PaymentConfig {
  amount: string;
  currency: 'USDC' | 'ETH';
  network: 'base' | 'base-sepolia';
}

const processPayment = async (
  config: PaymentConfig,
  walletAddress: string
): Promise<PaymentResult> => {
  const MINIMUM_AMOUNT = '0.0001';
  // implementation
};

// Bad
interface pc { a: string; c: string; n: string; }
const pp = async (cfg: pc, addr: string) => { /* ... */ };
```

## Questions?

- 📖 Check the [README](README.md) for usage examples
- 🔍 Browse [existing issues](https://github.com/Azure55562/pinion-os/issues)
- 💬 Start a [discussion](https://github.com/Azure55562/pinion-os/discussions) for questions

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

---

Thank you for helping make Pinion OS better! 🚀
