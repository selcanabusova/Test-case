# Simplifai Test Cases Report

## 1. Sign in with Google

### Description

Google hesabı vasitəsilə Simplifai-a daxil olmaq.

### Steps

1. “Sign in with Google” buttonuna klik etdim.
2. Düzgün Google hesabı seçərək daxil oldum.

### Expected Result

Hesaba uğurla daxil olunmalıdır.

### Actual Result

Hesaba uğurla daxil oldum.

### Database Check

```sql
SELECT * FROM profiles WHERE first_name = 'Selcan';
```

`2ca021f4-c85d-418f-89e4-4aa38605f9f2` id-yə məxsus istifadəçi mövcuddur.

---

# 2. Yanlış password vasitəsilə account-a daxil olmaq

### Description

Düzgün e-mail və yanlış şifrə daxil edərək hesaba daxil olmağa çalışmaq.

### Steps

1. Əsas səhifədə düzgün e-mail daxil etdim: `abusovaselcan@gmail.com`
2. Yanlış şifrə daxil etdim: `'`
3. “Log in” buttonuna klik etdim.

### Expected Result

Error mesajı ekranda görünməlidir.

### Actual Result

“Invalid login credentials” bildirişi ekranda göründü.

### Database Check

```sql
SELECT * FROM profiles WHERE first_name = 'Selcan';
```

`2ca021f4-c85d-418f-89e4-4aa38605f9f2` id-yə məxsus istifadəçi sistemə daxil ola bilmədi, çünki yanlış şifrə ilə daxil olmağa çalışmışdı.

---

# 3. Profildəki məlumatları dəyişdirmək

### Description

Soyadımı dəyişərək profilimi yenilədim.

### Steps

1. Simplifai frontend-də “Profile” hissəsinə keçid etdim.
2. Adımı olduğu kimi saxladım, lakin soyadımı dəyişdim.
3. Yanlış nömrə formatı yazdım. Prefiks hissəsi yox idi.
4. “Send code” buttonuna basdım.
5. E-maili olduğu kimi saxladım.
6. “Save changes” buttonuna basdım.

### Expected Result

Profil uğurla yenilənməlidir.

### Actual Result

“Profile updated successfully!” bildirişi ekranda göründü.

### Database Check

```sql
SELECT * FROM profiles WHERE last_name = 'Musayeva';
```

`2ca021f4-c85d-418f-89e4-4aa38605f9f2` id-yə məxsus istifadəçi mövcuddur.

---

# 4. Yanlış format ilə profil məlumatlarını dəyişdirmək

### Description

Soyad hissəsinə ədəd daxil edərək profil məlumatlarını yeniləməyə çalışmaq.

### Steps

1. Simplifai frontend-də “Profile” hissəsinə keçid etdim.
2. Soyad hissəsinə `12` daxil etdim.
3. Yanlış nömrə formatı yazdım.
4. “Send code” buttonuna basdım.
5. E-maili olduğu kimi saxladım.
6. “Save changes” buttonuna basdım.

### Expected Result

Error mesajı görünməlidir. Çünki string məlumat daxil edilməli olan hissəyə number daxil edilmişdi.

### Actual Result

“Profile updated successfully!” bildirişi ekranda göründü.

### Database Check

```sql
SELECT * FROM profiles WHERE last_name = '12';
```

`2ca021f4-c85d-418f-89e4-4aa38605f9f2` id-yə məxsus istifadəçi mövcuddur.

---

# 5. Dashboard-da yeni tətbiq yaratmaq

### Description

Düzgün məlumatlarla yeni application yaratmaq.

### Steps

1. Dashboard hissəsinə keçid etdim.
2. “Create your first application” buttonuna klik etdim.
3. E-mail və telefon nömrəmi daxil etdim.
4. “Choose” buttonuna klik etdim.
5. “Browse file” hissəsinə şəkil yerləşdirdim.
6. “Submit passport to review” buttonuna klik etdim.
7. Bütün məlumatları düzgün şəkildə daxil etdim.
8. “Approve” buttonuna klik etdim.

### Expected Result

Application yaradılmalıdır.

### Actual Result

“My Passport” application yaradıldı.

### Database Check

```sql
SELECT * FROM applications WHERE phone = '+994515688215';
```

`214cfb5c-da24-4ff9-88fb-9503e6e70f9b` id-yə məxsus tətbiq mövcuddur.

---

# 6. Yanlış məlumat formatı ilə tətbiq yaratmaq

### Description

Yanlış məlumat formatı ilə tətbiq yaratmağa çalışmaq.

### Steps

1. Dashboard hissəsinə keçid etdim.
2. “Create your first application” buttonuna klik etdim.
3. E-mail və telefon nömrəmi daxil etdim.
4. “Choose” buttonuna klik etdim.
5. Şəkil yerləşdirdim.
6. “Submit passport to review” buttonuna klik etdim.
7. First name və last name hissələrinə ədəd daxil etdim və tarix hissələrini boş saxladım.
8. “Approve” buttonuna klik etdim.

### Expected Result

Error mesajı görünməlidir.

### Actual Result

“My Passport” application yaradıldı.

### Database Check

```sql
SELECT * FROM applications WHERE created_at = '2026-05-09 03:41:25.308414+00';
```

`70767832-788d-4b04-bd79-6ed3666f653d` id-yə məxsus tətbiq yaradıldı.

---

# 7. “History” bölməsində tətbiq məlumatlarını tamamlamaq

### Description

Tətbiqdə çatışmayan məlumatların doldurulması.

### Steps

1. “History” hissəsinə keçid etdim.
2. “Continue” buttonuna klik etdim.
3. Tətbiqin detalları ekranda göründü.
4. Boş qalan tarixləri yazdım.
5. “Save Data” buttonuna klik etdim.

### Expected Result

Məlumatlar yadda saxlanılmalıdır.

### Actual Result

“Successfully Saved!” bildirişi ekranda göründü.

### Database Check

```sql
SELECT * FROM applications WHERE email = 'abusovaselcan@gmail.com';
```

`d32bee81-be68-4e42-9bf9-04817f5b2ad3` id-yə məxsus tətbiq mövcuddur və məlumatlar yenilənib.

---

# 8. Logout modulu vasitəsilə hesabdan çıxış etmək

### Description

Hesabdan çıxış edib yenidən daxil olmağa çalışmaq.

### Steps

1. “Logout” vasitəsilə hesabdan çıxdım.
2. “Login” hissəsində e-mail sahəsinə `@gmail.com` yazdım və şifrə daxil etdim.

### Expected Result

“Düzgün məlumatlar daxil et” bildirişi görünməlidir.

### Actual Result

“Please enter a valid email address” bildirişi ekranda göründü.

### Database Check

```sql
SELECT * FROM profiles WHERE first_name = 'Selcan';
```

`2ca021f4-c85d-418f-89e4-4aa38605f9f2` id-yə məxsus istifadəçi sistemə daxil ola bilmədi.

---

# 9. “Sign up” vasitəsilə yeni hesab yaratmaq

### Description

Düzgün məlumatlarla qeydiyyatdan keçmək.

### Steps

1. First name və last name hissələrinə string formatda məlumat daxil etdim.
2. Düzgün e-mail, telefon nömrəsi və şifrə daxil etdim.
3. “Create account” buttonuna klik etdim.

### Expected Result

Hesab uğurla yaradılmalıdır.

### Actual Result

Hesab uğurla yaradıldı.

### Database Check

```sql
SELECT * FROM profiles WHERE phone = '+994515688215';
```

`2ca021f4-c85d-418f-89e4-4aa38605f9f2` id-yə məxsus istifadəçi mövcuddur.

---

# 10. Yanlış telefon nömrəsi ilə qeydiyyatdan keçmək

### Description

Telefon nömrəsini string formatda daxil edərək qeydiyyatdan keçməyə çalışmaq.

### Steps

1. First name və last name hissələrinə string formatda məlumat daxil etdim.
2. Düzgün e-mail və şifrə daxil etdim.
3. Telefon nömrəsini `salam` olaraq daxil etdim.

### Expected Result

“Düzgün telefon nömrəsi daxil edin” bildirişi görünməlidir.

### Actual Result

“Enter a valid phone (e.g. +994XXXXXXXXX)” bildirişi ekranda göründü.

### Database Check

```sql
SELECT * FROM profiles WHERE phone = 'salam';
```

Sistemdə həmin istifadəçi mövcud deyil, çünki yanlış telefon nömrəsi ilə qeydiyyatdan keçməyə çalışılmışdı.
