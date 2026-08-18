# Generation Modes and Platform Adaptation

<!-- JIEKE AI SHOTLAB -->

## Text-to-video

Build both the visual state and the motion. Start with one subject and one primary action. Add optics, light, atmosphere, and sound only when they support the action.

## Image-to-video

Treat the supplied image as the first-frame authority for subject, composition, architecture, identity, palette, and light. Describe movement first: camera path, subject motion, environmental motion, time progression, and final state. If a source face, hand, or edge is visibly defective, recommend repairing the source before generation.

## First and last frame

Treat both images as fixed anchors. Describe the transition path, occlusion, speed curve, and invariants. Do not invent a third scene that conflicts with either anchor.

## Reference image and identity

Use a compact identity anchor containing age range, body shape, face geometry, hair, wardrobe, and one distinctive mark. Repeat it exactly when the platform supports reference images or seeds; do not promise absolute consistency from text alone.

## Audio

Write sound as separate sentences for ambience, action sounds, off-screen sound, dialogue, music, and silence. Keep dialogue short enough for the duration and direct the intention, pauses, breath, listening, and post-line reaction.

## Platform neutrality

When no generator is named, avoid proprietary controls, fixed seeds, unsupported negative fields, and assumed audio capabilities. Platform-specific settings belong in an optional section and must be labeled as such.
