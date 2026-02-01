# Draw Things Chat

> **Documentation-only repository**
>
> This repository contains _only_ the user documentation for **draw-things-chat** (Markdown + images).
> It intentionally contains **no LM Studio plugin code** and is **not installable**.
>
> Looking for the actual plugin? See: https://lmstudio.ai/ceveyne/draw-things-chat

## Table of Contents

- [Key Benefits](#key-benefits-and-use-cases)
- [Setup](#setup)
  - [Draw Things user on Mac](#setup-mac-draw-things)
  - [LM Studio user on Windows or Linux – MCP-based image generation](#setup-windows-linux-mcp)
  - [LM Studio user on Linux – Draw Things backend image generation](#setup-linux-draw-things)
- [Invoke formerly generated images: Metadata Query plugin draw-things-index](#setup-draw-things-index)
- [More detailed user docs](#detailed-user-docs)
- [Changelog](#changelog)
- [License](#license)

<a id="key-benefits-and-use-cases"></a>

## Key Benefits

As it's questionable if simply plugging Draw Things into LM Studio might be of any benefit, enlightening your vision-capable Agent-Model with your generated images can sometimes be... surprising.

**LM Studio Plugin: Draw Things supported by vision-capable Agents**

- Image-based "Reasoning" approach for vision-capable LLMs
- "Agentic" workflows for text2image, image2image & edit
- Re-use of any previously generates content (requires ["Metadata Query" plugin **draw-things-index**](https://github.com/ceveyne/draw-things-index-docs))
- All local
- Optional: distributed computing across your local network
- Maintain your favourite settings, models, LoRAs, etc., as custom presets to ensure the desired qualities of your Draw Things artwork.

<a id="setup"></a>

## Setup

<a id="setup-mac-draw-things"></a>

### Draw Things user on Mac

1. Prepare for `text2image`, `image2image` and `edit` modes by simply providing your favourite **Draw-Things**-settings for use with **draw-things-chat**:

- **Draw-Things** > Basic Settings > Load your preferences > Save as... `text2image.auto`, `image2image.auto`, `edit.auto`

![custom_configs_auto_draw_things(1)](<docs/images/custom_configs_auto_draw_things(1).jpeg>)

![custom_configs_auto_draw_things(2)](<docs/images/custom_configs_auto_draw_things(2).jpeg>)

- ⚠️ Note: These settings are stored to `<Your_configured_Model-Folder>/custom_configs.json`. It might be a good idea to keep this file in sync with the one located inside your Default-Model-Folder (`~/Library/Containers/com.liuliu.draw-things/Data/Documents/Models/custom_configs.json`)

2. Setup **Draw Things** gRPC server:

- **Draw-Things** > Advanced Settings: `API Server: enable`, `⚡️ gRPC  Port: 7859`, `Transport Layer Security: enable`, `Response Compression: enable`, `Enable Model Browsing: enable`

![advanced_settings_draw_things](docs/images/advanced_settings_draw_things.jpeg)

3. Prepare your **LM Studio Client** for the `vision-capability-primer`:

- Download the required helper-model `qwen/qwen3-vl-4b` to your **local** computer

![lms_developer_vision-capability-priming](docs/images/lms_developer_vision-capability-priming.jpeg)

4. Prepare your **LM Studio Server**:

- Download your preferred vision-capable agent-model (default: `qwen/qwen3-vl-30b`) to your **server** computer
- Set appropriate context length for your vision-capable agent-model
- Enable your **LM Studio Server**

![lms_developer_local_llm_service](docs/images/lms_developer_local_llm_service.jpeg)

![lms_developer_server_settings](docs/images/lms_developer_server_settings.jpeg)

5. Install the **draw-things-chat** plugin:

- https://lmstudio.ai/ceveyne/draw-things-chat

![run_in_LM_Studio](docs/images/run_in_LM_Studio.jpeg)

- Activate the **draw-things-chat** plugin with the model-loader

![lms_model-loader](docs/images/lms_model-loader.jpeg)

- Adjust the **draw-things-chat** plugin-settings to your local machine and network

![your_generator_draw-things_chat](docs/images/your_generator_draw-things_chat.jpeg)

<a id="setup-windows-linux-mcp"></a>

### LM Studio user on Windows or Linux - MCP-based image generation

1. Prepare your **LM Studio Client** for the `vision-capability-primer`:

- Download the required helper-model `qwen/qwen3-vl-4b` to your **local** computer
- ⚠️ Note: This model is not intended to be used for inference. Hence it doesn't need any GPU support.

![windows_mymodels](docs/images/windows_mymodels.jpeg)

2. Prepare your **LM Studio Server**:

- Download your preferred vision-capable agent-model (default: `qwen/qwen3-vl-30b`) to your **server** computer
- Set appropriate context length for your vision-capable agent-model
- Enable your **LM Studio Server**

3. Install the **draw-things-chat** plugin:

- https://lmstudio.ai/ceveyne/draw-things-chat

![run_in_LM_Studio](docs/images/run_in_LM_Studio.jpeg)

- Activate the **draw-things-chat** plugin by choosing it from the model-loader

![windows_generate_image()](<docs/images/windows_generate_image().jpeg>)

- Adjust the **draw-things-chat** plugin-settings to your local machine and network: Disable the built-in tool 'generate_image()' to use any MCP-tool for image-generation that delivers base64 as tool-result.

![windows_no_generate_image()](<docs/images/windows_no_generate_image().jpeg>)

<a id="setup-linux-draw-things"></a>

### LM Studio user on Linux - Draw-Things-backend image generation

1. Follow the instructions here to setup the [**Draw Things** gRPC Server](https://github.com/drawthingsai/draw-things-community/releases)

- For model-management and custom_configs it's easiest to use the Draw Things Client on macOS.

2. Prepare for `text2image`, `image2image` and `edit` modes by simply providing your favourite **Draw-Things**-settings for use with **draw-things-chat**:

- **Draw-Things** > Basic Settings > Load your preferences > Save as... `text2image.auto`, `image2image.auto`, `edit.auto`

![custom_configs_auto_draw_things(1)](<docs/images/custom_configs_auto_draw_things(1).jpeg>)

![custom_configs_auto_draw_things(2)](<docs/images/custom_configs_auto_draw_things(2).jpeg>)

- ⚠️ Note: These settings are stored to `<Your_gRPC-configured_Model-Folder>/custom_configs.json`. It might be a good idea to keep this file in sync with the one located inside your Default-Model-Folder (`~/Library/Containers/com.liuliu.draw-things/Data/Documents/Models/custom_configs.json`)

3. Prepare your **LM Studio Client** for the `vision-capability-primer`:

- Download the required helper-model `qwen/qwen3-vl-4b` to your **local** computer

![lms_developer_vision-capability-priming](docs/images/lms_developer_vision-capability-priming.jpeg)

4. Prepare your **LM Studio Server**:

- Download your preferred vision-capable agent-model (default: `qwen/qwen3-vl-30b`) to your **server** computer
- Set appropriate context length for your vision-capable agent-model
- Enable your **LM Studio Server**

![lms_developer_local_llm_service](docs/images/lms_developer_local_llm_service.jpeg)

![lms_developer_server_settings](docs/images/lms_developer_server_settings.jpeg)

5. Install the **draw-things-chat** plugin:

- https://lmstudio.ai/ceveyne/draw-things-chat

![run_in_LM_Studio](docs/images/run_in_LM_Studio.jpeg)

- Activate the **draw-things-chat** plugin with the model-loader

![lms_model-loader](docs/images/lms_model-loader.jpeg)

- Adjust the **draw-things-chat** plugin-settings to your local machine and network

![your_generator_draw-things_chat](docs/images/your_generator_draw-things_chat.jpeg)

<a id="setup-draw-things-index"></a>

### Invoke formerly generated images: Metadata Query plugin draw-things-index

See the [draw-things-index documentation](https://github.com/ceveyne/draw-things-index-docs) for setup instructions.

## Workflow example (Draw Things backend)

![draw-things-chat-workflow(1)](<docs/images/draw-things-chat-workflow(1).jpeg>)

![draw-things-chat-workflow(2)](<docs/images/draw-things-chat-workflow(2).jpeg>)

![draw-things-chat-workflow(4)](<docs/images/draw-things-chat-workflow(4).jpeg>)

## Usecase example (ComfyUI backend)

![index_image_draw-things-chat_comfyui](docs/images/index_image_draw-things-chat_comfyui.jpeg)

---

<a id="detailed-user-docs"></a>

## More detailed user docs

See [USER_GUIDE.md](docs/USER_GUIDE.md) for in-depth documentation, use cases, technical requirements, known issues, and more.

<a id="changelog"></a>

## Changelog

See [CHANGELOG.md](docs/CHANGELOG.md) for version history and release notes.

<a id="license"></a>

## License

MIT
