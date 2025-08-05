# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the Visual Studio Code (VS Code) open source repository - the complete source code for Microsoft's popular code editor. The project is built using TypeScript, web technologies, and Electron to create a cross-platform development environment.

## Development Commands

### Building and Compilation
- `npm run compile` - Full compilation of client and extensions
- `npm run watch` - Watch mode for development (rebuilds on changes)
- `npm run watch-client` - Watch only client code changes
- `npm run watch-extensions` - Watch only extension changes
- `npm run compile-web` - Compile for web version
- `npm run compile-cli` - Compile command line interface

### Testing
- `npm test` - General test command (see scripts folder for specific tests)
- `npm run test-browser` - Run browser-based unit tests
- `npm run test-node` - Run Node.js unit tests
- Scripts in `/scripts/` folder for integration tests:
  - `./scripts/test.sh` - Main test script
  - `./scripts/test-integration.sh` - Integration tests
  - `./scripts/test-web-integration.sh` - Web integration tests

### Code Quality
- `npm run eslint` - Run ESLint linting
- `npm run stylelint` - Run CSS/style linting
- `npm run hygiene` - Code hygiene checks
- `npm run valid-layers-check` - Validate architectural layers

### Development Scripts
- `./scripts/code.sh` - Launch development version
- `./scripts/code-web.sh` - Launch web development version
- `./scripts/code-server.sh` - Launch server version

## Architecture Overview

The codebase follows a layered architecture pattern:

### Core Layers (`src/vs/`)
- **`base/`** - Foundation utilities, cross-platform abstractions, common data structures
- **`platform/`** - Platform services, dependency injection infrastructure, service interfaces
- **`editor/`** - Text editor implementation, language services, syntax highlighting
- **`workbench/`** - Main application UI, views, panels, and feature integrations
- **`code/`** - Electron main process and application entry points
- **`server/`** - Remote development server implementation

### Key Architectural Patterns
- **Dependency Injection**: Services injected via constructor parameters using `@Injectable` decorators
- **Contribution Model**: Features register through contribution points and registries
- **Event-Driven**: Heavy use of event emitters and disposable patterns
- **Cross-Platform**: Platform-specific code isolated in separate files/folders

### Extension Architecture (`extensions/`)
Built-in extensions that ship with VS Code, including:
- Language features (`typescript-language-features/`, `html-language-features/`)
- Core functionality (`git/`, `debug-auto-launch/`, `emmet/`)
- Themes and UI (`theme-*/` folders)

## Code Conventions

### Indentation and Style
- Use **tabs**, not spaces for indentation
- Use PascalCase for types and enum values
- Use camelCase for functions, methods, properties, and variables
- Use "double quotes" for user-visible strings (externalized/localized)
- Use 'single quotes' for internal strings
- Arrow functions preferred: `x => x + 1` or `(x, y) => x + y`
- Always use curly braces for loops/conditionals
- Function declarations preferred over const arrow functions in top-level scopes

### File Organization
- Tests located in `src/vs/*/test/` folders alongside source
- Each major feature area has its own folder structure
- Platform-specific implementations separated (browser/, node/, electron-*)

### Important Notes
- All user-facing strings must be localized using `nls.localize()`
- All files require Microsoft copyright header
- Export types/functions only when needed across components
- Prefer async/await over Promise chains
- Use JSDoc comments for public APIs

## Common Development Patterns

### Service Registration
Services are registered in the DI container and injected via constructors:
```typescript
export class MyService {
	constructor(
		@IConfigurationService private readonly configService: IConfigurationService
	) {}
}
```

### Contribution Points
Features contribute to extension points:
```typescript
Registry.as<IViewsRegistry>(ViewExtensions.ViewsRegistry).registerViews([{
	id: 'myView',
	name: 'My View',
	// ...
}], viewContainer);
```

### Disposable Pattern
Resource cleanup using disposables:
```typescript
const disposable = someService.onDidChange(() => {
	// handle change
});
// Later: disposable.dispose()
```