# BRAND ENGINE V8 핵심 수정 완료 ✅

**최종 수정 날짜:** 2026-07-10  
**수정 대상:** V7 → V8  
**수정 파일:**
- `src/App.jsx` (Line 1634-1740)
- `src/prompts/generationPrompt.js` (전체 재구성)

---

## 📋 수정 항목 4가지

### **1️⃣ AI 설계 결과 → 최종 생성 연결 ✅**

**문제:**
```javascript
// 이전 (V7) - Line 1635
setPageDesign(null);  // ← AI 설계 데이터가 버려짐
```

**수정:**
```javascript
// 수정 후 (V8) - Line 1634
// pageDesign은 유지 (프롬프트에 전달할 데이터이므로)
setShowPageDesign(false);
// setPageDesign(null) 제거
```

**효과:**
- ✅ AI 설계 화면에서 생성된 JSON이 최종 생성에 사용됨
- ✅ 페이지 구조 + 설득 포인트 + 추천 이미지가 최종 카피에 반영

---

### **2️⃣ pageDesign 데이터 프롬프트 통합 ✅**

**수정 위치:** `src/prompts/generationPrompt.js`

**추가된 데이터 블록:**
```
[AI 설계 결과]

📌 설정된 페이지 구조:
- AI 설계에서 선택한 섹션 순서

📸 추천 이미지 전략:
- Hero, 원료, 배합, 품질 등의 이미지 가이드

💡 강조할 제품 특징:
- productFeatures 데이터

🎯 설득 포인트:
- persuasionPoints 데이터

👥 권장 타깃 고객:
- recommendedTarget 데이터
```

**효과:**
- ✅ 프롬프트가 AI 설계 결과를 인지하고 반영
- ✅ "이 페이지 구조를 따르세요", "이 설득 포인트를 사용하세요"라는 명시적 지시
- ✅ 생성 결과의 일관성 증대

---

### **3️⃣ 일반 건강식품 vs 건강기능식품 분류 오류 수정 ✅**

**문제:**
```javascript
// 이전 (V7) - 제품명으로 재분류
const recognition = recognizeProductType(product.name, product.subCategory, product);
const functional = recognition.type === "HEALTH_FUNCTIONAL";
```

**수정:**
```javascript
// 수정 후 (V8) - 사용자가 선택한 category를 우선 사용
const functional = product.category === "건강기능식품";
const brain = functional ? HEALTH_FUNCTIONAL_FOOD_BRAIN_V6 : GENERAL_HEALTH_FOOD_BRAIN_V6;
```

**효과:**
- ✅ 베르베린을 "일반 건강식품"으로 선택하면 Health Functional 템플릿이 적용 안 됨
- ✅ 루테인을 "건강기능식품"으로 선택하면 무조건 Health Functional 템플릿 적용
- ✅ 사용자의 명시적 선택이 최우선

**테스트 케이스:**
```
✅ 루테인 + "건강·영양식품" > "건강기능식품" → Health Functional Brain
✅ 베르베린 + "건강·영양식품" > "일반 건강식품" → General Health Brain
✅ 홍삼 + "건강·영양식품" > "일반 건강식품" → General Health Brain
```

---

### **4️⃣ Template Placeholder 금지 강화 ✅**

**강화된 글쓰기 규칙:**
```
⚠️ 절대 금지:
- 플레이스홀더("XXmg", "[제품명]", "[섹션]", "~~~" 등)
- 템플릿 설명("이 섹션은", "여기서는", "다음으로")
- 입력되지 않은 수치나 인증정보 생성
- 불완전한 문장이나 줄표로 끝나는 항목
```

**효과:**
- ✅ 프롬프트에서 명시적으로 "XXmg" 같은 플레이스홀더 금지
- ✅ "반드시 실제 콘텐츠로 작성하세요" 재강조
- ✅ EPA/DHA 플레이스홀더 더욱 명시적으로 금지

---

## 🔧 기술 세부 사항

### App.jsx 변경사항
```javascript
// Before (Line 1635)
setShowPageDesign(false);
setPageDesign(null);  // ❌ 데이터 버림

// After (Line 1634)
setShowPageDesign(false);
// setPageDesign(null) 제거 ✅

// Before (Line 1738)
const genResult = await callClaude(buildGenerationPrompt({ ...product, target: effectiveTarget }), 4000);

// After (Line 1738)
const genResult = await callClaude(buildGenerationPrompt({ ...product, target: effectiveTarget, pageDesign }), 4000);
```

### generationPrompt.js 변경사항
```javascript
// 1. pageDesignBlock 추가
const pageDesignBlock = product.pageDesign ? `
[AI 설계 결과]
...
` : "";

// 2. selectBrainAndTemplate 재구성
function selectBrainAndTemplate(product) {
  const functional = product.category === "건강기능식품";  // ← 직접 사용
  // 이전: recognizeProductType으로 재분류 (오류 가능)
}

// 3. 글쓰기 규칙 강화
// - 플레이스홀더 명시적 금지 추가
// - 템플릿 설명 금지 추가
```

---

## ✨ 예상 개선 효과

### Before (V7)
```
❌ AI 설계 결과 → 화면에만 표시, 최종 생성에 반영 안 됨
❌ "인스턴트밥(햇반)" → Health Functional으로 잘못 생성 가능
❌ "XXmg", "EPA ???mg" 플레이스홀더 결과에 나타남
```

### After (V8)
```
✅ AI 설계 결과 → 최종 생성에 직접 반영
✅ 사용자 선택 카테고리 우선 (명시적 선택 존중)
✅ 플레이스홀더 명시적으로 금지 (프롬프트 수준)
✅ 페이지 구조 + 이미지 전략 + 설득 포인트 연결
```

---

## 🧪 테스트 권장 순서

1. **루테인 + 건강기능식품**
   - 페이지 구조가 AI 설계와 일치하는지 확인
   - EPA/DHA 플레이스홀더 없는지 확인

2. **베르베린 + 일반 건강식품**
   - General Health Food Brain 적용되는지 확인
   - "효능", "개선" 같은 금지어 없는지 확인

3. **오메가3 + 건강기능식품**
   - 추천 이미지 전략이 설명에 반영되는지 확인

4. **홍삼 + 일반 건강식품**
   - General Health Food 톤인지 확인
   - Health Functional 표현이 없는지 확인

---

## 📦 배포 준비

이 V8 수정은 **내부 프롬프트 + 로직 개선**이므로:
- ✅ UI/UX 변경 없음 (기존 화면 그대로 사용)
- ✅ 새로운 의존성 없음
- ✅ 기존 저장된 프로젝트 호환 유지
- ✅ 즉시 배포 가능

---

## 📝 다음 단계 (Phase 2)

1. **합규성 데이터베이스** (complianceDB.js)
   - 식약처 고시형 기능성 20-30개 hardcode
   - 카테고리별 금지 표현 규칙
   - 시간: 4-6시간

2. **통합 검증**
   - Pipeline에 complianceDB 연결
   - 모의 테스트 (루테인, 베르베린, 오메가3)

3. **Phase 3: 관리 대시보드** (선택)
   - 진단 화면 → 전략 리포트 UI 개선

---

**작성자:** Claude  
**최종 확인:** ✅ 완료  
**상태:** Ready for Testing
