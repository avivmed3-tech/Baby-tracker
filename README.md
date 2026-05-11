# 🍼 Baby Tracker | מעקב התינוקת

אפליקציית מעקב חכמה לאמא של תינוקת — האכלות, שאיבות, החתלות ושינה — בעברית מלאה עם RTL ועיצוב מודרני בגוונים של ורוד, לבן וכחול.

A modern Hebrew baby-tracking PWA for nursing, pumping, bottle feeds, diaper changes and sleep.

🌐 **Live**: https://[your-username].github.io/baby-tracker/

---

## ✨ פיצ'רים | Features

- 🤱 **טיימר חי להנקה** — עם בחירת צד (ימני/שמאלי/שניהם)
- 💧 **טיימר שאיבה** — עם רישום אוטומטי של כמות במ"ל
- 🍼 **בקבוקים** — רישום ידני של זמן וכמות
- 💛💩 **החתלות** — רישום מהיר בלחיצה אחת
- 😴 **שינה** — מעקב אחרי שעות שינה
- 📊 **גרפים** — סטטיסטיקה ל-7 ימים אחרונים
- 📋 **היסטוריה** — מקובצת לפי תאריך עברי
- 🔔 **תזכורות** — האכלה ושאיבה
- ☁️ **סנכרון Supabase** — בין מובייל ומחשב
- 🎨 **עיצוב RTL** — Heebo + Fraunces, אופטימיזציה למובייל

---

## 🚀 התקנה | Setup

### 1. הקמת Supabase

1. הירשמי ל-[supabase.com](https://supabase.com) (חינם)
2. צרי פרויקט חדש
3. תחת **SQL Editor** הריצי את ה-SQL הבא:

```sql
-- טבלת משתמשים
CREATE TABLE users (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  username TEXT UNIQUE NOT NULL,
  password_hash TEXT NOT NULL,
  baby_name TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- טבלת אירועים
CREATE TABLE events (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  event_type TEXT NOT NULL,
  started_at TIMESTAMPTZ NOT NULL,
  ended_at TIMESTAMPTZ,
  duration_seconds INT,
  amount_ml INT,
  side TEXT,
  notes TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_events_user_started
  ON events(user_id, started_at DESC);

-- הפעלת RLS
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE events ENABLE ROW LEVEL SECURITY;

CREATE POLICY "anon all users" ON users
  FOR ALL USING (true) WITH CHECK (true);
CREATE POLICY "anon all events" ON events
  FOR ALL USING (true) WITH CHECK (true);
```

4. תחת **Settings → API** העתיקי:
   - `Project URL`
   - `anon public key`

### 2. שימוש באפליקציה

1. פתחי את האפליקציה
2. הזיני את ה-URL וה-Key של Supabase
3. צרי משתמש חדש
4. התחילי לעקוב 💕

---

## 🛠 ארכיטקטורה | Architecture

- **Frontend**: HTML יחיד עם React + Babel in-browser
- **Backend**: Supabase (PostgreSQL)
- **Auth**: SHA-256 hashing (client-side)
- **Hosting**: GitHub Pages
- **Fonts**: Heebo + Fraunces (Google Fonts)
- **Charts**: Chart.js
- **Architecture**: Single-file, mobile-first, RTL

---

## 📱 קיצור דרך לאייפון | iPhone Shortcut

1. פתחי את האתר ב-Safari
2. לחצי על כפתור השיתוף ⬆️
3. בחרי "הוסף למסך הבית"
4. האפליקציה תעבוד כמו אפליקציה מקומית

---

## 📄 License

MIT — ראי קובץ [LICENSE](LICENSE)
