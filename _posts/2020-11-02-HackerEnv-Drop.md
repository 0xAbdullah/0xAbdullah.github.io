---
layout: post
title: HackerEnv Drop (10.0.100.2)
---

اسم التحدي: drop

مستوى التحدي: سهل

في البداية نقوم بفحص السيرفر من خلال nmap لمعرفة المنافذ المفتوحة والخدمات.

```nmap TARGET-IP -p-```

![Nmap scan](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Drop/1-%20nmap.png)

`nmap TARGET-IP -p80 -sC -sT`

![Nmap scan](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Drop/1-%20nmap%202.png)

 
يوجد منفذ واحد 80 ونلاحظ ان الموقع يستخدم Drupal كـCMS والاصدار هو 7 بعد البحث عن الاصدار وجدت انه مصاب بـRCE (CVE-2018-7600)
بعد تحميل الثغره وتشغيلها وجدت ان النسخه هي 7.30 والاستغلال يعمل على هذه النسخة xD

![Web Exploit CVE-2018-7600](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Drop/3-%20WebExploit.png)
 
بعد استغلال الثغره ورفع Web shell الان فقط نحتاج نحصل على Reverse connection

![RC](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Drop/4-%20RCOMMAND.png)

![RC](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Drop/4-%20RC.png)

بعد ذلك نقوم بتحميل Linpeas لمساعدتنا على رفع صلاحيتنا على الخادم.

وقت عمل Linpeas لفت انتباهي ان هناك بملف باسم cleaningScript.sh يمكننا التعديل عليه

![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Drop/5-%20PrivEsc.png)

بعد التحقق من السكربت يتضح باننا يمكنا الكتابه

![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Drop/6-%20PrivEsc.png)

ثم استخدمت pspy لمراقبة العمليات على النظام وتم ملاحظة ان السكربت يحتاج دقيقه كاملة حتى يعمل مره اخرى بصلاحية root.

![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Drop/7-%20process.png)

كل الي نحتاجه الان فقط نقوم بالتعديل على الملف وننتظر دقيقه واحدة ونحصل على Reverse connection بصلاحية root

![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Drop/8-%20PrivEsc.png)

![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Drop/9-%20root.png)

وبكذا قدرنا نرفع صلاحيتنا الى root

انتهى.
