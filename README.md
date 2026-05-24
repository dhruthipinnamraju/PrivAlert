# PrivAlert
PrivAlert is an IoT-based smart surveillance and intrusion detection system built using ESP32, PIR motion sensing, OpenCV face recognition, Flask, and Telegram alerts.
When motion is detected, the ESP32 triggers the server, which captures an image from the laptop webcam, performs face recognition, and sends instant alerts for unknown visitors.

Features
Real-time motion detection using ESP32 + PIR sensor
Face recognition using OpenCV LBPH
Instant Telegram intrusion alerts with captured image
Secure encrypted facial embedding storage
Live web dashboard for monitoring and management
User enrollment and deletion system
Event logging and intrusion tracking
SSE-based live alert notifications
Cooldown protection against alert spam
System Workflow
PIR sensor detects motion
ESP32 sends request to Flask server (/motion)
Laptop webcam captures image
OpenCV performs face recognition
If recognized:
Access marked as authorized
If unknown:
Intruder alert triggered
Telegram notification sent with image

Because apparently humans now need embedded systems, computer vision, networking, encryption, and messaging APIs just to know who walked past a door.

Technologies Used
Python
Flask
OpenCV
ESP32
PIR Sensor
Telegram Bot API
HTML/CSS/JavaScript
Fernet Encryption

Installation
1. Clone Repository
git clone <your-repo-url>
cd PrivAlert
2. Install Dependencies
pip install -r requirements.txt
3. Configure Telegram

Edit these fields in server.py:

BOT_TOKEN = "YOUR_BOT_TOKEN"
CHAT_ID = "YOUR_CHAT_ID"

4. Run Server
python server.py

Server starts at:

http://localhost:5000

ESP32 Motion Trigger

ESP32 should send a GET request to:

http://<SERVER_IP>:5000/motion

when PIR motion is detected.

Dashboard Features

Enroll new users
Test recognition manually
View enrolled users
Delete users
Monitor intrusion logs
Receive live alert banners
Security Features
Encrypted facial embeddings using Fernet
Local storage only
Alert cooldown protection
Thread-safe motion handling
Future Improvements
Deep learning based recognition
Cloud dashboard deployment
Mobile application support
Multi-camera support
Voice alerts
Night vision integration
License

This project is for educational and research purposes.

