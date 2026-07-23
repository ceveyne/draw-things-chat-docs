# Changelog

Notable changes to this project will be documented in this file.

## Distribution

- **Source Code:** [LM Studio Hub](https://lmstudio.ai/ceveyne/draw-things-chat)
- **Documentation:** [GitHub Repository](https://github.com/ceveyne/draw-things-chat-docs)

---

## [0.1.48] - 2026-07-23 Revision 48

### Added

- Added support for image tags. When using the [find-image plugin](https://lmstudio.ai/ceveyne/find-image), images can now be tagged and found with exact tag filters.

---

## [0.1.47] - 2026-07-18 Revision 47

### Changed

- Picture search results now include their usable `pN` indexes.
- Improved tool descriptions

---

## [0.1.46] - 2026-07-17 Revision 46

### Changed

- New image previews now stay compact and consistent across chat media.
- Image previews now use their actual file type when sent to vision models.

### Fixed

- Images found via other tools (e.g. **find-image**) could incorrectly show up as newly generated variants.

---

## [0.1.45] - 2026-07-09 Revision 45

### Added

- Added support for new plugin **find-image**.

---

## [0.1.44] - 2026-06-26 Revision 44

### Added

- Added support for **analyse-image** version `0.1.10 (Revision 10)` or higher.

---

## [0.1.43] - 2026-06-20 Revision 43

### Fixed

- Restored automatic chat display of generated Images (`iN`) when `PREVIEW_IN_CHAT=false`; `image-*-iN` results are again injected into the agent turn without requiring inline preview output from the generator plugin.

---

## [0.1.42] - 2026-06-17 Revision 42

### Fixed

- External `generate_image` results (backend → Images, `image-*-iN.png`) were injected twice into the chat: once by the unified harvest/toolParams pipeline (Block 1, correct) and once by the legacy per-image injection path (Block 2, duplicate). Block 2 now filters `basenamesRaw` to `generated-image-*` filenames only; `image-*-iN.png` files are exclusively handled by Block 1.

---

## [0.1.41] - 2026-06-02 Revision 40/41

### Added

- Inference timing metrics are now logged after each generation: `[Timing] TTFT: X.XX s | Y.Y tok/sec | N tokens | Total: Z.ZZ s`. TTFT (Time to First Token) measures prompt-processing duration; tok/sec measures generation speed. Both values are written to the plugin log and enable direct backend comparison (MLX vs. GGUF) under identical conditions.
- Substring alias matching added to the capability registry: model IDs containing `qwen36-27b` or `qwen36-35b` now resolve to the corresponding Qwen 3.6 capability entry, enabling derivative / fine-tuned model names to be recognised automatically.

### Changed

- Removed the undici `bodyTimeout` / `headersTimeout` limits (set to `0`) so long prompt-processing phases — where the server is computing but has not yet emitted the first streaming token — no longer cause a `UND_ERR_BODY_TIMEOUT` abort. The OpenAI SDK client timeout is also set to `0` for the same reason.

---

## [0.1.39] - 2026-05-22 Revision 39

### Fixed

- Progress feedback during the 2nd pass (SeedVR2 refinement) in `mode: edit` now correctly shows `2nd Pass Step x/y (n%)` instead of bare `Step x/y`.
- Audit: `render_target.needs_upscaler` now reflects the actual zoom-pass decision instead of being recomputed from raw dimensions versus backend limits.
- Moodboard images in `mode: edit` with explicit `width`/`height` parameters now preserve their native aspect ratio. Previously, explicit output dimensions caused moodboard inputs to adopt the output AR; only the canvas image should adopt the output AR.

---

## [0.1.38] - 2026-05-20 Revision 38

### Changed

- Maximum render dimensions increased to 2048 × 2048 px (`maxWidth`, `maxHeight`).
- Agent model loading now supports any OpenAI-compatible server. When the LM Studio-proprietary `/api/v1/models` endpoint is unavailable (e.g. Unsloth Studio, RunPod vLLM, remote inference APIs), the plugin falls back to the standard `/v1/models` endpoint and validates the model against the capability registry (`capabilities.ts`). Vision gating is enforced via the registry in both paths.

### Fixed

- Zoom-pass audit: `inputs.canvas.original` now correctly shows the Pass-1 `backend_returned` dimensions instead of the pre-Pass-1 source canvas.
- Zoom pass (SeedVR2 refinement) now participates correctly in the overlay/Custom Config system. The overlay mode is resolved as `zoom` instead of falling through to `img2img`, and the model identifier is passed as `undefined` (not the raw `.ckpt` filename) so no false-alarm `Unknown model` error is logged. Custom Configs keyed `zoom.auto` are respected.

---

## [0.1.37] - 2026-05-15 Revision 37

### Changed

- Preview size for all MediaTypes (attachments, images, variants, pictures) unified under a single constraint (`w + h ≤ 1792 px`). One preview file now serves chat display, Vision Promotion, `analyse_image`, and `detect_object` consistently.
- Updated [deployment scenarios](./DEPLOYMENT.md).

---

## [0.1.36] - 2026-05-15 Revision 36

### Fixed

- Vision capability check now falls back to `capabilities.ts` when the server API does not return capability metadata (e.g. headless/remote servers with a plain `/v1/models` response). Models registered in `capabilities.ts` with `supportsVision: true` are now correctly accepted as agent models in such configurations.

---

## [0.1.35] - 2026-05-11 Revision 35

### Changed

- Switched default `edit` mode parameters from Qwen-Image to Flux.
- Improved agent instruction for Vision Promotion.

---

## [0.1.34] - 2026-05-07 Revision 34

### Added

- Added SeedVR2 model family support.
- Upscale pipeline extended with a 2-pass upscale process (Pass 1: standard generation; Pass 2: SeedVR2 refinement at target resolution).
- Added `upscale` tool.
- Added deployment scenario examples to the setup guide.

### Changed

- Updated vision instruction to improve accurate and detailed visual description.
- `i2iProfileUsed` audit type extended to include `"refine"`.
- Updated "LTX-2.3 22B [distilled] (6-bit)", "file": "ltx_2.3_22b_distilled_q6p.ckpt" to "LTX-2.3 22B [distilled] 1.1 (6-bit)", "file": "ltx_2.3_22b_distilled_1.1_q6p.ckpt"

### Fixed

- `normalizeInputBuffer` sum-constraint step: aspect ratio was distorted when both axes happened to land on different 64-multiples after independent rounding. Now uses candidate-based selection to best-preserve aspect ratio.

---

## [0.1.33] - 2026-04-27 Revision 33

### Added

- Added 8-bit S (`i8x`) model filenames for Z Image Turbo 1.0, Z Image Base 1.0, Qwen Image 2512, Qwen Image Edit 2511, FLUX.2 [klein] 4B, and LTX-2.3 (distilled, dev).
- Added support for Qwen3.6 27B (qwen/qwen3.6-27b).

### Changed

- Custom Configs now accept arbitrary postfixes (`text2image.<any>`, `image2image.<any>`, `edit.<any>`, `text2video.<any>`, `image2video.<any>`). Previously only the built-in model IDs (`auto`, `z-image`, `qwen-image`, `flux`, `ltx`, `custom`) were accepted as postfixes.

---

## [0.1.32] - 2026-04-21 Revision 32

### Changed

- Added support for Qwen3.6 35B АЗВ (qwen/qwen3.6-35b-a3b) as the new default LLM.
- `generate_image`: When a reference image (`canvas`) is provided but `mode` is omitted, the call now fails with an actionable error asking the user to specify `image2image`, `edit`, or `image2video` — instead of silently falling back to text-to-image.
- Improved support for external / 3rd-party image generation plugins.

### Fixed

- Images generated by 3rd-party-plugins in sequence now each get a unique index (`i1`, `i2`, `i3` …) — the counter no longer resets to `i1` on every turn.

---

## [0.1.31] - 2026-04-08 Revision 31

### Added

- Added additional guardrails for multiple tool-calls within an ongoing thinking process.
- Updated the [USER_GUIDE.md](USER_GUIDE.md#setup) with best practices for providing instructions based on my new tool: [playbook](https://lmstudio.ai/ceveyne/playbook).

---

## [0.1.30] - 2026-04-07 Revision 30

### Added

- Added Google Gemma 4 26B capabilities.
- Added an option to embed generation metadata in generated images.
- Added Post-Thinking Intercept: tool calls inside a reasoning/thinking block are now detected and executed correctly after the block closes.

### Changed

- Bumped `flatbuffers` dependency from `24.12.23` to `25.9.23`.

### Fixed

- Fixed race condition, which could cause attachments not to be imported correctly.
- Fixed Re-Generate recovery: when the user regenerates an AI response after attaching an image, the agent now correctly re-sees the attachment instead of silently omitting it.

---

## [0.1.29] - 2026-04-01 Revision 29

### Added

- Added Qwen3.5 9B capabilities.

### Changed

- Adjustments to progress display during image generation.
- Context-window warning threshold lowered from 32,768 to 16,384 tokens.
- Match Swift Int() truncation semantics — use Math.floor instead of Math.round when converting float tensors to uint8 pixels.
- `nextVariantV` counter is preserved when all image turns are deleted, so variant identifiers are never reused across generations.

---

## [0.1.28] - 2026-03-22 Revision 28

### Fixed

- Custom Config video presets (`image2video.custom`, `text2video.custom`) now correctly resolve the active model.

---

## [0.1.27] - 2026-03-21 Revision 27

### Added

- Added an option to auto-unload idleling agent-model on komplex `text2video` or `video2video` tool-calls.

### Changed

- Raised maximum `variants` from 3 to 4 (`text2image`, `image2image`, `edit`). Vision Promotion window expanded accordingly.
- Video encoding (`.mov` assembly) is restricted to `text2video` and `image2video` modes.

---

## [0.1.26] - 2026-03-17 Revision 26

### Added

- Added support for FLUX.2 [klein] 9B KV to the Flux model family (`flux-2-klein`).

### Changed

- Normalize default models to 6-bit versions.
- Raised `numFrames` maximum from 257 to 641 (25,6 s).

---

## [0.1.25] - 2026-03-15 Revision 25

### Fixed

- Fixed pre-build issues.

---

## [0.1.24] - 2026-03-15 Revision 24

### Changed

- Improved plugin runtime stability.

---

## [0.1.23] - 2026-03-15 Revision 23

### Changed

- Updated USER_GUIDE.md.

---

## [0.1.22] - 2026-03-15 Revision 22

### Added

- Added support for LTX-2.3 (22B) to the LTX model family (`ltx-2.3-distilled`, `ltx-2.3-dev`).

### Changed

- Improved gRPC-connection logging.

---

## [0.1.21] - 2026-03-14 Revision 21

### Added

- Added support for LTX-2 video generation.
- Added LTX-2 q6p model variants (`ltx_2_19b_distilled_q6p.ckpt`, `ltx_2_19b_dev_q6p.ckpt`) as new defaults.

---

## [0.1.20] - 2026-03-13 Revision 20

### Fixed

- Fixed `imageFormat` override not being resolved to explicit width/height before canvas normalization in `image2image`, `edit`, and `image2video` modes.

---

## [0.1.19] - 2026-03-13 Revision 19

### Fixed

- Fixed `normalizeInputBuffer` distortion bug: input images were improperly scaled, causing stretched/distorted results when the requested format differed from the source (e.g. portrait canvas → landscape output).

---

## [0.1.18] - 2026-03-13 Revision 17, 18

### Changed

- Changed pre-build process for LM Studio Hub.

---

## [0.1.16] - 2026-03-11 Revision 16

### Fixed

- Fixed TCD Trailing sampler support.

### Changed

- Qwen3.5 now uses the correct thinking-mode flag.
- Progress messages during generation are more informative.

---

## [0.1.15] - 2026-02-26 Revision 15

### Changed

- Added support for Qwen3.5 35B (qwen/qwen3-vl-30b) as the new default LLM.

---

## [0.1.14] - 2026-02-06 Revision 14

### Changed

- Slightly increased `steps` in `edit` mode for better prompt adherence with qwen-image.
- Minimal adjustments to generation defaults.
- Improved Brave Web Search result rendering.

---

## [0.1.13] - 2026-02-01 Revision 13

### Changed

- Optimized model-mapping-snapshot for [draw_things_index](https://lmstudio.ai/ceveyne/draw-things-index/)
- Updated user documentation

---

## [0.1.12] - 2026-01-30 Revision 12

### Changed

- `image2image` mode now supports moodboard (multiple reference images) via gRPC transport, same as `edit` mode.
- `current conversation tokens` now refers to the agent model's context length

---

## [0.1.11] - 2026-01-29 Revision 11

### Added

- New screenshots to reflect the recent update to LM Studio 0.4.0.

### Changed

- `index_image` tool calls no longer echo the model-mapping snapshot JSON back into the agent-model prompt history (token economy), while still passing the snapshot through to the index tool itself.

### Fixed

- README: Updated the model table to show flux_2_klein_9b_q6p.ckpt for all three modes (instead of just text2image with flux_1). Removed the "—" (not supported) markers for Flux.

---

## [0.1.10] - 2026-01-26 Revision 10

### Added

- Tool summaries now show the model family label alongside the exact Draw Things model file that was used, so you can quickly understand what re-generates an image.
- Added `review_image()` tool: lets you ask the agent to re-view any earlier created image, variant, or picture from tool-based generation or research.

### Changed

- Increased TTL for "vision-capability-priming" to 120 minutes.
- Updated user documentation to reflect the recent changes.

### Fixed

- Fixed draw-things-index `index_image` markdown table: `model_display` is now preserved through picture materialization.

---

## [0.1.9] - 2026-01-23 Revision 9

### Added

- Added `FLUX.2 [klein] 9B (6-bit)` as new default for flux model family.

### Fixed

- Fixed Agent behavior to avoid generations or tool results being displayed more than just once.

---

## [0.1.8] - 2026-01-22 Revision 8

### Changed

- Improved source display in draw-things-index results: Chat IDs now shown completely, project names displayed without file extension.

### Fixed

- Fixed picture indexing across multiple queries: `p` IDs are now stable per chat (including draw-things-index `project://` results) and no longer reset to `p1` per query.

---

## [0.1.7] - 2026-01-22 Revision 7

### Fixed

- Fixed a build issue that excluded necessary files from being uploaded.

---

## [0.1.6] - 2026-01-22 Revision 6

### Changed

- Improved Project file parser for [draw-things-index](https://lmstudio.ai/ceveyne/draw-things-index/).

---

## [0.1.5] - 2026-01-21 Revision 5

### Added

- Added support for [draw_things_index](https://lmstudio.ai/ceveyne/draw-things-index/), an LM Studio Plugin to search through your Draw Things generation history.

### Changed

- Changed Agent behavior to avoid generations or tool results being displayed more than just once.

### Fixed

- Small stability and error handling improvements.

---

## [0.1.4] - 2026-01-18 Revision 4

### Added

- Live progress indicator (gRPC only): Image generation now shows real-time progress (e.g., "Step 12/20 (60%)") in the chat UI.
- Explicit hard limit validation for width/height parameters with clear error messages.

### Changed

- Width/height descriptions now include maximum allowed values.
- Dimension validation: Requests exceeding limits are now rejected with actionable error messages instead of silent clamping.

### Fixed

- HTTP img2img: imageFormat shorthand no longer overrides explicitly provided width/height values (parity with gRPC).

---

## [0.1.3] - 2026-01-17 Revision 3

### Changed

- Adjusted number of attachments for Vision Promotion to match LM Studio client limit (5 images max).
- Updated user documentation.

### Fixed

- Generation progress now shown consistently for all modes (text-to-image, image-to-image, and edit).

---

## [0.1.2] - 2026-01-16 Revision 2

### Changed

- Unified preview sizes across all media types for consistent display quality.

---

## [0.1.1] - 2026-01-16 Revision 1

### Added

- Initial release
- Draw Things integration with vision-capable agents
- Support for text-to-image, image-to-image, and edit modes
- Vision Promotion system for attachments and generated images
- Orchestrator-based agent architecture
- Tool-based image generation interface
