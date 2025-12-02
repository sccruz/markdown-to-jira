# AGENTS.md: AI Collaboration Guide

This document provides essential context for AI models interacting with this markdown-to-jira project. Adhering to these guidelines will ensure consistency and maintain code quality.

## 1. Project Overview & Purpose

- **Primary Goal:** This project is a web-based tool designed to convert Markdown text into Jira markup, facilitating seamless integration of documentation into Jira.
- **Business Domain:** Project Management and Documentation.
- **Target Users:** Software developers, project managers, and technical writers who need to maintain rich formatting when working with Jira.

## 2. Core Technologies & Stack

- **Languages:** TypeScript (version 5+)
- **Frameworks & Runtimes:** Bun (version 1.2.11+), Node.js (version 18+)
- **Key Libraries/Dependencies:**
  - `marked` (9.1.0) - Markdown parsing and rendering
  - `highlight.js` (11.9.0) - Syntax highlighting for code blocks
  - `@types/bun` (^1.2.23) - TypeScript definitions for Bun
- **Platforms:** Web browsers (Chrome, Firefox, Safari, Edge with ES6+ support)
- **Package Manager:** Bun (primary), npm (compatibility)

## 3. Setup and Installation

- **Prerequisites:**
  - Bun runtime (version 1.2.11 or later)
  - Node.js (version 18 or later for compatibility)
  - Modern web browser with ES6+ support

- **Installation Steps:**
  1. Clone the repository:
     ```bash
     git clone <repository-url>
     cd markdown-to-jira
     ```
  2. Install dependencies:
     ```bash
     bun install
     ```
  3. Start the development server:
     ```bash
     bun serve.ts
     ```
  4. Access the application at the displayed URL (typically `http://localhost:3000`)

## 4. Build and Test Commands

- **Build for Production:**
  ```bash
  bun build.ts
  ```

- **Run Tests:**
  ```bash
  bun test
  ```

- **Run Tests with Verbose Output:**
  ```bash
  bun test --verbose
  ```

- **Run Specific Test File:**
  ```bash
  bun test convert.test.ts
  ```

## 5. Code Style Guidelines

- **TypeScript Configuration:**
  - Strict mode enabled (`"strict": true`)
  - No `any` types unless absolutely necessary
  - Explicit type annotations for function parameters and returns
  - Use interfaces for object shapes

- **Code Formatting:**
  - Indentation: 2 spaces
  - Line length: Maximum 100 characters
  - Naming conventions:
    - Functions: `camelCase`
    - Classes: `PascalCase`
    - Constants: `UPPER_SNAKE_CASE`

- **Comments:**
  - JSDoc for public functions and classes
  - Inline comments for complex logic
  - Clear, descriptive commit messages

## 6. Testing Instructions

- **Unit Tests:**
  - Located in `src/convert.test.ts`
  - Focus on conversion logic and edge cases
  - Test categories: formatting, edge cases, code blocks, integration

- **Test Examples:**
  ```typescript
  test.it('should render _ correctly', () => {
    test.expect(convert("__bold__").trim()).toEqual("*bold*");
    test.expect(convert("my__key my__key").trim()).toEqual("my\\_\\_key my\\_\\_key");
  });
  ```

- **Running Tests:**
  ```bash
  bun test                    # Run all tests
  bun test --verbose         # Verbose output
  bun test convert.test.ts   # Specific test file
  ```

## 7. Contribution Guidelines

- **Branching Strategy:**
  - Main branch: `main` (production-ready code)
  - Feature branches: `feature/description`
  - Bug fix branches: `fix/description`

- **Pull Request Workflow:**
  1. Fork the repository
  2. Create a feature branch: `git checkout -b feature/your-feature`
  3. Make changes and ensure tests pass
  4. Commit with descriptive messages
  5. Push to your fork: `git push origin feature/your-feature`
  6. Create a Pull Request with clear description

- **Commit Message Format:**
  ```
  type(scope): description

  Examples:
  feat(convert): add support for task lists
  fix(ui): resolve responsive layout issues
  docs(readme): update installation instructions
  ```

- **Code Review Process:**
  - All tests must pass
  - At least one reviewer approval required
  - Update documentation for new features
  - Ensure backward compatibility

## 8. Security Considerations

- **Dependencies:**
  - Regularly update dependencies to patch vulnerabilities
  - Use `bun audit` to identify security issues:
    ```bash
    bun audit
    ```

- **Client-Side Security:**
  - No server-side processing - all conversion happens in browser
  - No sensitive data handling
  - XSS prevention through proper HTML sanitization

## 9. Deployment Steps

- **Production Deployment:**
  1. Build the project:
     ```bash
     bun build.ts
     ```
  2. Deploy the contents of the `dist/` directory to your web server
  3. Ensure proper MIME types for `.html`, `.css`, and `.js` files

- **Development Deployment:**
  ```bash
  bun serve.ts  # Starts development server with hot reloading
  ```

## 10. Project Structure & Key Files

- **Core Files:**
  - `src/convert.ts` - Main conversion engine and JiraRenderer class
  - `src/index.ts` - UI controller and event handling
  - `src/index.html` - Main HTML interface
  - `src/style.css` - Styling and layout

- **Build Files:**
  - `build.ts` - Production build configuration
  - `serve.ts` - Development server
  - `tsconfig.json` - TypeScript configuration

- **Test Files:**
  - `src/convert.test.ts` - Unit tests for conversion logic

## 11. Key Classes and Functions

- **JiraRenderer Class:**
  - Extends `marked` library's `Renderer` class
  - Converts Markdown elements to Jira markup
  - Key methods: `paragraph()`, `heading()`, `strong()`, `em()`, `code()`, `table()`

- **Main Functions:**
  - `convert(markdown: string): string` - Main conversion function
  - `html(markdown: string): string` - HTML preview generation
  - `convertHtmlToJira(html: string): string` - HTML to Jira conversion

## 12. Known Issues and Future Enhancements

- **Known Issues:**
  - Complex table formatting may not convert perfectly
  - Nested list handling limitations
  - Custom HTML in Markdown may not convert properly
  - Limited browser support for older versions

- **Future Enhancements:**
  - Enhanced table support
  - Custom language mapping for code blocks
  - Export/import functionality
  - Jira API integration
  - CLI tool development

## 13. Development Workflow

- **Making Changes:**
  1. Always run tests before committing: `bun test`
  2. Ensure TypeScript compilation passes
  3. Test in multiple browsers
  4. Update documentation for new features

- **Debugging:**
  - Use `verbose()` function to enable debug logging
  - Check browser console for errors
  - Test with various Markdown inputs

## 14. Glossary

- **Bun:** Fast JavaScript runtime and package manager used for development
- **GFM (GitHub Flavored Markdown):** Extended Markdown with tables, strikethrough, task lists
- **Jira Wiki Markup:** Atlassian's markup language for Jira formatting
- **Marked:** JavaScript library for parsing and rendering Markdown
- **Renderer:** Class defining how Markdown elements convert to output format
- **Syntax Highlighting:** Adding color/formatting to code blocks based on language

## 15. AI Agent Specific Guidelines

- **When Making Changes:**
  - Always preserve the existing architecture (client-side SPA)
  - Maintain backward compatibility with existing conversion logic
  - Test changes with various Markdown inputs
  - Update tests when modifying conversion logic

- **Code Quality Standards:**
  - Follow existing TypeScript patterns and conventions
  - Maintain the modular structure of the codebase
  - Ensure all new functions have proper type annotations
  - Add JSDoc comments for new public functions

- **Testing Requirements:**
  - Add unit tests for new conversion features
  - Test edge cases and error conditions
  - Ensure existing tests continue to pass
  - Consider performance implications of changes

- **Documentation Updates:**
  - Update this AGENTS.md file when adding new features
  - Update the main DOCUMENTATION.md for significant changes
  - Maintain accurate API documentation
  - Keep installation and setup instructions current

---

*This AGENTS.md file should be updated whenever significant changes are made to the project structure, dependencies, or development workflow.*




