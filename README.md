# Minelium AI — تطبيق أندرويد

تطبيق مساعد ذكاء اصطناعي مستقل بنظام أكواد وباقات (Plus / Pro / Max).

## 1) تجهيز الإعدادات
افتح `www/config.js` وحط:
- `WORKER_URL` — رابط الـ Cloudflare Worker
- `SUPABASE_URL` — رابط مشروع Supabase
- `SUPABASE_ANON_KEY` — المفتاح العام (anon/publishable). **لا تحط service_role أبداً.**

## 2) السماح للتطبيق بالاتصال (مهم)
بـ Cloudflare ← الـ Worker ← Settings ← Variables، عدّل `ALLOWED_ORIGIN` (أو اتركه فقط هذا إذا الـ Worker مخصص للتطبيق) وضيف:
`https://localhost`
(إذا فيه روابط ثانية افصل بينها بفاصلة)
بدون هذا الخطوة التطبيق يطلع «origin not allowed».

## 3) بناء الـ APK بدون Android Studio (عبر GitHub)
1. سوّي مستودع جديد على github.com وارفع محتويات هذا المجلد كلها (بما فيها مجلد `.github`).
2. ادخل تبويب **Actions** ← **Build APK** ← **Run workflow**.
3. بعد ما يخلص (3–6 دقايق) نزّل الملف `minelium-ai-apk` من نفس الصفحة، وبداخله `app-debug.apk`.
4. انقله للموبايل وثبّته (فعّل «التثبيت من مصادر غير معروفة»).

## 3-ب) البناء على جهازك
```
npm install
npx cap add android
npx cap sync android
cd android && ./gradlew assembleDebug
```
الملف يطلع بـ `android/app/build/outputs/apk/debug/app-debug.apk`.

## ملاحظات
- نسخة الـ debug تنثبّت مباشرة. للنشر على Google Play تحتاج نسخة موقّعة (release) وحساب مطوّر.
- لتغيير اسم التطبيق أو الحزمة عدّل `capacitor.config.json` (`appName`, `appId`) قبل `cap add android`.
- أي تعديل بواجهة `www/index.html` تحتاج تبني APK جديد.
- الأيقونة الافتراضية أيقونة Capacitor؛ لتغييرها استعمل `@capacitor/assets`.
- الأكواد تنسوّى من لوحة الأدمن (زر النسخة الكاملة) مثل ما هو الحال الحين.

## الأيقونة والشعار
ملفات الأيقونة بمجلد `assets/` (icon-only / icon-foreground / icon-background) وتتحول تلقائياً لأيقونة التطبيق أثناء البناء عبر GitHub Actions. شعار داخل التطبيق: `www/logo.png`.
