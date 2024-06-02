# OpenClap Format

*A file format for the age of AI content production*

<img src="https://media.githubusercontent.com/media/jbilcke-hf/OpenClap-Format/main/teaser.jpg" width="100%" />

(The above screenshot is from [Clap Viewer](https://jbilcke-hf-clap-viewer.hf.space), a free open-source app to visualize `.clap` files)

## What is OpenClap?

**OpenClap** (`.clap`) is an open interchange format made to *contain AI-generated assets* so they can be shared between different apps and tools. It can be used to define either static or dynamic scenes, such as **generative movies or games**.

Please note that this is just a data format designed for storage and sharing. **It can store instructions, inputs and outputs for AI models so it will contain text and binary data such as prompts, images, sound, music and videos but this is not a runtime, a video model or a game engine.**

You can see it as the all-in-one, *AI-first* equivalent of the aptly named [prompt book](https://en.wikipedia.org/wiki/Prompt_book).

To quote Wikipedia:

> The prompt book, also called transcript, the bible or sometimes simply the book, is the copy of a production script that contains the information necessary to create a theatrical production from the ground up.
> It is a compilation of all blocking, business, light, speech and sound cues, lists of properties, drawings of the set, contact information for the cast and crew, and any other relevant information that might be necessary to help the production run smoothly.

Similar to theatre, an author and producer can use the **OpenClap** data structure in a very light way, only defining broad storylines, art directions and prompts, leaving the rest to interpretation by an AI rendering engine, creating opportunities for surprise, improvisation, and interactivity with audience (**OpenClap** files can include interactive layers).

This readiness for interactivity by means of time-indexed parameters makes it the ideal format to build AI apps by prompting multi-purpose [world models](https://worldmodels.github.io/) such as as [Pandora](https://huggingface.co/maitrix-org/Pandora), even more so than video models.

## Are you working on a similar project?

Are you working on a similar tool, AI game engine or storyboard-based project?

You may realize at some point that you also need to create your own proprietary `ClosedClap`, due to the convergence of technical needs and requirements.

But you know what: let's collaborate instead! Let's have a constructive conversation on how to build a common neutral format to connect our tools or platforms, so that [my users](https://huggingface.co/spaces/jbilcke-hf/ai-comic-factory?likers=true) can also become *your* users.


## Under the hood

An **OpenClap** file (`.clap`) is a compressed YAML stream of documents that describe all aspects of a scene:

- prompts
- image/video storyboards and moodboards
- timings and events
- 4D gaussian splatting videos (`.splatv`)
- main entities with a face and voice (eg. characters)
- prompts for agents (NPCs etc) and scripted world events
- generated outputs (images, videos, gaussian splatting videos)
- revisions (for versionning prompts and outputs)

**OpenClap** is an *open* format, and not just because it has no licence fees, be because it can also be extended.

You can reference any kind of file format in it for your content, and in the future it might also evolve to include production workflows such as embedded ComfyUI workflow files or Visual Blocks.

## Specification

I am working on an official specification document, as a proof of concept I've asked GPT-4 to convert the reference implementation to Markdown, but obviously some human proofreading and editing will be needed to create a solid spec.

Still, you can look at the draft here: [DRAFT.md](DRAFT.md)

## Implementations

Checkout the [list of projects tagged with "openclap"](https://github.com/topics/openclap)

**JavaScript package to manipulate .clap files ([code](https://github.com/jbilcke-hf/aitube-clap)):**

- NPM link: https://www.npmjs.com/package/@aitube/clap
- Supports Browser (client-side) and NodeJS (server-side)
- Utilities and helpers to manipulate `.clap` files (read a file,  write to it)

**Simple read-only app to visualize a .clap file (go see the [public beta](https://huggingface.co/spaces/jbilcke-hf/clap-viewer)):**

- App: https://huggingface.co/spaces/jbilcke-hf/clap-viewer
- Code: https://github.com/jbilcke-hf/clap-viewer
- Read-only .clap for now
- Scrolling features are not fully operational yet (there are bugs)

**React component to render a .clap timeline ([code](https://github.com/jbilcke-hf/aitube-timeline)):**

- It is used by Clap Viewer
- Read-only .clap for now
- NPM link: https://www.npmjs.com/package/@aitube/timeline
- Work in progress, it's not finished yet! (so there is no doc)
- Scrolling features are not fully operational yet (there are bugs)

**AI Comic Factory ([app](https://huggingface.co/spaces/jbilcke-hf/ai-comic-factory), [code](https://github.com/jbilcke-hf/ai-comic-factory))**:

- Web GUI, 100% standalone (you can choose your own backend/vendor)
- can save and load a `.clap`
- supported layer(s): interface, storyboard
- can extend a .clap with longer story and storyboards

**AI Stories Factory ([app](https://huggingface.co/spaces/jbilcke-hf/ai-stories-factory), [code](https://github.com/jbilcke-hf/ai-stories-factory)):**

- Web GUI, hosted on Hugging Face (not 100% standalone yet, it uses the AiTube API)
- can save and load a `.clap`
- supported layer(s): interface, storyboard, video, music, (sound soon)
- can generate videos from storyboards
- can export to mp4

**Clap Exporter ([code](https://github.com/jbilcke-hf/ai-tube-clap-exporter)):**

- A service to convert pre-generated assets (like video and music) to a video file
- The Clap Exporter doesn’t perform any AI generation, only the conversion to video
- REST API (send a .clap as HTTP POST to the / endpoint)
- can only load an existing `.clap`
- supports interface, storyboard, video, music, (sound soon)
- supported output formats are `.mp4` and `.webm`

**Broadway JS library ([code](https://github.com/jbilcke-hf/aitube-broadway)):**

- A NPM package to convert a script (as in “screenplay”) to a `.clap` file
- It only analyzes and converts the structure and text, it doesn’t generate images or videos
- Analysis doesn’t use AI, so it’s *fast* (converts a full script in about ~700ms)
- Can only support plain text for now (no PDFs)
- 

**Broadway API ([code](https://github.com/jbilcke-hf/aitube-broadway)):**

- A REST API to convert a script (as in “screenplay”) to a `.clap` file
- It only analyzes and converts the structure and text, it doesn’t generate images or videos
- Analysis doesn’t use AI, so it’s *fast* (converts a full script in about ~700ms)
- However, the API version may use AI in the future to analyze scripts
- Can only support plain text for now (no PDFs)
- 

**The AiTube API:**

- The AiTube API is currently reserved for private use by HF team.
- It is not possible at the current time to ask or pay to get access.
- REST API protected by a secret access token (which is not shared - see above)
- Can create, edit and extend `.clap` files
- Can export `.clap` files to video files

## Community and how to contribute

I am looking for contributors and people willing to implement
**OpenClap** in their own AI content creation tools.

so if you are looking into using **OpenClap** please contact `Julian Bilcke` (**@flngr* on X), for instance on Discord:

[Meet me on the OpenClap channel on Discord](https://discord.gg/AEruz9B92B).

## Roadmap

- [ ] Finish the DRAFT of the specification
- [x] Use it in production
- [x] First working implementation for NodeJS 
- [ ] C++ library (for native binding): TODO
- [x] Python (in progress): [py-aitube-clap](https://github.com/jbilcke-hf/py-aitube-clap)
- [x] NodeJS (released): [aitube-clap](https://github.com/jbilcke-hf/aitube-clap)
- [ ] Swift: TODO
- [ ] Go: TODO
- [ ] Java: TODO
- [ ] Haskell: TODO
