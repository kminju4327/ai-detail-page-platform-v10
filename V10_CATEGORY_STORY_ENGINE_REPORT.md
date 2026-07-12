# BRAND ENGINE V10 Category Story Engine

## 기준 버전
- BRAND_ENGINE_V9_1_4_story_flow_lock_stackblitz
- 기존 입력, 저장, 템플릿, Legacy fallback, 건강식품 전용 로직 유지

## 추가된 핵심 구조
- `src/engines/categoryStoryEngine.js`
- 상위 카테고리와 하위 카테고리를 분석해 Story Key 결정
- AI 설계 화면과 최종 V9 Renderer가 동일한 Category Story를 사용
- 카테고리별 필수 Story Flow Lock
- 카테고리별 핵심 특징, 설득 포인트, 추천 고객, 디자인, 추천 이미지 생성

## 지원 Story
- 건강·영양식품: 기존 일반 건강식품 / 건강기능식품 전문 Brain 유지
- 신선식품
- 축산물
- 수산물
- 가공식품
- 면·간편식
- 베이커리·디저트
- 음료
  - 커피 전용 분기
  - 차 전용 분기
  - 기타 음료
- 간식
- 조미료·식재료
- 선물세트
- 기타 식품 fallback

## 주요 수정
- 커피: 원산지·원두 → 로스팅 → 향미 → 추출 → 보관 → 페어링
- 수산물: 원산지 → 어획·양식 → 신선도·유통 → 손질 → 포장 → 조리 → 보관
- 축산물: 원산지 → 사육·생산 → 등급 → 부위 → 숙성·손질 → 포장 → 보관
- 카테고리와 맞지 않는 공통 건강식품 문구 제거
- Smart Card와 구매 기준도 카테고리 Story 기준으로 생성
- 한국어 조사 오류 일부 보정 (`베르베린는` 방지)
- 최종 Renderer 메타 버전 `10.0.0`

## 검증
- `npm ci` 성공
- `npm run build` 성공
- Vite 5.4 프로덕션 빌드 완료
