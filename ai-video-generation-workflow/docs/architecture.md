# Architecture Notes

## Overview

This project keeps both major workflow generations because the change between them is itself part of the engineering story.

The important difference is not simply "Seedance 2.0 versus Seedance 2.5." The important difference is how the surrounding system changed when the underlying model's capabilities changed.

## V1 — segmented pipeline

### Constraint

The target output was a 20–30 second promotional video, while the generation model was better suited to shorter clips.

### Design response

The workflow therefore introduced a segment-oriented generation layer:

1. Generate structured scene / narration segments.
2. Preserve per-segment context and metadata.
3. Submit one generation task per segment.
4. Save the returned task ID.
5. Poll the provider asynchronously.
6. Apply timeout / retry handling.
7. Collect completed video URLs.
8. Pass the clips to a local FFmpeg endpoint.
9. Concatenate clips into the final artifact.

### Trade-offs

**Advantages**
- Reached the desired total duration despite model limitations.
- Made failure visible at the segment level.
- Allowed individual segments to be regenerated or inspected.

**Costs**
- More nodes and state transitions.
- More points of failure.
- More complex context propagation.
- A separate FFmpeg service was required.
- Visual and narrative continuity between independently generated clips could vary.

## V2 — direct generation

### Changed assumption

Once longer native video generation became practical, the original duration workaround was no longer the best architecture.

### Design response

The workflow was simplified so that the model could generate a longer output directly, eliminating unnecessary segmentation and concatenation steps.

### Benefits

- Fewer external calls and state transitions.
- Less polling and metadata plumbing.
- No mandatory FFmpeg concatenation stage.
- Easier debugging and maintenance.
- Lower orchestration complexity.

## Engineering takeaway

The main lesson from the two versions is that architecture should follow constraints, not nostalgia.

V1 was appropriate because the model constraint was real. V2 became better precisely because it removed code and workflow logic that no longer solved an existing problem.

This repository intentionally preserves both versions to make that evolution visible.
