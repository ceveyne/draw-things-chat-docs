# Changelog

Notable changes to this project will be documented in this file.

## Distribution

- **Source Code:** [LM Studio Hub](https://lmstudio.ai/ceveyne/draw-things-chat/revisions)
- **Documentation:** [GitHub Repository](https://github.com/ceveyne/draw-things-chat)

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
