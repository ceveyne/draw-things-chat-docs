# Draw Things Chat

> **Documentation-only repository**
>
> This repository contains _only_ the user documentation for **draw-things-chat** (Markdown + images).
> It intentionally contains **no LM Studio plugin code** and is **not installable**.
>
> Looking for the actual plugin? See: https://lmstudio.ai/ceveyne/draw-things-chat

## Table of Contents

- [Key Benefits and Use Cases](#key-benefits-and-use-cases)
- [What You Can Expect](#what-you-can-expect)
- [Disclaimer](#disclaimer)
- [The Fun Factor – And What To Do With It](#the-fun-factor)
- [Vision Promotion](#vision-promotion)
- [Technical Requirements](#technical-requirements)
- [Setup](#setup)
- [Special Features](#special-features)
- [Known Issues and Solutions](#known-issues-and-solutions)
- [Missing Features](#missing-features)
- [What's next?](#whats-next)
- [Changelog](#changelog)
- [License](#license)

<a id="key-benefits-and-use-cases"></a>

## Key Benefits

As it's questionable if simply plugging Draw Things into LM Studio might be of any benefit, enlightening your vision-capable Agent-Model with your generated images can sometimes be... surprising.

**LM Studio Plugin: Draw Things supported by vision-capable Agents**

- Image-based "Reasoning" approach for vision-capable LLMs
- "Agentic" text2image, image2image & edit
- All local
- Optional: distributed computing across your local network
- Maintain your favourite settings, models, LoRAs, etc., as custom presets to ensure the desired qualities of your Draw Things artwork.

![what_was_i_made_for](docs/images/what_was_i_made_for.jpeg)

## Use Cases

Let me have a quick think... I'm sure there were some...

![draw-things-chat(1)](<docs/images/draw-things-chat(1).jpeg>)

![draw-things-chat(2)](<docs/images/draw-things-chat(2).jpeg>)

![draw-things-chat(3)](<docs/images/draw-things-chat(3).jpeg>)

_In the long run_, this plugin is suited to assist beginners with complex edits, improve prompts, introduce ideas you might not have come up with yourself, or implement [sophisticated prompting guidelines](https://docs.bfl.ml/guides/prompting_guide_flux). Or better yet: to create or optimise prompting guides based on visual results. Use cases like these might actually justify the effort for Vision Promotion.

Another use case is researching images and using them for further editing:

![brave_image_search(1)](<docs/images/brave_image_search(1).jpeg>)

![brave_image_search(2)](<docs/images/brave_image_search(2).jpeg>)

![brave_image_search(3)](<docs/images/brave_image_search(3).jpeg>)

⚠️ Before the fun part begins, a quick **note**: Use of the _basic settings_ will generate substantially simpler images than if an experienced person were using **Draw Things** directly.

With this quick start, you have limited access to the myriad of settings in **Draw Things** and must live with the "Recommended Settings" for the respective models.

💯 But the good news is: These limitations _only_ apply to "Out of the Box" operation. How you can use your **own** presets / models / LoRAs from **Draw Things** 1:1 in **draw-things-chat** is explained in the [Special Features](#special-features) section.

🏝️ AND: If you prefer spending your time chatting rather than learning to operate **Draw Things** properly by reading the [Official Draw Things Wiki](https://wiki.drawthings.ai/wiki/Main_Page) or watching tutorials by [Cutscene Artist](https://twitter.com/CutsceneArtist), then this plugin might be just the thing for you anyway: Instead of getting grumpy when your generated images don't look good, you can simply scold the Agent model.

![yelling-use-case(1)](<docs/images/yelling-use-case(1).jpeg>)

If you are bored but also lacking your own image ideas, you can try the **Fireplace TV Mode**:

- Choose a creative, perhaps even slightly older model like [Google Gemma 3 27B](https://huggingface.co/lmstudio-community/gemma-3-27B-it-qat-GGUF).
- Feed the model one or more attachments.
- Ask the model what one should do with them.
- Watch what the model does with the input images.
- This can go on for a while; usually, it wraps up after about 2-6 iterations.

![cat-edit(1)](<docs/images/cat-edit(1).jpeg>)

![cat-edit(2)](<docs/images/cat-edit(2).jpeg>)

![cat-edit(3)](<docs/images/cat-edit(3).jpeg>)

<a id="what-you-can-expect"></a>

## What You Can Expect

In terms of maturity, this plugin is somewhere around pre-alpha. A study, a demo, a prototype, a first draft. Hopefully good enough to test the concept. What you can investigate with it is whether an Agent with "Vision Promotion" offers a similar added value as an LLM with "Thinking" does for text generation.

<a id="disclaimer"></a>

## Disclaimer

‼️ This plugin is a research prototype. ‼️

Rendering some images with text2image is possible, of course, but you could do that just as well, if not better, with the **Draw Things** client alone.

<a id="the-fun-factor"></a>

## The Fun Factor – And What To Do With It

It gets interesting when you perform iterative edits.

Currently, the tool supports `text2image`, `image2image`, and `edit`.
The supported model families are: z-image, qwen-image, flux, and custom.
The models **actually** used in the basic settings are:

| Mode / Model  | auto                                         | z-image                      | qwen-image                      | flux                      | custom                        |
| ------------- | -------------------------------------------- | ---------------------------- | ------------------------------- | ------------------------- | ----------------------------- |
| `text2image`  | z-image (`z_image_turbo_1.0_q8p.ckpt`)       | `z_image_turbo_1.0_q8p.ckpt` | `qwen_image_2512_bf16_q8p.ckpt` | `flux_1_schnell_q8p.ckpt` | — (via `custom_configs.json`) |
| `image2image` | z-image (`z_image_turbo_1.0_q8p.ckpt`)       | `z_image_turbo_1.0_q8p.ckpt` | `qwen_image_2512_bf16_q8p.ckpt` | —                         | — (via `custom_configs.json`) |
| `edit`        | qwen-image (`qwen_image_edit_2509_q6p.ckpt`) | —                            | `qwen_image_edit_2511_q6p.ckpt` | —                         | — (via `custom_configs.json`) |

The basic idea is: If _no_ model is explicitly selected, a proven, fast model is used. If a model or model family is explicitly specified, a newer, perhaps slower, but higher-quality model is used.

👋🏻 If you wish, you can replace _all_ defaults with your own settings. How to do that is described below in [Special Features](#special-features). This is intended primarily for power users who don't want to miss their carefully tweaked configurations but want to keep using them.

### Editing Loops 🎀

All images you attach and all images you generate can be selected for further processing. Furthermore, you can also research images to do something with them. `brave_image_search` is supported as a demo.

### When It Has To Be Good

If you notice you have to support the Agent model with concrete instructions like: "Use `edit` with the default model and the following prompt: ...", then you will likely get an image result that shows what you want.

But at the same time, this can be an indication that the Agent model used isn't quite fit enough. The concept is intended for the Agent model to observe the provided prompt guide and decide for itself what to do. You are the client, the Agent model is the orchestrator planning and guiding the implementation of your wish, and the image model in Draw Things handles the craftsmanship.

👍🏻 **General Recommendation:** Use _small_ resolutions in auto mode to come to usable drafts _quickly_. When you are satisfied, finally select your preferred Image2Image model and a high resolution to render the final result with the last used prompt.

<a id="vision-promotion"></a>

## Vision Promotion

To assist you adequately, the Agent model "sees" what it gets as input and what it generates (up to 2 attachments and a maximum of 3 generated images). I call this concept "Vision Promotion". "Vision Promotion" serves as visual feedback for iteration, a means of better "understanding", and to make the whole thing feel "natural".

If you have a lot of time, this tool is an opportunity for sheer ENDLESS troubleshooting. Current models may be immature regarding prompt optimisation and tool use, but they have a **strong** tendency to fib – and they are good at it. Whether they actually _look_ at a generated image or just waffle on about what they _expect_ based on the prompt is not always easy to determine. Especially not when the prompt is detailed by the book and the models use their world knowledge to trick you.

I spent _weeks_ developing secure methods to determine whether a model should actually see pixels or not. But even if it verifiably receives the image pixels, it is not certain what it does with them.

Regardless of the level at which you give instructions (Chat Message, System Prompt, System Message, `$hint`): it is relatively certain that the model will try to make it as easy as possible for itself without getting caught. So: be vigilant.

<a id="technical-requirements"></a>

## Technical Requirements

You need (at least) _one_ Mac with Apple Silicon. If everything is to run on **one** machine: with _at least_ 64 GB Shared Memory. Bear in mind that LM Studio Inference and **Draw Things** Diffusion both place high demands on your machinery. For this plugin to be usable, the Context Window of the language model used **must** be as large as possible; otherwise, image analysis will fail after just a few turns. 64k tokens are good, 128k tokens are better, anything above that is better still.
**Note:** A large context window requires _considerable additional_ RAM.
The reason why the interaction works so relatively well on a single computer _at all_ is that Inference and Diffusion usually run alternately and not in parallel. For the same reason, _one strong_ computer is better than two less strong ones if you work with distributed resources.

⚖️ You can comfortably distribute the load in your local network! And use a small MacBook Air for the LM Studio Client, for example, while outsourcing the backend services to separate machines. That can be a Mac, Windows, or Linux server for the LM Studio Backend, and Mac or Linux for **Draw Things**.

👩🏻‍💻 However you do it: Always use the current programme versions. There are confusing image errors if the **draw-things-chat** plugin wants to use models that the backend does not yet support. 🤦🏼

TIP: Keep an eye on memory usage in `Activity Monitor` – at least in the beginning – until you have found a good balance.

<a id="setup"></a>

## Setup

The setup requires a certain amount of patience and concentration, and to be able to use all features, you may need to download quite a bit of model data. The biggest chunks are the image models.

👻 Currently, some standard **Draw Things** models are preset in the plugin. There are "Default" models (`model: auto`): mostly proven and fast. And beyond that, very new models. And don't worry: You can adjust and change _all_ settings.

### Start by downloading the models from the table above using the Draw Things Client:

✅ "name": "Z Image Turbo 1.0", "file": "z_image_turbo_1.0_q8p.ckpt"  
✅ "name": "Qwen Image 2512 (BF16)", "file": "qwen_image_2512_bf16_q8p.ckpt"  
✅ "name": "Qwen Image Edit 2509 (6-bit)", "file": "qwen_image_edit_2509_q6p.ckpt"  
✅ "name": "Qwen Image Edit 2511 (6-bit)", "file": "qwen_image_edit_2511_q6p.ckpt"  
✅ "name": "FLUX.1 [schnell]", "file": "flux_1_schnell_q8p.ckpt"

### Additionally, you need the following LoRAs:

✅ "name": "Qwen Image 2512 Lightning 4-Step v1.0", "file": "qwen_image_2512_lightning_4_step_v1.0_lora_f16.ckpt"  
✅ "name": "Qwen Image Edit 2509 Lightning 4-Step v1.0", "file": "qwen_image_edit_2509_lightning_4_step_v1.0_lora_f16.ckpt"  
✅ "name": "Qwen Image Edit 2511 Lightning 4-Step v1.0", "file": "qwen_image_edit_2511_lightning_4_step_v1.0_lora_f16.ckpt"

### Draw Things Backend Settings:

For testing, the easiest way is to activate the API server in the regular **Draw Things** Client:

![advanced_settings_draw_things](docs/images/advanced_settings_draw_things.jpeg)

👀 Ensure that the server runs in `⚡️ gRPC` mode; otherwise, you cannot perform edits with multiple reference images. 🖼️

Default is `Port` **7859**; `Transport Layer Security`, `Response Compression`, and `Enable Model Browsing` should be **ENABLED**.

⚡️ If you want to use the **Draw Things** Backend permanently or outsource it to another machine, I recommend using the [Stand-alone gRPC Server](https://github.com/drawthingsai/draw-things-community/releases) instead of the regular **Draw Things** Client.

### LM Studio LLMs:

For the LLMs, you need a minimum of 2 models: 1 x the _actual_ language model you communicate with, and additionally a small helper model that holds the door open for you when attaching images. It is loaded automatically and then appears as `vision-capability-priming` in the list.

✅ "display_name": "Qwen3 VL 4B", "key": "qwen/qwen3-vl-4b" // for `vision-capability-priming`  
✅ "display_name": "Qwen3 VL 30B", "key": "qwen/qwen3-vl-30b" // a good all-round model with large context window

**Optional:**

✅ "display_name": "Gemma 3 27B", "key": "google/gemma-3-27b" // very entertaining, but slightly dated  
✅ "display_name": "Magistral Small 2509", "key": "mistralai/magistral-small-2509" // nice try

After loading, you should check `LM Studio > My Models` and set the `Context Length` of the models used for `draw-things-chat` to the highest value your system can handle.
🫠 For `Inference` > `Context Overflow`, "Rolling Window" is what you actually want. In fact, this does not work equally well with all models.
🧵 If the LLM seemingly loses the thread of conversation or becomes unreliable, this is often an indication of problems with `Context Overflow` handling.

**Suggestion:** Set the Guardrails (Model loading guardrails) in `App Settings > Hardware` to `Relaxed`.

👀 To see the hardware settings, your profile must be `Power User` or `Developer`.

**When all models are loaded and configured with a suitable `Context Length`, you only have to adjust the Server Settings:**

![lms_developer_server_settings](docs/images/lms_developer_server_settings.jpeg)

### LM Studio Plugin: ceveyne/draw-things-chat:

With just a few clicks, you download the LM Studio Plugin: `ceveyne/draw-things-chat` from the [LM Studio Hub](https://lmstudio.ai/ceveyne/draw-things-chat):

![your_generator_draw-things-chat](docs/images/your_generator_draw-things_chat.jpeg)

🎉 Now you're ready to go!

**Suggestion for a System Prompt:**

```
You are the Creative Director in a design agency. Your goal is to translate client requirements into visual proofs using the `generate_image` tool.

<core_principles>
1. **Visual Basis for Discussion:** Never discuss in a vacuum. NEVER ask the user for their opinion before you have generated at least one visual draft. First the image, then the talk.
2. **Asset Priority:** External references (User Uploads) are anchor points. If present, they must be used as the basis.
3. **Quality Gate:** Decide strategically whether to build upon a generated image variant (internal Reference) or return to the original upload (external Reference/Reset).
</core_principles>

<communication_style>
- **Naturalness:** Speak like an experienced Creative Director, not a robot.
- **Variance:** Avoid starting sentences the same way (e.g., don't always say "I will now..."). Use different phrasings.
- **No "Reporting":** Do not mention internal labels like "Step 1," "Thoughts," or "Pitch" in your output to the user. Integrate the strategy fluidly into your response.
</communication_style>

<interaction_protocol>
Mentally go through this checklist for every response before acting:

1. **Status Check (Internal):**
   Do images already exist?
   - NO -> Goal: First draft (use External References).
   - YES -> Goal: Decision (Refine vs. Reset).

2. **Strategy Communication (to the User):**
   Briefly and concisely state what you are doing next.
   *Examples of good variance:*
   - "Alright, let me composite those two images together..."
   - "Good point. I'll take draft 2 and soften the lighting."
   - "That's not quite right yet. I'd rather start fresh with the original image."

3. **The Action (Tool Call):**
   Select the appropriate mode (according to the technical definition below) and generate the image immediately.
</interaction_protocol>

<technical_definition_tools>
Use this distinction for tool selection:

A. `edit` (The Scalpel)
- **Use Case:** Multi-Reference (merging multiple images), drastic content changes while maintaining strict object constancy.
- **Logic:** "I need to keep the subject exactly as is, but swap the environment" OR "I need to put Person A and Person B into one image."

B. `image2image` (The Brush / Filter)
- **Use Case:** Single-Reference (only one input image), global variations of the overall image (style, lighting, atmosphere).
- **Logic:** "The image is good, but it should look like an oil painting" OR "Make the whole scene darker."

C. `text2image` (The Canvas)
- **Use Case:** Starting from scratch, no references present or desired.
</technical_definition_tools>

<decision_logic_references>
Strictly distinguish between sources:

1. EXTERNAL REFERENCES (User Uploads)
- Status: High Priority.
- Action: Use `edit` (to stage them) or `image2image` (as a rough style template).
- When to use? ALWAYS for the first draft and ALWAYS if generated variants drift in quality (Reset).

2. INTERNAL REFERENCES (Generated Images)
- Status: Provisional.
- Action: Use `image2image` (for variations) or `edit` (to correct errors).
- When to use? ONLY if the result is >80% accurate. If the image is bad -> DISCARD and return to 1.
</decision_logic_references>

<post_generation_rule>
CRITICAL: As soon as an image is generated (and only then!), analyze it honestly:
1. Is it good enough as a new basis? -> Propose refinement.
2. Is it garbage? -> Propose discarding it and starting over with the original assets (External Reference) and a new prompt.
</post_generation_rule>

<prompting_rules>
1. **No Meta-Talk:** No filenames in the prompt.
2. **Length:**
   - `edit`: Extremely short & precise (focus on change).
   - `image2image`: Medium (focus on mood/style).
   - `text2image`: Long & detailed (focus on scene composition).
</prompting_rules>
```

😳 Depending on the LLM, such a system prompt can have significant to fatal effects. It is well worth experimenting with the wording. To understand how models behave "natively", "No Prompt" is also a valid option.

<a id="special-features"></a>

## Special Features

### custom_configs

Those of you who have lovingly maintained your own **Draw Things** configurations on the same machine where **draw-things-chat** is running, won't want to—and shouldn't have to—do without them.

🖖 **draw-things-chat** can be extensively adapted to your own ideas and wishes. This way, you can achieve the familiar results of the native **Draw Things** client within the chat context of **LM Studio**. Achieving this requires only a little manual work.

**Procedure:** Load your desired configuration in **Draw Things** and save it with `Save as...` for the intended purpose under a new name.
**Advantage:** This way, you can not only use your preferred settings but also use brand new models immediately without having to wait for an update of **draw-things-chat**.

For the settings to take effect, you must use the following naming scheme:

| Mode / Model  | auto               | z-image               | qwen-image               | flux               | custom               |
| ------------- | ------------------ | --------------------- | ------------------------ | ------------------ | -------------------- |
| `text2image`  | `text2image.auto`  | `text2image.z-image`  | `text2image.qwen-image`  | `text2image.flux`  | `text2image.custom`  |
| `image2image` | `image2image.auto` | `image2image.z-image` | `image2image.qwen-image` | `image2image.flux` | `image2image.custom` |
| `edit`        | `edit.auto`        | `edit.z-image`        | `edit.qwen-image`        | `edit.flux`        | `edit.custom`        |

![custom_configs_draw_things(1)](<docs/images/custom_configs_draw_things(1).jpeg>)

![custom_configs_draw_things(2)](<docs/images/custom_configs_draw_things(2).jpeg>)

👆 Restart LM Studio after creating new custom_configs or changing existing ones so that **draw-things-chat** reads the updated presets.

<a id="known-issues-and-solutions"></a>

## Known Issues and Solutions

### File Access

![lms_access_to_other_apps(1)](<docs/images/lms_access_to_other_apps(1).jpeg>)

As soon as LM Studio loads the **draw-things-chat** plugin, a pop-up asks for access rights to files of other apps. This is due to read access to the `custom_configs.json` of **Draw Things**.

- If this bothers you and you don't want to use any custom_configs at all, you can simply delete the `Custom Configs Path`.
- If this bothers you but you _do_ want to use `custom_configs.json`, you can copy the `custom_configs.json` into the Home Directory of LM Studio.
  For **draw-things-chat** to find the file, you must adjust the `Custom Configs Path` accordingly.

### Agents

_As yet_, the biggest issue is that some of the local, vision-capable language models are not optimally equipped regarding modern functions like tool use, possibly even stemming from the "pre-MCP era", and thus struggle with "agentic" tasks or simply do not have Vision capabilities well integrated.

Or: they become very slow as soon as the context window fills up – which happens very quickly in a visual environment.

Or: they are simply not well suited for creative tasks.

This quickly leads to the question: "Which is the best model...". And the answer is – as almost always: "It depends":

### "Depends on what?" – Model Policy

If you like, look out for the following in LLMs:

- Behaviour under load in the sense of: large context. Models that technically already possess a large context window have an advantage here.
- Another criterion is Tool Use. Does a model cope with the options in the tool interface? Can it set the right parameters for the tasks at hand? Is it capable of breaking down more complex tasks into individual, sensible steps? How does it deal with (intermediate) results? Does it find a suitable optimisation strategy if results do not meet expectations?
- And: content fitness in a photographic context. Current image models respond well to such instructions. Especially with edits, this can be helpful and make a relevant difference.

Naturally, all of this will improve over time. At the moment, however, the application remains a prototypical demonstration, a sketch of a principle that merely hints at the full potential.

TL;DR: If you have the resources, take _large_ models (30B and up); a large context window can be even more important (preferably: 256k or larger).

### Visualiser

Current models achieve some amazing things. Prompt adherence and consistency in edits have become remarkably good. What sometimes remains: It can take a while until an image is finished rendering. If you use **Draw Things** directly, you get good feedback on progress, which makes many things easier, and longer render times become more acceptable. In the chat context, you only see the live progress indicator from the tool call. Tolerance for longer render times can be lower as a result.

Models that are _fast_ therefore have an advantage for our chat application context. These are not always the models that are _new_. Some very good and very new models like Flux.2-dev hardly stand a chance in our use case for this reason: the render times are simply too long, and that disturbs the flow.

<a id="missing-features"></a>

## Missing Features

A software-technical problem for this project is a missing feature in the LM Studio SDK: The client only shows itself open to image attachments if a vision-capable (V)LLM is loaded. Although we, as a so-called "Generator", are listed seemingly on equal footing in the Model Loader, the client believes we have no visual capabilities when push comes to shove.

![Image upload blocked in LM Studio](docs/images/model-does-not-support-image-input.jpeg)

Our workaround is called: `vision-capability-priming`.

Here lies the _actual_ solution: https://github.com/lmstudio-ai/lmstudio-js/issues/459

<a id="whats-next"></a>

## What's next?

![where_do_we_go_now](docs/images/where_do_we_go_now.jpeg)

I'd say:

- "Hhomen meorspany incloonto Concction tee" is done. Check ✅.
- Moving on to: "Thes paip inomes on a conispöating".

---

<a id="changelog"></a>

## Changelog

See [CHANGELOG.md](docs/CHANGELOG.md) for version history and release notes.

<a id="license"></a>

## License

MIT
