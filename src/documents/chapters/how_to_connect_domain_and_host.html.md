---
layout: article
chapter: مباحث پیشرفته 
order: 28
title: چگونه یک دامین و یک هاست را به یکدیگر متصل کنیم
---
برای اضافه کردن یک ساب دامین به یک دامین یا به عبارت بهتر برای اضافه کردن یک دامین به یک هاست، نیازمند قدم های مختلفی هستیم:

تنظیم دی ان اس ها روی آن دامین یا ساب دامین = گفتن اینکه در صورت درخواست فلان دامنه (مثلا jadi3.undo.it) اینترنت باید درخواست کننده را به هاست ما هدایت کند
تنظیم وب سرور برای پاسخ مناسب دادن به آن دامین
توجه کنید که این جریان هیچ ربطی به مفهوم ساب دامین ندارد. ساب دامین هم درست مثل دامین عمل می کند: یک اسم است که توسط یک سرویس به نام DNS که وظیفه تبدیل آدرس های انسان-فهم اینترنتی به عددهای آی پی کامپیوتر-فهم را بر عهده گرفته. من شخصا از سرویس های دی ان اس رایگان روی اینترنت از جمله http://freedns.afraid.org برای کارهای عمومی ام استفاده می کنم. پس اولین قدم برای من رفتن به آنجا، رجیستر کردن دی ان اس مورد نظر و ارسال آن به آی پی سرویس دهنده وب است.

<img src=/images/freednsafraidorg.png>

حالا هر کس بزند jadi3.undo.it از آنجا به وب سرور من هدایت خواهد شد. قدم بعدی این است که به وب سرورم بگویم هر کس به این آدرس آمد کدام دایرکتوری به عنوان محتویات آن آدرس وب به او نمایش داده شود. من قبلا در تنظیمات آپاچی (که مثل همه تنظیمات دیگر در etc/ قرار دارد) داشتم:
<pre>
  <VirtualHost *>
     ServerAdmin jadijadi@gmail.com
     DocumentRoot /home/jadi/public_html/
     ServerName www.freekeyboard.net 
     ErrorLog /home/jadi/www-error_log
     CustomLog /home/jadi/www-access_log combined
  </VirtualHost>
</pre>
و حالا فقط کافی است این را هم اضافه کنم که:


<pre >
  <VirtualHost *>
     ServerAdmin jadijadi@gmail.com
     DocumentRoot /home/jadi/public_html/
     ServerName jadi3.undo.it
     ErrorLog /home/jadi/www-error_log
     CustomLog /home/jadi/www-access_log.jadi common
  </VirtualHost>
</pre >


تا به وب سرور گفته باشم که اگر کسی اومد و با آدرس jadi3.undo.it کار داشت بهش محتویات فلان دایرکتوری رو نشون بده. می بنین که من اینجا همون دایرکتوری قبلی رو معرفی کردم و در نتیجه هر کس آدرس http://jadi3.undo.it رو بزنه به همون جایی می رسه که بقیه دامین های من می رسن. مشخصه که هر تغییری در فایل تنظیمات نیازمند ری استارت سرویس است. سرویس آپاچی رو ری استارت می کنم:
<pre>
  # /etc/init.d/apache2 restart
</pre >
نکات مهم در مورد این مقاله

- دامین undo.it متعلق به من نیست. یک دامین آزاد است برای هر کسی که دوست دارد روی آن ساب دامین تعریف کند
- تلفظ صحیح دامین ظاهرا دومین است
- این مقاله نمی تواند راهنمای اینکار برای کسی باشد که درک نمی کند چکار می کند. دو قدم تنظیم دی ان اس و تنظیم وب سرور در هر محیط ممکن است به شکل متفاوتی انجام شود
- این روش برای مبارزه با سانسور توصیه نمی شود (: اول سانسورچی اینقدر کم خرد است که تمام اینترنت را سانسور کرده پس همه از فیلتر رد می شوند و دوم اینکه من قرار نیست با چهار نفر مشتری سایت هر روز در حال فرار باشم. طبق تجربه من سانسور حداکثر ده درصد خواننده ها را کم کرده و در عوض ماندن روی همین دامین هر روز خواننده های جدید را اضافه می کند.
- سرویس های دی ان اس درکل اینترنت باید پخش بشن. یعنی در لحظه تعریف سرور دی ان اس من می دونه که صاحب فلان دامین است و باید اونو به فلان آدرس بفرسته ولی طول می کشه اینو همه بفهمن. گفته می شه که لازمه بیست و چهار ساعت فرصت بدین تا همه دنیا بفهمن که این آدرس باید بره فلان جا
- اسم های دامنه رایگان (مثلا همین آندو دات ایت) بامزه هستن ولی خیلی خوب نیستن. مثلا اگر چیزی روی این آدرس ها درست کنین ممکنه از نظر خیلی از سایت ها ذاتا اسپم باشه (:
