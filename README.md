# 출장/여행 비용 종합 (Tripmanagment)

출장·여행 경비 입력, 1/N 정산, 예상 비용 검토용 웹앱입니다. `index.html` 한 파일로 동작합니다.

## 기능
- **출장/여행 관리**: 여정 등록(해외/국내·당일), 지출 입력(직접·텍스트), 마스터 환율, 1/N 정산·송금 안내·입금 계좌, 엑셀·PDF 출력
- **비용 검토**: 예상 여정, 교통비·숙박비(객실 수)·식비(끼니)·준비비·기타 자동 계산, 사전 비용/현지 경비, 일정 상세
- **인원 정보**: 출장자 명단(구분·팀·연락처·계좌), 텍스트·스프레드시트 입력
- **조회 전용 QR**: 여정·예상 여정별 QR / 링크

## 운영 주소
- GitHub Pages (Firebase 저장): https://intiipu-prog.github.io/Tripmanagment/
- claude.ai 페이지: https://claude.ai/artifact/Fguv3QmGZRVR9ShhqVF6jd

## 편집자 모드 / 뷰어 모드
- **뷰어 모드**: 로그인 없이 누구나 여정·비용 검토를 조회합니다. 인원 정보 메뉴와 입력·수정·삭제 버튼이 보이지 않습니다.
- **편집자 모드**: 화면 위 [편집자 로그인] → Firebase Authentication(이메일/비밀번호) 계정으로 로그인하면 입력·수정할 수 있습니다.
- 편집자는 [뷰어로 보기]로 뷰어 화면을 미리 볼 수 있습니다.

## 저장소 동작
- `*.github.io` 에서 열면 **Firebase(Firestore)** 에 저장하고, claude.ai에서 열면 claude 서버 DB에 저장합니다 (같은 `index.html`).
- Firebase 프로젝트: **tripmanagment-a8c30**
- Firestore 컬렉션: `trips`, `plans`, `people` (문서 = `{ json, updatedAt }`)
- Firestore 규칙: 여정·비용 검토는 읽기 모두 허용, 인원 정보는 로그인한 편집자만 읽기. 쓰기는 모두 로그인한 편집자만.

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /trips/{id}  { allow read: if true; allow write: if request.auth != null; }
    match /plans/{id}  { allow read: if true; allow write: if request.auth != null; }
    match /people/{id} { allow read, write: if request.auth != null; }
  }
}
```

## GitHub 백업 (`data/`)
- `data/trips.json` — 출장/여행 전체 (지출 내역·환율·입금 계좌 포함)
- `data/plans.json` — 비용 검토(예상 여정) 전체
- `data/people.json` — 인원 정보 전체
- 편집자가 [⚙ 설정 → GitHub 자동 백업]에 토큰을 저장하면, 데이터가 바뀌고 30초 뒤 위 파일에 자동 커밋됩니다. 토큰은 그 기기 브라우저에만 저장됩니다.
- 토큰: GitHub → Settings → Developer settings → Fine-grained tokens → 이 저장소만 선택 → **Contents: Read and write**
- 복원: [⚙ 설정 → GitHub 백업(data/)에서 불러오기]
- ⚠️ 공개 저장소이므로 `data/` 의 내용(이름·연락처·계좌번호 포함)은 누구나 볼 수 있습니다.

## 업데이트
`index.html` 을 고치면 이 저장소와 claude.ai 페이지에 같은 파일을 함께 올립니다.
