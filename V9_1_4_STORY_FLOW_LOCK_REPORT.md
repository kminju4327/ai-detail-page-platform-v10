# BRAND ENGINE V9.1.4 Story Flow Lock

## 적용 내용

- 기존 V7 입력, AI 설계, 프로젝트 저장, 템플릿, Legacy Renderer 유지
- AI 설계의 pageStructure 순서를 우선 보존
- 제품군별 필수 섹션이 빠졌을 때만 자동 보충
- 베르베린: 원료 → 배합 → 품질 → 섭취 → CTA
- 오메가3: EPA·DHA 함량 → 원료 → 품질 → 섭취 → CTA
- 신선식품: 산지 → 품종 → 수확·선별 → 포장·배송 → 보관 → CTA
- 신선식품 포장·배송 전용 intent 및 카피 추가
- 제품군별 CTA 분리
- Hero 카피 패턴 라이브러리 확장
- Repetition Engine V2: 제목·본문 전체 유사도 검사 및 반복 완화
- Smart Card 문자열 호환 유지
- V9 오류 시 Legacy Renderer 자동 복귀 유지

## 모드 전환

기본값: `VITE_RENDERER_MODE=v9`

Legacy 복귀: `VITE_RENDERER_MODE=legacy`
