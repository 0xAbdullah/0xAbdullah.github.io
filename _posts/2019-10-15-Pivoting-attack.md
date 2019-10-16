---
layout: post
title: Pivoting attack
---
**ما هو Pivoting attack**:
> هو تكنيك يستخدم بعد اختراق أحد الأجهزة على الشبكة ليمكن المهاجم من التنقل بالشبكة والدخول بعمق وفحص واختراق باقي الأجهزة الأخرى التي لا يمكن الوصول لها من خارج الشبكة (Remotely).

في هذا الشرح ستتعلم بشكل عملي كيف يمكن ان يستغل المهاجم أحد الأجهزة المخترقة على الشبكة والوصول لباقي الأجهزة او الخدمات على الشبكة حتى لو كانت معزولة من الوصول الخارجي واختراق الشبكة بشكل أعمق.
![](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/PiovtingAttck/0-%20diagram.png)

في هذا السيناريو نلاحظ ان هناك خادم ويب يمكن الوصول له من الخارج وأيضا هناك شبكة داخلية لا يمكن الوصول لها من الخارج، واستطاع المهاجم من الوصول للخادم واختراقه ولكن لا يمكنه الوصول لشبكة الداخلية للمنظمة (الى الان فقط ).

اول ما سيقوم به المهاجم بعد اختراق الخادم هو معرفة ماهي الأجهزة المتصلة مع الخادم في الشبكة من خلال عمل arp scan
>###### ما هو بروتوكول ARP ([ويكيبيديا](https://ar.wikipedia.org/wiki/%D8%A8%D8%B1%D9%88%D8%AA%D9%88%D9%83%D9%88%D9%84_%D8%AD%D9%84_%D8%A7%D9%84%D8%B9%D9%86%D8%A7%D9%88%D9%8A%D9%86 "ويكيبيديا"))

![](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/PiovtingAttck/1-%20ARP.PNG)

نلاحظ ان الخادم متواجد في Subnet 192.168.8.0/24 وأيضا متصل في Subnet اخر (192.168.209.0/24)

لتوضيح: سأحاول ان اتصل في IP:192.168.209.128 الإجابة قبل الصورة: لن نتمكن بطبيعة الحال 

![](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/PiovtingAttck/3-%20ping%20other%20subnet.PNG)
هل هذا يعني لا يمكن الوصول له؟ بالتأكيد يمكن الوصول له ولكن في الوقت الحالي لا يوجد شيء يقوم بعملية التوجيه (Router) لتتمكن من الاتصال به بشكل مباشر!

سيقوم المهاجم باستخدام الخادم المخترق كموجه (Router) بينه وبين الشبكة الداخلية، من خلال استخدام Autoroute script في Meterpreter حيث سيسمح للمهاجم من الوصول لشبكة الأخرى.
![](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/PiovtingAttck/4-%20autoroute.PNG)

بعد ذلك يقوم المهاجم باستخدام socks4a auxiliary كبروكسي لتمرير اتصاله من خلاله للهدف
![](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/PiovtingAttck/5-%20Proxy%20setting.png)

بعد ذلك يقوم المهاجم بضبط اعدادات اداة ProxyChains التي ستساعدة على تمرير الاتصال للبروكسي
> nano /etc/proxychains.conf

![](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/PiovtingAttck/6-%20ProxyCh.png)

في الخطوه التاليه التي سيقوم بها المهاجم هي فحص الشبكة ومعرفة الأجهزة المتصله.
> ملاحظة: هناك الكثير من الأدوات التي ستساعدك في Metasploit بعملية Scanning لشبكة ولكن ساقوم باستخدام NetCat.

في السكربت التالي سيقوم بعملية Ping Sweep لمعرفه الاجهزه المتصلة على منفذ 80

    #!/bin/bash
    
    for ip in $(seq 1 255); do
      nc -n -v -z -w 1 192.168.209.$ip 80 2>&1 | grep open
    done
    
![](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/PiovtingAttck/7-%20ping%20sweep.PNG)

نلاحظ ان المهاجم اكتشف احد الأجهزة المتصلة بالشبكة على منفذ 80, سيقوم بمحاولة استعراضه من المتصفح.
![](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/PiovtingAttck/8-%20ping%20and%20open%20website.PNG)

 لا يوجد اتصال! فقط يحتاج المهاجم ضبط اعدادات البروكسي للمتصفح.
 ![](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/PiovtingAttck/9-%20proxy%20seting.PNG)
 
 سيقوم المهاجم بضبط الاعدادات للمتصفح شبة المدخلة في socks4a auxiliary.
 ![](https://raw.githubusercontent.com/0xAbdullah/0xAbdullah.github.io/master/MyFiles/PiovtingAttck/10-%20finle%20result.PNG)
 
 اخيراً استطاع المهاجم للوصول لشبكة الداخليه وخدماتها, وبنفس التكنيك يمكنه اختراق باقي الاجهزه والتنقل في الشبكة بعمق!
