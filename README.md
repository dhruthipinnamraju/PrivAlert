# PrivAlert
PrivAlert is an IoT-based smart surveillance and intrusion detection system built using ESP32, PIR motion sensing, OpenCV face recognition, Flask, and Telegram alerts.
When motion is detected, the ESP32 triggers the server, which captures an image from the laptop webcam, performs face recognition, and sends instant alerts for unknown visitors.

---

## How it works

```
PIR fires → ESP32 GET /motion → server opens webcam → face recognised?
                                      ├─ YES → green LED + unlock relay
                                      └─ NO  → red LED blinks + Telegram alert sent
```

---

## Repository layout

```
privalert/
├── server.py               # Flask server (runs on your laptop)
├── privalert_esp32.ino     # Arduino sketch (flashed to ESP32)
├── secrets.h.example       # ESP32 credentials template  ← copy → secrets.h
├── .env.example            # Server credentials template ← copy → .env
├── .gitignore
├── requirements.txt
└── README.md
```

---

## Quick start

### 1 — Clone

```bash
git clone https://github.com/your-username/privalert.git
cd privalert
```

### 2 — Server setup

```bash
pip install -r requirements.txt
cp .env.example .env          # then edit .env with your Telegram credentials
python server.py
# Dashboard → http://localhost:5000
```

### 3 — ESP32 setup

1. Copy `secrets.h.example` → `secrets.h`
2. Fill in your WiFi SSID, password, and the laptop's local IP
3. Open `privalert_esp32.ino` in the Arduino IDE
4. Install libraries: **WiFi**, **HTTPClient**, **ArduinoJson**
5. Select your ESP32 board and flash

---

## Wiring

| ESP32 pin | Component |
|-----------|-----------|
| GPIO 2    | PIR OUT   |
| GPIO 12   | Green LED (+ 220 Ω) |
| GPIO 13   | Red LED   (+ 220 Ω) |
| GPIO 14   | Relay IN  |
| 3V3 / GND | PIR VCC / GND |

---

## Environment variables (server)

Stored in `.env` — never committed.

| Variable | Default | Description |
|----------|---------|-------------|
| `TELEGRAM_BOT_TOKEN` | *(empty)* | Bot token from @BotFather — leave blank to disable alerts |
| `TELEGRAM_CHAT_ID`   | *(empty)* | Your Telegram user/chat ID |
| `TOLERANCE` | `80` | Recognition threshold — lower = stricter |
| `ENROLL_SAMPLE_COUNT` | `30` | Frames captured during enrollment |
| `ENROLL_DURATION_SEC` | `10` | Enrollment window in seconds |
| `ALERT_COOLDOWN_SEC`  | `15` | Minimum seconds between Telegram alerts |
| `MOTION_CAPTURE_ATTEMPTS` | `5` | Webcam retry attempts per PIR trigger |
| `MOTION_CAPTURE_DELAY`    | `0.5` | Seconds between retries |
| `EMBEDDINGS_DIR` | `encrypted_embeddings` | Folder for encrypted face data |
| `KEY_FILE`       | `secret.key` | Fernet encryption key path |

---

## ESP32 credentials (firmware)

Stored in `secrets.h` — never committed.

```cpp
#define WIFI_SSID     "your_network"
#define WIFI_PASSWORD "your_password"
#define SERVER_IP     "192.168.x.x"   // laptop's LAN IP
```

---

## API endpoints

| Method | Path | Description |
|--------|------|-------------|
| `GET`  | `/` | Web dashboard |
| `GET`  | `/motion` | ESP32 PIR trigger — opens webcam and recognises |
| `POST` | `/recognize` | Manual recognition (raw JPEG body) |
| `POST` | `/enroll` | Start enrollment `{"name": "Alice"}` |
| `GET`  | `/enroll/status/<name>` | Enrollment progress |
| `GET`  | `/users` | List enrolled users |
| `DELETE` | `/delete/<name>` | Remove a user |
| `GET`  | `/status` | System status JSON |
| `GET`  | `/log` | Event log (last 100) |
| `POST` | `/log/clear` | Clear log |
| `GET`  | `/alerts/stream` | SSE stream for live dashboard alerts |

---

## Security notes

- `secret.key` and `.env` are in `.gitignore` — **never commit them**
- `encrypted_embeddings/` contains biometric data — also excluded from git
- The server binds to `0.0.0.0:5000` — run on a trusted local network or behind a VPN
- `debug=False` is set in production mode by default

---

## License

MIT
