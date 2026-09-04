# حلقة الأترجة الافتراضية

برنامج متابعة حلقة قرآنية: صفحة للطالبة فيها مقدار اليوم والتقييم والتفسير
والتكاليف، ولوحة للمعلمة فيها الحضور والتقرير الأسبوعي وعجلة الحظ.

## الروابط

| | |
|---|---|
| الموقع (Firebase) | https://atheer-aee17.web.app |
| الموقع (GitHub Pages) | https://ahmadabd733-bot.github.io/atheer/ — يتوقف إن صار المستودع خاصًّا |
| لوحة Firebase | https://console.firebase.google.com/project/atheer-aee17 |

الموقعان يقدّمان الملفات نفسها ويتصلان بقاعدة البيانات نفسها.

## بنية المشروع

```
utrujjah-halaqa.html   البرنامج كاملًا: الواجهة والمنطق وتفسير السعدي المضمّن
index.html             تحويل إلى البرنامج
write/                 49 صورة لأوجه المصحف (الأنعام والأعراف، الصفحات 128-176)
مصحف صوتي/             تلاوة الأوجه — محلية فقط، لا تُرفع (تُبثّ من archive.org)
تفسير سورة *.txt       مصدر التفسير الذي بُني منه المضمَّن — لا يُرفع
firebase.json          إعداد الاستضافة وقواعد القاعدة
database.rules.json    قواعد الوصول: لا قراءة ولا كتابة بلا تسجيل دخول
```

## من المحلي إلى السحابي

```bash
# 1) الشفرة والصور إلى GitHub (ينشر تلقائيًا على GitHub Pages)
git add -A && git commit -m "..." && git push

# 2) الموقع إلى استضافة Firebase
firebase deploy --only hosting

# 3) قواعد قاعدة البيانات بعد تعديل database.rules.json
firebase deploy --only database

# أو الاثنان معًا
firebase deploy
```

## قاعدة البيانات

Firebase Realtime Database على `atheer-aee17-default-rtdb.firebaseio.com`.
البيانات تحت `utrujjah/<رمز الحلقة>/` وتنقسم إلى `halaqa/settings`
و`halaqa/roster` و`days/<التاريخ>` و`benefits/<المعرّف>`.

القواعد تشترط تسجيل الدخول، فلا يقرأ أحد ولا يكتب بلا حساب الحلقة.
الطالبة تُدخل كلمة المرور مرة واحدة على جهازها.

## النشر التلقائي

`.github/workflows/deploy.yml` ينشر إلى Firebase عند كل دفع إلى main،
لكنه **يتخطّى النشر بهدوء** ما لم يُضبط سرّ `FIREBASE_SERVICE_ACCOUNT`
في إعدادات المستودع (Settings ← Secrets and variables ← Actions).
قيمته ملف JSON لحساب خدمة له دورا `firebasehosting.admin`
و`firebasedatabase.admin`.

## تنبيهات

- مجلد الصوت (نحو 193 ميغابايت) مستثنى من Git ومن الاستضافة.
- عند تعديل `firebase.json` تأكّدي أن `.git/**` باقٍ في قائمة `ignore`.
