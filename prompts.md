# 테스트에 쓴 프롬프트 모음

노트북 `pose_tool.ipynb` 4번 셀과 같은 내용입니다. 실행하면 `samples/prompts_used.json` 에 같은 내용이 저장됩니다.

- **BASE_01** 은 확정된 기준안(BASE-01) 원문입니다. 수정하지 않습니다.
- 실험 2 변형(P2_*)은 "같은 포즈에서 프롬프트만 바꾸는 실험"을 위한 것이며, BASE-01 자체를 바꾸는 것이 아닙니다.
- Klein 4B는 증류 모델이라 negative prompt와 guidance를 쓰지 않습니다 (4스텝, guidance_scale=1.0).
- 경로 A에서는 아래 장면 프롬프트 앞에 `A_sketch` 또는 `A_openpose` 지시문을 붙입니다. 경로 B 다듬기에서는 `B_refine` 를 붙입니다.

## 시드
- BASE-01 후보: 101, 202, 303
- 실험 2·3: 101, 202 (조건만 바꾸고 시드는 같게)

## 실험 배치

| 실험 | 포즈 | 프롬프트 |
|---|---|---|
| BASE-01 (경로 A) | pose_01 | BASE_01 × {스케치 참조, 뼈대 참조} |
| BASE-01 (경로 B) | pose_01 | BASE_01 (SDXL 초안 → Klein 다듬기) |
| 실험 2: 같은 포즈, 다른 프롬프트 | pose_01 | BASE_01, P2_1_SUBJECT, P2_2_STYLE, P2_3_CONFLICT |
| 실험 3: 같은 프롬프트, 다른 포즈 | pose_01, pose_02, pose_03 | BASE_01, BASE_01_NEUTRAL |

실험 2·3은 BASE-01에서 점수가 가장 높았던 **A-openpose** 경로로 실행했습니다.

## 프롬프트 ↔ 결과 파일 ↔ 측정값 (Colab T4, 2026-11-13 실행)

파일 이름 규칙: `samples/<폴더>/<포즈>__<프롬프트 키>__<경로>__s<시드>.png` (+ `_verify.png` 검증 오버레이). 판정과 PCK는 `samples/results.json`의 값 그대로이며, 자동 2D 관절 비교 결과입니다(눈으로 본 판정은 README 참고).

| 실험 | 프롬프트 키 | 포즈 | 경로 | 폴더 | seed 101 | seed 202 | seed 303 | 평균 PCK |
|---|---|---|---|---|---|---|---|---|
| BASE-01 | BASE_01 | pose_01 | A-sketch | base01_runs | FAIL 0.36 | FAIL 0.21 | FAIL 0.29 | 0.29 |
| BASE-01 | BASE_01 | pose_01 | A-openpose | base01_runs | PARTIAL 0.64 | FAIL 0.36 | PARTIAL 0.57 | 0.52 |
| BASE-01 | BASE_01 (SDXL 판) | pose_01 | B-draft | pathB_runs | PARTIAL 0.50 | FAIL 0.07 | FAIL 0.00 | 0.19 |
| BASE-01 | BASE_01 | pose_01 | B | pathB_runs | FAIL 0.43 | FAIL 0.00 | FAIL 0.29 | 0.24 |
| 실험 2 | BASE_01 | pose_01 | A-openpose | exp2_same_pose | PARTIAL 0.64 | FAIL 0.36 | - | 0.50 |
| 실험 2 | P2_1_SUBJECT | pose_01 | A-openpose | exp2_same_pose | PARTIAL 0.50 | FAIL 0.36 | - | 0.43 |
| 실험 2 | P2_2_STYLE | pose_01 | A-openpose | exp2_same_pose | PARTIAL 0.57 | FAIL 0.43 | - | 0.50 |
| 실험 2 | P2_3_CONFLICT | pose_01 | A-openpose | exp2_same_pose | FAIL 0.21 | FAIL 0.29 | - | 0.25 |
| 실험 3 | BASE_01 | pose_01 | A-openpose | exp3_same_prompt | PARTIAL 0.64 | FAIL 0.36 | - | 0.50 |
| 실험 3 | BASE_01 | pose_02 | A-openpose | exp3_same_prompt | PARTIAL 0.79 | PARTIAL 0.50 | - | 0.64 |
| 실험 3 | BASE_01 | pose_03 | A-openpose | exp3_same_prompt | PARTIAL 0.64 | SUCCESS 0.86 ※ | - | 0.75 |
| 실험 3 | BASE_01_NEUTRAL | pose_01 | A-openpose | exp3_same_prompt | PARTIAL 0.86 | PARTIAL 0.64 | - | 0.75 |
| 실험 3 | BASE_01_NEUTRAL | pose_02 | A-openpose | exp3_same_prompt | SUCCESS 1.00 | SUCCESS 1.00 | - | 1.00 |
| 실험 3 | BASE_01_NEUTRAL | pose_03 | A-openpose | exp3_same_prompt | PARTIAL 0.71 | PARTIAL 0.57 | - | 0.64 |

※ 자동 SUCCESS이지만 눈으로 보면 신체가 합쳐진 기형입니다(README 실험 3 참고).
대표 이미지: `output_01.png` = BASE_01 / A-openpose / s101 (PARTIAL), `output_02.png` = NEUTRAL / pose_02 / s101 (SUCCESS), `output_03.png` = NEUTRAL / pose_03 / s101 (자동 PARTIAL, 눈으로는 앉지 않아 실패).

## 장면 프롬프트

### BASE_01
- 바꾼 것: 확정 기준 프롬프트 (대조군)

**Klein 4B**
```
A 1990s retro film photograph, YouTube-thumbnail style eye-catching composition, 16:9 wide shot, front view, full body visible from head to feet. A woman in her twenties wearing a black yoga top and blue yoga leggings is in the center of a yoga studio surrounded by mirrors on all walls. She holds a deliberately bizarre, unstable pose. On the left side of the frame stands a wooden bench; only her right hip is perched on the very edge of the bench, and her right leg is folded cross-legged on top of the bench. Her left leg is planted firmly on the floor on the right side of the frame, slightly bent, supporting her weight. Her upper body leans strongly toward the right side of the frame, forming a dramatic diagonal line, and she hugs a large, fluffy pillow that sticks out from the right edge of the frame at chest height, held up by a sturdy steel stand underneath. One cheek is pressed into the pillow, with half of her face still visible in profile. Both arms wrap around the pillow, with elbows and wrists clearly visible and not hidden behind her torso. She looks like she is lying face-down, sitting, and standing on one leg all at once. Warm yellow lighting, soft film grain, slightly faded colors, gentle vintage softness, natural skin texture. No text, no logos.
```

**SDXL (경로 B 초안, 77토큰 이내)**
```
1990s film photo, young woman in black yoga top and blue yoga leggings, mirrored yoga studio, wooden bench on left, big fluffy pillow on steel stand on right, warm yellow light, film grain, full body, front view
```

### BASE_01_NEUTRAL
- 바꾼 것: BASE_01에서 자세 문장만 제거

**Klein 4B**
```
A 1990s retro film photograph, YouTube-thumbnail style eye-catching composition, 16:9 wide shot, front view, full body visible from head to feet. A woman in her twenties wearing a black yoga top and blue yoga leggings is in the center of a yoga studio surrounded by mirrors on all walls. On the left side of the frame stands a wooden bench. A large, fluffy pillow sticks out from the right edge of the frame at chest height, held up by a sturdy steel stand underneath. Warm yellow lighting, soft film grain, slightly faded colors, gentle vintage softness, natural skin texture. No text, no logos.
```

**SDXL (경로 B 초안, 77토큰 이내)**
```
1990s film photo, young woman in black yoga top and blue yoga leggings, mirrored yoga studio, wooden bench on left, big fluffy pillow on steel stand on right, warm yellow light, film grain, full body, front view
```

### P2_1_SUBJECT
- 바꾼 것: 인물·복장만 변경 (50대 남성, 회색 정장)

**Klein 4B**
```
A 1990s retro film photograph, YouTube-thumbnail style eye-catching composition, 16:9 wide shot, front view, full body visible from head to feet. A man in his fifties wearing a gray business suit and a white shirt is in the center of a yoga studio surrounded by mirrors on all walls. He holds a deliberately bizarre, unstable pose. On the left side of the frame stands a wooden bench; only his right hip is perched on the very edge of the bench, and his right leg is folded cross-legged on top of the bench. His left leg is planted firmly on the floor on the right side of the frame, slightly bent, supporting his weight. His upper body leans strongly toward the right side of the frame, forming a dramatic diagonal line, and he hugs a large, fluffy pillow that sticks out from the right edge of the frame at chest height, held up by a sturdy steel stand underneath. One cheek is pressed into the pillow, with half of his face still visible in profile. Both arms wrap around the pillow, with elbows and wrists clearly visible and not hidden behind his torso. He looks like he is lying face-down, sitting, and standing on one leg all at once. Warm yellow lighting, soft film grain, slightly faded colors, gentle vintage softness, natural skin texture. No text, no logos.
```

**SDXL (경로 B 초안, 77토큰 이내)**
```
1990s film photo, middle-aged man in gray business suit, mirrored yoga studio, wooden bench on left, big fluffy pillow on steel stand on right, warm yellow light, film grain, full body, front view
```

### P2_2_STYLE
- 바꾼 것: 질감·조명만 변경 (현대 디지털 사진, 차가운 백색 주광)

**Klein 4B**
```
A sharp modern digital photograph, YouTube-thumbnail style eye-catching composition, 16:9 wide shot, front view, full body visible from head to feet. A woman in her twenties wearing a black yoga top and blue yoga leggings is in the center of a yoga studio surrounded by mirrors on all walls. She holds a deliberately bizarre, unstable pose. On the left side of the frame stands a wooden bench; only her right hip is perched on the very edge of the bench, and her right leg is folded cross-legged on top of the bench. Her left leg is planted firmly on the floor on the right side of the frame, slightly bent, supporting her weight. Her upper body leans strongly toward the right side of the frame, forming a dramatic diagonal line, and she hugs a large, fluffy pillow that sticks out from the right edge of the frame at chest height, held up by a sturdy steel stand underneath. One cheek is pressed into the pillow, with half of her face still visible in profile. Both arms wrap around the pillow, with elbows and wrists clearly visible and not hidden behind her torso. She looks like she is lying face-down, sitting, and standing on one leg all at once. Cool white daylight, crisp details, vivid clean colors, high dynamic range, natural skin texture. No text, no logos.
```

**SDXL (경로 B 초안, 77토큰 이내)**
```
modern digital photo, young woman in black yoga top and blue yoga leggings, mirrored yoga studio, wooden bench on left, big fluffy pillow on steel stand on right, cool white daylight, sharp, full body, front view
```

### P2_3_CONFLICT
- 바꾼 것: 자세 문장을 '똑바로 선 자세'로 바꿔 포즈 조건과 충돌시킴

**Klein 4B**
```
A 1990s retro film photograph, YouTube-thumbnail style eye-catching composition, 16:9 wide shot, front view, full body visible from head to feet. A woman in her twenties wearing a black yoga top and blue yoga leggings is in the center of a yoga studio surrounded by mirrors on all walls. She stands upright between the bench and the pillow with both feet on the floor and her arms relaxed at her sides. On the left side of the frame stands a wooden bench. A large, fluffy pillow sticks out from the right edge of the frame at chest height, held up by a sturdy steel stand underneath. Warm yellow lighting, soft film grain, slightly faded colors, gentle vintage softness, natural skin texture. No text, no logos.
```

**SDXL (경로 B 초안, 77토큰 이내)**
```
1990s film photo, young woman in black yoga top and blue yoga leggings, mirrored yoga studio, wooden bench on left, big fluffy pillow on steel stand on right, warm yellow light, film grain, full body, front view, standing upright, arms at sides
```

## 경로별 지시문 (장면 프롬프트 앞에 붙임)

### A_sketch
```
Image 1 is a flat color pose-and-layout sketch, not a style reference. Copy only the body pose and the positions of the objects from it. In the sketch the black block is the torso, the red limbs are the person's right arm and right leg, the blue limbs are the left arm and left leg, the brown table is the bench, the white rounded block is the pillow and the gray post is the steel stand. Generate a new, fully photographic image in which the person holds exactly this pose and the objects are in the same places. Do not draw the sketch, colored limbs, lines or dots. Scene: 
```

### A_openpose
```
Image 1 is an OpenPose skeleton: colored lines and dots on black marking the joints of one person (head, shoulders, elbows, wrists, hips, knees, ankles). It is a pose guide only. Generate a new, fully photographic image in which the person's joints are at the same positions as in the skeleton. Do not draw the skeleton, lines, dots or the black background. Scene: 
```

### B_refine
```
Image 1 is a draft photo. Keep the person's exact body pose, limb positions, head position, camera framing and the positions of the bench, the pillow and the steel stand unchanged. Only improve realism, anatomy, clothing, lighting and texture to match this description: 
```

### SDXL negative prompt
```
text, logo, watermark, extra limbs, extra legs, extra arms, missing limbs, deformed hands, multiple people, cropped body, cartoon, drawing, stick figure
```
