# City-Parivartan 🌆

**City-Parivartan** is a web-based community civic portal designed to bridge the gap between citizens and municipal authorities. The platform empowers users to report local infrastructure issues—such as broken roads, garbage heaps, and drainage overflows—while providing municipal admins with a dedicated back-end to track, update, and resolve complaints efficiently.

---

## 🚀 Key Features

### 👤 Citizen Module
* **Issue Reporting:** Submit complaints regarding municipal services with localized categories.
* **Category Filtering:** Specialized tracking for **Roads**, **Garbage**, **Drainage**, **Water Supply**, and **Animal Rescue**.
* **Status Tracking:** Real-time visibility into whether a complaint is "Pending", "In Progress", or "Resolved".
* **User Profile:** Manage account settings, history, and active grievance forms.

### 👑 Administration Module
* **Admin Dashboard:** Centralized panel to monitor city-wide public grievances.
* **Complaint Management:** Update the status of active issues and assign operational tasks.
* **Live Notifications:** Real-time WebSocket-powered communication alerts for new reports.

---

## 📁 Repository Structure

The project follows an organized Flask/Python modular architecture combining procedural python back-end routing with frontend templates:

```text
├── app.py                  # Main Flask application entry point
├── config.py               # Database connections and app configurations
├── socket_utils.py         # Real-time WebSocket handlers for administrative alerts
│
├── 🐍 Python Modules (Routing & Logic)
│   ├── index.py / login1.py / register.py / forget.py   # Auth & Session system
│   ├── user.py / profile.py / user_issues.py            # Citizen account logic
│   ├── complaints.py / report.py                        # Grievance registration
│   ├── admin_complaints.py / admin_issue.py             # Administrative logic
│   └── [roads/garbage/drainage/water/animal_rescue].py   # Categorized issue logic
│
└── 🎨 Frontend (Templates & Assets)
    ├── style.css / style_admin.css                     # Main layouts and UI themes
    ├── base.html / navbar.html / sidebar.html          # Global layout scaffolding
    └── *.html                                           # Modular system views
```

---

## 🛠️ Tech Stack

* **Backend:** Python, Flask
* **Real-Time:** WebSockets (`socket_utils.py`)
* **Frontend:** HTML5, CSS3
* **Database:** SQL (configured in `config.py`)

---

## 💻 Local Installation & Setup

### 1. Clone the Repository
```bash
git clone https://github.com
cd City-Parivartan
```

### 2. Set Up a Virtual Environment
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies
Ensure you have Flask and its necessary extensions installed:
```bash
pip install Flask flask-socketio
```
*(Note: Update your database-specific drivers as specified inside your `config.py` file).*

### 4. Configure the Application
Open `config.py` and modify the environment variables, secret keys, and local database connection strings to match your environment setup.

### 5. Run the Application
```bash
python app.py
```
Open your web browser and navigate to `http://127.0.0.1:5000` to interact with the application.

---

## ☁️ Cloud Deployment Notes

Because this project utilizes WebSockets (`flask-socketio`) for live administrative notifications, traditional serverless environments (like AWS Lambda or standard Vercel) are not ideal due to session persistent constraints. Use the options below for deployment:

### Option A: Google Cloud Run (Containerized Docker Deployment)
1. **Create a `Dockerfile`** in the root directory:
   ```dockerfile
   FROM python:3.11-slim
   WORKDIR /app
   COPY . /app
   RUN pip install --no-cache-dir -r requirements.txt
   EXPOSE 8080
   CMD ["gunicorn", "--worker-class", "eventlet", "-w", "1", "-b", "0.0.0.0:8080", "app:app"]
   ```
2. **Build and Deploy to Google Cloud Run:**
   Ensure Session Affinity is **enabled** in your Google Cloud Run settings to prevent WebSocket transport errors.

### Option B: Render or DigitalOcean App Platform
1. **Build Command:** `pip install -r requirements.txt`
2. **Start Command:** `gunicorn --worker-class eventlet -w 1 -b 0.0.0.0:$PORT app:app`
3. **Environment Variables:** Securely bind database production links and your Flask `SECRET_KEY` inside the platform setting dashboard instead of hardcoding them in `config.py`.

---

## 🤝 Contributor Guidelines

We welcome contributions to enhance **City-Parivartan**! Follow these instructions to maintain a clean codebase:

1. **Fork the Repository:** Create a personal copy of the project on GitHub.
2. **Create a Feature Branch:** Break down changes into clean, contextual feature branches:
   ```bash
   git checkout -b feature/amazing-new-capability
   ```
3. **Keep Code Modular:** If adding a new civic problem category, mimic the isolated structural design seen in `roads.py` or `garbage.py`.
4. **Test Real-Time Functions:** Ensure updates to complaint tables correctly fire hooks through `socket_utils.py`.
5. **Open a Pull Request:** Describe your changes explicitly, reference any related active bug issues, and submit for peer review.

---

## 📝 License
This project is open-source. Please check the core repository page for active licensing details.
