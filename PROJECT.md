# 데이터센터 모니터링 맵

회사 데이터센터 위치를 인터랙티브 지도에 표시하고, 실시간 기상·대기질 정보를 조회하는 단일 페이지 웹 애플리케이션.

## 기술 스택

- **지도**: [Leaflet.js](https://leafletjs.com/) + CartoDB Dark Matter 타일 (다크 테마)
- **기상 API**: [Open-Meteo](https://open-meteo.com/) — 무료, 키 불필요
- **대기질 API**: [WAQI (aqicn.org)](https://waqi.info/) — 현재 `demo` 토큰 사용
- **호스팅**: GitHub Pages (정적 파일)

## 파일 구조

```
index.html    — 전체 앱 (HTML + CSS + JS 단일 파일, ~850줄)
PROJECT.md    — 이 문서
```

## 주요 기능

- **다크 테마 지도** — CartoDB Dark Matter 베이스맵
- **데이터센터 마커** — 🏢 빌딩 이모지, 펄싱 애니메이션, 호버 시 다크 테마 tooltip
- **클릭 확대** — `flyTo`로 해당 위치 줌인
- **사이드 패널** — 기온, 습도, 풍속, 날씨, AQI, PM2.5, PM10, O₃, NO₂ 표시
- **로딩 스켈레톤** — API 호출 중 shimmer 애니메이션
- **전체 지도 복귀** — "전체 지도로 돌아가기" 버튼 + ESC 키
- **실시간 시계** — 헤더에 현재 시각 표시

## 등록된 데이터센터

| ID | 이름 | 주소 | 위도 | 경도 |
|----|------|------|------|------|
| sel20-30 | SEL20/30 | 경기도 안양시 동안구 관양동 1743 | 37.3980 | 126.9653 |
| sel21 | SEL21 | 서울특별시 양천구 목동동로 323 | 37.5284 | 126.8728 |
| sel22 | SEL22 | 서울특별시 용산구 원효로3가 1-2 | 37.5340 | 126.9640 |
| sel23 | SEL23 | 서울특별시 금천구 가산로9길 80 | 37.4754 | 126.8878 |
| sel25 | SEL25 | 경기도 안양시 동안구 엘에스로 126 | 37.3894 | 126.9512 |
| sel27 | SEL27 | 경기도 안양시 동안구 관양동 869 LG유플러스 평촌2센터 | 37.3989 | 126.9685 |

## 데이터센터 추가 방법

`index.html` 내 `DATA_CENTERS` 배열에 객체를 추가:

```javascript
{
    id: 'sel99',           // 고유 ID (소문자, 하이픈 가능)
    name: 'SEL99',         // 표시 이름
    address: '주소',       // 상세 주소
    lat: 37.0000,          // 위도 (WGS84)
    lng: 126.0000,         // 경도 (WGS84)
}
```

좌표는 [Google Maps](https://maps.google.com)에서 해당 위치를 우클릭하면 확인 가능.

## API 관련 참고

### Open-Meteo (기상)
- 무료, API 키 불필요
- 엔드포인트: `https://api.open-meteo.com/v1/forecast`
- 요청 항목: `temperature_2m`, `relative_humidity_2m`, `weather_code`, `wind_speed_10m`

### WAQI (대기질)
- 현재 `demo` 토큰 사용 — 한국 지역 데이터가 제한적일 수 있음
- 정확한 데이터를 위해 무료 토큰 발급 권장: https://aqicn.org/data-platform/token/
- 발급 후 `index.html` 내 `WAQI_TOKEN` 변수 값을 교체

## 알려진 제한사항

- SEL25 좌표는 미검증 상태 (사용자 확인 필요)
- WAQI `demo` 토큰으로는 일부 지역 대기질 데이터가 누락될 수 있음
- 모든 API 호출은 클라이언트(브라우저)에서 직접 수행 — CORS 제한에 의존
