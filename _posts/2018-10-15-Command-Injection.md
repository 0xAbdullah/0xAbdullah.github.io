---
layout: post
title: Command injection
---
في هذا الدرس سنتحدث عن:-

- ماهي ثغرة Command Injection
- أكتشاف واستغلال الثغرة
- ترقيع الثغرة


ماهي ثغرة Command Injection

```
هو ضعف أمني يسمح للمهاجم بأن يقوم بحقن اوامر يتم تنفيذها على مستوى النظام من خلال (forms, cookies, HTTP headers etc.) ويعود ذلك الى عدم تصفية المدخلات والتاكد من صحتها وسلامتها.
```

 أكتشاف واستغلال الثغرة:
 
  من خلال الكود التالي نلاحظ بأن المبرمج قام بعمل تطبيق يطلب من المستخدم ادخال اسم ملف ليتم قراءته واستخدام دالة system لتنفيذ الامر وعرض المخرجات
  ```
  <html>
<head><title>0xAbdullah LAB | Command iNj</title></head>
<body>
<p><b>Enter custom file name to read it</b></p>
<p><i>Files: test.txt, myname.txt</i></p>
<form action="" method="post">
<input type="text" name="filename" value="" />
<input type="submit" name="submit" value="Submit" />
</form>

<?php

if (isset($_POST['submit'])) {
    $file = $_POST['filename'];
    system("cat $file");
}
?>

</body>
<html>
  ```
  
  في هذه الحالة سيقوم التطبيق باخذ المدخلات من المستخدم وتنفيذها ومن ثم عرض المخرجات
  مثال:
  
  ![](https://s3.eu-west-2.amazonaws.com/uploads.3alampro.com/2018/September/vtGGJHD7O1JpvhkPajHN2lLJaA4332cBpClXlhFB.gif)
  
  ولكن المبرمج لم يقوم بتصفية المدخلات ووثق بالمستخدم ثقة كبيرة وهنا سياتي استغلال المهاجم لهذا الضعف من خلال حقن امر بجانب الامر الذي وضعة المبرمج.
  ```
  امر pwd هو لعرض مسارك الحالي في لينكس وامر ls هو لعرض محتويات المجلد
  ```
  مثال:
  
  ![](https://s3.eu-west-2.amazonaws.com/uploads.3alampro.com/2018/September/jFbCDrwigm9nyqUbRaaWsHLPOhTlK2OpeSTsceG9.gif)
  
  كما نلاحظ تم استغلال الثغرة من خلال حقن امر بجانب المدخلات المطلوبة من المستخدم الامر لا يتوقف عند عرض المسار الحالي او معرفة مكونات المجلد الحالي فقط, بل يمكن المهاجم قراءه ملفات حساسة على الخادم أو استدعاء ملفات خارجية ووضعها على الخادم وبالطبع هذا الامر يعتمد على (الصلاحيات).
ترقيع الثغرة:

لترقيع الثغرة سنقوم بـ استخدام دالة escapeshellcmd ومهمة الدالة هي تصفيه المدخلات والتاكد من صحتها قبل تمريرها وفي حالة قام المستخدم ادخال أي من الرموز التالية

```
&#;`|*?~<>^()[]{}$\, \x0A and \xFF. ' and "
```
سيتم اضافة باك سلاش قبل المدخل ليتم منعه من التنفيذ

الان سنقوم باضافه الدالة والمحاولة مرة اخرى لأستغلال الثغرة

```
<html>
<head><title>0xAbdullah LAB | Command iNj</title></head>
<body>
<p><b>Enter custom file name to read it</b></p>
<p><i>Files: test.txt, myname.txt</i></p>
<form action="" method="post">
<input type="text" name="filename" value="" />
<input type="submit" name="submit" value="Submit" />
</form>

<?php

if (isset($_POST['submit'])) {
    $file = $_POST['filename'];
    $file = escapeshellcmd($file);
    system("cat $file");
}
?>

</body>
<html>
```

مثال بعد ترقيع الثغرة:

![](https://s3.eu-west-2.amazonaws.com/uploads.3alampro.com/2018/September/KA7LrXFtPP4OJrxSJ9PEs75zSXc9c3QWCmeBgQFO.gif)

كما نلاحظ لم يتمكن المهاجم من تنفيذ الاوامر المحقونه.
