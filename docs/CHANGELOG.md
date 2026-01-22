# Changelog

Notable changes to this project will be documented in this file.

## Distribution

- **Source Code:** [LM Studio Hub](https://lmstudio.ai/ceveyne/draw-things-chat/revisions)
- **Documentation:** [GitHub Repository](https://github.com/ceveyne/draw-things-chat-docs)

---

## [0.1.0] - 2026-01-22 Revision 8

### Changed

- Improved source display in draw-things-index results: Chat IDs now shown completely, project names displayed without file extension

### Fixed

- Fixed picture indexing across multiple queries: `p` IDs are now stable per chat (including draw-things-index `project://` results) and no longer reset to `p1` per query.

---

## [0.1.0] - 2026-01-22 Revision 7

### Fixed

- Fixed a build issue that excluded necessary files from being uploaded

---

## [0.1.0] - 2026-01-22 Revision 6

### Changed

- Improved Project file parser for [draw-things-index](https://lmstudio.ai/ceveyne/draw-things-index/)

---

## [0.1.0] - 2026-01-21 Revision 5

### Added

- Added support for [draw_things_index](https://lmstudio.ai/ceveyne/draw-things-index/), an LM Studio Plugin to search through your Draw Things generation history.

### Changed

- Changed Agent behavior to avoid generations or tool results being displayed more than just once

### Fixed

- Small stability and error handling improvements

---

## [0.1.0] - 2026-01-18 Revision 4

### Added

- Live progress indicator (gRPC only): Image generation now shows real-time progress (e.g., "Step 12/20 (60%)") in the chat UI
- Explicit hard limit validation for width/height parameters with clear error messages

### Changed

- Width/height descriptions now include maximum allowed values
- Dimension validation: Requests exceeding limits are now rejected with actionable error messages instead of silent clamping

### Fixed

- HTTP img2img: imageFormat shorthand no longer overrides explicitly provided width/height values (parity with gRPC)

## [0.1.0] - 2026-01-17 Revision 3

### Changed

- Adjusted number of attachments for Vision Promotion to match LM Studio client limit (5 images max)
- Updated user documentation

### Fixed

- Generation progress now shown consistently for all modes (text-to-image, image-to-image, and edit)

---

## [0.1.0] - 2026-01-16 - Revision 2

### Changed

- Unified preview sizes across all media types for consistent display quality

---

## [0.1.0] - 2026-01-16 - Revision 1

### Added

- Initial release
- Draw Things integration with vision-capable agents
- Support for text-to-image, image-to-image, and edit modes
- Vision Promotion system for attachments and generated images
- Orchestrator-based agent architecture
- Tool-based image generation interface
