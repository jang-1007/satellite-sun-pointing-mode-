# 🌞 Light-Tracking Motor Control with MPU6050 & Bluetooth Telemetry

**Arduino 기반 조도센서 + MPU6050 + 모터 제어 시스템**

---

## 📘 개요
이 프로젝트는 **조도센서(CdS)**, **MPU6050 자이로 센서**, 그리고 **모터 제어 회로**를 이용하여  
밝은 방향을 자동으로 탐색하며 회전하는 시스템이다.  
추가로 **Bluetooth(SoftwareSerial)** 을 통해 실시간 센서 데이터와 모터 상태를 전송한다.

---

## ⚙️ 결선도

| 구성 요소 | 핀 번호 | 설명 |
|------------|----------|------|
| 모터 속도 제어 | D5 (PWM) | `analogWrite()`로 PWM 제어 |
| 모터 방향 CW | D3 | 정방향 (시계 방향) |
| 모터 방향 CCW | D4 | 역방향 (반시계 방향) |
| CdS 센서 1 | A1 | 왼쪽 조도 감지 |
| CdS 센서 2 | A3 | 오른쪽 조도 감지 |
| MPU6050 | I2C (SDA/SCL) | 가속도/자이로 측정 |
| Bluetooth TX/RX | D10, D11 | SoftwareSerial 통신 (9600bps) |

---

## 🧠 동작 개요

1. **조도센서(CdS)** 로 좌우 밝기를 측정한다.  
2. 두 센서의 값에 따라 모터 방향을 결정한다.  
   - 두 쪽 다 **밝으면 정지**,  
   - 두 쪽 다 **어두우면 CW 탐색 회전**,  
   - **밝기 차이**가 있으면 더 밝은 쪽을 향하도록 회전 방향 결정  
3. **히스테리시스**를 적용하여 조명 변화에 따른 진동 방지.  
4. **MPU6050**을 통해 기울기(pitch)를 계산하고,  
   **Bluetooth + Serial** 로 실시간 센서 데이터를 CSV 형태로 전송한다.

---

## 🧩 주요 기능

| 기능 | 설명 |
|------|------|
| **MPU6050 초기화 및 필터링** | I2C로 데이터를 읽고, Complementary Filter를 적용하여 pitch 계산 |
| **조도 감지 로직** | CdS 센서값 비교를 통해 밝은 방향으로 회전 |
| **모터 제어 (PWM + 방향)** | CW, CCW, Stop 제어 및 최소 PWM 임계치 설정 |
| **실시간 Telemetry 전송** | Serial + Bluetooth로 데이터 전송 (CSV 형식) |

---

## 🎛️ 주요 상수

| 상수명 | 설명 | 기본값 |
|---------|------|--------|
| `TH_BRIGHT` | 밝음 기준 (조도 낮을수록 밝음) | 150 |
| `TH_DARK` | 어두움 기준 | 500 |
| `MIN_PWM` | 정지마찰 극복 최소 PWM | 180 |
| `PWM_CW`, `PWM_CCW` | 회전 시 PWM 값 | 220 |
| `alpha_acc`, `alpha_comp` | 필터 계수 (가속도/보정용) | 0.9 / 0.98 |

---

## 📤 Telemetry 데이터 포맷 (CSV)
시리얼 모니터 또는 블루투스로 전송되는 데이터 형식:


예시:

- **MotorDir:** `+1`(CW), `-1`(CCW), `0`(정지)  
- **pitch:** MPU6050 기반 필터링된 기울기 각도(°)

---

## ⚙️ 코드 동작 순서


---

## 🔧 개발 환경

- **보드:** Arduino UNO / MEGA (MEGA 권장)  
- **IDE:** Arduino IDE 2.x  
- **라이브러리:**  
  - `Wire.h` (I2C 통신)  
  - `SoftwareSerial.h` (Bluetooth 통신)  
  - `Arduino.h` (기본 라이브러리)

---

## 🧭 참고 사항

- CdS 센서의 특성(밝을수록 저항 감소)에 따라 **TH_BRIGHT**, **TH_DARK** 값은 보정 필요  
- PWM 제어는 모터 드라이버 회로와 함께 사용해야 함  
- Bluetooth 수신 시 CSV 로그를 시리얼 모니터나 PC 앱(예: RealTerm, CoolTerm)으로 확인 가능

---

## 📄 라이선스
MIT License — 자유롭게 수정 및 배포 가능
