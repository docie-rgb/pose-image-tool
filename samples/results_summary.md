# 결과 요약 (자동 생성)

경로 A = FLUX.2 Klein 4B 참조 이미지 방식 (**정식 ControlNet 아님**) / 경로 B = SDXL OpenPose ControlNet 초안 + Klein 4B 다듬기

## 조건별 평균 PCK

- BASE-01 경로 A-스케치: 평균 PCK 0.29 (n=3)
- BASE-01 경로 A-뼈대: 평균 PCK 0.52 (n=3)
- BASE-01 경로 B-초안(SDXL): 평균 PCK 0.19 (n=3)
- BASE-01 경로 B-다듬기: 평균 PCK 0.24 (n=3)
- 실험2 BASE_01: 평균 PCK 0.50 (n=2)
- 실험2 P2_1_SUBJECT: 평균 PCK 0.43 (n=2)
- 실험2 P2_2_STYLE: 평균 PCK 0.50 (n=2)
- 실험2 P2_3_CONFLICT: 평균 PCK 0.25 (n=2)
- 실험3 pose_01 × BASE_01: 평균 PCK 0.50 (n=2)
- 실험3 pose_02 × BASE_01: 평균 PCK 0.64 (n=2)
- 실험3 pose_03 × BASE_01: 평균 PCK 0.75 (n=2)
- 실험3 pose_01 × BASE_01_NEUTRAL: 평균 PCK 0.75 (n=2)
- 실험3 pose_02 × BASE_01_NEUTRAL: 평균 PCK 1.00 (n=2)
- 실험3 pose_03 × BASE_01_NEUTRAL: 평균 PCK 0.64 (n=2)

## 전체 기록

| 실험 | 포즈 | 프롬프트 | 경로 | 시드 | 판정 | PCK | 좌우반전 PCK | BASE 규칙 | 경고 | 파일 |
|---|---|---|---|---|---|---|---|---|---|---|
| base | pose_01 | BASE_01 | A-openpose | 101 | PARTIAL | 0.64 | 0.29 | 7/9 | missing joints: l_elbow, l_wrist | samples/base01_runs/pose_01__BASE_01__A-openpose__s101.png |
| base | pose_01 | BASE_01 | A-openpose | 202 | FAIL | 0.36 | 0.57 | 6/9 | missing joints: l_elbow, l_wrist; left/right swapped? | samples/base01_runs/pose_01__BASE_01__A-openpose__s202.png |
| base | pose_01 | BASE_01 | A-openpose | 303 | PARTIAL | 0.57 | 0.29 | 7/9 | missing joints: l_elbow, l_wrist; 3 people detected (mirror reflection?) | samples/base01_runs/pose_01__BASE_01__A-openpose__s303.png |
| base | pose_01 | BASE_01 | A-sketch | 101 | FAIL | 0.36 | 0.21 | 6/9 | missing joints: l_ankle | samples/base01_runs/pose_01__BASE_01__A-sketch__s101.png |
| base | pose_01 | BASE_01 | A-sketch | 202 | FAIL | 0.21 | 0.50 | 6/9 | left/right swapped? | samples/base01_runs/pose_01__BASE_01__A-sketch__s202.png |
| base | pose_01 | BASE_01 | A-sketch | 303 | FAIL | 0.29 | 0.14 | 5/9 | missing joints: l_elbow, l_wrist; 2 people detected (mirror reflection?) | samples/base01_runs/pose_01__BASE_01__A-sketch__s303.png |
| base | pose_01 | BASE_01 | B | 101 | FAIL | 0.43 | 0.14 | 6/9 | missing joints: l_elbow, l_wrist | samples/pathB_runs/pose_01__BASE_01__B__s101.png |
| base | pose_01 | BASE_01 | B | 202 | FAIL | 0.00 | 0.00 | 5/9 | missing joints: r_shoulder, r_elbow, r_wrist, l_shoulder, l_elbow, l_wrist | samples/pathB_runs/pose_01__BASE_01__B__s202.png |
| base | pose_01 | BASE_01 | B | 303 | FAIL | 0.29 | 0.29 | 5/9 | missing joints: l_shoulder, l_elbow, l_wrist | samples/pathB_runs/pose_01__BASE_01__B__s303.png |
| base | pose_01 | BASE_01 | B-draft | 101 | PARTIAL | 0.50 | 0.29 | 5/9 | missing joints: l_elbow, l_wrist, l_ankle | samples/pathB_runs/pose_01__BASE_01__B-draft__s101.png |
| base | pose_01 | BASE_01 | B-draft | 202 | FAIL | 0.07 | 0.14 | 2/9 | missing joints: r_elbow, r_wrist, l_knee, l_ankle | samples/pathB_runs/pose_01__BASE_01__B-draft__s202.png |
| base | pose_01 | BASE_01 | B-draft | 303 | FAIL | 0.00 | 0.00 | 3/9 | missing joints: nose, r_ankle | samples/pathB_runs/pose_01__BASE_01__B-draft__s303.png |
| exp2 | pose_01 | BASE_01 | A-openpose | 101 | PARTIAL | 0.64 | 0.29 | 7/9 | missing joints: l_elbow, l_wrist | samples/exp2_same_pose/pose_01__BASE_01__A-openpose__s101.png |
| exp2 | pose_01 | BASE_01 | A-openpose | 202 | FAIL | 0.36 | 0.57 | 6/9 | missing joints: l_elbow, l_wrist; left/right swapped? | samples/exp2_same_pose/pose_01__BASE_01__A-openpose__s202.png |
| exp2 | pose_01 | P2_1_SUBJECT | A-openpose | 101 | PARTIAL | 0.50 | 0.29 | 7/9 | missing joints: l_elbow, l_wrist; 2 people detected (mirror reflection?) | samples/exp2_same_pose/pose_01__P2_1_SUBJECT__A-openpose__s101.png |
| exp2 | pose_01 | P2_1_SUBJECT | A-openpose | 202 | FAIL | 0.36 | 0.29 | 7/9 | missing joints: l_elbow, l_wrist | samples/exp2_same_pose/pose_01__P2_1_SUBJECT__A-openpose__s202.png |
| exp2 | pose_01 | P2_2_STYLE | A-openpose | 101 | PARTIAL | 0.57 | 0.29 | 7/9 | missing joints: l_elbow, l_wrist | samples/exp2_same_pose/pose_01__P2_2_STYLE__A-openpose__s101.png |
| exp2 | pose_01 | P2_2_STYLE | A-openpose | 202 | FAIL | 0.43 | 0.50 | 7/9 | missing joints: l_shoulder, l_elbow, l_wrist | samples/exp2_same_pose/pose_01__P2_2_STYLE__A-openpose__s202.png |
| exp2 | pose_01 | P2_3_CONFLICT | A-openpose | 101 | FAIL | 0.21 | 0.29 | 4/9 | missing joints: l_elbow, l_wrist, r_knee, r_ankle, l_knee, l_ankle; 5 people detected (mirror reflection?) | samples/exp2_same_pose/pose_01__P2_3_CONFLICT__A-openpose__s101.png |
| exp2 | pose_01 | P2_3_CONFLICT | A-openpose | 202 | FAIL | 0.29 | 0.29 | 4/9 | missing joints: r_wrist, l_wrist, l_knee, l_ankle; 3 people detected (mirror reflection?) | samples/exp2_same_pose/pose_01__P2_3_CONFLICT__A-openpose__s202.png |
| exp3 | pose_01 | BASE_01 | A-openpose | 101 | PARTIAL | 0.64 | 0.29 | 7/9 | missing joints: l_elbow, l_wrist | samples/exp3_same_prompt/pose_01__BASE_01__A-openpose__s101.png |
| exp3 | pose_01 | BASE_01 | A-openpose | 202 | FAIL | 0.36 | 0.57 | 6/9 | missing joints: l_elbow, l_wrist; left/right swapped? | samples/exp3_same_prompt/pose_01__BASE_01__A-openpose__s202.png |
| exp3 | pose_01 | BASE_01_NEUTRAL | A-openpose | 101 | PARTIAL | 0.86 | 0.43 | 7/9 | 2 people detected (mirror reflection?) | samples/exp3_same_prompt/pose_01__BASE_01_NEUTRAL__A-openpose__s101.png |
| exp3 | pose_01 | BASE_01_NEUTRAL | A-openpose | 202 | PARTIAL | 0.64 | 0.21 | 7/9 | missing joints: r_wrist, l_wrist; 2 people detected (mirror reflection?) | samples/exp3_same_prompt/pose_01__BASE_01_NEUTRAL__A-openpose__s202.png |
| exp3 | pose_02 | BASE_01 | A-openpose | 101 | PARTIAL | 0.79 | 0.14 | - | missing joints: l_wrist; 2 people detected (mirror reflection?) | samples/exp3_same_prompt/pose_02__BASE_01__A-openpose__s101.png |
| exp3 | pose_02 | BASE_01 | A-openpose | 202 | PARTIAL | 0.50 | 0.50 | - | - | samples/exp3_same_prompt/pose_02__BASE_01__A-openpose__s202.png |
| exp3 | pose_02 | BASE_01_NEUTRAL | A-openpose | 101 | SUCCESS | 1.00 | 0.14 | - | 3 people detected (mirror reflection?) | samples/exp3_same_prompt/pose_02__BASE_01_NEUTRAL__A-openpose__s101.png |
| exp3 | pose_02 | BASE_01_NEUTRAL | A-openpose | 202 | SUCCESS | 1.00 | 0.14 | - | 2 people detected (mirror reflection?) | samples/exp3_same_prompt/pose_02__BASE_01_NEUTRAL__A-openpose__s202.png |
| exp3 | pose_03 | BASE_01 | A-openpose | 101 | PARTIAL | 0.64 | 0.14 | - | 2 people detected (mirror reflection?) | samples/exp3_same_prompt/pose_03__BASE_01__A-openpose__s101.png |
| exp3 | pose_03 | BASE_01 | A-openpose | 202 | SUCCESS | 0.86 | 0.14 | - | 2 people detected (mirror reflection?) | samples/exp3_same_prompt/pose_03__BASE_01__A-openpose__s202.png |
| exp3 | pose_03 | BASE_01_NEUTRAL | A-openpose | 101 | PARTIAL | 0.71 | 0.14 | - | 4 people detected (mirror reflection?) | samples/exp3_same_prompt/pose_03__BASE_01_NEUTRAL__A-openpose__s101.png |
| exp3 | pose_03 | BASE_01_NEUTRAL | A-openpose | 202 | PARTIAL | 0.57 | 0.14 | - | 3 people detected (mirror reflection?) | samples/exp3_same_prompt/pose_03__BASE_01_NEUTRAL__A-openpose__s202.png |