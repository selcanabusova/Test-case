# Simplifai Backend API Test Cases Report

---

# 1. GET /applications

### Description

Bearer token vasitəsilə authorize-dan keçərək qeydiyyatdan keçmiş user-ə aid tətbiqlərin siyahısını əldə etmək.

### Request

```http
GET https://smplifai-backend.vercel.app/api/applications
```

### Response

Status Code: `200 OK`

```json
{
  "id": "Valid ID",
  "user_id": "Valid User ID",
  "type": "Fashion",
  "email": null,
  "phone": null,
  "passport_data": null,
  "file_url": null,
  "status": "DRAFT",
  "created_at": "Yaranma tarixi"
}
```

### Actual Result

Yaradılan və qeydiyyatdan keçmiş bütün tətbiqlərin siyahısı ekranda göründü.

### Database Check

```sql
SELECT * FROM applications 
WHERE email = 'abusovaselcan@gmail.com';
```

Aşağıdakı id-lərə məxsus tətbiqlər mövcuddur:

- 214cfb5c-da24-4ff9-88fb-9503e6e70f9b
- 70767832-788d-4b04-bd79-6ed3666f653d
- b940f7d6-ecb1-4b0d-9c8e-8bb300e65253
- fa2c4697-0716-4c89-8331-ae89564c64c8
- d32bee81-be68-4e42-9bf9-04817f5b2ad3

---

# 2. GET /application

### Description

Yanlış URL vasitəsilə tətbiqlər siyahısını əldə etməyə çalışmaq.

### Request

```http
GET https://smplifai-backend.vercel.app/api/application
```

### Response

Status Code: `404 Not Found`

```json
{
  "success": false,
  "message": "Route not found",
  "path": "/api/application"
}
```

### Actual Result

404 Not Found xətası ekranda göründü. Çünki URL yanlış yazılmışdı.

### Database Check

```sql
SELECT * FROM applications 
WHERE email = 'abusovaselcan@gmail.com';
```

Mövcud tətbiqlər dəyişmədi.

---

# 3. POST /applications

### Description

Mövcud user üçün yeni tətbiq yaratmaq.

### Request

```http
POST https://smplifai-backend.vercel.app/api/applications
```

### Body

```json
{
  "type": "Fashion"
}
```

### Response

Status Code: `201 Created`

```json
{
  "id": "Valid ID",
  "user_id": "Valid User ID",
  "type": "Fashion",
  "email": null,
  "phone": null,
  "passport_data": null,
  "file_url": null,
  "status": "DRAFT",
  "created_at": "Yaranma tarixi",
  "updated_at": "Dəyişdirilmə tarixi"
}
```

### Actual Result

Yeni tətbiq yaradıldı.

### Database Check

```sql
SELECT * FROM applications 
WHERE type = 'Fashion';
```

`28164c81-5e31-44c1-8037-73a244192e84`
id-yə məxsus tətbiq yaradıldı.

---

# 4. POST /applications

### Description

Yanlış JSON formatı ilə yeni tətbiq yaratmağa çalışmaq.

### Request

```http
POST https://smplifai-backend.vercel.app/api/applications
```

### Body

```json
{
  "type":
}
```

### Response

Status Code: `400 Bad Request`

```json
{
  "success": false,
  "message": "Unexpected token '}', \"{\\n  \\\"type\\\": \\n}\" is not valid JSON"
}
```

### Actual Result

İstifadəçi xətası ekranda göründü.

### Database Check

```sql
SELECT * FROM applications 
WHERE type = 'null';
```

Success. No rows returned.

Heç bir tətbiq yaradılmadı.

---

# 5. POST /applications/submit

### Description

Yeni tətbiq yaradaraq bütün məlumatları daxil etmək.

### Request

```http
POST https://smplifai-backend.vercel.app/api/applications/submit
```

### Response

Status Code: `200 OK`

### Actual Result

Yeni tətbiq uğurla yaradıldı.

```json
{
  "id": "14cbb377-68f4-4a15-a240-8093e383d4bb",
  "user_id": "2ca021f4-c85d-418f-89e4-4aa38605f9f2",
  "type": "My Business",
  "email": "selcan@example.com",
  "phone": "+994501234567",
  "passport_data": {
    "surname": "Abuşova",
    "firstName": "Selcan"
  },
  "status": "pending"
}
```

### Database Check

```sql
SELECT * FROM applications 
WHERE type = 'My Business';
```

`a705f55c-7c35-437c-9cd4-1bf99843bd1a`
id-li tətbiq yaradıldı.

---

# 6. POST /applications/submit

### Description

Telefon sahəsinə string tipli məlumat daxil edərək tətbiq yaratmaq.

### Request

```http
POST https://smplifai-backend.vercel.app/api/applications/submit
```

### Response

Status Code: `201 Created`

### Actual Result

Yeni tətbiq yaradıldı.

```json
{
  "phone": "HELLO"
}
```

### Database Check

```sql
SELECT * FROM applications 
WHERE phone = 'HELLO';
```

`98e598aa-dac7-4e05-905e-07599283016b`
id-yə məxsus tətbiq yaradıldı.

---

# 7. GET /applications/{id}

### Description

Mövcud tətbiqi ID vasitəsilə əldə etmək.

### Request

```http
GET https://smplifai-backend.vercel.app/api/applications/98e598aa-dac7-4e05-905e-07599283016b
```

### Response

Status Code: `200 OK`

### Actual Result

Mövcud ID-yə uyğun tətbiq ekranda göründü.

### Database Check

```sql
SELECT * FROM applications 
WHERE id = '98e598aa-dac7-4e05-905e-07599283016b';
```

Məlumat bazasında həmin tətbiq mövcuddur.

---

# 8. GET /applications/{id}

### Description

Mövcud olmayan ID ilə tətbiq axtarmaq.

### Request

```http
GET https://smplifai-backend.vercel.app/api/applications/10
```

### Response

Status Code: `404 Not Found`

```json
{
  "success": false,
  "message": "Invalid token"
}
```

### Actual Result

Tətbiq tapılmadı.

### Database Check

```sql
SELECT * FROM applications 
WHERE id = '10';
```

Heç bir nəticə qaytarılmadı.

---

# 9. PATCH /applications/{id}/status

### Description

Tətbiqin məlumatlarını yanlış body ilə dəyişdirməyə çalışmaq.

### Request

```http
PATCH https://smplifai-backend.vercel.app/api/applications/98e598aa-dac7-4e05-905e-07599283016b/status
```

### Body

```json
{
  "phone": "0513788213"
}
```

### Response

Status Code: `400 Bad Request`

```json
{
  "success": false,
  "message": "status is required"
}
```

### Actual Result

Tətbiqin məlumatları dəyişmədi.

### Database Check

```sql
SELECT * FROM applications 
WHERE phone = '0513788213';
```

Success. No rows returned.

---

# 10. PATCH /applications/{id}/status

### Description

Yanlış status ilə tətbiqin statusunu dəyişməyə çalışmaq.

### Request

```http
PATCH https://smplifai-backend.vercel.app/api/applications/98e598aa-dac7-4e05-905e-07599283016b/status
```

### Body

```json
{
  "status": "Name"
}
```

### Response

Status Code: `404 Not Found`

```json
{
  "success": false,
  "message": "Application not found"
}
```

### Actual Result

Status dəyişdirilmədi.

### Database Check

```sql
SELECT * FROM applications 
WHERE status = 'Name';
```

Success. No rows returned.

---

# 11. PATCH /applications/{id}/passport

### Description

Tətbiqin passport məlumatlarını qismən dəyişdirmək.

### Request

```http
PATCH https://smplifai-backend.vercel.app/api/applications/14cbb377-68f4-4a15-a240-8093e383d4bb/passport
```

### Response

Status Code: `200 OK`

### Actual Result

Passport məlumatları uğurla dəyişdirildi.

```json
{
  "firstName": "John",
  "surname": "Doe",
  "passportNo": "A1234567"
}
```

### Database Check

```sql
SELECT * FROM applications 
WHERE id = '14cbb377-68f4-4a15-a240-8093e383d4bb';
```

Passport məlumatları yeniləndi.

---

# 12. DELETE /applications/{id}

### Description

Mövcud tətbiqi ID vasitəsilə silmək.

### Request

```http
DELETE https://smplifai-backend.vercel.app/api/applications/14cbb377-68f4-4a15-a240-8093e383d4bb
```

### Response

Status Code: `204 Deleted Successfully`

### Actual Result

Məlumat uğurla silindi.

### Database Check

```sql
SELECT * FROM applications 
WHERE id = '14cbb377-68f4-4a15-a240-8093e383d4bb';
```

Success. No rows returned.

---

# 13. DELETE /applications/{id}

### Description

ID-ni yanlış formatda daxil edərək tətbiqi silməyə çalışmaq.

### Request

```http
DELETE https://smplifai-backend.vercel.app/api/applications/ikiyuzondordcfb5c-da24-4ff9-88fb-9503e6e70f9b
```

### Response

Status Code: `500 Internal Server Error`

```json
{
  "success": false,
  "message": "invalid input syntax for type uuid"
}
```

### Actual Result

Tətbiq silinmədi. Çünki ID yanlış formatda daxil edilmişdi.

### Database Check

```sql
SELECT * FROM applications 
WHERE id = '214cfb5c-da24-4ff9-88fb-9503e6e70f9b';
```

Tətbiq məlumatları silinməmişdi.
