# Markdown to Jira Documentation

## 1. Project Overview

### Purpose and Goals
The **Markdown to Jira** project is a web-based tool designed to convert Markdown formatted text into Jira's wiki markup format. This tool addresses the common problem where users cannot directly paste Markdown content as comments in Jira, requiring manual conversion to Jira's specific markup syntax.

### Key Features
- **Real-time conversion**: Live preview of Markdown to Jira markup conversion
- **Comprehensive markup support**: Handles headings, lists, tables, code blocks, links, images, and more
- **Code syntax highlighting**: Supports multiple programming languages with proper syntax highlighting
- **GitHub Flavored Markdown (GFM)**: Full support for GFM features including tables, strikethrough, and task lists
- **Web-based interface**: Simple, responsive three-panel layout for input, preview, and output
- **Client-side processing**: No server required - all conversion happens in the browser

### Target Audience
- **Software developers** who need to document issues, pull requests, or technical specifications in Jira
- **Project managers** who want to maintain rich formatting when moving content between platforms
- **Technical writers** who work with both Markdown and Jira systems
- **Teams** that use Jira for issue tracking but prefer Markdown for documentation

## 2. Architecture and Design

### Overall Architecture
The project follows a **client-side single-page application (SPA)** architecture with the following characteristics:

- **Monolithic frontend**: All functionality contained within a single web application
- **No backend dependencies**: Pure client-side processing using modern web APIs
- **Modular TypeScript**: Well-structured modules with clear separation of concerns

### Major Components

#### Core Components
1. **Conversion Engine** (`convert.ts`)
   - Main conversion logic and Jira renderer
   - HTML to Jira markup transformation
   - Code block processing and syntax highlighting

2. **User Interface** (`index.html`, `index.ts`, `style.css`)
   - Three-panel layout (input, preview, output)
   - Real-time conversion with event listeners
   - Responsive design with CSS Grid

3. **Build System** (`build.ts`, `serve.ts`)
   - Bun-based build process
   - Development server with hot reloading
   - Production build optimization

#### Design Patterns
- **Custom Renderer Pattern**: Extends the `marked` library's `Renderer` class to create Jira-specific output
- **Strategy Pattern**: Different processing strategies for code blocks vs. regular content
- **Observer Pattern**: Event-driven UI updates for real-time conversion
- **Factory Pattern**: Language mapping for code syntax highlighting

### Module Interactions
```
index.ts (UI Controller)
    ↓
convert.ts (Conversion Engine)
    ↓
marked (Markdown Parser) + JiraRenderer (Custom Renderer)
    ↓
highlight.js (Syntax Highlighting)
```

## 3. Setup and Installation

### Prerequisites
- **Bun Runtime**: Version 1.2.11 or later
- **Node.js**: Version 18 or later (for compatibility)
- **Modern Web Browser**: Chrome, Firefox, Safari, or Edge with ES6+ support

### Installation Steps

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd markdown-to-jira
   ```

2. **Install dependencies**:
   ```bash
   bun install
   ```

3. **Start development server**:
   ```bash
   bun serve.ts
   ```

4. **Access the application**:
   Open your browser and navigate to the URL displayed in the terminal (typically `http://localhost:3000`)

### Building for Production

1. **Create production build**:
   ```bash
   bun build.ts
   ```

2. **Deploy the `dist/` folder** to your web server

### Dependencies

#### Core Dependencies
- **marked**: Markdown parsing and rendering
- **highlight.js**: Syntax highlighting for code blocks
- **@types/bun**: TypeScript definitions for Bun runtime
- **bun-types**: Bun-specific type definitions

#### Development Dependencies
- **TypeScript**: Type checking and compilation
- **Bun**: Runtime and build tool

## 4. Module/Component Documentation

### 4.1 Conversion Engine (`convert.ts`)

#### Purpose
The conversion engine is the core module responsible for transforming Markdown content into Jira wiki markup format.

#### Key Classes and Functions

##### `JiraRenderer` Class
**Purpose**: Custom renderer that extends the `marked` library's `Renderer` class to output Jira-specific markup.

**Key Methods**:
- `paragraph(text: string)`: Converts paragraphs to Jira format
- `heading(text: string, level: number)`: Converts headings (h1-h6) to Jira format
- `strong(text: string)`: Converts bold text to `*text*`
- `em(text: string)`: Converts italic text to `_text_`
- `code(code: string, lang: string)`: Converts code blocks with syntax highlighting
- `table(header: string, body: string)`: Converts tables to Jira table format
- `link(href: string, title: string, text?: string)`: Converts links to Jira format

##### `convert(markdown: string): string`
**Purpose**: Main conversion function that processes Markdown input and returns Jira markup.

**Input**: Raw Markdown string
**Output**: Jira wiki markup string
**Dependencies**: `marked` library, `JiraRenderer` class

**Usage Example**:
```typescript
import { convert } from './convert';

const markdown = "# Hello World\n\nThis is **bold** text.";
const jiraMarkup = convert(markdown);
// Output: "h1. Hello World\n\nThis is *bold* text."
```

##### `html(markdown: string): string`
**Purpose**: Converts Markdown to HTML for preview functionality.

**Input**: Raw Markdown string
**Output**: HTML string with syntax highlighting
**Dependencies**: `marked` library, `HTMLRenderer` class, `highlight.js`

##### `convertHtmlToJira(html: string): string`
**Purpose**: Converts HTML elements to Jira markup format.

**Input**: HTML string
**Output**: Jira wiki markup string

**Supported HTML Elements**:
- `<strong>`, `<b>` → `*text*`
- `<em>`, `<i>` → `_text_`
- `<u>` → `+text+`
- `<s>`, `<strike>`, `<del>` → `-text-`
- `<code>` → `{{text}}`
- `<pre>` → `{noformat}...{noformat}`
- `<blockquote>` → `{quote}...{quote}`
- `<h1-h6>` → `h1. text` through `h6. text`
- `<a href="url">text</a>` → `[text|url]`
- `<img src="url">` → `!url!`

#### Dependencies
- **Internal**: `processCodeBlockLines`, `fixCommentedCodeBlocks`, `fixDoubleUnderscore`
- **External**: `marked`, `highlight.js`

### 4.2 User Interface (`index.ts`)

#### Purpose
Handles user interactions and coordinates between input, preview, and output components.

#### Key Functions

##### Event Handling
```typescript
input.addEventListener('input', () => {
  output.value = convert(input.value);
  preview.innerHTML = html(input.value);
});
```

**Purpose**: Real-time conversion as user types in the input textarea.

##### Global API
```typescript
window.convert = convert;
```

**Purpose**: Exposes the conversion function globally for external use.

#### Dependencies
- **Internal**: `convert`, `html` from `./convert`
- **External**: DOM API

### 4.3 Build System

#### `build.ts`
**Purpose**: Production build configuration using Bun's build API.

**Features**:
- Entry point: `src/index.html`
- Minification enabled
- Source maps for debugging
- Browser target optimization

#### `serve.ts`
**Purpose**: Development server with hot reloading.

**Features**:
- Serves the main HTML file
- Automatic TypeScript compilation
- Hot reloading for development

### 4.4 Testing (`convert.test.ts`)

#### Purpose
Unit tests for the conversion engine, focusing on edge cases and specific formatting issues.

#### Test Coverage
- **Underscore handling**: Tests for proper escaping of `__` in various contexts
- **Code block processing**: Ensures code blocks maintain proper formatting
- **Bold text conversion**: Verifies `__bold__` converts to `*bold*`

#### Running Tests
```bash
bun test
```

## 5. API Documentation

### Public API

#### `convert(markdown: string): string`
Converts Markdown text to Jira wiki markup.

**Parameters**:
- `markdown` (string): Input Markdown text

**Returns**:
- `string`: Jira wiki markup

**Example**:
```typescript
const result = convert("# Header\n\n**Bold** text");
// Returns: "h1. Header\n\n*Bold* text"
```

#### `html(markdown: string): string`
Converts Markdown to HTML for preview.

**Parameters**:
- `markdown` (string): Input Markdown text

**Returns**:
- `string`: HTML with syntax highlighting

#### `verbose()`
Enables debug logging for the conversion process.

**Usage**:
```typescript
import { verbose } from './convert';
verbose(); // Enables console.log debugging
```

### Supported Markdown Features

#### Text Formatting
- **Bold**: `**text**` → `*text*`
- **Italic**: `*text*` → `_text_`
- **Strikethrough**: `~~text~~` → `-text-`
- **Inline code**: `` `code` `` → `{{code}}`

#### Headers
- `# Header 1` → `h1. Header 1`
- `## Header 2` → `h2. Header 2`
- `### Header 3` → `h3. Header 3`
- And so on through `h6.`

#### Lists
- **Unordered**: `- item` → `* item`
- **Ordered**: `1. item` → `# item`
- **Task lists**: `- [x] done` → `[x] done`

#### Code Blocks
- **Fenced code blocks**:
  ```markdown
  ```javascript
  console.log("Hello");
  ```
  ```
  Converts to:
  ```
  {code:language=javascript|borderStyle=solid|theme=RDark|linenumbers=true|collapse=false}
  console.log("Hello");
  {code}
  ```

#### Tables
- Converts Markdown tables to Jira table format with `||` for headers and `|` for cells

#### Links and Images
- **Links**: `[text](url)` → `[text|url]`
- **Images**: `![alt](url)` → `!url!`

## 6. Testing

### Testing Strategy
The project employs a **unit testing approach** using Bun's built-in test runner.

### Test Framework
- **Bun Test**: Native testing framework included with Bun
- **Test Location**: `src/convert.test.ts`
- **Coverage**: Focus on conversion logic and edge cases

### Running Tests
```bash
# Run all tests
bun test

# Run tests with verbose output
bun test --verbose

# Run specific test file
bun test convert.test.ts
```

### Test Categories
1. **Formatting Tests**: Verify correct conversion of text formatting
2. **Edge Case Tests**: Handle special characters and escaping
3. **Code Block Tests**: Ensure proper code block processing
4. **Integration Tests**: End-to-end conversion scenarios

### Test Examples
```typescript
test.it('should render _ correctly', () => {
  test.expect(convert("__bold__").trim()).toEqual("*bold*");
  test.expect(convert("my__key my__key").trim()).toEqual("my\\_\\_key my\\_\\_key");
});
```

## 7. Contribution Guidelines

### Development Workflow

#### Branching Strategy
- **Main branch**: `main` - production-ready code
- **Feature branches**: `feature/description` - new features
- **Bug fix branches**: `fix/description` - bug fixes

#### Pull Request Process
1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/your-feature`
3. **Make changes and test**: Ensure all tests pass
4. **Commit changes**: Use descriptive commit messages
5. **Push to your fork**: `git push origin feature/your-feature`
6. **Create a Pull Request**: Provide clear description of changes

#### Coding Standards

#### TypeScript Standards
- **Strict mode**: All code must pass TypeScript strict checks
- **Type annotations**: Explicit types for function parameters and returns
- **Interface definitions**: Use interfaces for object shapes
- **No `any` types**: Avoid `any` unless absolutely necessary

#### Code Style
- **Indentation**: 2 spaces
- **Line length**: Maximum 100 characters
- **Naming conventions**:
  - Functions: `camelCase`
  - Classes: `PascalCase`
  - Constants: `UPPER_SNAKE_CASE`
- **Comments**: JSDoc for public functions and classes

#### Commit Message Format
```
type(scope): description

- Use conventional commits format
- Types: feat, fix, docs, style, refactor, test, chore
- Scope: convert, ui, build, test
- Description: Clear, concise description
```

#### Example Commit Messages
```
feat(convert): add support for task lists
fix(ui): resolve responsive layout issues
docs(readme): update installation instructions
```

### Code Review Process
1. **Automated checks**: All tests must pass
2. **Code review**: At least one reviewer approval required
3. **Documentation**: Update documentation for new features
4. **Backward compatibility**: Ensure changes don't break existing functionality

## 8. Known Issues and Future Enhancements

### Known Issues

#### Current Limitations
1. **Complex table formatting**: Some complex table structures may not convert perfectly
2. **Nested list handling**: Deep nesting in lists may not render correctly
3. **Custom HTML**: Raw HTML in Markdown may not convert to proper Jira markup
4. **Image sizing**: No support for image dimensions in Jira markup
5. **Code block language detection**: Limited language mapping for syntax highlighting

#### Browser Compatibility
- **Internet Explorer**: Not supported (requires modern ES6+ features)
- **Older mobile browsers**: May have limited functionality

### Future Enhancements

#### Planned Features
1. **Enhanced table support**: Better handling of complex table structures
2. **Custom language mapping**: User-configurable language mappings for code blocks
3. **Export functionality**: Save converted content to files
4. **Import functionality**: Load Markdown files from disk
5. **Theme customization**: Multiple UI themes and color schemes
6. **Keyboard shortcuts**: Hotkeys for common operations
7. **Undo/redo functionality**: History management for conversions
8. **Batch processing**: Convert multiple files at once

#### Technical Improvements
1. **Performance optimization**: Lazy loading for large documents
2. **Accessibility improvements**: Better screen reader support
3. **Mobile responsiveness**: Enhanced mobile interface
4. **Offline support**: Service worker for offline functionality
5. **Plugin system**: Extensible architecture for custom renderers

#### Integration Features
1. **Jira API integration**: Direct posting to Jira issues
2. **GitHub integration**: Import from GitHub issues and pull requests
3. **CLI tool**: Command-line interface for batch processing
4. **Browser extension**: Chrome/Firefox extension for direct conversion
5. **VS Code extension**: Integration with popular code editors

## 9. Glossary

### Technical Terms

#### **Bun**
A fast JavaScript runtime and package manager that serves as the primary development tool for this project.

#### **GFM (GitHub Flavored Markdown)**
An extension of standard Markdown that adds features like tables, strikethrough, and task lists.

#### **Jira Wiki Markup**
Atlassian's proprietary markup language used in Jira for formatting text, similar to but distinct from standard Markdown.

#### **Marked**
A popular JavaScript library for parsing and rendering Markdown content.

#### **Renderer**
A class that defines how parsed Markdown elements should be converted to output format (HTML or Jira markup).

#### **Syntax Highlighting**
The process of adding color and formatting to code blocks based on the programming language.

### Markup Terms

#### **Code Block**
A section of text that represents code, typically enclosed in triple backticks (```) in Markdown.

#### **Fenced Code Block**
A code block that includes language specification for syntax highlighting.

#### **Wiki Markup**
A lightweight markup language used in wikis and collaboration platforms for formatting text.

#### **Jira Macros**
Special Jira-specific formatting codes like `{code}`, `{quote}`, `{noformat}` that provide rich formatting options.

---

*This documentation is maintained alongside the codebase and should be updated when new features are added or existing functionality changes.*
