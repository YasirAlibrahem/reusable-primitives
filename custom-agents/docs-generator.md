---
name: docs-generator
description: Generate initial docs in markdown from source code, and store as technical reference in /docs
---

# Prompt for Generating Reference Documentation

This document contains a comprehensive prompt template for generating technical reference documentation for Python packages. Use this prompt with AI coding agents (Claude, Copilot, Codex, etc.) to automatically generate high-quality reference documentation.

---

## The Prompt

```markdown
# Generate Python Package Reference Documentation

Create a comprehensive reference document for this Python package/library following these requirements:

## Document Structure

1. **Filename:** `REFERENCE.md` (or `API.md` if preferred)

2. **Front Matter:**
   - Document title
   - Current package version (extract from `__about__.py` or `pyproject.toml`)
   - Last updated date
   - Brief description of the package
   - Table of contents with deep links

3. **Organization:** Group by logical categories:
   - Core/Main classes (primary user-facing classes)
   - Supporting classes (helper/utility classes)
   - Data classes and types
   - Exception classes (all custom exceptions)
   - Module-level functions (standalone utility functions)
   - Constants and module-level variables
   - Type aliases and type definitions
   - Enums (if applicable)
   - Decorators (if custom decorators exist)
   - Protocols/Abstract Base Classes

## What to Document

### For Each Class:
- **Location:** Module path
- **Description:** What the class does and when to use it
- **Inheritance:** Parent classes
- **Constructor signature:** Full signature with type hints
- **Constructor parameters:** Name, type, description, default values, whether optional/required
- **Class attributes:** Static/class-level variables
- **Properties:** Getters/setters with types
- **Methods:** 
  - Signature with full type hints
  - Parameters with types and descriptions
  - Return type and description
  - Raises: All exceptions that can be raised
  - Deprecation warnings if applicable
- **Usage example:** Short code snippet showing typical usage

### For Each Function:
- **Location:** Module path
- **Signature:** Full signature with type hints
- **Description:** What it does
- **Parameters:** Name, type, description, defaults
- **Returns:** Type and description
- **Raises:** All exceptions
- **Usage example:** If non-trivial

### For Each Exception:
- **Inheritance chain:** What it inherits from
- **When raised:** Conditions that trigger it
- **How to handle:** Best practices for catching/handling

### For Constants/Module Variables:
- **Type:** Type annotation
- **Value:** Current/default value
- **Purpose:** When/how to use it

## Formatting Requirements

1. **Code blocks:** Use proper Python syntax highlighting with triple backticks
2. **Type hints:** Include all type hints exactly as in source code
3. **Links:** Use relative links to source files with line numbers: `[ClassName](path/to/file.py#L123)`
4. **Hierarchy:** Use proper markdown heading levels (##, ###, ####)
5. **Optional parameters:** Clearly mark with "optional" and show defaults
6. **Deprecations:** Add "⚠️ **Deprecated:**" warnings prominently
7. **Visibility:** Document only PUBLIC interfaces (no leading underscore methods)
   - Exception: Briefly mention common internal utilities if relevant for advanced users
8. **Cross-references:** Link related classes/functions to each other

## Content Guidelines

1. **Be precise:** Use exact type hints from the code
2. **Be complete:** Don't skip parameters or return types
3. **Be consistent:** Use same formatting throughout
4. **Be helpful:** Add context about when to use each component
5. **Show relationships:** Explain how classes interact
6. **Include examples:** At least one per major class/function
7. **Note side effects:** Mention if methods mutate state, do I/O, etc.
8. **Performance hints:** Note if certain operations are expensive

## Additional Sections to Include

1. **Installation:** How to install the package and optional dependencies
2. **Quick Start:** Minimal example showing common use cases
3. **Usage Patterns:** Common patterns and idioms
4. **Best Practices:** Recommended approaches
5. **CLI Reference:** If the package has a command-line interface
6. **Custom Extensions:** How to create plugins/extensions if supported
7. **Migration Guide:** If there are deprecated features
8. **See Also:** Links to README, contributing guide, examples
9. **Version Compatibility:** Python version requirements
10. **Optional Dependencies:** What features require which extras

## Source Analysis

Analyze the following to extract information:
1. All Python files in `src/` or main package directory
2. Parse docstrings (Google, NumPy, or Sphinx style)
3. Extract type hints from function signatures
4. Read `__all__` exports to identify public API
5. Check `__init__.py` for package-level exports
6. Read version from `__about__.py`, `__version__.py`, or `pyproject.toml`
7. Identify abstract methods (ABC, NotImplementedError)
8. Find all custom exception classes
9. Extract constants (UPPERCASE variables)
10. Look for CLI entry points in `pyproject.toml` or `setup.py`

## Exclusions

Do NOT document:
- Private methods/functions (leading underscore) unless critically important
- Test files
- Internal implementation details
- Third-party dependencies' APIs (only mention required dependencies)
- Build/packaging files

## Output Format

Generate valid Markdown that:
- Renders properly on GitHub
- Has working anchor links in TOC
- Has consistent indentation
- Uses code fences for all code
- Escapes special markdown characters appropriately
- Has proper line breaks between sections

## Example Entry Template

### Class Documentation Template

```markdown
### `ClassName`

**Module:** `package.module`  
**Inherits:** `ParentClass`

Brief description of what this class does and when to use it.

#### Constructor

\```python
ClassName(
    required_param: str,
    optional_param: int = 10,
    *,
    keyword_only: bool = False
)
\```

**Parameters:**
- `required_param` (str): Description of required parameter
- `optional_param` (int, optional): Description. Default: `10`
- `keyword_only` (bool, optional): Description. Default: `False`

**Raises:**
- `ValueError`: If parameter validation fails

**Example:**
\```python
obj = ClassName("value", optional_param=20)
\```

#### Attributes

- `public_attr` (str): Description of attribute
- `another_attr` (int): Another attribute

#### Properties

##### `property_name` (type)

Description of what this property represents.

**Getter:** Returns the property value  
**Setter:** Sets the property value (if applicable)

#### Methods

##### `method_name(param: type) -> ReturnType`

Description of what the method does.

**Parameters:**
- `param` (type): Parameter description

**Returns:**
- `ReturnType`: Description of return value

**Raises:**
- `ExceptionType`: When this is raised

**Example:**
\```python
obj = ClassName("value")
result = obj.method_name("param")
\```
```

### Function Documentation Template

```markdown
### `function_name(param1: type1, param2: type2 = default) -> ReturnType`

**Module:** `package.module`

Brief description of what the function does.

**Parameters:**
- `param1` (type1): Description of first parameter
- `param2` (type2, optional): Description of second parameter. Default: `default`

**Returns:**
- `ReturnType`: Description of what is returned

**Raises:**
- `ValueError`: When parameters are invalid
- `IOError`: When I/O operations fail

**Example:**
\```python
result = function_name("value", param2=100)
print(result)
\```
```

### Exception Documentation Template

```markdown
### `ExceptionName`

**Module:** `package.exceptions`  
**Inherits:** `ParentException`

Description of when and why this exception is raised.

#### Constructor

\```python
ExceptionName(message: str, *, context: Optional[dict] = None)
\```

**Parameters:**
- `message` (str): Error message
- `context` (dict, optional): Additional context information

**Example:**
\```python
try:
    risky_operation()
except ExceptionName as e:
    print(f"Error: {e}")
    # Handle the error
\```
```

## Specific Instructions for This Codebase

Analyze the package structure starting from the main `src/` directory. Pay special attention to:
- Entry points defined in the main `__init__.py` file
- Abstract base classes that define the public interface
- Plugin systems or extension points
- Configuration options and their defaults
- CLI commands and arguments
- Relationship between classes (which classes work together)
- Common usage patterns in docstrings or examples

## Quality Checklist

Before finalizing, ensure:
- [ ] All public classes are documented
- [ ] All public methods have parameter descriptions
- [ ] All return types are specified
- [ ] All exceptions are documented
- [ ] Code examples compile and make sense
- [ ] Links to source code are correct
- [ ] Table of contents has working links
- [ ] Consistent formatting throughout
- [ ] No placeholder text (TODO, FIXME, etc.)
- [ ] Deprecation warnings are prominent
- [ ] Optional dependencies are clearly marked

## Generate the Documentation

Generate the complete reference documentation now, following all the guidelines above.
```

---

## Usage Instructions

### With Claude/Copilot in VS Code

1. Open the repository in VS Code
2. Open GitHub Copilot Chat
3. Copy the prompt above
4. Add: "Analyze this workspace and generate REFERENCE.md"
5. Review and refine the generated documentation

### With Command Line Tools

```bash
# Using aider
aider --message "$(cat docs/PROMPT_REFERENCE_GENERATION.md)"

# Using fabric (with ai pattern)
cat docs/PROMPT_REFERENCE_GENERATION.md | fabric --pattern generate-docs

# Using llm CLI
llm "$(cat docs/PROMPT_REFERENCE_GENERATION.md)\n\nAnalyze the files in src/"
```

### With GitHub Copilot Workspace

1. Create a new issue with the title: "Generate technical reference documentation"
2. In the issue body, paste the prompt
3. Assign to Copilot coding agent
4. Review the generated PR

---

## Customization Tips

### For Different Project Types

**Web API (FastAPI/Flask):**
- Add "Endpoints" section with route documentation
- Include request/response schemas
- Add authentication/authorization details

**CLI Tools:**
- Expand CLI Reference section
- Add subcommand documentation
- Include configuration file format

**Libraries with Complex Configuration:**
- Add "Configuration" section
- Document environment variables
- Include configuration file examples

**Data Science/ML Libraries:**
- Add "Model Architecture" section
- Document data formats and schemas
- Include performance benchmarks

### For Large Codebases

Add these sections:
```markdown
## Architecture Overview
- High-level component diagram
- Design patterns used
- Module dependencies

## Performance Considerations
- Time complexity for key operations
- Memory usage guidelines
- Optimization tips

## Advanced Topics
- Concurrency and thread safety
- Extending the library
- Internal APIs (for contributors)
```

### For Multi-Language Projects

```markdown
## Language Bindings
- Python API reference (this document)
- JavaScript/TypeScript bindings
- Rust FFI interface
- C/C++ wrapper API
```

---

## Maintenance

### Keeping Documentation Current

1. **Automation:** Set up CI to detect API changes
2. **Version tagging:** Include version info in each release
3. **Changelog integration:** Link to CHANGELOG.md
4. **Regular audits:** Review quarterly for accuracy

### CI Integration Example

```yaml
# .github/workflows/docs-check.yml
name: Check Documentation

on: [pull_request]

jobs:
  check-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Check if API changed
        run: |
          # Compare __init__.py __all__ with documented items
          python scripts/check_docs_coverage.py
```

---

## Related Documentation Standards

- **PEP 257:** Docstring Conventions
- **PEP 484:** Type Hints
- **Google Style Guide:** Python docstrings
- **NumPy Style Guide:** NumPy/SciPy documentation
- **Sphinx:** reStructuredText documentation
- **MkDocs:** Material for MkDocs conventions

---

## Tools for Auto-Generation

- **pdoc3:** Automatic API documentation from docstrings
- **Sphinx:** Comprehensive documentation generation
- **mkdocstrings:** MkDocs plugin for API docs
- **pydoc-markdown:** Convert docstrings to Markdown
- **LLM-based tools:** Claude, GPT-4, Copilot for AI-assisted generation

---

## Examples of Good Reference Documentation

- [requests](https://docs.python-requests.org/en/latest/api/) - Simple, clear API docs
- [Django](https://docs.djangoproject.com/en/stable/ref/) - Comprehensive reference
- [pytest](https://docs.pytest.org/en/stable/reference.html) - Well-organized
- [NumPy](https://numpy.org/doc/stable/reference/) - Detailed mathematical docs
- [FastAPI](https://fastapi.tiangolo.com/reference/) - Modern API documentation

---

**Last Updated:** December 13, 2025  
**Prompt Version:** 1.0
