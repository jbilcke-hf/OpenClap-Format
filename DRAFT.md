# WARNING

This draft has been generated using GPT-4 and has not been proofread by a human yet. It most probably contains inacurracies.

One known issue is that constants are `UPPERCASE` in this document, but some are still `lowercase` in the reference implementation (this will be fixed in the implementation).


# OpenClap File Format Specification

Draft Revision: 1 (2024-06-26)
Author: GPT-4

Adapted from the TypeScript library which is the reference implementation.

## 1. Introduction

This document specifies the **OpenClap** file format, which is used to serialize AI video projects into a compressed stream of YAML documents. The format is designed to include information regarding metadata, entities, scenes, and segments, ensuring a structure suitable for AI-driven video generation.

## 2. Conventions Used in This Document

The key words **"MUST"**, **"MUST NOT"**, **"REQUIRED"**, **"SHALL"**, **"SHALL NOT"**, **"SHOULD"**, **"SHOULD NOT"**, **"RECOMMENDED"**, **"MAY"**, and **"OPTIONAL"** in this document are to be interpreted as described in RFC 2119.

## 3. Structure of the OpenClap File

An **OpenClap** file is composed of a sequence of YAML documents, representing various components of an AI video project. These documents MUST be serialized in the following order:

1. **ClapHeader**

2. **ClapMeta**

3. **ClapEntity** objects

4. **ClapScene** objects

5. **ClapSegment** objects

The entire YAML sequence MUST be compressed using gzip for efficient storage and transmission.

### 3.1 ClapHeader

The `ClapHeader` provides the format version and counts of entities, scenes, and segments included in the file.

#### 3.1.1 Fields

- **format** (ClapFormat): The format version, e.g., "clap-0".

- **numberOfEntities** (number): The total number of entities.

- **numberOfScenes** (number): The total number of scenes.

- **numberOfSegments** (number): The total number of segments.


### 3.2 ClapMeta

The `ClapMeta` provides metadata about the project.


#### 3.2.1 Fields


- **id** (`string`): Unique identifier for the metadata. MUST follow the UUID v4 convention.

- **title** (`string`): Title of the project.

- **description** (`string`): Description of the project.

- **synopsis** (`string`): Synopsis of the project.

- **licence** (`string`): Licensing information.

- **orientation** (`ClapMediaOrientation`): Media orientation, e.g., "landscape".

- **durationInMs** (`number`): Duration in milliseconds.

- **width** (`number`): Width of the video.

- **height** (`number`): Height of the video.

- **defaultVideoModel** `(string`): Default video model.

- **extraPositivePrompt** (`string[]`): Additional positive prompts (Note: it is still unsure if we keep it in this form).

- **screenplay** (`string`): Screenplay text.

- **isLoop** (`boolean`): Indicates if the project should loop when the player reaches the end.

- **isInteractive** (`boolean`): Indicates if the project is interactive, which means there will be no end. This indicate to **OpenClap**-compatible players that they should hide the timeline cursor.


### 3.3 ClapEntity


A `ClapEntity` represents an entity in the project, such as a character.


#### 3.3.1 Fields

- **id** (`string`): Unique identifier for the entity.

- **category** (`ClapSegmentCategory`): Category of the entity.

- **triggerName** (`string`): Trigger name associated with the entity.

- **label** (`string`): Label of the entity.

- **description** (`string`): Description of the entity.

- **author** (`string`): Author of the entity.

- **thumbnailUrl** (`string`): URL of the thumbnail image for the entity.

- **seed** (`number`): Random seed for generation.

- **imagePrompt** (`string`): Prompt for generating the image.

- **imageSourceType** (`ClapAssetSource`): Source type of the image.

- **imageEngine** (`string`): Engine used for generating the image.

- **imageId** (`string`): Identifier of the image.

- **audioPrompt** (`string`): Prompt for generating the audio.

- **audioSourceType** (`ClapAssetSource`): Source type of the audio.

- **audioEngine** (`ClapEntityAudioEngine`): Engine used for generating the audio.

- **audioId** (`string`): Identifier of the audio.

- **appearance** (`ClapEntityAppearance`): Appearance of the entity.


- **age** (`number`): Age of the entity.

- **region** (`ClapEntityRegion`): Region associated with the entity.

- **gender** (`ClapEntityGender`): Gender of the entity. Might be deprecated in the future and replaced by `ClapEntityAppearance`.


### 3.4 ClapScene


A `ClapScene` describes scenes within the project, including events.



#### 3.4.1 Fields



- **id** (`string`): Unique identifier for the scene. MUST follow the UUID v4 convention.

- **scene** (`string`): Description of the scene.

- **line** (`string`): Line identifier associated with the scene.

- **rawLine** (`string`): Raw line text.

- **sequenceFullText** (`string`): Full text of the sequence.

- **sequenceStartAtLine** (`number`): Start line of the sequence.

- **sequenceEndAtLine** (`number`): End line of the sequence.

- **startAtLine** (`number`): Start line of the scene.

- **endAtLine** (`number`): End line of the scene.

- **events** (`ClapSceneEvent[]`): List of events in the scene.



### 3.5 ClapSegment


A `ClapSegment` represents a segment of time within the timeline of the project, with an optional start and end time.


#### 3.5.1 Fields


- **id** (`string`): Unique identifier for the segment. MUST follow the UUID v4 convention.

- **track** (`number`): Track number of the segment. Overlapping segments are technically possible, but might not be desired (unless specific cases like multiple revisions of the same segment). It is up to the implementation to decide what to do with overlapping segments (eg. run an algorithm to un-overlap them).

- **startTimeInMs** (`number`): Start time in *milliseconds*. Optional.

- **endTimeInMs** (`number`): End time in *milliseconds*. Optional.

- **category** (`ClapSegmentCategory`): Category of the segment.

- **entityId** (`string`): Identifier of the associated entity.

- **sceneId** (`string`): Identifier of the associated scene.

- **prompt** (`string`): Prompt for generating content in the segment.

- **label** (`string`): Label of the segment.

- **outputType** (`ClapOutputType`): Output type of the segment.

- **renderId** (`string`): Identifier of the render.

- **status** (`ClapSegmentStatus`): Status of the segment.

- **assetUrl** (`string`): URL of the asset. See `ClapAssetSource` for the list of accepted asset sources.

- **assetDurationInMs** (`number`): Duration of the asset in *milliseconds*.

- **assetSourceType** (`ClapAssetSource`): Source type of the asset.

- **assetFileFormat** (`string`): File format of the asset (eg. mime type and/or codec).

- **createdAt** (`string`): Creation timestamp. Must be an *ISO 8601* date string.

- **createdBy** (`ClapAuthor`): Creator of the segment.

- **editedBy** (`ClapAuthor`): Editor of the segment.

- **outputGain** (`number`): Output gain value.

- **seed** (`number`): Random seed for generation. MUST be an integer between `0` and `2147483648`. If `0` or left empty a random seed will be used by the tools.


## 4. Serialization and Compression

The series of *YAML* documents MUST be serialized and then compressed using *gzip*. The resulting MIME type MUST be `application/x-gzip`.


### 4.1 YAML Serialization



Each component (`ClapHeader`, `ClapMeta`, `ClapEntity`, `ClapScene`, and `ClapSegment`) SHOULD be serialized to a separate YAML document. The following steps MUST be followed for serialization and compression:


1. Serialize each component to $YAML*.

2. Concatenate the *YAML* strings in the prescribed order.

3. Compress the combined string using the *gzip* compression algorithm.



## 5. Deserialization and Decompression



When reading an **OpenClap** file, the following steps MUST be adhered to:



1. Decompress the file using gzip.

2. Parse the YAML documents in the order they appear.

3. Construct the in-memory data structures representing `ClapProject` components.



## 6. Example



Below is an example **OpenClap** YAML structure before compression:



```yaml

- format: clap-0

  numberOfEntities: 1

  numberOfScenes: 1

  numberOfSegments: 1

- id: 123e4567-e89b-12d3-a456-426614174000

  title: Example Project

  description: An example AI video project.

  synopsis: This is an example synopsis.

  licence: MIT

  orientation: landscape

  durationInMs: 5000

  width: 1920

  height: 1080

  defaultVideoModel: SVD

  extraPositivePrompt: []

  screenplay: Example screenplay

  isLoop: false

  isInteractive: false

- id: 1

  category: character

  triggerName: trigger_1

  label: Example Entity

  description: An example entity.

  author: human

  thumbnailUrl: http://example.com/thumbnail.jpg

  seed: 12345

  imagePrompt: Example image prompt

  imageSourceType: REMOTE

  imageEngine: ExampleEngine

  imageId: img_001

  audioPrompt: Example audio prompt

  audioSourceType: REMOTE

  audioEngine: ExampleAudioEngine

  audioId: audio_001

  age: 25

  gender: male

  region: european

  appearance: friendly

- id: 1

  scene: Example Scene

  line: Line 1

  rawLine: Raw line 1

  sequenceFullText: Example full text

  sequenceStartAtLine: 1

  sequenceEndAtLine: 10

  startAtLine: 1

  endAtLine: 5

  events:

    - id: evt_001

      type: action

      description: Example action

      behavior: Neutral

      startAtLine: 1

      endAtLine: 2

- id: 1

  track: 0

  startTimeInMs: 1000

  endTimeInMs: 2000

  category: video

  entityId: 1

  sceneId: 1

  prompt: Example prompt

  label: Example label

  outputType: video

  renderId: render_001

  status: to_generate

  assetUrl: http://example.com/asset.mp4

  assetDurationInMs: 1000

  assetSourceType: REMOTE

  assetFileFormat: mp4

  createdAt: 2022-01-01T00:00:00Z

  createdBy: human

  editedBy: human

  outputGain: 0

  seed: 12345

```


## 7. Types and Enumerations



This section details the types and enumerations used in the **OpenClap** file format.



### 7.1 Enumerations


#### 7.1.1 ClapFormat

Describes the format version of the **OpenClap** file
:
- **CLAP_0**: "clap-0"

- **CLAP_1**: and greated values are reserved for future use.


Note: During the DRAFT phase of the specification, we will use `clap-0`

#### 7.1.2 ClapSegmentCategory

Defines categories for different types of segments.

- **SPLAT**: A Gaussian splatting scene. Can be a 4D Gaussian splatting clip (a. `.splatv` scene).

- **MESH**: A 3D mesh

- **DEPTH**: A depth map

- **EVENT**: an abstract event happening in the timeline (ie. not a visual or acoustic event).

- **INTERFACE**: an interface element, such as a title, subtitle, or UI button.

- **PHENOMENON**: a prompt defining a reaction to the scene. An analogy with conventional programming would be to say that this is an "event handler".

- **VIDEO**: A video clip. Can also be used as a storyboard, moodboard, or final video.

- **STORYBOARD**: a storyboard or moodboard.

- **TRANSITION**: a transition between two shots.

- **CHARACTER**: intervention of a character in the shot

- **LOCATION**: describe the location of a shot

- **TIME**: describe the time of a shot (roman era, modern, day, night, timezone etc)

- **ERA**: "era" (deprecated, use TIME instead)

- **LIGHTING**: "lighting"

- **WEATHER**: "weather"

- **ACTION**: "action"

- **MUSIC**: "music"

- **SOUND**: "sound"

- **DIALOGUE**: "dialogue"

- **STYLE**: "style"

- **CAMERA**: "camera"

- **GENERIC**: "generic"



#### 7.1.3 ClapMediaOrientation

Defines the orientation for media.

- **LANDSCAPE**: "landscape"

- **PORTRAIT**: "portrait"

- **SQUARE**: "square"

This is just a preference, the implementation is free to bypass this value (eg. use the user's device orientation instead)

#### 7.1.4 ClapOutputType

Defines output types for segments.

- **TEXT**: a plain text

- **ANIMATION**: (TODO: to be defined by @Julian)

- **INTERFACE**: (TODO: to be defined by @Julian)

- **EVENT**: (TODO: to be defined by @Julian)

- **PHENOMENON**: (TODO: to be defined by @Julian)

- **TRANSITION**: (TODO: to be defined by @Julian)

- **IMAGE**: an image output such as an image in `image/jpeg`, `image/png`, `image/webp` or `image/heic` formats.

- **VIDEO**: a video output such as a video in `video/mp4` or `video/webm` formats.

- **AUDIO**: an audio output such as a speech, sound or music track in `audio/mp3`, `audio/wav` formats.



#### 7.1.5 ClapSegmentStatus

Defines the status of a `ClapSegment`'s output (the `assetUrl`).

- **TO_GENERATE**: the output has not been generated yet

- **TO_INTERPOLATE**: (optional) a low-FPS version of the output has been generated, and FPS upscaling is still in progress

- **TO_UPSCALE**: (optional) a low-resolution version of the output has been generated, and upscaling is still in progress

- **COMPLETED**: the output has reached its final stage, and won't be updated further

- **ERROR**: the output generation has failed (depending on what your tool does, you might also with to set it back to `TO_GENERATE`) 


#### 7.1.6 ClapAssetSource

Defines the source type of assets.

- **REMOTE**: "REMOTE"

- **PATH**: "PATH"

- **DATA**: "DATA"

- **PROMPT**: "PROMPT"

- **EMPTY**: "EMPTY"



#### 7.1.7 ClapEntityGender

Defines gender types for entities.

- **male**

- **female**

- **person**

- **object**



#### 7.1.8 ClapEntityRegion

Defines regions for entities.

- **global**

- **american**

- **european**

- **british**

- **australian**

- **canadian**

- **indian**

- **french**

- **italian**

- **german**

- **chinese**



#### 7.1.9 ClapEntityAudioEngine

Defines audio engines for entities.

- **ElevenLabs**

- **XTTS**

- **Parler-TTS**

- **OpenVoice**



#### 7.1.10 ClapSegmentFilteringMode

Defines filtering mode for segments.

- **START**: start of a segment must be within the range.

- **END**: end of a segment must be within the range.

- **ANY**: any end of a segment must be within the range.

- **BOTH**: both ends of a segment must be within the range.



### 7.2 Types

This section describes the various type definitions used in the **OpenClap** format.



#### 7.2.1 ClapAuthor

Defines the type of author.

- **"auto"**: automatically edited using basic if/else logical rules

- **"ai"**: edited using a large language model

- **"human"**: edited by a human

- **string**: custom author type



#### 7.2.2 ClapVoice

Describes the attributes of a voice.

- **name** (string)

- **gender** (ClapEntityGender)

- **age** (number)

- **region** (ClapEntityRegion)

- **timbre** (string: "high" | "neutral" | "deep")

- **appearance** (string)

- **audioEngine** (ClapEntityAudioEngine)

- **audioId** (string)



#### 7.2.3 ClapSceneEvent

Describes an event within a scene.

- **id** (string): Unique identifier for the event.

- **type** (string: "description" | "dialogue" | "action"): Type of the event.

- **character** (string): Character involved in the event, if applicable.

- **description** (string): Description of the event.

- **behavior** (string): Behavior during the event.

- **startAtLine** (number): Starting line of the event.

- **endAtLine** (number): Ending line of the event.



#### 7.2.4 ClapHeader, ClapMeta, ClapEntity, ClapScene, ClapSegment

These types have been described in the sections 3.1 to 3.5 respectively.



## 8. Compliance and Future Considerations



### 8.1 Compliance

To ensure compliance with the OpenClap specification, implementations MUST strictly adhere to the prescribed serialization, structure, and compression processes as defined in this document. Non-compliant implementations MAY result in files that are not interoperable with existing tools and programs designed to read or write OpenClap files.



### 8.2 Extensibility

Future versions of this specification MAY introduce new fields or types. Existing fields MAY evolve. Backwards compatibility SHALL be maintained whenever possible. Developers are encouraged to closely follow updates to the specification to ensure ongoing compatibility with future versions.



## 9. Security Considerations



Implementations that read, write, or otherwise process OpenClap files SHOULD be aware of potential security risks, especially when handling remote or file path assets as defined by the `ClapAssetSource` type. Specifically:



- **REMOTE**: Ensure proper validation of all URLs to mitigate potential exploits, such as URL redirection attacks.

- **PATH**: Handle file paths with care to prevent potential directory traversal attacks and ensure paths are valid and do not expose sensitive files on the filesystem.

- **DATA**: Validate `data:` URIs to prevent data injection or related security issues.



An implementation processing OpenClap files MUST ensure that untrusted files and sources do not compromise system security.
