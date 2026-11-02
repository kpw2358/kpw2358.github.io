# Recon map: 여신티켓 / YeoTi 피부연구소 (Web, 모바일 웹 우선)

Scope: `skinLog/lab/intro` 의 "YeoTi 피부연구소" (셀피 AI 피부 분석 → 솔루션 추천) 와, 이 기능이 닿는 주변 흐름 (시술 탐색 → 병원 상품 → 예약/결제)
For: 미정. 본인 사업 / 니치 / 판매용 중 어느 쪽인지 확인 필요 (아래 "열린 질문" 참고)
Date: 2026-11-02

> 이 문서는 공개 페이지와 앱스토어 소개글만 근거로 했다. 로그인한 화면, 앱 내부, JS 번들, 네트워크 호출은 보지 않았다.
> 그래서 "관찰" 과 "추정" 을 구분해서 적었다. 추정에는 confidence 를 붙였다.

## Sources

| # | source | URL | notes |
| --- | --- | --- | --- |
| 1 | 대상 페이지 (intro) | https://www.yeoshin.co.kr/skinLog/lab/intro | 본문은 제목, 이미지 2장, 말풍선, CTA 1개뿐. 메타 설명에 "셀피만 찍으면 AI로 피부 분석부터 솔루션 추천까지" |
| 2 | 메인 사이트 | https://www.yeoshin.co.kr | 랭킹, 배너, 퀵메뉴, 시술 상품 리스트, 여신TV, 후기, 푸터 |
| 3 | App Store (KR) | https://apps.apple.com/kr/app/id1228558605 | 기능 설명, 규모 수치, 평점 4.8 (약 1.1만), 릴리스 노트, 권한 |
| 4 | 도움말/공지 센터 | https://help.yeoshin.co.kr | **열리지 않음** (client_error). 이 소스는 비어 있음 |
| 5 | 병원 입점 문의 | https://plus.yeoshin.co.kr/contact | 푸터 링크로만 확인, 열어보지 않음 |
| 6 | 시술 백과 블로그 | https://blog.yeoshin.co.kr | 앱스토어 소개글에서 언급, 열어보지 않음 |
| 7 | 이용약관 / 개인정보처리방침 | 푸터 링크 | 열어보지 않음. 얼굴 사진 처리 조항은 따로 확인해야 함 |

빈 칸: 사용자 본인 계정으로 본 화면, 공개 워크스루 영상, 변경 로그는 아직 없다.

## Core loop

셀피 한 장으로 피부 나이와 상태를 보고, 그 결과로 맞춤 시술을 추천받아 병원 상품 예약·결제까지 이어지는 것. (추천 → 예약 연결은 앱스토어 설명과 메인 사이트 구조에서 추정, confidence: medium)

## Screens

| ID | screen | route / how to reach | purpose | key components | states seen |
| --- | --- | --- | --- | --- | --- |
| S01 | YeoTi 피부연구소 소개 | `/skinLog/lab/intro` | 기능 소개와 측정 시작 유도 | 제목, 이미지 2장, 말풍선 "내 얼굴 나이는 몇 살?", 메인 CTA "AI로 얼굴 나이 측정하기", 앱 열기 배너, 하단 탭 | 모바일 웹 filled. 로딩/오류 미확인 |
| S02 | 셀피 촬영/업로드 | S01 CTA 이후 (**관찰 못함**) | 얼굴 사진 입력 | 카메라/갤러리 선택, 촬영 가이드, 동의 | 추정만. 앱스토어에 카메라/사진 권한 명시 |
| S03 | 분석 중 | S02 이후 (**관찰 못함**) | 대기 | 로딩 표시 | 추정 |
| S04 | 분석 결과 | (**관찰 못함**) | 얼굴 나이와 7개 항목 결과 표시 | 얼굴 나이, 탄력, 주름, 피부결, 피부톤, 모공, 기미 | 항목 목록은 관찰(소개 문구), 화면 자체는 추정 |
| S05 | 솔루션 추천 | S04 이후 (**관찰 못함**) | 결과 기반 시술/상품 추천 | 추천 시술 카드, 병원 상품 링크 | 추정 |
| S06 | 홈 | `/` | 진입, 랭킹, 프로모션 | 검색/찜/장바구니 아이콘, 인기 시술 TOP 10, 배너 캐러셀, 퀵메뉴 10개, 상품 리스트, 여신TV, 후기 | filled. 비로그인 상태로 관찰 |
| S07 | 탐색 | `/explore?tab=RANKING` | 시술 랭킹/탐색 | 탭, 랭킹 리스트 | 경로만 관찰 |
| S08 | 후기 | `/reviews` | 시술 후기 목록 | 별점, 요약, 일부 잠금 | 경로만 관찰. 홈에서 "로그인하고 자세히 보기" 잠금 확인 |
| S09 | 티켓/예약 | `/ticketReservationList?tab=TICKET&status=UNUSED` | 구매한 티켓, 예약 목록 | 탭(TICKET), 상태 필터(UNUSED) | 경로만 관찰. 로그인 필요 추정 |
| S10 | 마이 | `/myPage` | 개인 영역 | 포인트, 쿠폰, 등급 등 (추정) | 경로만 관찰 |
| S11 | 시술 상품 상세 | 홈 카드 탭 (**관찰 못함**) | 가격, 병원, 후기, 결제 방식 | 가격(VAT 포함), 별점, 스크랩 수, 결제 방식 라벨 | 카드 요약만 관찰 |
| S12 | 병원 상세 | (**관찰 못함**) | 위치, 영업시간, 시술, 가격, 전문의 | 앱스토어 설명에서 추정 | 추정 |
| S13 | 예약/결제 | (**관찰 못함**) | 상담신청, 예약, 앱 결제 | "앱 결제 / 앱 예약 / 단독 앱 결제 / 상담신청" 4가지 방식 라벨 | 라벨만 관찰 |
| S14 | 로그인/가입 | (**관찰 못함**) | 계정 생성 | 웰컴 혜택 7,000P + 10만 원 쿠폰 문구 | 추정 |

S02-S05 는 이 요청의 중심인데 비로그인·앱 밖에서는 보이지 않는다. 실제 화면은 **사용자 본인 계정으로 사용자가 직접 진행하면서** 스크린샷을 `replica/screens/` 에 넣어주면 채울 수 있다.

## Flows

```
F01 방문자가 AI 얼굴 나이를 측정한다 (핵심)
    S01 소개 -> (CTA) -> S02 촬영/업로드 -> S03 분석 중 -> S04 결과 -> S05 솔루션 추천
    happy path clicks: 관찰된 것은 1회 (CTA). 전체는 미확인
    edge: 얼굴 미검출, 흐린/어두운 사진, 카메라 권한 거부, 비로그인 상태, 앱 전용일 가능성, 분석 실패
    (S02 이후는 추정, confidence: medium)

F02 추천에서 예약까지
    S05 추천 -> S11 상품 상세 -> S13 예약/결제 -> S09 티켓/예약
    edge: 결제 방식별 분기 (앱 결제 / 앱 예약 / 현장 결제 / 상담신청), 로그인 요구
    (추정, confidence: medium)

F03 시술 탐색
    S06 홈 -> (랭킹 / 퀵메뉴 / 검색) -> S11 상품 상세
    happy path clicks: 2 (홈 -> 상품 카드 -> 상세)
    edge: 검색 결과 없음, 지역 필터, 비로그인 후기 잠금

F04 후기 열람 -> 로그인 유도
    S08 후기 목록 -> 잠긴 후기 -> S14 로그인
    (관찰: 홈 후기 섹션의 "로그인하고 자세히 보기")

F05 웹 방문자를 앱으로 유도
    아무 화면 -> "여신티켓 APP에서 열기" 배너 / 푸터 QR
    (관찰) 웹은 앱 설치 퍼널 역할이 큼
```

숫자로 이길 목표: F01 은 소개 화면에서 결과까지 **CTA 1회 + 사진 1장** 이 상한선으로 보인다. 단 이것은 마케팅 문구 기준이고, 실제 클릭 수는 직접 해봐야 안다.

## Components

| component | variants | states | used on |
| --- | --- | --- | --- |
| Button (CTA) | primary 큰 버튼, 앱 열기 "열기" | default, (그 외 미확인) | S01, S06 |
| 말풍선 | 일러스트 위 문구 | default | S01 |
| 하단 탭 | 5개: 홈, 탐색, 후기, 티켓/예약, 마이 | active/inactive | 전체 |
| 앱 열기 배너 | 닫기 아이콘 포함 | shown, dismissed | 전체 |
| 랭킹 리스트 | 순위 + 변동 아이콘(유지/상승/하락) | filled | S06, S07 |
| 배너 캐러셀 | 약 20개 프로모션 | filled | S06 |
| 퀵메뉴 | 아이콘 그리드 10개 | filled | S06 |
| 상품 카드 | 가격, 별점, 스크랩 수, 결제 방식 라벨 | filled | S06, S11 |
| 칩 필터 | 전체 / #피부관리 / #스킨부스터 | selected/unselected | S06 |
| 후기 카드 | 별점, 요약, 잠금 아이콘 | open, locked | S06, S08 |
| 영상 카드 (여신TV) | 해시태그 | filled | S06 |
| 푸터 | 약관, 문의, SNS, 인증 마크, 언어 선택(KO/EN/ZH/JA) | filled | 전체 |
| 스크랩(찜) 아이콘 | | on/off | 상품 카드, 헤더 |
| 장바구니 아이콘 | | | 헤더 |

## Inferred data model

```
User          id, 계정 식별자(이메일/소셜, 추정), 포인트 잔액, 등급, 언어, 가입 혜택 수령 여부
              evidence: 웰컴 혜택 문구, 앱스토어(등급별 할인, 출석 포인트, 추천 포인트)
              confidence: medium

SkinAnalysis  id, user_id, selfie_ref, face_age, scores{elasticity, wrinkle, texture, tone, pore, pigmentation}, created_at
              evidence: S01 소개 문구(7개 항목), 앱스토어 "셀피로 피부 나이 추정"
              confidence: medium (항목명은 관찰, 필드 구조는 추정)
              주의: 얼굴 사진은 민감한 개인정보. 보관 방식은 원본 약관 확인 필요

Treatment     id, name, category(인모드, 슈링크, 울쎄라...), 랭킹, 순위 변동
              evidence: S06 랭킹, 검색 키워드 20개
              confidence: high

Hospital      id, name, location, hours, 전문의 수/진료과, 입점 상태
              evidence: 앱스토어 "병원 검색", "의사 정보"
              confidence: medium

Product       id, hospital_id, treatment_id, price(VAT 포함), 결제방식(app_pay | app_reserve | app_only_pay | consult), rating, scrap_count
              evidence: S06 상품 카드
              confidence: high

Review        id, user_id, product/hospital_id, rating, body, photos, locked_for_guest
              evidence: S06/S08, 앱스토어 "후기 작성 포인트"
              confidence: medium

Reservation   id, user_id, product_id, status(UNUSED 외 미확인), scheduled_at, pay_method
              evidence: `/ticketReservationList?tab=TICKET&status=UNUSED`, 락스크린 알림 릴리스 노트
              confidence: medium

Coupon / PointLedger   id, user_id, type, amount, expires_at
              evidence: 웰컴 7,000P, 10만 원 쿠폰, 출석 체크, 추천인 포인트
              confidence: medium

Scrap(찜)     user_id, product_id          evidence: 찜 아이콘                        confidence: medium
Gift          id, sender_id, recipient, product_id   evidence: 릴리스 노트 3.14.0 "선물하기"  confidence: guess
```

Relationships: User 1-n SkinAnalysis, User 1-n Review, User 1-n Reservation, Hospital 1-n Product, Treatment 1-n Product, Product 1-n Review, SkinAnalysis n-n Treatment (추천, 규칙인지 모델인지 미확인)

## Feature matrix

See `features.csv`. 총 35행: 만들 것 28행 (must 10, should 10, could 8), skip 7행.

## Out of scope (cannot or should not be cloned)

- 병원 네트워크 (약 2.7만 곳 입점)와 계약, 정산 구조. 서비스의 핵심 자산이다.
- 누적 후기 180만 건과 사용자 600만 명. 복제 불가.
- 의료광고·의료기관 관련 규제 요건. 시술 정보·후기·가격 노출은 의료법 광고 규제를 받는다. 클론을 만든다면 법률 검토가 먼저다.
- 부작용 안심케어 (비용 보장). 보험/보장 파트너 계약이 필요하다.
- AI 피부 분석 모델 자체. 원본 모델·학습 데이터는 알 수 없고 가져올 수도 없다. 대체 모델을 따로 구해야 하고, 정확도와 의료적 주장은 별개의 문제다.
- 결제·정산 (PG, 현장 앱결제 연동), 외국인 여권 인증, 녹음 상담.
- 브랜드, 로고, 이미지, 일러스트, 문구. 원본의 것은 쓰지 않는다.

## Size

Screens 14 (이 중 관찰 6, 추정 8), flows 5, entities 10. Hard parts: ① 병원/상품 공급 쪽 (콘텐츠와 입점이 없으면 빈 앱) ② AI 피부 분석 품질과 얼굴 사진 개인정보 처리 ③ 결제/예약 연동과 의료광고 규제. Size: **피부 분석 기능만 분리하면 M, 예약·결제까지 포함한 마켓플레이스 전체는 XL (재조정 필요)**.

## 열린 질문 (다음 단계 전에 정해야 함)

1. 클론 대상 범위: 피부연구소(분석 + 추천)만 할 건가, 병원 예약 마켓플레이스까지 할 건가. 기본 제안은 앞쪽만.
2. 누구를 위한 것인가: 본인 사업 / 니치 / 판매. 이름과 브랜드는 반드시 다르게 가야 한다.
3. S02-S05 는 직접 계정으로 진행하며 스크린샷을 주면 보강 가능. 이용약관상 경쟁 서비스 개발 목적의 사용이 막혀 있을 수 있어서 약관은 먼저 읽어봐야 한다.
