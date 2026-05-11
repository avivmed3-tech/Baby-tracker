# CLAUDE.md - baby-tracker

> הקשר פרויקט עבור Claude. קובץ זה נטען בתחילת כל session ב-Claude Code.

## סקירה כללית

**baby-tracker** - אפליקציית מעקב לתינוקת בעברית. מעקב אחרי הנקה, שאיבה, בקבוקים, החתלות (פיפי/קקי) ושינה.

- **שפה**: עברית (RTL)
- **קהל יעד**: אישי (האמא של אביב)
- **סטטוס**: פיתוח אישי, לא מסחרי

## ארכיטקטורה

- **קובץ יחיד**: `index.html` (~1791 שורות)
- **Frontend**: React 18 + Babel in-browser (כמו שאר הפרויקטים)
- **Backend**: Supabase (PostgreSQL)
- **Auth**: SHA-256 hashing client-side + טבלת users (כמו ב-Maneger-app)
- **Hosting**: GitHub Pages
- **Charts**: Chart.js v4
- **Fonts**: Heebo (body) + Fraunces (display headings)

## עיצוב

### פלטה
- ורוד: `#FFB6C9`, `#FF8FB1`, `#FF6B9D`, `#E91E63`
- כחול: `#90CAF9`, `#64B5F6`, `#42A5F5`, `#1E88E5`
- רקעים: `#FFF5F8` (קרם ורוד), `#F0F7FF` (קרם כחול)
- טקסט: `#2D2438` (סגול-שחור), `#5C4E6B` (אפור-סגול)

### עקרונות
- מובייל ראשון (iPhone priority)
- כפתורי גישה מהירה (2 עמודות)
- מודלים מלמטה (slide-up)
- Bottom navigation עם 4 טאבים
- אפקטי backdrop-filter על ה-nav

## סכמת DB

```sql
users (id, username, password_hash, baby_name, created_at)
events (id, user_id, event_type, started_at, ended_at, duration_seconds, amount_ml, side, notes, created_at)
```

### סוגי אירועים (event_type)
- `nursing` - הנקה (עם side: right/left/both)
- `pumping` - שאיבה (עם side + amount_ml)
- `bottle` - בקבוק (עם amount_ml)
- `pee` - פיפי
- `poo` - קקי
- `sleep` - שינה

## פיצ'רים מרכזיים

1. **טיימרים חיים** - הנקה ושאיבה עם persistent state ב-localStorage
2. **גישה מהירה** - 6 כפתורים על מסך הבית, כל אחד מציג "לפני X זמן"
3. **סטטיסטיקות** - גרף 7 ימים, ממוצעים שבועיים
4. **היסטוריה** - מקובצת לפי תאריך עברי
5. **תזכורות** - האכלה ושאיבה (toggle בהגדרות)

## הגדרות פיתוח של אביב

- עובד מאייפון
- מעדיף קובץ HTML יחיד
- העדפות תקשורת: עברית, סגנון ישיר ומכוון תוצאה
- מעדיף סיכומים תמציתיים אחרי שינויים
- להתייעץ לפני שינויים גדולים

## הערות לעתיד

- [ ] תזכורות push אמיתיות (Service Worker)
- [ ] גרף כמויות שאיבה במ"ל
- [ ] ייצוא לרופא/טיפת חלב (PDF)
- [ ] PWA install banner
- [ ] Service Worker עם cache versioning (לשקול אחרי שיתייצב)

## glossary עברי-אנגלי

| עברית | English |
|--------|---------|
| הנקה | nursing |
| שאיבה | pumping |
| בקבוק | bottle |
| החתלה | diaper change |
| צד (ימני/שמאלי) | side (right/left) |
| כמות במ"ל | amount in ml |
| משך | duration |
