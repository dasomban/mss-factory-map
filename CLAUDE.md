# MSS 해외 공장 출고량 시각화 — 프로젝트 노트

## 결과물
- **라이브 URL**: https://dasomban.github.io/mss-factory-map/
- **GitHub 레포**: https://github.com/dasomban/mss-factory-map
- **로컬 HTML**: `C:\Users\User\Downloads\factory_map.html`
- **로컬 레포**: `C:\Users\User\factory-map\index.html`

---

## 파일 구성

| 파일 | 역할 |
|------|------|
| `C:\Users\User\Downloads\MSS 공장 주소 정제 데이터_다솜 수정1 (지윤 버전 반영).xlsx` | **원본 소스** (91개 공장 + 11개 주소미확인) |
| `C:\Users\User\Downloads\MSS 공장 주소_정제완료.xlsx` | **정제 완료 Excel** (run_clean2.py 출력) |
| `C:\Users\User\Downloads\run_clean2.py` | 주소 파싱 + Excel 정제 스크립트 |
| `C:\Users\User\Downloads\update_viz.py` | 정제 Excel → factory_map.html 생성 스크립트 |
| `C:\Users\User\Downloads\factory_map.html` | 최종 시각화 HTML |

---

## 데이터 업데이트 방법

### 1. 원본 Excel 교체
새 데이터로 원본 파일 교체 (시트 구조 유지):
- Sheet 0: `주소 취합 완료` — 컬럼: 공장명, CODE, 출고량(pcs), 사용 주소, 권역, 항구, 국가, 위도, 경도
- Sheet 1: `주소 정보 재확인 필요` — 컬럼: 공장명, CODE, 출고량(pcs), 원본 주소, 제외 사유

### 2. 주소 정제 실행
```bash
cd C:\Users\User\Downloads
python run_clean2.py
```
→ `MSS 공장 주소_정제완료.xlsx` 덮어쓰기

### 3. 새 공장 좌표 추가 (신규 공장이 있을 경우)
`update_viz.py` 상단 `OLD_COORDS` 딕셔너리에 추가:
```python
"CODE값": (위도, 경도),  # 공장명, 지역
```

### 4. 시각화 HTML 생성
```bash
python update_viz.py
```
→ `factory_map.html` 갱신

### 5. GitHub 배포
```bash
cp C:\Users\User\Downloads\factory_map.html C:\Users\User\factory-map\index.html
cd C:\Users\User\factory-map
git add index.html
git commit -m "Update factory data YYYY-MM"
git push
```

---

## 핵심 설정값

### 볼륨 단위
- **표시 단위**: pcs (원본값 그대로)

### 포트 → 권역 매핑 (`update_viz.py` PORT_REGION)
| Port | 권역 |
|------|------|
| Hai Phong | 베트남 북부 (하이퐁 항) |
| Ho Chi Minh | 베트남 남부 (호치민 항) |
| Da Nang | 베트남 중부 (다낭 항) |
| Yangon | 미얀마 (양곤 항) |
| Shanghai/Ningbo | 중국 동부 (상하이/닝보 항) |
| Dalian | 중국 동북부 (다롄 항) |
| Qingdao | 중국 북부 (칭다오 항) |
| Guangzhou/Shenzhen | 중국 남부 (광저우/선전 항) |
| Weihai | 중국 (웨이하이 항) |
| Tanjung Priok | 인도네시아 (탄중프리옥 항) |
| Semarang | 인도네시아 (스마랑 항) |
| Busan | 한국 (부산 항) |
| Manila | 필리핀 (마닐라 항) |

### 국가 코드 추론 (CODE 3~5번째 문자)
VN=베트남, CN=중국, MY=미얀마, ID=인도네시아, PH=필리핀, KR=한국

### Python 경로
`C:\Users\User\AppData\Local\Programs\Python\Python311\python.exe`

---

## 현재 현황 (2026-05 기준)
- 집계 공장: **91개**
- 주소 미확인: **11개**
- 총 출고량: **1,031,589 pcs**
- 최대 권역: 베트남 북부 32.7% (42개 공장)
