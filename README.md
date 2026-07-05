# ระบบรายงานขาด ลา มาสาย และโดดเรียนของนักเรียน

เว็บแอป Streamlit สำหรับอ่านข้อมูลจาก Google Sheets แบบ read only และแสดงรายงานเฉพาะนักเรียนที่มีสถานะผิดปกติ ได้แก่ ขาด, ลาป่วย, ลากิจ, มาสาย และโดดเรียน โดยไม่แสดงสถานะ `มา`

## โครงสร้างไฟล์

```text
attendance_app/
├── app.py
├── requirements.txt
├── README.md
└── .streamlit/
    └── secrets.toml.example
```

## การเตรียม Google Cloud Project

1. ไปที่ Google Cloud Console: https://console.cloud.google.com/
2. สร้าง Project ใหม่ หรือเลือก Project ที่มีอยู่แล้ว
3. เปิดใช้งาน Google Sheets API
4. ไปที่ IAM & Admin > Service Accounts
5. กด Create service account
6. ตั้งชื่อ Service Account แล้วสร้างให้เสร็จ
7. เข้า Service Account ที่สร้างไว้ แล้วไปที่ Keys
8. กด Add key > Create new key
9. เลือก JSON แล้วดาวน์โหลด credential

## การตั้งค่า secrets.toml

1. คัดลอกไฟล์ `.streamlit/secrets.toml.example`
2. เปลี่ยนชื่อเป็น `.streamlit/secrets.toml`
3. เปิดไฟล์ JSON credential ที่ดาวน์โหลดมา
4. นำค่าจาก JSON ไปใส่ใน `.streamlit/secrets.toml` ตาม key ที่ตรงกัน

ตัวอย่างโครงสร้าง:

```toml
[gcp_service_account]
type = "service_account"
project_id = "xxxx"
private_key_id = "xxxx"
private_key = "-----BEGIN PRIVATE KEY-----\nxxxx\n-----END PRIVATE KEY-----\n"
client_email = "xxxx@xxxx.iam.gserviceaccount.com"
client_id = "xxxx"
auth_uri = "https://accounts.google.com/o/oauth2/auth"
token_uri = "https://oauth2.googleapis.com/token"
auth_provider_x509_cert_url = "https://www.googleapis.com/oauth2/v1/certs"
client_x509_cert_url = "xxxx"
```

## การแชร์ Google Sheet

1. เปิด Google Sheet ต้นทาง
2. กด Share
3. นำค่า `client_email` จาก Service Account ไปเพิ่มเป็นผู้มีสิทธิ์
4. ตั้งสิทธิ์เป็น Viewer ก็เพียงพอ เพราะแอปอ่านข้อมูลอย่างเดียว

Google Sheet ID ที่ใช้ในแอป:

```text
1hDAtGtj4V31AuVLoiOPqFjq5LI3vXu6nUM48sJ6Lk8o
```

ชีตที่ต้องมี:

- `Students`
- `Attendance_Log`

## คอลัมน์ที่ต้องมี

ชีต `Students`

- ห้องเรียน
- เลขที่
- เลขประจำตัว
- ชื่อ-สกุล

ชีต `Attendance_Log`

- timestamp
- date
- day
- level
- classroom
- student_id
- student_name
- periods
- status
- teacher_username
- teacher_name
- note

## การติดตั้ง

```bash
pip install -r requirements.txt
```

## การรันแอป

```bash
streamlit run app.py
```

## หมายเหตุสำคัญ

ผลลัพธ์สุดท้ายจะแสดงเฉพาะสถานะ:

- ขาด
- ลาป่วย
- ลากิจ
- มาสาย
- โดดเรียน

สถานะ `มา` และสถานะอื่นที่ไม่อยู่ในรายการข้างต้นจะไม่ถูกนำมาแสดงในรายงาน
