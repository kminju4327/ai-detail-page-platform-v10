# V9.1.2 Object Item Output Fix

## 문제
Smart Card 데이터가 일부 출력 경로에서 객체 배열로 남아 `[object Object]`로 표시되었습니다.

## 수정
- 공통 `formatSectionItem` / `normalizeSectionItems` 유틸 추가
- 화면 미리보기, 복사, HTML 내보내기, 공용 복사 유틸 모두에서 객체 항목을 문자열로 정규화
- `{ label, value }`는 `라벨 · 값` 형식으로 출력
- null, undefined, `[object Object]` 값 차단

## 검증
- `npm ci` 성공
- `npm run build` 성공
