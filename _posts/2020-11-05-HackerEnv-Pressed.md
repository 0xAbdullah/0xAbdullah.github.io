---
layout: post
title: HackerEnv Pressed (10.0.102.2)
---



اسم التحدي: Pressed
الصعوبة: متوسط\متقدم

في البداية نقوم بفحص الخادم لاستكشاف المنافذ والخدمات من خلال nmap
 
![Nmap scan](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/1-%20nmap.png) 
 
نلاحظ ان الموقع يعمل على Wordpress الاصدار الخامس, بعد البحث عن ثغرات لهذا الاصدار وجدت هناك ثغره تسمح لك برفع Web shell " WordPress Crop-image Shell Upload " ولكن تطلب منك اسم مستخدم وكلمة مرور.
لذلك راح نستخدم اداة WPScan لمعرفة المستخدمين الموجودين اولاً.

`wpscan --url http://10.0.102.2/ --enumerate u`

![WPscan](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/2-%20WPSCAN%20UE.png) 

بعد عملية Enumeration وجدنا يوزر واحد وهو admin

الان سنقوم بالتخمين على اليوزر باستخدام Rockyou.txt wordlist

`wpscan --url http://10.0.102.2/ --passwords rockyou.txt --usernames admin --max-threads 10`

![WPscan](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/3-%20WPSCAN%20BF.png) 

بعد التخمين استطعنا استخراج الباسورد, الان يمكننا استغلال الثغرة السابقه من خلال Metasploit.
 
 ![MSF](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/4-%20MSF%20exploit%20setup.png) 

الان نقوم بأستغلال الثغره.
 
![MSF](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/5-%20MSF%20exploited.png) 

بعد تمكنا من الوصول نقوم بعملية Enumeration على مستوى النظام, لتصعيد صلاحيتنا الى root

بعد بحث طويل عن ثغرات او misconfiguration تسمح لي بتصعيد صلاحياتي ولكن لم اجد شيء 

كانت اخر محاوله هي الحل الابسط xD 

نلاحظ ان هناك يوزر اخر على الخادم "hackerenv" والباسورد هو نفس اسم اليوزر ☹

![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/6-%20PrivEsc.png) 

بعد التحويل على اليوزر الاخر نلاحظ بانه مضاف الى shadow group

![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/7-%20PrivEsc.png) 

بمعنى اننا نستطيع قراءه ملف الشادو والتعديل عليه.
 
 
![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/8-%20PrivEsc.png) 
 
الان فقط كل الي نحتاجه لرفع صلاحياتنا الى root هو التعديل على الملف.

سنقوم بنسخ الهاش الخاص بـhackerenv ونستبدله بالنجمه الموجوده على يوزر الروت ونحفظه على جهازنا.
 
![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/9-%20PrivEsc.png) 
 
 
ونقوم بفتح python server لتحميل الملف من الجهاز على الخادم واستبداله.
 
![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/10-%20PrivEsc.png) 
 
بعد ذلك الان فقط نقوم بتحميله على الخادم واستبداله بالملف القديم. 

![PrivEsc](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/HackerEnv/Pressed/11-%20PrivEsc.png) 

وبكذا قدرنا نصعد صلاحيتنا الى root

انتهى.
