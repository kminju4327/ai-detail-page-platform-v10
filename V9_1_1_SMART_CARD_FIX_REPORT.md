# BRAND ENGINE V9.1.1 Smart Card Fix

## 수정 원인
V9 Smart Card Engine은 `{ label, value }` 객체 배열을 반환하지만 기존 V7 미리보기·복사·HTML 내보내기 로직은 문자열 배열을 기대했습니다. 이 때문에 최종 결과에 `[object Object]`가 출력되었습니다.

## 수정 내용
- Smart Card 객체를 `원료 · 값` 형태의 문자열로 정규화해 기존 UI와 완전 호환
- 향후 카드 전용 UI 확장을 위해 원본 객체는 `section.cards`에 보존
- `smart_cards`, `trust`, `cta`, `section` 타입의 화면 라벨을 한국어로 추가
- Mock 메타 버전을 `9.1.1`로 갱신

## 유지 사항
제품 입력, AI 설계, 템플릿, 저장, 기존 Legacy Renderer 및 자동 Fallback은 변경하지 않았습니다.
