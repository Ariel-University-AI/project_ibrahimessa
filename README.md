# 🏗️ SteelCalculating

> **זיהוי וספירה אוטומטית של ברזלים מתוך תוכניות הנדסיות באמצעות בינה מלאכותית**

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)
[![Status](https://img.shields.io/badge/Status-In%20Development-orange)]()
[![ML](https://img.shields.io/badge/ML-Object%20Detection-red?logo=tensorflow)]()

---

## 📋 תוכן עניינים

- [📖 תיאור הפרויקט](#-תיאור-הפרויקט)
- [❗ הבעיה שאנחנו פותרים](#-הבעיה-שאנחנו-פותרים)
- [🎯 קהל היעד](#-קהל-היעד)
- [✨ פיצ'רים עיקריים](#-פיצרים-עיקריים)
- [🏛️ ארכיטקטורת המערכת](#-ארכיטקטורת-המערכת)
- [🤖 מודל ה-ML](#-מודל-ה-ml)
- [📁 מבנה הפרויקט](#-מבנה-הפרויקט)
- [⚙️ התקנה והתחלה](#-התקנה-והתחלה)
- [🚀 שימוש](#-שימוש)
- [📊 Dataset ונתוני אימון](#-dataset-ונתוני-אימון)
- [🧪 בדיקות](#-בדיקות)
- [🗺️ מפת דרכים](#-מפת-דרכים)
- [🤝 תרומה לפרויקט](#-תרומה-לפרויקט)
- [📄 רישיון](#-רישיון)
- [📬 יצירת קשר](#-יצירת-קשר)

---

## 📖 תיאור הפרויקט

**SteelCalculating** היא אפליקציה מבוססת בינה מלאכותית המאפשרת לזהות, לסווג ולספור ברזלי בניין (בידוד פלידה) מתוך תוכניות הנדסיות — בין אם מדובר בתמונות סרוקות או קובצי PDF.

האפליקציה מפחיתה באופן דרמטי את הזמן הנדרש לספירה ידנית של ברזלים בתוכניות, מסייעת במניעת טעויות אנוש ומייצרת דוח מסכם מסודר לשימוש בשטח.

---

## ❗ הבעיה שאנחנו פותרים

בענף הבנייה, מהנדסים וקבלנים נדרשים לספור ולסווג ברזלים (מוטות פלדה, רשתות, אטבים וכו') ידנית מתוך תוכניות הנדסיות. תהליך זה:

- ⏱️ **גוזל שעות עבודה** — תוכנית מורכבת יכולה לדרוש ספירה של מאות ברזלים
- ❌ **מועד לטעויות** — ספירה ידנית מביאה לשגיאות בהזמנות חומרים
- 💸 **יקר** — שגיאות בחישוב כמויות מובילות לגידול בעלויות הפרויקט
- 📄 **לא ניתן לאוטומציה בכלים קיימים** — כלי CAD רגילים אינם מבצעים זיהוי אוטומטי מתמונות סרוקות

**הפתרון שלנו:** העלאת תמונה או PDF של תוכנית הנדסית → זיהוי אוטומטי ← דוח ספירה מסכם.

---

## 🎯 קהל היעד

| קהל | שימוש עיקרי |
|-----|-------------|
| 🏗️ **מהנדסי בניה** | אימות כמויות פלדה לפני הזמנה |
| 👷 **קבלנים** | הכנת כתב כמויות מהיר מתוכניות |
| 🎓 **סטודנטים להנדסה אזרחית** | לימוד וסימולציה של קריאת תוכניות |
| 📐 **חברות ייעוץ הנדסי** | אוטומציה של תהליכי בקרת כמויות |

---

## ✨ פיצ'רים עיקריים

- 📤 **העלאת קבצים** — תמיכה ב-`JPG`, `PNG`, `PDF` (כולל PDF רב-עמודים)
- 🔍 **זיהוי אוטומטי** — Object Detection מבוסס Deep Learning לזיהוי ברזלים בתוכניות
- 🏷️ **סיווג סוגי ברזל** — זיהוי קוטר, סוג (עגול/משולש/רשת), וכמות
- 📊 **דוח מסכם** — יצוא לטבלה (Excel/CSV) עם פירוט לפי סוג ברזל
- 🖼️ **ויזואליזציה** — הצגת תוכנית עם Bounding Boxes על כל ברזל שזוהה
- ☁️ **ממשק Web** — ממשק משתמש פשוט ונגיש דרך הדפדפן

---

## 🏛️ ארכיטקטורת המערכת

```
┌─────────────────────────────────────────────────────────┐
│                      Frontend (Web UI)                  │
│              HTML / CSS / JavaScript                    │
└──────────────────────┬──────────────────────────────────┘
                       │ HTTP / REST API
┌──────────────────────▼──────────────────────────────────┐
│                    Backend (API Server)                 │
│                   Python / FastAPI                      │
│                                                         │
│  ┌─────────────────┐    ┌──────────────────────────┐   │
│  │  PDF → Image    │    │   Pre-processing         │   │
│  │  Converter      │───▶│   (Resize, Enhance,      │   │
│  │  (pdf2image)    │    │    Normalize)             │   │
│  └─────────────────┘    └──────────────┬─────────-─┘   │
│                                        │                │
│  ┌─────────────────────────────────────▼─────────────┐  │
│  │             ML Inference Engine                   │  │
│  │   ┌──────────────────┐  ┌─────────────────────┐  │  │
│  │   │  Object Detection │  │  Classification     │  │  │
│  │   │  (YOLOv8 / RT-   │  │  (Steel Type &      │  │  │
│  │   │   DETR)           │  │   Diameter)         │  │  │
│  │   └──────────────────┘  └─────────────────────┘  │  │
│  └───────────────────────────────────────────────────┘  │
│                                                         │
│  ┌───────────────────────────────────────────────────┐  │
│  │           Results & Report Generator              │  │
│  │         (JSON → Excel / CSV / PDF Report)         │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────┐
│                   Database / Storage                    │
│              SQLite (local) / PostgreSQL                │
│              File Storage (תוכניות ותוצאות)            │
└─────────────────────────────────────────────────────────┘
```

---

## 🤖 מודל ה-ML

### גישה כפולה: זיהוי עצמים + סיווג

#### 1. Object Detection — זיהוי מקומות הברזל

| פרמטר | ערך |
|-------|-----|
| **ארכיטקטורה** | YOLOv8 / RT-DETR |
| **Input** | תמונה מסרוקת / PDF Render (1024×1024px) |
| **Output** | Bounding Boxes + Confidence Score |
| **Task** | זיהוי מיקום ברזלים בתוכנית |

#### 2. Classification — סיווג סוג וקוטר הברזל

| פרמטר | ערך |
|-------|-----|
| **ארכיטקטורה** | EfficientNet-B3 / ResNet-50 |
| **Input** | Crop של Bounding Box שזוהה |
| **Output** | סוג ברזל + קוטר (ממ"ר) |
| **מחלקות לדוגמה** | `φ8`, `φ10`, `φ12`, `φ16`, `φ20`, `mesh`, `stirrup` |

#### Pipeline מלא

```
תמונה/PDF → Pre-process → YOLO Detection → Crop ROI → Classifier → תוצאה
```

#### מדדי ביצוע יעד

| מדד | יעד |
|-----|-----|
| mAP@0.5 (Detection) | ≥ 0.85 |
| Classification Accuracy | ≥ 90% |
| זמן עיבוד לעמוד | < 5 שניות |

---

## 📁 מבנה הפרויקט

```
SteelCalculating/
│
├── 📁 frontend/                  # ממשק משתמש
│   ├── index.html
│   ├── styles/
│   │   └── main.css
│   └── scripts/
│       └── app.js
│
├── 📁 backend/                   # שרת ה-API
│   ├── main.py                   # FastAPI entry point
│   ├── routes/
│   │   ├── upload.py             # נקודות קצה להעלאת קבצים
│   │   └── results.py            # נקודות קצה לתוצאות
│   ├── services/
│   │   ├── pdf_converter.py      # המרת PDF לתמונה
│   │   ├── preprocessor.py       # עיבוד מקדים של תמונה
│   │   ├── detector.py           # הרצת YOLO Detection
│   │   ├── classifier.py         # סיווג סוג הברזל
│   │   └── report_generator.py   # יצירת דוח סופי
│   └── models/
│       ├── detection_model.pt    # משקלי YOLO (לא ב-Git)
│       └── classification_model.pt
│
├── 📁 ml/                        # קוד אימון מודלים
│   ├── notebooks/
│   │   ├── 01_data_exploration.ipynb
│   │   ├── 02_model_training.ipynb
│   │   └── 03_evaluation.ipynb
│   ├── train_detector.py
│   ├── train_classifier.py
│   └── evaluate.py
│
├── 📁 data/                      # נתונים (לא ב-Git)
│   ├── raw/                      # תמונות גולמיות
│   ├── annotated/                # תמונות עם אנוטציות YOLO
│   └── processed/                # נתונים מעובדים לאימון
│
├── 📁 tests/                     # בדיקות
│   ├── test_detector.py
│   ├── test_classifier.py
│   └── test_api.py
│
├── 📁 docs/                      # תיעוד
│   ├── api_reference.md
│   └── model_architecture.md
│
├── .env.example                  # משתני סביבה לדוגמה
├── .gitignore
├── requirements.txt
├── docker-compose.yml
├── Dockerfile
└── README.md
```

---

## ⚙️ התקנה והתחלה

### דרישות מקדימות

- Python `3.10+`
- `pip` או `conda`
- CUDA 11.8+ (אופציונלי — לאימוץ GPU)
- Poppler (לתמיכה ב-PDF)

### 1. שיבוט המאגר

```bash
git clone https://github.com/your-username/SteelCalculating.git
cd SteelCalculating
```

### 2. יצירת סביבה וירטואלית

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

### 3. התקנת תלויות

```bash
pip install -r requirements.txt
```

### 4. הגדרת משתני סביבה

```bash
cp .env.example .env
# ערוך את קובץ .env לפי הצורך
```

### 5. הורדת משקלי המודל

```bash
python scripts/download_models.py
```

### 6. הפעלת השרת

```bash
cd backend
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

פתח את הדפדפן בכתובת: `http://localhost:8000`

### 🐳 הפעלה עם Docker

```bash
docker-compose up --build
```

---

## 🚀 שימוש

### ממשק Web

1. פתח את האפליקציה בדפדפן
2. לחץ על **"העלה תוכנית"** ובחר קובץ `JPG`, `PNG` או `PDF`
3. המתן לעיבוד (בדרך כלל 3-10 שניות)
4. צפה בתוצאות — תוכנית עם סימון הברזלים + טבלת ספירה
5. לחץ **"ייצא דוח"** כדי להוריד `Excel` / `CSV`

### API (למפתחים)

#### העלאת תמונה לזיהוי

```http
POST /api/v1/detect
Content-Type: multipart/form-data

file: <קובץ תמונה/PDF>
```

**תגובה לדוגמה:**

```json
{
  "status": "success",
  "processing_time_ms": 2340,
  "page_count": 1,
  "detections": [
    {
      "id": 1,
      "type": "rebar",
      "diameter_mm": 12,
      "label": "φ12",
      "confidence": 0.94,
      "bounding_box": { "x": 120, "y": 340, "width": 45, "height": 45 }
    }
  ],
  "summary": {
    "φ8":  3,
    "φ10": 7,
    "φ12": 15,
    "φ16": 8,
    "mesh": 2
  },
  "annotated_image_url": "/results/abc123/annotated.jpg",
  "report_url": "/results/abc123/report.xlsx"
}
```

#### קבלת דוח קיים

```http
GET /api/v1/results/{job_id}
```

---

## 📊 Dataset ונתוני אימון

### מקורות נתונים

| מקור | סוג | כמות משוערת |
|------|-----|-------------|
| תוכניות הנדסיות ציבוריות | PDF / תמונה | 500+ |
| תרומות קבלנים ומהנדסים | PDF / תמונה | TBD |
| Synthetic Data (מחולל) | תמונה | 2,000+ |

### פורמט אנוטציה

אנו משתמשים בפורמט **YOLO v8**:

```
# <class_id> <cx> <cy> <width> <height>  (ערכים נורמליזציה 0-1)
0 0.512 0.340 0.043 0.043
1 0.720 0.218 0.052 0.052
```

| Class ID | תיאור |
|----------|-------|
| 0 | φ8 |
| 1 | φ10 |
| 2 | φ12 |
| 3 | φ16 |
| 4 | φ20 |
| 5 | mesh (רשת) |
| 6 | stirrup (אטב) |

### כלי אנוטציה מומלצים

- [Roboflow](https://roboflow.com/) — כלי ענן לאנוטציה ו-augmentation
- [LabelImg](https://github.com/HumanSignal/labelImg) — כלי desktop בקוד פתוח
- [CVAT](https://cvat.ai/) — פלטפורמת אנוטציה מתקדמת

---

## 🧪 בדיקות

### הרצת כל הבדיקות

```bash
pytest tests/ -v
```

### בדיקות ספציפיות

```bash
# בדיקות ה-API בלבד
pytest tests/test_api.py -v

# בדיקות מודל הזיהוי
pytest tests/test_detector.py -v

# בדיקות סיווג
pytest tests/test_classifier.py -v
```

### כיסוי קוד

```bash
pytest --cov=backend tests/ --cov-report=html
```

---

## 🗺️ מפת דרכים

### גרסה 1.0 (MVP) — Q2 2026
- [x] ממשק העלאת קבצים
- [x] עיבוד מקדים של תמונות וPDF
- [ ] מודל זיהוי בסיסי (YOLOv8)
- [ ] דוח CSV/Excel
- [ ] ממשק Web בסיסי

### גרסה 1.5 — Q3 2026
- [ ] שיפור דיוק המודל (Fine-tuning)
- [ ] תמיכה בתוכניות רב-עמודים
- [ ] ייצוא לפורמט PDF מעוצב
- [ ] תמיכה בשפות מרובות (EN / AR)

### גרסה 2.0 — Q4 2026
- [ ] לוח מחוונים לניהול פרויקטים
- [ ] אינטגרציה עם AutoCAD / Revit
- [ ] API ציבורי עם מפתחות
- [ ] אפליקציה לנייד (PWA)
- [ ] זיהוי ממדים וחישוב משקל אוטומטי

---

## 🤝 תרומה לפרויקט

תרומות מתקבלות בברכה! אנא פעלו לפי ה-flow הבא:

1. **Fork** את המאגר
2. צרו **branch** חדש:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. בצעו את השינויים ו-**commit**:
   ```bash
   git commit -m "feat: הוספת תמיכה ב-φ25"
   ```
4. **Push** ל-branch:
   ```bash
   git push origin feature/your-feature-name
   ```
5. פתחו **Pull Request** עם תיאור מפורט

### הנחיות קוד

- עקבו אחר [PEP 8](https://peps.python.org/pep-0008/) לקוד Python
- הוסיפו Docstrings לכל פונקציה ציבורית
- ודאו שכל הבדיקות עוברות לפני PR
- כתבו בדיקות לפיצ'רים חדשים

---

## 📄 רישיון

פרויקט זה מופץ תחת **רישיון MIT**. ראה קובץ [LICENSE](LICENSE) לפרטים מלאים.

```
MIT License © 2026 SteelCalculating Team
```

---

## 📬 יצירת קשר

| ערוץ | פרטים |
|------|-------|
| 📧 **אימייל** | contact@steelcalculating.io |
| 🐛 **דיווח באגים** | [GitHub Issues](https://github.com/your-username/SteelCalculating/issues) |
| 💬 **שאלות** | [GitHub Discussions](https://github.com/your-username/SteelCalculating/discussions) |

---

<div align="center">

**נבנה עם ❤️ עבור ענף הבנייה הישראלי**

*SteelCalculating — חישוב חכם, בניה בטוחה*

</div>
