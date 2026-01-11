# Change Log

All notable changes to the "chatledger-antigravity" extension will be documented in this file.

Check [Keep a Changelog](http://keepachangelog.com/) for recommendations on how to structure this file.

## [1.1.3] - 2026-01-11

### Fixed
- Code block detection for fences without language identifiers
  - Both ```` ``` ```` and ```` ```yaml ```` now correctly detected
  - Tracks fence length to ensure matching open/close
  - Strips trailing carriage returns before pattern matching

## [1.1.2] - 2026-01-11

### Added
- `Chatledger: Reset Built-in Sensitive Patterns to Default` command
  - Restores the 9 default patterns to User settings
  - Clears any Workspace overrides
  - Confirmation dialog before reset

## [1.1.1] - 2026-01-11

### Fixed
- Code block detection now properly handles all CommonMark cases:
  - Opening/closing fences must use the same character (` or ~)
  - Closing fence must not have content after it
  - Handles 3+ backticks/tildes and up to 3 spaces of indentation

### Added
- Toolbar buttons in Sensitive Content view: Export Now, List Conversations, Validate Patterns, Show Status
- Extension version displayed in view title (e.g., "Sensitive Content v1.1.1")
- Icons for toolbar commands

## [1.1.0] - 2026-01-11

### Added
- **Configuration Settings** (SPEC.md alignment)
  - `chatledger.censorSensitiveContent` - Enable/disable censoring of sensitive content
  - `chatledger.builtInSensitivePatterns` - Fully editable built-in patterns (9 defaults)
  - `chatledger.additionalSensitivePatterns` - User-defined supplemental patterns
  - `chatledger.scanCodeBlocks` - Toggle code block scanning for patterns
- `Chatledger: Validate Sensitive Content Patterns` command
- **Sensitive Content Censoring** (src/export/censorer.ts)
  - Length-preserved censoring with `***CENSORED***` format
  - Applied before writing files when `censorSensitiveContent` is enabled
  - Pattern validation function for validating regex patterns
- **Code Block Scanning Toggle**
  - Sanitizer and censorer now respect `scanCodeBlocks` setting
  - Skips content inside fenced code blocks when disabled
- **Activity Bar Sensitive Content View** (src/views/sensitiveContentView.ts)
  - Dedicated Activity Bar view with shield icon
  - Hierarchical tree showing files and line-level matches
  - Click file to open, click line to jump and select text
  - Badge showing count of files with sensitive content
  - Per-file and per-match dismiss functionality
  - Review state tracking (dismissed items persist during session)
  - Auto-refresh after export cycles
  - Empty state with helpful message and Export Now link

### Changed
- Replaced single `sensitivePatterns` setting with two-tier approach (builtIn + additional)
- Sensitive patterns now configurable from settings (previously hardcoded)
- Exporter now applies censoring before writing markdown files

## [1.0.0] - 2026-01-10

### Added
- Complete extension implementation per SPEC.md
- **Core Infrastructure**
  - Logger utility with timestamped output channel
  - Configuration manager for VS Code settings
  - TypeScript type definitions for API responses
- **Server Communication**
  - Cross-platform LSP server detector (Windows, Linux, macOS)
  - API client with connection caching and retry
- **Export System**
  - Content hasher for change detection (SHA256)
  - Markdown formatter for trajectory conversion
  - Sensitive content detector with built-in patterns
  - Export orchestrator with workspace filtering
  - Polling system with event detection
- **User Experience**
  - 10 commands in Command Palette
  - First-run onboarding experience
  - Configuration schema with settings
- **Unit Tests**
  - Formatter tests: sanitizeSummary, generateFilename, convertToMarkdown
  - Hasher tests: hash generation consistency and determinism
  - Sanitizer tests: sensitive pattern matching for API keys, passwords, tokens
- **Documentation**
  - Comprehensive README.md
  - Technical specification (SPEC.md)
  - Project guidance for AI assistants (CLAUDE.md)

### Fixed
- VS Code engine requirement lowered from ^1.107.0 to ^1.75.0 for compatibility
- Extension activation event changed from `workspaceContains:*` to `onStartupFinished`
- Path normalization for workspace filtering
- Sensitive content scanning runs after all exports complete (non-blocking)
