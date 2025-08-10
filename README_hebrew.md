
````markdown
# twill_throttle

מודול פייתון המספק דקורטורים להגבלת קצב קריאות לפונקציות, עם תמיכה גם לפונקציות סינכרוניות וגם לאסינכרוניות.

---

## תכונות

- הגבלת קריאות לפונקציה בודדת לשנייה/דקה (`FuncPerMin`)
- הגבלת קריאות משותפת בין מספר פונקציות (`SharedRateLimiter`)
- תמיכה בפונקציות סינכרוניות ואסינכרוניות
- נעילה Thread-safe ו-async-safe
- קל לשימוש ולהטמעה בפרויקטים קיימים

---

## התקנה

ניתן להתקין באמצעות pip (אם המודול מפורסם ב-PyPI):

```bash
pip install twill_throttle
````

או פשוט להעתיק את קבצי המודול לפרויקט שלך.

---

## דוגמאות שימוש

### דוגמה 1 – הגבלת קריאות לפונקציה בודדת

```python
from twill_throttle import FuncPerMin

@FuncPerMin(max_calls_per_minute=3)
def greet(name):
    print(f"היי, {name}!")

for i in range(5):
    greet(f"משתמש {i+1}")  # הקריאות האחרונות יחכו עד שהדקה תסתיים
```

---

### דוגמה 2 – הגבלה משותפת לפונקציות מרובות

```python
import asyncio
from twill_throttle import SharedRateLimiter

shared_limiter = SharedRateLimiter(max_calls_per_minute=4)

@shared_limiter
def process_data(data):
    print(f"מעבד נתונים: {data}")

@shared_limiter
async def async_task(task_id):
    print(f"מבצע משימה אסינכרונית {task_id}...")
    await asyncio.sleep(0.5)
    print(f"סיים משימה {task_id}")

async def main():
    process_data("קובץ 1")
    process_data("קובץ 2")
    await asyncio.gather(
        async_task(1),
        async_task(2),
        async_task(3)
    )

asyncio.run(main())
```

---

## שימוש מומלץ

* השתמש ב-`FuncPerMin` להגבלת קריאות לפונקציה ספציפית בלבד.
* השתמש ב-`SharedRateLimiter` כאשר תרצה להגביל את סך הקריאות המשותף של מספר פונקציות.
* שים לב ששני הדקורטורים יחכו עד שיתאפשר קריאה נוספת בהתאם להגבלה.

---

## רישיון

קוד זה מופץ תחת רישיון MIT.

---

