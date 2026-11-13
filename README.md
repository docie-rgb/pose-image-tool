# 원하는 포즈로 이미지 만드는 도구

## 도구 설명
- 관절 좌표와 소품(벤치·베개·강철 받침대) 배치를 **사람이 직접 지정**해 포즈 조건 이미지를 만듭니다. 그 조건으로 FLUX.2-klein 4B가 같은 자세의 사진을 생성합니다.
- 생성된 사진에서 OpenPose 뼈대를 다시 추출해 목표 뼈대와 비교합니다(PCK와 BASE-01 자세 규칙). 이렇게 해서 "정말 같은 자세인지"를 숫자로 남깁니다.
- 사용법 한 줄: `pose_tool.ipynb`를 Colab GPU 런타임에서 열고, 3-5 셀에서 포즈를 승인한 뒤 위에서부터 순서대로 실행합니다.

### 두 가지 경로
| 경로 | 방식 | 정식 ControlNet? |
|---|---|---|
| **A (기본)** | 포즈 스케치 또는 OpenPose 뼈대를 Klein 4B의 **참조 이미지(`image=`)** 로 넣고, 프롬프트로 자세를 따르라고 지시 | **아니오.** Klein 4B에는 공식 OpenPose ControlNet이 없습니다 |
| **B (A가 실패할 때)** | SDXL + `xinsir/controlnet-openpose-sdxl-1.0`으로 자세를 고정한 초안을 만들고, Klein 4B가 질감을 다듬음 | SDXL 단계만 정식 ControlNet |

## 사용법
1. GitHub의 `pose_tool.ipynb`를 Colab에서 엽니다. [런타임] → [런타임 유형 변경] → GPU(T4 이상)를 고릅니다.
2. 0~3번 셀을 실행합니다. `samples/pose_01_layout.png`에서 뼈대·벤치·베개·받침대 위치를 확인하고, 3-5 셀에서 `POSE_APPROVED = True`로 바꿉니다.
3. 4~10번 셀을 실행합니다. 모델 로드 → BASE-01 생성(경로 A) → 필요하면 경로 B 순서입니다.
4. 11~15번 셀을 실행합니다. 실험 2·3 → 요약 → zip 다운로드 순서입니다. 받은 `samples/`를 이 저장소에 올립니다.
5. 마지막 마크다운 셀(16번)에 관찰을 적습니다.

## 폴더 구조
```
pose-image-tool/
├── README.md
├── pose_tool.ipynb          # Colab 튜토리얼 노트 (셀마다 "이 셀이 하는 일" 주석)
├── prompts.md               # BASE-01 원문 + 실험 변형 프롬프트 + 경로별 지시문
└── samples/
    ├── pose_01.png              # BASE-01 포즈 참조 이미지 (소품 + 마네킹 스케치, 경로 A 입력)
    ├── pose_01_openpose.png     # 같은 포즈의 OpenPose 뼈대 (경로 B 입력)
    ├── pose_01_layout.png       # 사람 확인용 (격자·좌표·관절/소품 이름)
    ├── pose_01_keypoints.json   # 관절 좌표 + 소품 상자
    ├── pose_02*, pose_03*       # 실험 3용 비교 포즈 (선 자세 손 들기 / 벤치에 바르게 앉기)
    ├── output_01.png / .json / _verify.png   # BASE-01 결과 (실행 후 생성)
    ├── output_02.png, output_03.png          # 실험 3 결과 (실행 후 생성)
    ├── base01_runs/, pathB_runs/, exp2_same_pose/, exp3_same_prompt/   # 모든 생성 결과 + 검증 이미지
    ├── *_grid.png               # 비교 그리드
    ├── results.json             # 생성·검증 전체 기록
    └── results_summary.md       # 조건별 PCK 요약표
```

## 사용 모델과 라이선스
| 모델 | 용도 | 라이선스 | 크기 |
|---|---|---|---|
| `black-forest-labs/FLUX.2-klein-4B` | 경로 A 생성, 경로 B 다듬기 | Apache-2.0 | 생성 7.8GB + 텍스트 인코더 8.0GB |
| `stabilityai/stable-diffusion-xl-base-1.0` (fp16) | 경로 B 초안 | CreativeML Open RAIL++-M | 6.9GB |
| `xinsir/controlnet-openpose-sdxl-1.0` | 경로 B 포즈 조건 | Apache-2.0 | 2.5GB |
| `lllyasviel/Annotators` body_pose_model | 결과 검증(OpenPose 재추출) | CMU OpenPose, 비상업 | 0.2GB |

텍스트 인코더와 생성 모델을 따로 올립니다. 경로 B 모델도 Klein과 번갈아 올립니다. 그래서 GPU에 한 번에 올라가는 것은 10GB 안팎이고, 무료 Colab T4(15GB)를 기준으로 짰습니다.

## 테스트 결과
> **아직 GPU에서 실행하지 않았습니다.** 아래는 실행 후 `samples/results_summary.md`와 각 `_verify.png`를 보고 채웁니다.
> 자세가 재현되지 않으면 다른 자세로 바꾸지 않고, 실패한 그대로 차이와 원인을 적습니다.

- 포즈 1 (BASE-01 복합 자세) → 프롬프트: BASE_01 → 경로 ___ → 결과: (판정 / PCK / 어긋난 관절)
- 포즈 2 (선 채로 오른손 들기) → 프롬프트: BASE_01_NEUTRAL → 결과:
- 포즈 3 (벤치에 바르게 앉기) → 프롬프트: BASE_01_NEUTRAL → 결과:

## 한계 (실행 전 예상, 실행 후 확인해서 고쳐 쓸 것)
- 경로 A는 정식 ControlNet이 아니라서 자세를 따르는 정도가 보장되지 않습니다. 커뮤니티 테스트에서도 "대략 비슷한 정도"라는 평가가 있습니다.
- BASE-01처럼 관절이 겹치고 한쪽 다리가 접힌 자세는 생성 모델과 OpenPose 검출기 모두 틀리기 쉽습니다. 거울 반사 때문에 사람이 여러 명으로 검출될 수도 있습니다.
- 경로 B의 SDXL은 프롬프트를 77토큰까지만 읽습니다. 그리고 Klein으로 다듬는 단계에서 자세가 다시 흐트러질 수 있어서, 초안과 최종 결과를 따로 검증합니다.
- PCK는 2D 관절 위치만 비교합니다. 손가락, 발목 방향, 얼굴을 묻은 정도는 `_verify.png`를 눈으로 확인합니다.
