# Ondap_embed

## 폴더 구조

```text
Ondap_embed/
├─ main/                          # app_main: 초기화·컴포넌트 연결
├─ components/
│  ├─ ondap_app/                  # 대기·녹음·업로드·재생·오류 상태 전환
│  ├─ ondap_board/                # 핀·버튼·LED·앰프 enable·마이크 전원 감지
│  ├─ ondap_audio/                # I2S PDM 입력·I2S 출력·버퍼·음량
│  ├─ ondap_wake/                 # 호출어·음성 구간 감지 (ESP-SR)
│  ├─ ondap_provisioning/         # Wi-Fi 최초 설정·기기 등록
│  ├─ ondap_credentials/          # 기기 인증정보 저장 (NVS)
│  ├─ ondap_connectivity/         # Wi-Fi·시간 동기화·연결 복구
│  └─ ondap_device_api/           # Ondap_api 기기용 API 호출·업로드·다운로드
├─ boards/
│  ├─ r2_evt/                     # R2 EVT 기판 핀·sdkconfig 조각
│  └─ devkit_s3/                  # ESP32-S3 개발보드 (기판 없이 개발할 때)
├─ partitions/                    # 앱·NVS·음성 모델 파티션 설계
├─ tests/                         # 컴포넌트·실기 테스트
├─ docs/                          # 핀 배정·실물 검증 기록·플래시 방법
└─ .github/workflows/             # ESP-IDF 빌드
```

## 필요해질 때 추가할 것

| 위치 | 내용 | 추가 시점 |
|---|---|---|
| `components/ondap_update/` | OTA·롤백 | 기기 파일럿 배포 전 |
| `tools/` | 생산 기록·검사 지그 | 양산 준비 |

## 온답 저장소 구성

| 저장소 | 역할 |
|---|---|
| [Ondap_app_senior](https://github.com/dimigo-Paradise/Ondap_app_senior) | 어르신 앱 (Android·iOS). 이야기 녹음, 가족 답장 듣기. **MVP의 어르신 쪽 접점** |
| [Ondap_app_carer](https://github.com/dimigo-Paradise/Ondap_app_carer) | 보호자 앱 (Android·iOS). 요약 확인, 답장 녹음·승인 |
| [Ondap_api](https://github.com/dimigo-Paradise/Ondap_api) | HTTP API 서버 (Python·FastAPI). **DB를 소유하는 유일한 저장소** |
| [Ondap_server](https://github.com/dimigo-Paradise/Ondap_server) | 백그라운드 작업 (Python·Celery). STT·마스킹·요약·오디오 변환. DB 직접 접근 없음 |
| [Ondap_embed](https://github.com/dimigo-Paradise/Ondap_embed) | 스피커 기기 펌웨어 (ESP-IDF). MVP 이후 |

```text
senior app ─┐
            ├─ HTTPS ─▶ Ondap_api ─▶ PostgreSQL · S3 · FCM/APNs
carer app  ─┘              │  ▲
                           ▼  │   작업 발행(Redis) ↓ / 결과 보고(내부 API) ↑
                       Ondap_server ─▶ STT · 요약 API
```
