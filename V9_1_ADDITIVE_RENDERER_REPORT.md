# BRAND ENGINE V9.1 Additive Renderer

## 구현 원칙
- 기존 입력, AI 설계, 템플릿, 프로젝트 저장, 컴플라이언스 및 Legacy Mock 코드를 삭제하지 않았습니다.
- V9 Renderer는 Mock 최종 생성 단계에만 추가되었습니다.
- V9 오류 발생 시 기존 Legacy Renderer로 자동 복귀합니다.
- `VITE_RENDERER_MODE=legacy`로 설정하면 기존 Renderer만 사용할 수 있습니다.

## 추가 파일
- `src/engines/v9/intentEngine.js`
- `src/engines/v9/copyLibrary.js`
- `src/engines/v9/smartCardEngine.js`
- `src/engines/v9/repetitionEngine.js`
- `src/engines/v9/rendererEngine.js`

## 포함 기능
1. AI 설계의 pageStructure를 Intent로 변환
2. 제품별 소비자용 카피 생성
3. 히어로와 첫 섹션 중복 제거
4. EPA·DHA, 원료, 품질, 섭취 정보 기반 Smart Card
5. null/undefined/빈 값 제거
6. 반복 표현 완화
7. Legacy 자동 Fallback

## 검증
- `npm install` 성공
- `npm run build` 성공
- StackBlitz용 공개 npm registry 설정 포함
