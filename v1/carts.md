# Konimbo API Docs

## הזמנות
### תוכן עניינים
0. [חזור לדף הראשי](https://github.com/konimboltd/api-documentation)
1. [הקדמה](#user-content-הקדמה)
2. [EndPoints](#user-content-endpoints)
3. [פירוט השדות](#user-content-פירוט-השדות)
4. [עגלה](#user-content-עגלה)
6. [עריכת עגלה](#user-content-עריכת-עגלה)

### הקדמה

לכל הזמנה יש עגלת קניות
לכל עגלה יש:
* המוצרים שנרכשו
* השדרוגים שנרכשו
* ההנחות שהתקבלו
* פרטי תשלומים

### EndPoints

* [GET /{apiVersion}/carts/{id}](#user-content-עגלה)
* [PUT /{apiVersion}/carts/{id}](#user-content-עריכת-עגלה)

### פירוט השדות עגלה
שם השדה | סכמה | ניתן לכתיבה | הסבר
:---|:---|---:|---:
id | Integer | לא | מספר מזהה העגלה
items | Object | לא |  המוצרים בעגלה
upgrades | Object | לא |  השדרוגים של העגלה
shipping | Object | לא |  פרטי משלוח
payments | Object | לא |  פרטי תשלומים
total_price | Integer | לא |  סהכ מחיר העגלה
var_in_json | Object | לא | שדות נוספים (מבנה נתונים)
discounts | Object | כן | הנחות
destroy_discounts | Object | כן | מחיקת הנחות
 

### עגלה
#### תיאור
בעזרת השירות הזה, ניתן להחזיר מידע על עגלה ספציפית.


#### EndPoint
```
GET /{apiVersion}/carts/{id}?token={yourToken}
```
#### Parameters
##### בקשת שדות מסויימים
כדי שהשרת יידע איזה שדות להחזיר יש לציין בפרמטר `attributes` את השדות המבוקשים מופרדים בפסיקים. 

[לרשימת השדות](#user-content-פירוט-השדות)

לדוגמא: `attributes=id,payment_status,total_price`

במידה ולא נשלח פרמטר זה, יוחזרו כל השדות המורשים למשתמש זה.

#### Responses
* 200 - The requested order
* 401 - Unauthorized
* 404 - No results found
* 500 - Internal error

#### דוגמאות
הקריאה הבאה תחזיר את כל הפרמטרים המורשים של הזמנה זו

```
GET /v1/carts/5552478?token=794fc87483d6b5d519a98300e7bfcffa7a7aba008160c61cbf9b50fb59ed35cf HTTP/1.1
Host: api.konimbo.co.il

RESPONSE:
{
    "id": "5552478",
    "items": [
        {
            "id": 1404903,
            "title": "ckvckv",
            "code": "",
            "quantity": 1,
            "price": null
        }
    ],
    "upgrades": [],
    "shipping": {
        "id": 4520,
        "title": null,
        "price": null
    },
    "payments": {
        "number_of_payments": 1,
        "special_first_payment": null,
        "single_payment": null
    },
    "total_price": "",
    "discounts": [
        {
            "price": "15.0",
            "title": "ziv test4",
            "quantity": 1,
            "line_item_id": null,
            "item_id": null,
            "discount_value": "12.0",
            "discount_type": "₪",
            "lock_me": false,
            "type_name": null
        }
    ]
}
```

הקריאה הבאה תחזיר את שדות הid, items לעגלה 5552478

```
GET /v1/carts/5552478?token=794fc87483d6b5d519a98300e7bfcffa7a7aba008160c61cbf9b50fb59ed35cf&attributes=id,items
HTTP/1.1
Host: api.konimbo.co.il

RESPONSE:
{
    "id": "5552478",
    "items": [
        {
            "id": 1404903,
            "title": "ckvckv",
            "code": "",
            "quantity": 1,
            "price": null
        }
    ]
}
```



### עריכת עגלה
#### תיאור
בעזרת השירות הזה, ניתן להוסיף הנחה לעגלה ספציפית.
העגלה חייבת להיות פתוחה לעריכה ולא נעולה על מנת שיהיה ניתן לעדכן אותה.
#### EndPoint
PUT /v1/carts/5552478?token=794fc87483d6b5d519a98300e7bfcffa7a7aba008160c61cbf9b50fb59ed35cf HTTP/1.1
Host: api.konimbo.co.il

#### שדות שניתן לעדכן לעגלה

##### discounts
ניתן להוסיף הנחה חדשה לעגלה

שם השדה | סכמה | ניתן לכתיבה | הסבר
:---|:---|---:|---:
title | String | כן  | כותרת ההנחה
quantity | String | כן  | כמות ההנחה
discount_value | String | כן | ערך ההנחה
discount_type | String | כן | סוג ההנחה ("₪" או "%")
unit_price | String | כן | ערך ההנחה ליחידה 
type_name | String | כן | הערה על הסטטוס (טקסט חופשי)
line_item_id | Integer | כן | מזהה השורה בעגלה שאליה נרצה לשייך את ההנחה
total_price | Integer | לא |  סהכ מחיר העגלה
coupon_code | Integer | כן |  קוד הקופון
var_in_json | string/json | לא | שדות נוספים (מבנה נתונים)שדות נוספים (מבנה נתונים) | לא

שדות נוספים יש להקים דרך הממשק ניהול מול התמיכה של קונימבו

דוגמאות:

הוספת הנחה לעגלה

```
PUT /v1/cartsa/5552478?token=794fc87483d6b5d519a98300e7bfcffa7a7aba008160c61cbf9b50fb59ed35cf HTTP/1.1
Host: api.konimbo.co.il

BODY:
{
    "token": "794fc87483d6b5d519a98300e7bfcffa7a7aba008160c61cbf9b50fb59ed35cf",
    "cart": {
        "discounts": [{
            "title": "הנחת מקורבים",
            "quantity": 1,
            "discount_value": 12,
            "unit_price": 15,
            "discount_type": "₪",
            "var_in_json": "{\"TROLLEY_KEY\":\"14408\",\"DEAL_NO\":\"1843\",\"line_item_id\":\"18310813\"}" //this is stringify JSON
        }]
    }
}
```

#### מחיקת הנחות
ניתן למחוק הנחות על מנת לאפשר עדכון של הנחות, ניתן למחוק את כל ההנחות או רק הנחות מסויימות לפי השדה:
type_name

על מנת למחוק את כולם יש להוסיף:
<br>
destroy_discounts": {"type_name": "all"}
<br>
destroy_discounts": {"type_name": "como"}
````
{
    "token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "cart": {
    	"destroy_discounts": {"type_name": "all"},
        "discounts": [{
            "title": "הנחת מקורבים",
            "quantity": 1,
            "discount_value": 10,
            "unit_price": 15,
            "discount_type": "%",
            "line_item_id": 17134928,
            "coupon_code": "12345",
            "type_name": "itam"
        }]
    }
}
````

על מנת למחוק הנחה ספציפית יש להוסיף:
<br>
destroy_discounts": {"line_item_discount_ids": "*line_item_discount_id*"}
<br>
כאשר ה-line_item_discount_id הוא ערך דינמי של מזהה ההנחה בעגלה
<br>
*אם רוצים למחוק כמה הנחות, אפשר לשרשר אותן עם פסיקים, כמו בדוגמא הבאה:
````
{
    "token": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "cart": {
    	"destroy_discounts": {
            "line_item_discount_ids": "123456,234567"
        },
        "discounts": []
    }
}
````

#### Responses
* 200 - The updated order
* 400 - Bad Request
* 401 - Unauthorized
* 404 - No results found
* 422 - Unprocessable Entity
* 500 - Internal error
