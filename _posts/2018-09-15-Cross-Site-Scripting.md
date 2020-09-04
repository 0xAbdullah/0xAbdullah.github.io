---
layout: post
title: Cross-Site Scripting (XSS)
---

في هذا الدرس سنتحدث عن:-
- ما هي ثغرة XSS
- انواع ثغرة XSS
- اضرار الثغرة
 - الحماية من الثغرة
 - تطبيق عملي
 
 
 ماهي ثغرة XSS:
 
 > البرمجة عبر المواقع / هجوم حقن الشيفرة المصدريّة عبر موقع وسيط (بالإنجليزية: (Cross-site scripting (XSS) هي أحد أنواع الهجوم التي يتعرض لها الأنظمة الحاسوبية، ونجدها خصوصاً في تطبيقات الإنترنت عبر ما يسمى برمجة بالحقن، التي يلجأ فيها بعض مستخدمي الإنترنت المخربين لإدخال بعض الجمل البرمجية للصفحات التي يستعرضها الآخرون.
 
- [wikipedia](https://ar.wikipedia.org/wiki/%D8%A8%D8%B1%D9%85%D8%AC%D8%A9_%D8%B9%D8%A8%D8%B1_%D9%85%D9%88%D8%A7%D9%82%D8%B9)

انواع ثغرة XSS:
- Reflected
- stored
- DOM-based

اضرار الثغرة:
> تعتمد ثغرة XSS على استغلال المدخلات التي يتم ادخالها المهاجم وتكون بالغالب مبرمجه بـ لغة Javascript أو html حيث يتمكن المهاجم من سرقة "" لأنتحال شخصيتك في الموقع المستهدف أو تحويلك الى صفحة اخرى مشابهه للموقع المستهدف كـ صفحة مزورة يتمكن من خلالها سرقة حسابات المستخدمين أو تحويل المستخدمين لتحميل برمجيات خبيثه كـ برمجيات تجسسيه او فدية

الحماية من الثغرة:

المتضررين من الغثره هم المستخدم والمبرمج

### المستخدم
>     لابد على المستخدم ان يتسخدم اخر اصدار من المتصفح وايضا استخدام اضافه NoScript وعدم الدخول على الروابط القادمه من طرف مجهول
### المبرمج
>     لابد على المبرمج التاكد من صحه مدخلاته وخلوها من الاخطاء التي تمكن المهاجم من استغلال ثغره XSS ويقوم بفلتره مدخلاته

تطبيق عملي:

هنا لدي كود بسيط مهمته اخذ قيمة name من المدخلات وطباعتها بالصفحه وهنا سنطبق استغلال الغثره وكيف نحمي المدخلات.

```php
<html>
<head><title>0xAbdullah LAB | XSS</title></head>
<body>
<form action="" method="post">
<input type="text" name="name" value="" />
<input type="submit" name="submit" value="Submit" />
</form>

<?php
 if (isset($_POST['submit'])) {
 $name= $_POST['name'];
 echo "Welcome $name";
}
?>

</body>
<html>
```

على سبيل المثال ساقوم بأدخال اسمي ومن ثم سيقوم بطباعته كتالي: Welcome Abdullah
![enter image description here](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/XSS/1.gif)

ولكن ماذا لو قمت ارسال كود html هل سيقوم بطباعته؟ الاجابه نعم بكل تاكيد بسبب أن المدخلات لم يتم فلترتها سياخذ المدخل من قبل المهاجم وطباعته مثل ماتم ارساله!
![enter image description here](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/XSS/2.gif)

الان سنقوم بـتجربه كود Javascript مهمته هو تحويلنا الى موقع اخر
```javascript
<script> document.location = 'https://3alam.pro';</script>
```
![enter image description here](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/XSS/3.gif)

الان نريد ان نقوم بفلترة المدخل لدينا لكي يتم منع استغلال الثغرة وذلك يتم عن طريق استخدام داله بأسم (**strip_tags**) مهمة الدالة هي حذف اكواد html وطباعه النص

```php
<html>
<head><title>0xAbdullah LAB | XSS</title></head>
<body>
<form action="" method="post">
<input type="text" name="name" value="" />
<input type="submit" name="submit" value="Submit" />
</form>

<?php
 if (isset($_POST['submit'])) {
 $name= $_POST['name'];
 echo strip_tags("Welcome $name");
}
?>

</body>
<html>
```

قمنا بوضع الدالة قبل عملية الطباعه, ليتم فلترة المدخلات من المتغير "name" ومن ثم طباعتها. الان نعيد اختبار المدخلات لدينا بمثل ما فعلنا سابقاً .
![enter image description here](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/XSS/4.gif)

كما نلاحظ تم فتلرة المدخلات وترقيع الثغرة بهذه السهولة.
