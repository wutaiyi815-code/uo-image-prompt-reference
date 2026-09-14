# Single Step Prompt Catalog

This catalog contains atomic tasks only. Apply one entry at a time. The user defines how multiple entries should be combined or ordered.

## Shared Prompt Rules

- Inspect the image content before writing the prompt.
- State only the requested visual change and the details that must remain unchanged.
- Use visible evidence for model identity, garment type, color, material, accessories, pose, composition, and background.
- For garment replacement and recolor, identify the product silhouette with a concise fit term when visible, such as `regular`, `fitted`, `boxy`, `oversized`, `baggy`, or `jorts`.
- For front and three-quarter model images, normally use the product front image. For a back-view model image, use the product back image. Override this only when visual inspection proves another reference is the accurate product side.
- Do not include workflow commentary or execution constraints in the text sent to the image model.

## Add Three Quarter Side View

**Task ID:** `missing-side-view`

**Use when:** A front-facing or near-front model image exists and the requested result is a full-body three-quarter side view.

**References:**

- Figure 1: front-facing or near-front model image.

**Prompt template:**

```text
保持原图模特的身份、面部特征、发型、身材比例、服装款式、服装颜色、材质细节、鞋子、背景和棚拍光线不变。仅调整模特的身体朝向和站姿：让模特的身体转为接近正侧面的 3/4 侧视角，身体朝向画面右侧，相对于镜头侧转约 70–80 度。镜头主要看到身体正面，同时保留少量身体侧面，不要变成完全 90 度的纯正面。肩部和胯部朝向一致，模特保持自然放松的动作。保持全身完整入镜，平视机位，弱透视、自然棚拍比例。除视角和姿态外，不要修改任何人物或服装细节。
```

**Output role:** Full-body three-quarter side-view model image.

**Processor compatibility template:**

**Task ID:** `missing-side-view-processor-legacy`

```text
Adjust the model to a three-quarter side view in a full-body shot, with both arms hanging naturally by the sides and the head facing directly forward. Maintain the original head-to-body ratio and framing while ensuring the overall pose is natural and harmonious.
```

This compatibility entry records the processor's former built-in wording. Prefer `missing-side-view` unless the user explicitly requests the legacy template.

## Add Front View

**Task ID:** `missing-front-view`

**Use when:** A product-accurate model image exists in another angle and the requested result is a full-body front view.

**References:**

- Figure 1: product-accurate model image used as the identity, garment, and styling source.

**Prompt template:**

```text
Adjust the character's pose to a full-body front view, with weight on one leg and a natural standing posture.
```

**Output role:** Full-body front-view model image.

## Add Back View

**Task ID:** `missing-back-view`

**Use when:** A front-facing or near-front product-accurate model image exists and the requested result is a model back view.

**References:**

- Figure 1: front-facing or near-front product-accurate model image.
- Figure 2: product back image.

**Prompt template:**

```text
Generate a back view of the model. The Product design worn by the model in the rear view matches the back view of the product. Do not alter the cut or fit (looseness) of the tops and bottoms. Do not change the background, angle, or shot size of the original image. Avoid fine noise, high-frequency textures, gritty grain, or cluttered, intricate details.
```

This is the updated back-view prompt supplied in `Temp.docx`.

**Output role:** Model back-view image.

### Processor Back View Compatibility

Use these only when retaining the processor's three-reference contract:

- Figure 1: product-accurate front model image.
- Figure 2: product front flat lay.
- Figure 3: product back flat lay.

**Task ID:** `missing-back-view-gpt-simple`

```text
生成背视图，人物重心放在其中一条腿上。
```

**Task ID:** `missing-back-view-gemini-simple`

```text
生成全身景别的背视图，人物重心放在其中一条腿上。
```

## Add Model Pose

**Task ID:** `adding-pose`

**Use when:** An existing model image should retain its person, styling, product, and setting while changing to a newly specified pose.

**References:**

- Figure 1: model image to edit.

**Writing method:** Analyze Figure 1 and explicitly retain its visible identity, hairstyle, garment, accessories, framing, and setting. Describe one concrete, anatomically feasible pose, including weight distribution, leg position, hand placement, head direction, and expression only where relevant.

**Example:**

```text
Keep the same female model, facial identity, long dark hair, white-and-black cap, black graphic cropped T-shirt, faded baggy jeans, brown belt, tan boots, necklace, and blue handbag. Preserve the original full-body vertical street-fashion composition and urban cafe entrance setting. Create a new calm pose: stand upright with weight resting on the left leg, right knee softly relaxed and toe angled slightly outward; place the right hand naturally inside the front jeans pocket while the left hand holds the blue handbag low by her side. Turn her head gently toward the camera with a composed neutral expression.
```

Adapt every visible attribute and pose instruction to the actual image. Do not copy the example's clothing, accessories, setting, or pose unless they match the task.

**Output role:** Model image with the requested new pose.

### Arms Down Standing Pose

**Task ID:** `adding-pose-arms-down`

**References:**

- Figure 1: model image to edit.

```text
将图中人物动作更改为双手放下，自然垂于身体两侧，并调整为立正站姿。
```

## Product Try On

**Task ID:** `product-wear`

**Use when:** A product image must replace the corresponding garment worn by a model.

**References:**

- Figure 1: model image.
- Figure 2: correct product image for the model's visible side.

**Prompt template:**

```text
Replace the {source garment and color} worn by the {person} in Figure 1 with the {target garment and color} from Figure 2, maintaining the {fit term} fit and silhouette shown in Figure 2, while keeping the model's pose, posture, other clothing, and overall composition from Figure 1 unchanged.
```

Visually determine the garment noun and fit term. Mention specific structure only when it helps preserve the product, such as length, waistband, sleeve shape, hem, or leg width.

**Processor compatibility template:**

```text
Replace the {garment} worn by the person in Figure 1 with the {garment} from Figure 2, preserving the Figure 2 {fit notes} fit, keeping the person's pose and all other clothing from Figure 1 unchanged.
```

**Output role:** Model image wearing the product from Figure 2.

## Change Product Color

**Task ID:** `change-color`

**Use when:** The model already wears the same product style but in a different color.

**References:**

- Figure 1: model image to recolor.
- Figure 2: same product in the correct color and matching visible side.

**Prompt template:**

```text
图1模特穿的{产品}与图2的款式一致，但是颜色不同。请将图1的{产品}配色改为与图2一致。
```

When the product has multiple colors, stripes, panels, cords, trims, or contrasting details, name only the color relationships needed to reproduce Figure 2 accurately. Do not turn a color-only task into garment replacement unless visual inspection shows that the products are not actually the same style.

**Processor compatibility template:**

```text
Change the color of the {garment} worn by the person in Figure 1 to match the {garment} in Figure 2, preserving the Figure 2 {fit notes} fit, while keeping the silhouette, pattern placement, trims, and decorative details consistent with Figure 2.
```

**Output role:** Model image with product colors matching Figure 2.

## Replace Model Identity

**Task ID:** `same-person`

**Use when:** Multiple model images should use the same person's identity while retaining each target image's pose.

**References:**

- Figure 1: target image whose pose and composition must be retained.
- Figure 2: identity reference model.

**Prompt template:**

```text
将图1的模特替换为图2的模特，并保持图1的动作不变。
```

**Output role:** Figure 1 with the identity from Figure 2 and the original Figure 1 pose.

## Remove Arm Tattoo

**Task ID:** `remove-arm-tattoo`

**Use when:** The requested edit is limited to removing a visible tattoo from the model's arm.

**References:**

- Figure 1: model image containing the arm tattoo.

**Prompt template:**

```text
去除图中模特手臂上的纹身，其他内容保持不变。
```

**Output role:** Model image with the arm tattoo removed.
