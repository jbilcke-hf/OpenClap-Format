# OpenClap Format

*A file format for the age of AI content production*

Attention: This format is experimental and not standardized yet, so please exercise caution and reach out if you implement it.

## What is OpenClap?

**OpenClap** is an open interchange format designed for AI content, from pre-generated multi-hour movies and infinite streams of 4D gaussian splatting videos (`.splatv`) to real-time interactive video game experiences.

Since it contains all the elements needed to generate an AI movie, you can see it as the AI-oriented equivalent of the aptly named [prompt book](https://en.wikipedia.org/wiki/Prompt_book) (or [pitch bible](https://en.wikipedia.org/wiki/Bible_(screenwriting))).

To quote Wikipedia:

> The prompt book, also called transcript, the bible or sometimes simply the book, is the copy of a production script that contains the information necessary to create a theatrical production from the ground up.
> It is a compilation of all blocking, business, light, speech and sound cues, lists of properties, drawings of the set, contact information for the cast and crew, and any other relevant information that might be necessary to help the production run smoothly.

Is is an interesting parallel since **OpenClap** actually shares more with the world of theatre and live performances rather than the world of pre-generated movies and cinema.

An author and producer can use **OpenClap** format in a very light way, only defining broad storylines, art directions and prompts, leaving the rest to interpretation by an AI rendering engine, creating opportunities for surprise, improvisation, and interactivity with audience (**OpenClap** files can include interactive layers).

## Under the hood

An **OpenClap** file (`.clap`) is a compressed YAML stream of documents that describe all aspects of a scene:

- prompts
- image/video storyboards and moodboards
- timings and events
- entities (eg. characters)
- generated outputs (images, videos, gaussian splatting videos)
- revisions (for versionning prompts and outputs)

**OpenClap** currently defines the *content* itself, not *how* it is produced. In the future, the format might evolve to introduce production workflows such as ComfyUI or Visual Blocks.

## Specification

I am working on an official specification document, as a proof of concept I've asked GPT-4 to convert the reference implementation to Markdown, but obviously some human proofreading and editing will be needed to create a solid spec.

Still, you can look at the draft here: [DRAFT.md](DRAFT.md)

## Implementations

Checkout the [list of projects tagged with "openclap"](https://github.com/topics/openclap)

**JavaScript package to manipulate .clap files ([code](https://github.com/jbilcke-hf/aitube-clap)):**

- NPM link: https://www.npmjs.com/package/@aitube/clap
- Supports Browser (client-side) and NodeJS (server-side)
- Utilities and helpers to manipulate `.clap` files (read a file,  write to it)

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
