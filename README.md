# 🧠 Database Schema Documentation

## 🗂️ Tables Overview

هذه الوثيقة تشرح تصميم قاعدة البيانات. تحتوي على وصف لكل جدول، وأعمدته، وعلاقاته مع الجداول الأخرى.

---

## 📘 Table: `Users`

| Column         | Type       | Description                     |
|----------------|------------|---------------------------------|
| ID             | uniqueid   | المفتاح الأساسي للمستخدم       |
| firstName      | nvarchar   | الاسم الأول                     |
| lastName       | nvarchar   | الكنية                          |
| motherName     | nvarchar   | اسم الأم                       |
| city           | nvarchar   | اسم المدينة                     |
| email          | nvarchar   | البريد الإلكتروني               |
| password       | nvarchar   | كلمة المرور                     |
| phoneNumber    | nvarchar   | رقم الهاتف                      |
| birthDate      | date       | تاريخ الميلاد                   |
| gender         | bit        | الجنس                           |
| userName       | nvarchar   | اسم المستخدم                    |

📌 **العلاقات**:
- مرتبط بـ `Suggestions`, `Complaints`, `UsersInstitutions`

---

## 📘 Table: `Employees`

| Column         | Type       | Description                     |
|----------------|------------|---------------------------------|
| ID             | uniqueid   | معرف الموظف                    |
| firstName      | nvarchar   | الاسم الأول                     |
| lastName       | nvarchar   | الكنية                          |
| fatherName     | nvarchar   | اسم الأب                        |
| email          | nvarchar   | البريد الإلكتروني               |
| password       | nvarchar   | كلمة المرور                     |
| role           | enum       | الدور (صلاحيات الموظف)         |
| institutionID  | uniqueid   | المؤسسة التابع لها الموظف      |
| gender         | bit        | الجنس                           |

📌 **العلاقات**:
- مرتبط بـ `Complaints`

---

## 📘 Table: `Suggestions`

| Column         | Type       | Description                     |
|----------------|------------|---------------------------------|
| ID             | uniqueid   | معرف الاقتراح                  |
| title          | text       | عنوان الاقتراح                 |
| description    | text       | وصف الاقتراح                   |
| suggestionDate | date       | تاريخ الاقتراح                 |
| priority       | nvarchar   | الأولوية                       |
| status         | nvarchar   | الحالة                         |
| userID         | uniqueid   | المستخدم الذي قدم الاقتراح     |
| likeCount      | bigint     | عدد الإعجابات                  |

📌 **العلاقات**:
- مرفق به ملفات من `SuggestionsAttachments`

---

## 📘 Table: `SuggestionsAttachments`

| Column      | Type     | Description                      |
|-------------|----------|----------------------------------|
| ID          | uniqueid | معرف المرفق                      |
| suggestionID| uniqueid | معرف الاقتراح المرتبط به         |
| path        | nvarchar | مسار الملف المرفق                |

---

## 📘 Table: `Complaints`

| Column        | Type     | Description                      |
|---------------|----------|----------------------------------|
| ID            | uniqueid | معرف الشكوى                     |
| title         | nvarchar | عنوان الشكوى                    |
| description   | text     | وصف الشكوى                      |
| complaintDate | date     | تاريخ تقديم الشكوى              |
| status        | nvarchar | الحالة                          |
| priority      | nvarchar | الأولوية                        |
| userID        | uniqueid | مقدم الشكوى                     |
| institutionID | uniqueid | المؤسسة المعنية                 |
| empID         | uniqueid | الموظف المعالج للشكوى          |

📌 **العلاقات**:
- مرفق بها ملفات من `ComplaintsAttachments`

---

## 📘 Table: `ComplaintsAttachments`

| Column      | Type     | Description                      |
|-------------|----------|----------------------------------|
| ID          | uniqueid | معرف المرفق                      |
| complaintID | uniqueid | معرف الشكوى المرتبط بها          |
| path        | nvarchar | مسار الملف                       |

---

## 📘 Table: `UsersInstitutions`

| Column        | Type     | Description                      |
|---------------|----------|----------------------------------|
| ID            | uniqueid | معرف العلاقة                    |
| institutionID | uniqueid | المؤسسة                         |
| userID        | uniqueid | المستخدم                        |
| isActive      | bit      | هل العلاقة مفعّلة               |

---

## 📘 Table: `Institutions`

| Column      | Type     | Description                      |
|-------------|----------|----------------------------------|
| ID          | uniqueid | معرف المؤسسة                    |
| name        | nvarchar | اسم المؤسسة                     |
| logo        | text     | شعار المؤسسة                    |
| parentID    | uniqueid | المؤسسة الأم (إن وجدت)          |
| description | text     | وصف المؤسسة                     |
| cityID      | uniqueid | المدينة التابع لها المؤسسة      |

📌 **العلاقات**:
- مرتبطة بـ `Decisions`, `Polls`, `Complaints`

---

## 📘 Table: `Cities`

| Column     | Type     | Description                      |
|------------|----------|----------------------------------|
| ID         | uniqueid | معرف المدينة                    |
| name       | nvarchar | اسم المدينة                     |
| description| text     | وصف المدينة                     |
| parentID   | uniqueid | المدينة الأم (للدعم الهرمي)     |

---

## 📘 Table: `Polls`

| Column        | Type     | Description                      |
|---------------|----------|----------------------------------|
| ID            | uniqueid | معرف الاستطلاع                 |
| title         | nvarchar | عنوان الاستطلاع                |
| description   | text     | وصف الاستطلاع                  |
| isMultiChoice | bit      | هل يمكن اختيار أكثر من خيار     |
| institutionID | uniqueid | المؤسسة المعنية بالاستطلاع     |

📌 **العلاقات**:
- يحتوي على خيارات في جدول `Answers`

---

## 📘 Table: `Answers`

| Column  | Type     | Description                          |
|---------|----------|--------------------------------------|
| ID      | uniqueid | معرف الجواب                         |
| pollID  | uniqueid | الاستطلاع المرتبط به                 |
| count   | bigint   | عدد الأصوات                          |
| name    | nvarchar | نص الجواب                           |

---

## 📘 Table: `Decisions`

| Column        | Type     | Description                      |
|---------------|----------|----------------------------------|
| ID            | uniqueid | معرف القرار                     |
| title         | nvarchar | عنوان القرار                    |
| description   | text     | وصف القرار                      |
| institutionID | uniqueid | المؤسسة مصدر القرار             |

📌 **العلاقات**:
- مرفق به ملفات من `DecisionsAttachments`

---

## 📘 Table: `DecisionsAttachments`

| Column    | Type     | Description                      |
|-----------|----------|----------------------------------|
| ID        | uniqueid | معرف المرفق                      |
| decisionID| uniqueid | معرف القرار المرتبط              |
| path      | nvarchar | مسار الملف                       |

---


