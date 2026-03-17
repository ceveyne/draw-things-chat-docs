# Changelog

Notable changes to this project will be documented in this file.

## Distribution

- **Source Code:** [LM Studio Hub](https://lmstudio.ai/ceveyne/draw-things-chat/revisions)
- **Documentation:** [GitHub Repository](https://github.com/ceveyne/draw-things-chat-docs)

---

## [0.1.0] - 2026-03-17 Revision 26

## Added

- Added support for FLUX.2 [klein] 9B KV to the Flux model family (`flux-2-klein`).

### Changed

- Normalize default models to 6-bit versions.
- Raised `numFrames` maximum from 257 to 641 (25,6 s).

---

## [0.1.0] - 2026-03-15 Revision 25

### Fixed

- Fixed pre-build issues.

---

## [0.1.0] - 2026-03-15 Revision 24

### Changed

- Improved plugin runtime stability.

---

## [0.1.0] - 2026-03-15 Revision 23

### Changed

- Updated USER_GUIDE.md.

---

## [0.1.0] - 2026-03-15 Revision 22

### Added

- Added support for LTX-2.3 (22B) to the LTX model family (`ltx-2.3-distilled`, `ltx-2.3-dev`).

### Changed

- Improved gRPC-connection logging.

---

## [0.1.0] - 2026-03-14 Revision 21

### Added

- Added support for LTX-2 video generation.
- Added LTX-2 q6p model variants (`ltx_2_19b_distilled_q6p.ckpt`, `ltx_2_19b_dev_q6p.ckpt`) as new defaults.

---

## [0.1.0] - 2026-03-13 Revision 20

### Fixed

- Fixed `imageFormat` override not being resolved to explicit width/height before canvas normalization in `image2image`, `edit`, and `image2video` modes.

---

## [0.1.0] - 2026-03-13 Revision 19

### Fixed

- Fixed `normalizeInputBuffer` distortion bug: input images were improperly scaled, causing stretched/distorted results when the requested format differed from the source (e.g. portrait canvas → landscape output).

---

## [0.1.0] - 2026-03-13 Revision 17, 18

### Changed

- Changed pre-build process for LM Studio Hub.

---

## [0.1.0] - 2026-03-11 Revision 16

### Fixed

- Fixed TCD Trailing sampler support.

### Changed

- Qwen3.5 now uses the correct thinking-mode flag.
- Progress messages during generation are more informative.

---

## [0.1.0] - 2026-02-26 Revision 15

### Changed

- Added support for Qwen3.5 35B (qwen/qwen3-vl-30b) as the new default LLM.

---

## [0.1.0] - 2026-02-06 Revision 14

### Changed

- Slightly increased `steps` in `edit` mode for better prompt adherence with qwen-image.
- Minimal adjustments to generation defaults.
- Improved Brave Web Search result rendering.

---

## [0.1.0] - 2026-02-01 Revision 13

### Changed

- Optimized model-mapping-snapshot for [draw_things_index](https://lmstudio.ai/ceveyne/draw-things-index/)
- Updated user documentation

---

## [0.1.0] - 2026-01-30 Revision 12

### Changed

- `image2image` mode now supports moodboard (multiple reference images) via gRPC transport, same as `edit` mode.
- `current conversation tokens` now refers to the agent model's context length

---

## [0.1.0] - 2026-01-29 Revision 11

### Added

- New screenshots to reflect the recent update to LM Studio 0.4.0.

### Changed

- `index_image` tool calls no longer echo the model-mapping snapshot JSON back into the agent-model prompt history (token economy), while still passing the snapshot through to the index tool itself.

### Fixed

- README: Updated the model table to show flux_2_klein_9b_q6p.ckpt for all three modes (instead of just text2image with flux_1). Removed the "—" (not supported) markers for Flux.

---

## [0.1.0] - 2026-01-26 Revision 10

### Added

- Tool summaries now show the model family label alongside the exact Draw Things model file that was used, so you can quickly understand what re-generates an image.
- Added `review_image()` tool: lets you ask the agent to re-view any earlier created image, variant, or picture from tool-based generation or research.

### Changed

- Increased TTL for "vision-capability-priming" to 120 minutes.
- Updated user documentation to reflect the recent changes.

### Fixed

- Fixed draw-things-index `index_image` markdown table: `model_display` is now preserved through picture materialization.

---

## [0.1.0] - 2026-01-23 Revision 9

### Added

- Added `FLUX.2 [klein] 9B (6-bit)` as new default for flux model family.

### Fixed

- Fixed Agent behavior to avoid generations or tool results being displayed more than just once.

---

## [0.1.0] - 2026-01-22 Revision 8

### Changed

- Improved source display in draw-things-index results: Chat IDs now shown completely, project names displayed without file extension.

### Fixed

- Fixed picture indexing across multiple queries: `p` IDs are now stable per chat (including draw-things-index `project://` results) and no longer reset to `p1` per query.

---

## [0.1.0] - 2026-01-22 Revision 7

### Fixed

- Fixed a build issue that excluded necessary files from being uploaded.

---

## [0.1.0] - 2026-01-22 Revision 6

### Changed

- Improved Project file parser for [draw-things-index](https://lmstudio.ai/ceveyne/draw-things-index/).

---

## [0.1.0] - 2026-01-21 Revision 5

### Added

- Added support for [draw_things_index](https://lmstudio.ai/ceveyne/draw-things-index/), an LM Studio Plugin to search through your Draw Things generation history.

### Changed

- Changed Agent behavior to avoid generations or tool results being displayed more than just once.

### Fixed

- Small stability and error handling improvements.

---

## [0.1.0] - 2026-01-18 Revision 4

### Added

- Live progress indicator (gRPC only): Image generation now shows real-time progress (e.g., "Step 12/20 (60%)") in the chat UI.
- Explicit hard limit validation for width/height parameters with clear error messages.

### Changed

- Width/height descriptions now include maximum allowed values.
- Dimension validation: Requests exceeding limits are now rejected with actionable error messages instead of silent clamping.

### Fixed

- HTTP img2img: imageFormat shorthand no longer overrides explicitly provided width/height values (parity with gRPC).

---

## [0.1.0] - 2026-01-17 Revision 3

### Changed

- Adjusted number of attachments for Vision Promotion to match LM Studio client limit (5 images max).
- Updated user documentation.

### Fixed

- Generation progress now shown consistently for all modes (text-to-image, image-to-image, and edit).

---

## [0.1.0] - 2026-01-16 Revision 2

### Changed

- Unified preview sizes across all media types for consistent display quality.

---

## [0.1.0] - 2026-01-16 Revision 1

### Added

- Initial release
- Draw Things integration with vision-capable agents
- Support for text-to-image, image-to-image, and edit modes
- Vision Promotion system for attachments and generated images
- Orchestrator-based agent architecture
- Tool-based image generation interface
