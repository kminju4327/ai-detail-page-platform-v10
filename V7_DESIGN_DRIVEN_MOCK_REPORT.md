# BRAND ENGINE V7 — Design-Driven Mock 수정 보고서

## 수정 목표
API 키가 없는 Mock 모드에서 Mock가 제품별 상세페이지 문장을 임의로 창작하지 않고, AI 설계 결과(`pageDesign`)를 최종 상세페이지 JSON으로 조립하도록 변경했습니다.

## 변경된 최종 생성 흐름

제품 입력
→ Brain 기반 AI 설계 생성
→ `pageDesign` 생성
→ 템플릿 선택
→ Mock 최종 생성
→ `pageDesign.pageStructure` 순서와 내용을 그대로 상세페이지 JSON으로 변환

## Mock가 사용하는 AI 설계 필드
- `pageDesign.aiSummary` → Hero 서브카피 및 분석 정보
- `pageDesign.productDiagnosis.strongestPoint` → Hero 핵심 기준
- `pageDesign.pageStructure` → 최종 섹션 순서·제목·본문
- `pageDesign.productFeatures` → 핵심 특징 목록
- `pageDesign.purchasePoints` 또는 `persuasionPoints` → 선택 기준
- `pageDesign.designReason` → 포지셔닝 정보

## 제거한 동작
- Mock 내부의 베르베린·루테인·홍삼·사과별 별도 카피 창작
- Brain 이름에 따라 Fresh Food 문장을 새로 만드는 최종 생성 분기
- 최종 Mock가 AI 설계와 무관한 고정 문장을 삽입하는 구조

## 템플릿 역할
템플릿은 최종 콘텐츠의 순서·카피·Brain을 변경하지 않습니다. 색상, 폰트, 레이아웃, 카드와 이미지 배치 등 시각 표현만 담당합니다.

## 개발 확인용 메타
Mock 결과에는 아래 값이 포함됩니다.

```json
{
  "mock_meta": {
    "mode": "design-driven",
    "source": "pageDesign",
    "templateRole": "visual-only"
  }
}
```

## API 연결 이후
추후 Claude API를 연결해도 입력과 출력 JSON 구조는 동일하게 유지됩니다. Mock 조립기를 Claude 생성 결과로 교체하면 되므로 UI와 템플릿 구조를 다시 바꿀 필요가 없습니다.

## 검증
- `npm ci` 완료
- `npm run build` 완료
- Vite production build 정상 통과
