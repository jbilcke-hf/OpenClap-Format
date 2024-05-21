
## Addendum

## 1. Before you read

The following IS NOT a proposal to add OpenClap to AAF,
but simply a private study done using GPT-4 to see how it could be possible one day to convert an OpenClap file to AAF.

## 1.1 Advanced Authoring Format (AAF) Compatibility

To ensure compatibility with the Advanced Authoring Format (AAF), we outline the necessary mapping between OpenClap and the AAF structures. This section provides guidelines on how OpenClap’s data model can integrate with AAF's existing specifications.

### 1.2 Mapping OpenClap Data Model to AAF

AAF is a professional file interchange format designed for the exchange of digital media and metadata. It incorporates a sophisticated data model to support the interchange of authored media data, including timelines, metadata, and media essence.

This section describes how the various elements of the OpenClap data model can be mapped to the AAF data structure.

#### 1.2.1 ClapHeader and AAF Descriptive Metadata

The `ClapHeader` in OpenClap can be mapped to AAF’s descriptive metadata to indicate the format version and counts of entities, scenes, and segments.


- **ClapHeader.format** -> AAF `Identification.Description`

- **ClapHeader.numberOfEntities** -> AAF `MobID`

- **ClapHeader.numberOfScenes** -> AAF `MobID`

- **ClapHeader.numberOfSegments** -> AAF `MobID`



#### 1.2.2 ClapMeta and AAF Content Description

The `ClapMeta` object in OpenClap includes metadata about the project which can be mapped to various AAF metadata fields.



- **ClapMeta.id** -> AAF `Identification.InstanceUID`

- **ClapMeta.title** -> AAF `Mob.Name`

- **ClapMeta.description** -> AAF `Mob.Comment`

- **ClapMeta.synopsis** -> AAF `DescriptiveObjectFramework`

- **ClapMeta.licence** -> AAF `ContentStorage`

- **ClapMeta.orientation** -> Custom descriptive metadata in AAF

- **ClapMeta.durationInMs** -> AAF `Mob.Sequence.Length`

- **ClapMeta.width** and **ClapMeta.height** -> AAF `DescriptiveClip.FrameLayout`

- **ClapMeta.defaultVideoModel** -> Custom taxonomy in AAF metadata

- **ClapMeta.extraPositivePrompt** -> AAF `CommentMarker`

- **ClapMeta.screenplay** -> Custom descriptive metadata in AAF

- **ClapMeta.isLoop** -> Custom descriptive metadata in AAF

- **ClapMeta.isInteractive** -> Custom descriptive metadata in AAF



#### 1.2.3 ClapEntity and AAF Composition Mob

The `ClapEntity` object in OpenClap can correspond to the `Composition Mob` in AAF, which represents logical groupings of media assets.



- **ClapEntity.id** -> AAF `Composition Mob UID`

- **ClapEntity.category** -> AAF `User defined data or Custom subclass`

- **ClapEntity.triggerName** -> AAF `Slot.EditRate`

- **ClapEntity.label** -> AAF `MobSlot.Name`

- **ClapEntity.description** -> AAF `MobSlot.Comment`

- **ClapEntity.author** -> AAF `DescriptiveMarkerText`

- **ClapEntity.thumbnailUrl** -> Custom metadata field linked to a locator in AAF

- **ClapEntity.seed** -> Custom descriptive metadata in AAF

- **ClapEntity.imagePrompt** -> Custom descriptive metadata in AAF

- **ClapEntity.imageSourceType** -> AAF `Locator`

- **ClapEntity.imageEngine** -> AAF `CodecDefinition`

- **ClapEntity.imageId** -> Custom metadata in AAF

- **ClapEntity.audioPrompt** -> Custom descriptive metadata in AAF

- **ClapEntity.audioSourceType** -> AAF `Locator`

- **ClapEntity.audioEngine** -> AAF `CodecDefinition`

- **ClapEntity.audioId** -> Custom metadata in AAF

- **ClapEntity.age** -> Custom descriptive metadata in AAF

- **ClapEntity.gender** -> Custom taxonomy in AAF

- **ClapEntity.region** -> Custom taxonomy in AAF

- **ClapEntity.appearance** -> Custom descriptive metadata in AAF



#### 1.2.4 ClapScene and AAF Event Mob

The `ClapScene` can be represented as AAF `Event MobReferences`.



- **ClapScene.id** -> AAF `MobID`

- **ClapScene.scene** -> Custom descriptive metadata in AAF

- **ClapScene.line** -> AAF `Timeline Track`

- **ClapScene.rawLine** -> Custom descriptive metadata in AAF

- **ClapScene.sequenceFullText** -> Custom descriptive metadata in AAF

- **ClapScene.sequenceStartAtLine** -> AAF `SourceClip.StartTime`

- **ClapScene.sequenceEndAtLine** -> AAF `SourceClip.Length`

- **ClapScene.startAtLine** -> AAF `Event.StartTime`

- **ClapScene.endAtLine** -> AAF `Timeline Track`



The `ClapSceneEvent` type in OpenClap maps to AAF’s events within a timeline track:



- **ClapSceneEvent.id** -> AAF `Event UID`

- **ClapSceneEvent.type** -> Custom marker in AAF (e.g., EventType)

- **ClapSceneEvent.character** -> AAF `Descriptive Marker Text`

- **ClapSceneEvent.description** -> AAF `Event Comment`

- **ClapSceneEvent.behavior** -> Custom descriptive metadata

- **ClapSceneEvent.startAtLine** and **ClapSceneEvent.endAtLine** -> AAF `Timeline Track`



#### 1.2.5 ClapSegment and AAF Source Clips

`ClapSegment` aligns with AAF's `Source Clips` and related components.



- **ClapSegment.id** -> AAF `SourceClip UID`

- **ClapSegment.track** -> AAF `Timeline Track Number`

- **ClapSegment.startTimeInMs** -> AAF `SourceClip.StartTime`

- **ClapSegment.endTimeInMs** -> AAF `SourceClip.Length`

- **ClapSegment.category** -> User-defined metadata in AAF

- **ClapSegment.entityId** -> AAF `SourceMob SourceID`

- **ClapSegment.sceneId** -> AAF `MobID`

- **ClapSegment.prompt** -> Custom descriptive metadata in AAF

- **ClapSegment.label** -> AAF `MobSlot.Name`

- **ClapSegment.outputType** -> Custom classification in AAF

- **ClapSegment.renderId** -> Custom descriptive metadata in AAF

- **ClapSegment.status** -> Custom metadata in AAF

- **ClapSegment.assetUrl** -> AAF `Locator.Location`

- **ClapSegment.assetDurationInMs** -> AAF `Descriptive Marker Length`

- **ClapSegment.assetSourceType** -> AAF `Locator.Type`

- **ClapSegment.assetFileFormat** -> Custom classification in AAF

- **ClapSegment.createdAt** -> AAF `MobID.CreationTime`

- **ClapSegment.createdBy** -> User-defined metadata in AAF

- **ClapSegment.editedBy** -> User-defined metadata in AAF

- **ClapSegment.outputGain** -> Custom field in AAF

- **ClapSegment.seed** -> User-defined metadata in AAF


### 1.3 Implementation Considerations

While mapping OpenClap data to AAF:

1. **Data Translation**: Ensure that all non-standard fields in OpenClap are correctly translated to AAF's “user-defined” fields or through custom metadata extensions in AAF.

2. **Application Specifications**: Follow AAF's application-specific guidelines to maintain integrity and compatibility of the data.

3. **Interface Specifications**: Use AAF’s interface specifications to handle APIs and data interchange effectively.


### 1.4 Example Mapping


Here is an example of how a simple `ClapMeta` object would map to AAF metadata fields:

**ClapMeta JSON:**


```yaml

id: "123e4567-e89b-12d3-a456-426614174000"

title: "Example Project"

description: "An example AI video project."

synopsis: "This is an example synopsis."

licence: "MIT"

orientation: "landscape"

durationInMs: 5000

width: 1920

height: 1080

defaultVideoModel: "SVD"

extraPositivePrompt: []

screenplay: "Example screenplay"

isLoop: false

isInteractive: false

```


**AAF Metadata Mapping:**


```

AAF Identification.InstanceUID = "123e4567-e89b-12d3-a456-426614174000"

AAF Mob.Name = "Example Project"

AAF Mob.Comment = "An example AI video project."

AAF DescriptiveObjectFramework = "This is an example synopsis."

AAF ContentStorage = "MIT"

Custom Metadata Field (Orientation) = "landscape"

AAF Mob.Sequence.Length = 5000

AAF DescriptiveClip.FrameLayout = [Width: 1920, Height: 1080]

Custom Metadata Field (DefaultVideoModel) = "SVD"

Custom Descriptive Metadata (Extra Positive Prompt) = []

Custom Metadata Field (Screenplay) = "Example screenplay"

Custom Metadata Field (IsLoop) = false

Custom Metadata Field (IsInteractive) = false

```
