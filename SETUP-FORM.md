# חיבור טופס ההרשמה

דף ההרשמה (`index.html`) שולח את הפרטים ל-Google Form, והתשובות נכנסות לגיליון
שהתוכנה קוראת. מי שנרשם מאושר אוטומטית.

**זמן: כ-10 דקות, פעם אחת.**

---

## 1. יצירת הטופס

[forms.google.com](https://forms.google.com) → טופס ריק.

שתי שאלות, שתיהן מסוג **תשובה קצרה**, ושתיהן **חובה**:

| # | שאלה |
|---|---|
| 1 | שם מלא |
| 2 | כתובת Gmail |

## 2. שליפת כתובת השליחה ומזהי השדות

בטופס: **⋮ → Get pre-filled link** → למלא ערכי דמה (`AAA` ו-`BBB`) → **Get link** → להעתיק.

הקישור ייראה כך:

```
https://docs.google.com/forms/d/e/1FAIpQL.../viewform?usp=pp_url&entry.123456=AAA&entry.789012=BBB
```

מתוכו צריך שלושה דברים:

- **כתובת שליחה** — הקישור עד `/viewform`, כשמחליפים אותו ב-`/formResponse`
- **מזהה שדה השם** — `entry.123456` (זה שערכו `AAA`)
- **מזהה שדה המייל** — `entry.789012` (זה שערכו `BBB`)

## 3. הזנה ל-`index.html`

לערוך את הקובץ כאן ב-GitHub ולמלא את שלוש השורות:

```js
const FORM = {
  action: 'https://docs.google.com/forms/d/e/1FAIpQL.../formResponse',
  nameField: 'entry.123456',
  emailField: 'entry.789012'
}
```

## 4. פרסום הגיליון

בטופס: **Responses → Link to Sheets** → יצירת גיליון חדש.

בגיליון שנפתח: **File → Share → Publish to web** →
בתפריט הפורמט לבחור **Comma-separated values (.csv)** → **Publish** → להעתיק את הכתובת.

## 5. חיבור לתוכנה

ב-`.env` של הקוד:

```
MAIN_VITE_SIGNUP_SHEET_URL=<הכתובת מהשלב הקודם>
```

ואז לעדכן גם ב-GitHub Secrets:

```bash
gh secret set SIGNUP_SHEET_URL --repo 5645hm-a11y/beit-hakolnoa --body "<הכתובת>"
```

ולשחרר גרסה חדשה.

---

## איך זה מתנהג

| מצב | תוצאה |
|---|---|
| נרשם דרך הטופס | ✅ נכנס אוטומטית |
| נמצא ב-`allowlist.json` | ✅ נכנס |
| לא נמצא באף אחד | ❌ מסך חסימה |

**להסרת גישה:** למחוק את השורה מהגיליון. תופס בהפעלה הבאה של התוכנה אצלו.

> **חשוב לדעת:** כל מי שממלא את הטופס מקבל גישה. אין דרך לאמת שהוא באמת חבר
> בקבוצת הצ'אט — ה-Chat API של גוגל זמין רק לחשבונות Workspace עסקיים ולא
> לחשבונות gmail.com. אם צריך שליטה, אל תפרסמו את הקישור לדף ההרשמה בפומבי,
> או השאירו את `MAIN_VITE_SIGNUP_SHEET_URL` ריק ואשרו ידנית ב-`allowlist.json`.
