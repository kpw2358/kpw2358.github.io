---
name: add-life-event
description: index.html의 "굵직한 인생사"(#services) 섹션에 새 약력 카드를 추가한다. 사용자가 약력, 경력, 인생사 항목을 추가해 달라고 할 때 사용.
---

# 약력 카드 추가

`index.html`의 `#services` 섹션에 카드 하나를 추가한다.

## 절차

1. 사용자에게 제목, 한 줄 설명, (선택) Font Awesome 4 아이콘 이름을 받는다. 아이콘이 없으면 `fa-star`를 쓴다.
2. `index.html`에서 `<section id="services">` 안의 `.row`를 찾는다.
3. 기존 카드와 같은 형식으로 `.row`의 마지막 카드 뒤에 추가한다:

```html
<div class="col-lg-3 col-md-6 text-center">
  <div class="service-box mt-5 mx-auto">
    <i class="fa fa-4x fa-ICON text-primary mb-3 sr-icons"></i>
    <h3 class="mb-3">제목</h3>
    <p class="text-muted mb-0">설명</p>
  </div>
</div>
```

4. 한 줄에 카드가 4개(`col-lg-3`)이므로, 5개째부터는 새 `.row`를 만들거나 사용자에게 레이아웃을 확인한다.
5. 사용자가 쓴 문구는 그대로 쓰고 임의로 고치지 않는다.
6. 변경 내용을 요약해 보여주고, 커밋은 사용자가 요청할 때만 한다.
