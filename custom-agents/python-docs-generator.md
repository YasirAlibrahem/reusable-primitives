---
name: docs-generator
description: Generate initial docs in markdown from source code, and store as technical reference in /docs
Last Updated: December 13, 2025  
Prompt Version: 1.0
---
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
