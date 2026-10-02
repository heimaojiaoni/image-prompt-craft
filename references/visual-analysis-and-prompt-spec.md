# Visual analysis and prompt specification

Read this reference whenever a supplied image must be reverse-described or selectively edited. The measurements below are estimates from a 2D image, not recovered production metadata. Use only the dimensions relevant to the image, but cover each applicable dimension in the quantitative readout.

## Evidence and confidence

Use three evidence labels internally and, when useful, in the readout:

- **Measured:** pixel dimensions, aspect ratio, normalized subject/face bounds, visible crop edges, or a color sampled from an available raster.
- **Estimated:** screen-space pose, visual proportions, apparent lens class, light direction, texture scale, palette shares, and focus zones.
- **Unknown:** true focal length, camera-subject distance without a scale reference, exposure metadata, hidden surfaces, exact physical material values, and obscured joint positions.

For estimates, use a range and confidence (`high / medium / low`) when it changes the prompt. Do not overburden the user with a confidence table for obvious features. If a number would be false precision, state a directional or qualitative estimate instead.

## Copy-ready prompt with inline mathematical parameters (required output)

The deliverable is a fluent Chinese prompt whose visual details carry their parameters inline. The copy-ready prompt itself must contain the measurements; a parameter table followed by ordinary prose does not satisfy the request. Avoid tables, stacked `key=value` fields, JSON-like blocks, and long metadata headers. Write coherent paragraphs in visual order, placing a compact parenthetical formula directly after the feature it qualifies. For example:

> 竖版封面构图（画幅 `R=9:16`；主体框 `B_s=(x,y,w,h)≈(0.56,0.04,0.40,0.91)`）；人物中心约 `C_s=(0.76,0.49)`，头顶留白 `m_top≈4%`。镜头轻微俯拍（`Pitch≈+8°`，估计；`Yaw≈0°`、`Roll≈0°`），近景透视自然，真实焦距未知。人物微笑强度 `I_smile≈0.3/1`，躯干后倾约 `12°–18°`……`

> 柔光从画面左上方进入（方位约 `10点钟`，软度高），阴影对比约 `1.6:1`（估计）；主色比例约为暖白 `40%`、粉色 `35%`、灰紫 `20%`、金色点缀 `5%`。脸部清晰度 `S_face≈4/5`、背景 `S_bg≈1/5`……

The examples illustrate inline formatting, not fixed values or a mandatory tag vocabulary. Use concise notation that a person and an image model can both read: `R=...` for ratio, `B=(x,y,w,h)` for normalized bounding boxes, `C=(x,y)` for centers, `Pitch/Yaw/Roll` in degrees, `I∈[0,1]` for expression/texture intensity, `r∈[0,1]` for visual roughness estimates, and `S∈[0,5]` for sharpness/background complexity. Define coordinates as normalized from the upper-left. Keep each formula adjacent to its Chinese description and distribute formulas across the relevant sentences; do not collect them into a parameter dump.

Cover every applicable requested dimension within the prose: canvas and crop; subject bounds and occupancy; shot size, relative distance, focal range if supported, camera pitch/yaw/roll; visible pose and joint angles; face proportions and expression; hair grouping and skin microtexture; material response/roughness; key-light direction and approximate ratio; color proportions and HEX references; regional sharpness; background complexity; post-processing; and requested changes versus preserved details. Use `≈` or “估计” for inferred values, add a short confidence cue when uncertainty matters, and write “未知/无法从单图推断” where physical data cannot be recovered. Never invent precision to fill a field. If a dimension truly does not apply, omit it rather than inserting `N/A` noise.

After the positive prose, add one concise `负面约束：` line in Chinese. It should protect source features and prevent likely failures. For edits, state the requested change and preserved features in fluent prose, with relevant values repeated inline only where that helps maintain control. The quantitative readout before the prompt may be brief, but never let it replace the parameter-rich prompt itself. Do not add model-specific flags unless the user names the target model and its syntax is known.

## Quantitative visual readout

Use a compact card or short grouped list. Normalize x/y coordinates to `[0,1]` from the upper-left; report percentages when easier to read. Keep the same image coordinate convention throughout.

### Frame, composition, and camera

- **Canvas:** width × height when available; aspect ratio as `w:h`; orientation. State whether borders or a crop are visible.
- **Subject occupancy:** subject bounding box `(x, y, width, height)` normalized; center point; approximate fraction of canvas area and fraction of canvas height. Identify crop intersections (head, limbs, clothing) and margins. For multiple subjects, give one box per subject.
- **Composition:** subject center of mass, head/eye line, horizon or dominant vanishing lines, negative-space regions, foreground occlusion, and any rule-of-thirds or symmetry relationship. Use normalized coordinates or percentages when useful.
- **Shot size and camera distance:** describe the visible crop (close-up, medium, full figure, etc.). Physical camera distance is unknown without scale or metadata; use a relative distance class unless a grounded range is possible.
- **Focal length / perspective:** estimate an equivalent focal-length *range* only from perspective cues (facial/nose exaggeration, near-far size change, edge stretch, background compression). Label it low/medium/high confidence. Lens equivalence is not reliably recoverable from a single image; if weakly supported, describe perspective as wide / neutral / compressed and leave millimeters unknown.
- **Camera pitch, yaw, roll:** estimate in degrees or ranges only where the view supports it. Pitch sign: positive means camera tilted down; negative means tilted up. Yaw sign: positive means viewpoint is to the subject's image-right; negative means image-left. Roll sign: positive means the horizon rises toward image-right. Describe the visible low/high/eye-level angle even if a degree estimate is weak.
- **Subject head and torso orientation:** keep separate from camera yaw. For screen-space head yaw, positive means the face turns toward image-right and negative toward image-left; report head pitch (chin up/down) and roll separately. Include torso turn/lean and the visible shoulder/pelvis axis.

### Pose, face, hair, and expression

- **Pose and joints:** describe the action first, then estimate visible joint angles in degrees (shoulder, elbow, wrist, hip, knee, ankle as applicable). State the angle convention when giving a number (interior flexion angle); use ranges. Include torso lean/twist, weight-bearing leg, hand shape, gesture, and foreshortening. Mark occluded joints unknown rather than completing them from imagination.
- **Face proportions:** give face-box width/height, eye-line height, interocular spacing as a fraction of face width, and approximate nose-to-mouth/chin relationships when resolution supports them. Describe eye tilt/shape, brow, nose, lips, jaw, and asymmetry from visible evidence. Do not prescribe a different “ideal” face.
- **Expression intensity:** use a simple `0–1` estimate for salient cues (brow tension, eyelid openness, mouth tension, smile, gaze intensity); one overall estimate is enough when cues are consistent. Name gaze direction and emotional tone separately from intensity.
- **Hair grouping:** capture outer silhouette, parting, length, volume, flow direction, and a small number of major locks/groups (for example, 3–7 visible clumps only when useful). Describe flyaways and strand separation at the level visible in the source; do not invent strand counts.
- **Skin microtexture:** describe pore/grain scale relative to face size and output resolution (fine / medium / coarse, or a pixel-scale range if the image is actually available at adequate resolution). Note visible texture strength and retouching. Avoid claims about pores hidden by blur, makeup, or low resolution.

### Materials, light, color, focus, and finish

- **Material response:** separate skin, hair, cloth, leather, metal, glass, and other visible surfaces. Describe sheen, highlight width, reflection sharpness, transparency, weave, and edge wear. If using a PBR-like roughness value, label it as a visual rendering estimate on `0–1` (0 smooth/glossy, 1 diffuse/matte); prefer a range or words such as satin/matte over false precision.
- **Lighting:** estimate key direction in image coordinates or clock position, elevation, apparent source size/softness, fill direction, rim/backlight, shadow direction, and contrast. A key-to-fill ratio or stop difference is approximate visual intent, not measured studio data. Name ambient/bounce contribution and color cast where visible.
- **Color distribution:** list the few dominant color families and approximate shares summing to 100% (round to 5% or 10%). Include HEX references only when sampled from accessible pixels or clearly label them as visual approximations. Identify local accent colors and separate subject from background color when useful.
- **Sharpness by region:** rate face/eyes, hair, hands, clothing, and background as relative sharpness zones or `0–5` scores (0 softest, 5 sharpest). Include depth of field, focus plane, motion blur, grain, and any intentional softening. Avoid requesting uniformly maximal sharpness when the source has selective focus.
- **Background complexity:** rate `0–5` (0 blank, 5 highly layered/detailed); describe scene category, major shapes, horizon, depth planes, blur, and object placement. Preserve object positions and negative space.
- **Post-processing:** infer only visible effects: contrast curve, saturation, white balance, highlight roll-off, shadow tint, bloom/halation, haze, vignette, grain/noise, sharpening, chromatic aberration, and retouching. Use relative/qualitative settings unless a numeric value can be grounded; do not invent application-specific sliders.
- **Negative constraints:** turn locked source features and likely failures into a short, specific list (for example, no camera-angle drift, no changed hand gesture, no costume redesign, no extra fingers, no invented background objects). Include only relevant exclusions.

## Choosing style language from evidence

Identify medium from visible evidence before writing style terms. Examples of evidence include natural optical blur, realistic skin highlight roll-off, lens distortion and sensor grain for photography; depth separation and painted edge transitions for 2.5D; consistent specular response and volumetric form for 3D CG; flat fills and controlled outlines for cel shading; or visible brush marks, layered pigment, and selective edge loss for painting. Images can mix media. Describe the actual mix rather than forcing one category.

Use plain visual descriptors that explain what to render. A style name can be included if the evidence or user request supports it, but never let a stock label replace the observed palette, line treatment, material, lighting, focus, or finish. For a requested style conversion, update only traits essential to that style and explicitly retain the source's geometric staging unless the user specifies otherwise.

## Edit scope ledger

Before drafting, write an internal two-column inventory:

| Change as requested | Preserve from source |
| --- | --- |
| Character identity, identifying hair/eyes/accessories/motifs/colors | Camera, crop, position, perspective, pose, body proportions, garment construction, lighting, background, material response, finish |

Replace the generic example fields with the actual request. Treat exact named attributes as changes even under a broad “everything else unchanged” instruction. For “only replace the character,” constrain the change to character identity cues and fit signature motifs/colors into existing clothing shapes. For a style-only request, keep all geometry fixed and change only rendering attributes necessary to achieve that style. If both a broad style word and specific style parameters are provided, honor the named parameters and preserve every other unmentioned field.

## Prompt assembly

Write the positive prompt as one coherent description, ordered for reliable generation:

1. Main subject, number of subjects, and explicit requested identity/edit.
2. Canvas, crop, subject bounds/placement, shot size, camera view, perspective, and lens range if supported.
3. Pose, visible joint relationships, gesture, body/head orientation, gaze, and expression.
4. Face, hair groups, costume construction, identifying details, accessories, and materials.
5. Background layout and complexity.
6. Light direction/softness/contrast, palette shares or HEX approximations, focus/sharpness zones, texture, and post-processing.
7. Preservation language that guards the highest-risk unchanged attributes.

Use one separate negative line. It should reinforce concrete constraints, not introduce new style instructions that conflict with the positive prompt. For transformation tasks, follow the prompt with a brief change/preserved ledger; include only meaningful fields, not a duplicate of every sentence.
