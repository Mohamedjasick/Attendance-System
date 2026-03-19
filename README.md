# 🎓 Digital Face Recognition Attendance System

A web-based automatic attendance management system that uses **deep learning face recognition** to identify students and mark their attendance in real time — no manual entry required.

![Dashboard Screenshot](https://raw.githubusercontent.com/Mohamedjasick/Attendance-System/main/static/images/dashboard.png)

---

## 📸 Demo

> Student walks in front of a webcam → Face is detected and recognized → Attendance is marked automatically in the database.

---

## ✨ Features

- **Real-time face recognition** using FaceNet (InceptionResnetV1) + MTCNN for face alignment
- **Fast nearest-neighbor search** via Facebook's FAISS index (512-dim embeddings)
- **Student registration** with webcam-captured face images
- **Attendance dashboard** with a 30-day bar chart
- **Attendance records** filtered by Daily / Weekly / Monthly / All
- **CSV export** of attendance with IST-formatted timestamps
- **Clear attendance history** with a single click
- **Cooldown/debounce** mechanism to prevent duplicate entries
- **Production-ready** WSGI server via Waitress (Windows-friendly)

---

## 🗂️ Project Structure

```
Attendance System/
│
├── app.py                  # Flask application & all API routes
├── model.py                # FaceNet model, FAISS index, training logic
├── serve.py                # Waitress production server entry point
├── attendance.db           # SQLite database (auto-created)
├── train_status.json       # Background training progress tracker
│
├── dataset/
│   ├── face_encodings.faiss        # Trained FAISS index
│   ├── student_ids.pkl             # Student ID map for FAISS
│   ├── haarcascade_frontalface_default.xml
│   └── <student_id>/               # One folder per student (face images)
│
├── static/
│   ├── css/style.css
│   ├── js/
│   │   ├── camera_add_student.js
│   │   ├── camera_mark.js
│   │   └── dashboard.js
│   └── images/bg.png
│
└── templates/
    ├── index.html
    ├── add_student.html
    ├── mark_attendance.html
    └── attendance_record.html
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| Face Detection | MTCNN (facenet-pytorch) |
| Face Recognition | FaceNet — InceptionResnetV1 (VGGFace2 pretrained) |
| Vector Search | FAISS (Facebook AI Similarity Search) |
| Database | SQLite |
| Frontend | HTML, CSS, JavaScript |
| WSGI Server | Waitress |
| Deep Learning | PyTorch |

---

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/Mohamedjasick/Attendance-System.git
cd attendance-system
```

### 2. Create and activate a virtual environment

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS/Linux
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install flask waitress pandas pytz torch torchvision facenet-pytorch faiss-cpu opencv-python pillow
```

> **GPU users:** Replace `faiss-cpu` with `faiss-gpu` and install the CUDA-compatible version of PyTorch from [pytorch.org](https://pytorch.org).

---

## 🚀 Usage

### Start the server

```bash
python serve.py
```

Then open your browser at: **http://127.0.0.1:5000**

---

### Workflow

#### Step 1 — Register a Student
1. Go to **Add Student**
2. Fill in Name, Roll No, Class, Section, Registration No.
3. Capture ~50 face images using the webcam
4. Submit

#### Step 2 — Train the Model
1. Click **Train Model** from the dashboard
2. Wait for the progress bar to reach 100%
3. The FAISS index is now built with all registered students

#### Step 3 — Mark Attendance
1. Go to **Mark Attendance**
2. Point the webcam at a student
3. The system recognizes the face and logs the timestamp automatically

#### Step 4 — View Records
- Go to **Attendance Records** to view/filter logs
- Use **Download CSV** to export records
- Use **Clear History** to reset attendance data

---

## 🔌 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | Dashboard |
| GET/POST | `/add_student` | Register new student |
| POST | `/upload_face` | Upload captured face images |
| GET | `/train_model` | Start background model training |
| GET | `/train_status` | Poll training progress |
| GET | `/mark_attendance` | Attendance marking page |
| POST | `/recognize_face` | Recognize face from image |
| GET | `/attendance_record` | View records (period filter) |
| GET | `/download_csv` | Export attendance as CSV |
| GET | `/clear_attendance_history` | Delete all attendance records |
| GET | `/students` | List all registered students |
| DELETE | `/students/<id>` | Delete a student |
| GET | `/attendance_stats` | 30-day chart data (JSON) |

---

## 📋 Notes

- Attendance timestamps are stored in **UTC** and displayed in **IST (Asia/Kolkata)**
- A **5-second cooldown** per student prevents duplicate entries during live recognition
- The model confidence threshold is set to **0.5** — faces below this are rejected
- Student face images are stored locally in `dataset/<student_id>/` and are **not uploaded anywhere**
- The `dataset/` folder and `attendance.db` are excluded from version control via `.gitignore`

---

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## 🙋 Author

Made with ❤️ — contributions and feedback are welcome!
