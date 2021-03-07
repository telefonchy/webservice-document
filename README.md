## # **Response Structure**
```json

{
    "status": "{STRING: ERROR_TYPE}",
    "code": "{INTEGER: ERROR_CODE}",
    "message": "{STRING: ERROR MESSAGE}",
    "data": []
}
```

|code|status|description|
| :-------: | :-------: | -------: |
200 | `Ok`|درخواست موفقیت آمیز
400 | `Validations-Error`|خطای اعتبار سنجی پارامترهای ارسالی
401 | `Unauthorized`|خطای احراز هویت با توکن وب سرویس
402 | `Failed`|خطای ناخواسته در سیستم
404 | `Not-Found`|مسیر یافت نشد


## # **Paginator**

درصورتی که اطلاعات ریسپانس زیاد باشد، سیستم به صورت اتوماتیک اطلاعات در چند بخش ارسال میکند.

*درصورت اینکه روتی دارای چنین امکانی باشد، به این  قسمت لینک میشوید.*

`paginator:` اطلاعات صفحه بندی اطلاعات

- `current:` صفحه کنونی

- ‍‍`before:` صفحه قبلی

- ‍‍`next:` صفحه بعدی

- `last:` آخرین صفحه

- `total_pages:` کل صفحات

- `total_items:` تعداد کل رکوردها

---

## # **دریافت اطلاعات پایه سرویس**


با استفاده از دستور زیر میتوانید موارد زیر را دریافت کنید:

- لیست سرویس ها
- خطوط فعال
- شارژ خطوط
- داخلی های خطوط

اطلاعات پایه سرویس را سعی کنید در دیتابیس محلی خود ذخیره کنید تا بتوانید از اطلاعات ذخیره شده در روت هایی که در آینده به سیستم اضافه میشود به راحتی استفاده کنید.

    `[GET]` https://telefonchy.com/webservice/v1/services

**Header**

|name|require|type|description
| ------- | :-------: | :-------: | :------- |
| `webservice-token` | `yes` | String | you can generate a webservice-token from [customer panel](https://telefonchy.com/my/auth).

### **Example**
---

**Request**
```sh
# curl shell code

curl -i -H "webservice-token: {YOUR_TOKEN}" https://telefonchy.com/webservice/v1/services
```

```php
// php curl code

$ch = curl_init();

curl_setopt($ch, CURLOPT_URL,"https://telefonchy.com/webservice/v1/services");

curl_setopt($ch, CURLOPT_POST, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$headers = [
    'webservice-token: {YOUR_TOKEN}',
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$server_output = curl_exec ($ch);
curl_close ($ch);

// dd code ;)
dump(json_decode($server_output));
die(1);

```

**Response**
```json
{
    "status": "Ok",
    "code": 200,
    "message": null,
    "data": [
        {
            "service_id": "{ANY_STRING}",
            "trunks": [
                {
                    "number": "021XXXXXXXXXXX",
                    "credit": {
                        "value": 21064,
                        "expired_at": "1399-06-15 15:41:27"
                    }
                }
            ],
            "extens": [
                {
                    "number": "1510",
                    "name": "کارشناس1"
                }
            ]
        }
    ]
}
```

### **Response Fields Description**

---

`service_id:` شناسه سرویس. *(بسیار مهم)*

`trunks:` اطلاعات خطوط

- `id:` شناسه خط فعال
- `number:` شماره خط

`credit:` آبجکت شارژ خط

- `value:` مقدار شارژ باقی مانده از خط
- `expired_at:` تاریخ اتمام شارژ

`extens:` داخلی های سرویس

- `number:` شماره داخلی خط
- `name:` نام اپراتور متصل به داخلی

---

## # **دریافت لیست تماس ها**

با استفاده از دستور زیر میتوانید موارد زیر را دریافت کنید:

- لیست تماس ها
- مشخصات تماس
- نمایش شماره مبدا و مقصد به همراه اطلاعات مخاطب

لیست کامل تمام تماس های انجام شده به همراه وضعیت تماس

    `[GET | POST]` https://telefonchy.com/webservice/v1/calls

#### **Prerequisites**

to use `service_id`, you must get services from [دریافت اطلاعات پایه سرویس](##دریافت-اطلاعات-پایه-سرویس) and store data in your database for another usage.

**Header**

|name|require|type|description
| ------- | :-------: | :-------: | :------- |
| `webservice_token` | `yes` | String | you can generate a webservice_token from [customer panel](https://telefonchy.com/my/auth).

**Body**

|name|require|type|description
| ------- | :-------: | :-------: | :------- |
| `service_id` | `yes` | String | you can get service_id from [service route](####Prerequisites).
| `page` | `no` | Int | integer number for pagination default = 1.
| `sort` | `no` | String | this variable must be `DESC` or `ASC` defaulu = DESC.

### **Example**
---

**Request**
```sh
# curl shell code

# Get method calling
curl -i -H "webservice_token: {YOUR_TOKEN}"  'https://telefonchy.com/webservice/v1/calls?service_id={YOUR_SERVICE_ID}&page=1&sort=DESC'

#POST method calling
curl -H "webservice_token: {YOUR_TOKEN}" -d "service_id={YOUR_SERVICE_ID}&page=1&sort=DESC" https://telefonchy.com/webservice/v1/calls
```

```php
// php curl code

#GET method calling
$ch = curl_init();

curl_setopt($ch, CURLOPT_URL,"https://telefonchy.com/webservice/v1/calls?service_id={YOUR_SERVICE_ID}&page=1");

curl_setopt($ch, CURLOPT_POST, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$headers = [
    'webservice_token: {YOUR_TOKEN}',
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$server_output = curl_exec ($ch);
curl_close ($ch);

// dd code ;)
dump(json_decode($server_output));
die(1);
```

```php
#POST method calling
$ch = curl_init();

curl_setopt($ch, CURLOPT_URL,"https://telefonchy.com/webservice/v1/services?service_id={YOUR_SERVICE_ID}&page=1");

$post_fields = [
    'service_id' => '{YOUR_SERVICE_ID}',
    'page' => 1,
    'sort' => 'DESC'
];

curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_POSTFIELDS, $post_fields);

curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$headers = [
    'webservice_token: {YOUR_TOKEN}',
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$server_output = curl_exec ($ch);
curl_close ($ch);

// dd code ;)
dump(json_decode($server_output));
die(1);
```

**Response**
```json
{
    "status": "Ok",
    "code": 200,
    "message": null,
    "data": {
        "calls": [
            {
                "service_id": "{YOUR_SERVICE_ID}",
                "call_id": "71966",
                "type": "incoming",
                "trunk": "21xxxxxxxx",
                "status": "BUSY",
                "note": null,
                "call_source": {
                    "number": "91xxxxxxxx",
                    "contact": {
                        "location": "مشهد",
                        "contact_id": "24626",
                        "name": null,
                        "company": null,
                        "email": null,
                        "address": null,
                        "note": null
                    }
                },
                "call_dest": {
                    "number": "91xxxxxxxx",
                    "contact": null
                },
                "time_wait": "6",
                "time_talk": "0",
                "price_wait": "17",
                "price_talk": "0",
                "price_record": "0",
                "price_saved": "0",
                "price": 17,
                "created_at": "1399-04-21 12:02:18",
                "ended_at": "1399-04-21 12:02:24"
            }
        ]
    },
    "paginator": {
        "current": 1,
        "before": 1,
        "next": 1,
        "last": 1,
        "total_pages": 1,
        "total_items": 1
    }
}
```

### **Response Fields Description**
---

`calls:` لیست تماس ها

- `service_id:` شناسه سرویس

- `call_id:` شناسه تماس

- `type:` **outgoing | incoming | local**
    
    `local:` تماس های داخلی
    
    `incoming:` تماس های ورودی

    `outgoing:` تماس خروجی

- ‍‍‍`trunk:` شماره خط سرویس مربوطه

- `status:` **ANSWERED | BUSY | NO ANSWER | CONGESTION**

    `ANSWERED:` با موفقیت پاسخ داده شده

    `BUSY:` خط مشغول بوده و تماس قطع شده
    
    `NO ANSWER:` پاسخ داده نشده

    `CONGESTION:` به دلیل شلوغی خط تماس قطع شده است

- `note:` توضیحات تماس که از طریق پنل مشتری قابل ثبت است.

- `call_source:` اطلاعات تماس گیرنده

    - `number:` شماره تماس گیرنده

    - `contact(nullable):` اطلاعات شماره تماس گیرنده که برابر با اطلاعات دفترچه تلفن است
        - `location(nullable)`: نام شهرمخاطب

        - `contact_id:` شناسه مخاطب

        - `name(nullable):` نام مخاطب

        - `company(nullable):` شرکت مخاطب

        - `email(nullable):` ایمیل مخاطب

        - `address(nullable):` آدرس مخاطب

        - `note(nullable):` توضیحات مخاطب

        - `numbers:` لیست شماره های مربوط به مخاطب

            - `number_id‍:` شناسه شماره

            - `number:` شماره مخاطب

            - `type:` نوع شماره مخاطب

- `call_dest:` اطلاعات مقصد تماس

    - `number:` شماره مقصد تماس

    - `contact(nullable):` اطلاعات شماره مقصد که برابر با اطلاعات دفترچه تلفن است
        - `location(nullable)`: نام شهرمخاطب

        - `contact_id:` شناسه مخاطب

        - `name(nullable):` نام مخاطب

        - `company(nullable):` شرکت مخاطب

        - `email(nullable):` ایمیل مخاطب

        - `address(nullable):` آدرس مخاطب

        - `note(nullable):` توضیحات مخاطب

        - `numbers:` لیست شماره های مربوط به مخاطب

            - `number_id‍:` شناسه شماره

            - `number:` شماره مخاطب

            - `type:` نوع شماره مخاطب

- `time_wait:` مدت زمان انتظار تماس گیرنده

- `time_talk:` مدت زمان صحبت

- `price_wait:` هزینه مدت انتظار

- `price_talk:` هزینه مکالمه

- `price_record:` هزینه ظبط تماس

- `price_saved:` هزینه صرفه جویی بسته مکالمه

- `price:` هزینه کل

- `created_at:` تاریخ و ساعت ثبت تماس

- `ended_at:` تاریخ و ساعت پایان تماس 

- `price_wait:` 1
- `price_talkd:` 1,
- `price_record:` 1,
- `price_saved:` 1,
- `price:` 0

`paginator:` [اطلاعات صفحه بندی اطلاعات](##Paginator)

---

## # **دریافت تاریخ سرور**

با استفاده از این دستور تاریخ سرور را دریافت کنید

</div>

    `[GET]` https://telefonchy.com/webservice/v1/current/date

**Header**

|name|require|type|description
| ------- | :-------: | :-------: | :------- |
| `webservice_token` | `yes` | String | you can generate a webservice_token from [customer panel](https://telefonchy.com/my/auth).

### **Example**
---

**Request**
```sh
# curl shell code

curl -i -H "webservice_token: {YOUR_TOKEN}" https://telefonchy.com/webservice/v1/current/date
```

```php
// php curl code

$ch = curl_init();

curl_setopt($ch, CURLOPT_URL,"https://telefonchy.com/webservice/v1/current/date");

curl_setopt($ch, CURLOPT_POST, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$headers = [
    'webservice_token: {YOUR_TOKEN}',
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$server_output = curl_exec ($ch);
curl_close ($ch);

// dd code ;)
dump(json_decode($server_output));
die(1);

```

**Response**
```json
{
    "status": "Ok",
    "code": 200,
    "message": null,
    "data": {
        "unix": 1594640783,
        "persian": {
            "full": "1399-04-23 16:16:23",
            "date": {
                "string": "1399-04-23",
                "day": 23,
                "month": 4,
                "year": 1399
            },
            "time": {
                "string": "16:16:23",
                "hour": 16,
                "min": 16,
                "sec": 23
            }
        },
        "gregorian": {
            "full": "2020-07-13 16:16:23",
            "date": {
                "string": "2020-07-13",
                "day": 13,
                "month": 7,
                "year": 2020
            },
            "time": {
                "string": "16:16:23",
                "hour": 16,
                "min": 16,
                "sec": 23
            }
        }
    }
}
```

## # **دریافت لیست استان ها و شهرهای زیر مجموعه**

    `[GET]` https://telefonchy.com/webservice/v1/locations

**Header**

|name|require|type|description
| ------- | :-------: | :-------: | :------- |
| `webservice_token` | `yes` | String | you can generate a webservice_token from [customer panel](https://telefonchy.com/my/auth).

### **Example**
---

**Request**
```sh
# curl shell code

curl -i -H "webservice_token: {YOUR_TOKEN}" https://telefonchy.com/webservice/v1/locations
```

```php
// php curl code

$ch = curl_init();

curl_setopt($ch, CURLOPT_URL,"https://telefonchy.com/webservice/v1/locations");

curl_setopt($ch, CURLOPT_POST, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

$headers = [
    'webservice_token: {YOUR_TOKEN}',
];

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$server_output = curl_exec ($ch);
curl_close ($ch);

// dd code ;)
dump(json_decode($server_output));
die(1);

```

**Response**
```json
{
    "status": "Ok",
    "code": 200,
    "message": null,
    "data": {
        "locations": [
            {
                "province": "آذربایجان شرقی",
                "cities": [
                    {
                        "location_id": "1",
                        "name": "آذرشهر"
                    }
                ]
            }
        ]
    }
```

### **Response Fields Description**

---

- `locations:` لیست اطلاعات استان ها و شهرهای زیر مجموعه

    - `province:` نام استان

    - ‍`cities:` لیست شهرهای زیر مجموعه

        - `location_id:` شناسه شهر
        
        این شناسه در روت های بعدی که به سیستم اضافه میشود مورد نیاز است.

        - `name:` نام شهر

---

## # **CDR(Call-Detail Record)**

یکی از رویدادهای تعبیه شده در سیستم است به این صورت که در صورت اتمام هر تماس اطلاعات به آدرسی که مشتری در پنل برای این رویداد تعریف کرده باشد، ارسال خواهد کرد.

اطلاعاتی که ارسال میکند به صورت زیر است: 


```json
{
    "service_id": "{YOUR_SERVICE_ID}",
    "call_id": "71966",
    "type": "incoming",
    "trunk": "21xxxxxxxx",
    "status": "BUSY",
    "note": null,
    "call_source": {
        "number": "91xxxxxxxx",
        "contact": {
            "location": "مشهد",
            "contact_id": "24626",
            "name": null,
            "company": null,
            "email": null,
            "address": null,
            "note": null
        }
    },
    "call_dest": {
        "number": "91xxxxxxxx",
        "contact": null
    },
    "time_wait": "6",
    "time_talk": "0",
    "price_wait": 1,
    "price_talk": 2,
    "price_record": 2,
    "price_saved": 3,
    "price": 3,
    "created_at": "1399-04-21 12:02:18",
    "ended_at": "1399-04-21 12:02:24"
}


```
- `service_id:` شناسه سرویس

- `call_id:` شناسه تماس

- `type:` **outgoing | incoming | local**
    
    `local:` تماس های داخلی
    
    `incoming:` تماس های ورودی

    `outgoing:` تماس خروجی

- ‍‍‍`trunk:` شماره خط سرویس مربوطه

- `status:` **ANSWERED | BUSY | NO ANSWER | CONGESTION**

    `ANSWERED:` با موفقیت پاسخ داده شده

    `BUSY:` خط مشغول بوده و تماس قطع شده
    
    `NO ANSWER:` پاسخ داده نشده

    `CONGESTION:` به دلیل شلوغی خط تماس قطع شده است

- `note:` توضیحات تماس که از طریق پنل مشتری قابل ثبت است.

- `call_source:` اطلاعات تماس گیرنده

    - `number:` شماره تماس گیرنده

    - `contact(nullable):` اطلاعات شماره تماس گیرنده که برابر با اطلاعات دفترچه تلفن است
        - `location(nullable)`: نام شهرمخاطب

        - `contact_id:` شناسه مخاطب

        - `name(nullable):` نام مخاطب

        - `company(nullable):` شرکت مخاطب

        - `email(nullable):` ایمیل مخاطب

        - `address(nullable):` آدرس مخاطب

        - `note(nullable):` توضیحات مخاطب

        - `numbers:` لیست شماره های مربوط به مخاطب

            - `number_id‍:` شناسه شماره

            - `number:` شماره مخاطب

            - `type:` نوع شماره مخاطب

- `call_dest:` اطلاعات مقصد تماس

    - `number:` شماره مقصد تماس

    - `contact(nullable):` اطلاعات شماره مقصد که برابر با اطلاعات دفترچه تلفن است
        - `location(nullable)`: نام شهرمخاطب

        - `contact_id:` شناسه مخاطب

        - `name(nullable):` نام مخاطب

        - `company(nullable):` شرکت مخاطب

        - `email(nullable):` ایمیل مخاطب

        - `address(nullable):` آدرس مخاطب

        - `note(nullable):` توضیحات مخاطب

        - `numbers:` لیست شماره های مربوط به مخاطب

            - `number_id‍:` شناسه شماره

            - `number:` شماره مخاطب

            - `type:` نوع شماره مخاطب

- `time_wait:` مدت زمان انتظار تماس گیرنده

- `time_talk:` مدت زمان صحبت

- `price_wait:` هزینه مدت انتظار

- `price_talk:` هزینه مکالمه

- `price_record:` هزینه ظبط تماس

- `price_saved:` هزینه صرفه جویی بسته مکالمه

- `price:` هزینه کل

- `created_at:` تاریخ و ساعت ثبت تماس

- `ended_at:` تاریخ و ساعت پایان تماس 
---
