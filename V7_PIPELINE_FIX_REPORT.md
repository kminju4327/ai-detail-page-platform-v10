# BRAND ENGINE V7 최종 생성 파이프라인 수정 보고서

## 확인된 실제 실행 경로

`이 템플릿으로 생성` → `handleGenerateWithTemplate()` → `App.jsx` 내부 `runPipeline()` → `generationPromptV8.js`

따라서 현재 화면의 최종 생성은 `src/services/pipeline.js`가 아니라 `App.jsx` 내부 파이프라인을 사용합니다.

## 실제 원인

1. API 키가 없으면 `App.jsx` 내부 `callClaude()`가 Mock을 실행합니다.
2. Mock은 프롬프트 문자열에서 `제품명`, `제품 분류`를 다시 추출했습니다.
3. `extractPromptField()`의 정규식에서 `\\s`가 올바르게 이스케이프되지 않아 제품 분류를 읽지 못했습니다.
4. 분류 추출 실패 시 `mainCategory = "신선식품"`이 기본값으로 사용됐습니다.
5. 그 결과 베르베린과 루테인도 Fresh Food Brain으로 생성됐습니다.

## 함께 발견된 연결 오류

- 제품 분석 Mock이 객체를 반환하는데 `JSON.parse(analysisResult)`를 실행해 항상 파싱 실패 처리됐습니다.
- AI 설계 결과인 `pageDesign`이 `buildDetailPageGenerationPrompt()`에 전달되지 않았습니다.
- App 내부 Mock과 `src/services/claudeClient.js` Mock이 중복 존재합니다. 현재 화면에서는 App 내부 Mock이 실제 실행됩니다.

## 적용한 수정

- 제품 분류 추출 정규식 수정
- Mock 호출 시 프롬프트 재해석 대신 구조화된 `product` 객체를 직접 전달
- `analysis`, `generation`, `compliance`, `remediation`, `regenerate` 단계 구분 추가
- 분석 결과가 객체 또는 JSON 문자열 모두 처리되도록 수정
- `pageDesign`을 최종 V8 생성 프롬프트에 전달
- 건강·영양식품 Mock은 제품명과 카테고리에 맞는 Brain을 직접 사용
- 신선식품 Mock의 Placeholder 문장과 자동 조사 조합 제거
- 프로덕션 빌드 검증 완료

## 남은 구조 개선 권장사항

현재 앱에는 두 생성 체계가 공존합니다.

- 실제 화면: `App.jsx` 내부 `runPipeline()` / `callClaude()`
- 별도 모듈: `src/services/pipeline.js` / `src/services/claudeClient.js`

이번 수정은 실제 실행 경로를 우선 정상화했습니다. 다음 단계에서는 두 체계를 하나의 서비스 모듈로 통합해야 향후 수정 파일이 엇갈리지 않습니다.
