# BRAND ENGINE V7 — Intent-to-Copy Mock Renderer

## 변경 목적
API 없이 개발하는 Mock 단계에서 AI 설계 문장을 최종 상세페이지에 그대로 복사하지 않고, 설계의 의도만 이어받아 소비자용 카피로 변환합니다.

## 역할 분리
- AI 설계: 무엇을 말할지 결정
- Mock Renderer: 소비자에게 어떻게 말할지 결정
- Template: 색상, 폰트, 레이아웃, 카드, 이미지 배치만 담당

## 주요 수정
1. pageStructure의 제목·설명을 `choice / amount / ingredient / formula / quality / usage / closing` 의도로 분류
2. 제품군별 구매자 관점의 제목과 본문으로 변환
3. `입력된`, `확인된 정보만`, `추측하지 않음` 같은 내부 지시 문구가 최종 카피에 노출되지 않도록 분리
4. EPA·DHA, 순도, 실제 함량 등은 값이 있을 때만 출력
5. null, 빈 문자열, 문자열 "null"을 모든 리스트에서 제거
6. 최종 CTA가 없을 경우 자동 추가
7. 원래 AI 설계 의도는 `source_intent`에 남겨 디버깅 가능

## Mock 메타
최종 JSON의 `mock_meta.mode`가 `intent-to-copy`이면 새 Renderer가 사용된 것입니다.
