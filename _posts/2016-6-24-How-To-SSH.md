---
layout: post
title:  "آموزش کانفیگ ssh"
date:   2016-03-23 14:57:04 +0430
categories: ﺗﺴﺖ
---
# آموزش کانفیگ SSH 

سلام دوستان چند روز پیش میخواستم بوسیله گوشی اندرویدی که دارم به لپتاپ خودم ssh کنم که متاسفانه آموزش خوبی در این رابطه پیدا نکردم برای همین خودم دست بکار شدم.

## SSH چیست ؟
بطور خلاصه و ساده بخوام بگم SSH یه شل امنه به این منظور که به شما دسترسی به Shell و یا همون ترمینال خودمون بوسیله یک پروتکل رمزنگاری شده رو به ما میده.


## شروع کار با SSH
برای کار با SSH  شما نیاز به یک نسخه server و یک نسخه client دارید سرور میشه دستگاهی که شما میخواید بهش ssh بزنید و کلاینت دستگاهیه که باهاش ssh میزنید در اینجا مثلا گوشی موبایل.
در این آموزش من از Xubuntu استفاده میکنم بطور کلی شما از هر توزیع دیگه ای میتونید استفاده کنید فرقی ایجاد نمیکنه, ssh معمولا بصورت پیش فرض روی سیستم عامل نصبه
اگر به هر دلیلی نصب نبود میتونید با دستور زیر نصبش کنید

```bash
sudo apt-get install openssh-server
```
حالا کافیه با دستور زیر از هر دستگاهی به سرور خودتون ssh بزنید توجه داشته باشید که هر دو دستگاه باید در یک شبکه باشند در غیر اینصورت کار نمیکنه

```bash
ssh username@ip:21
```
برای دیدن  ip خودتون از دستور زیر استفاده کنید

```bash
ifocnfig -a
```

![ifconfig command linux][img1]

خب حالا میخوایم از طریق دستگاه اندرویدی که داریم به سرور ssh بزنیم, برای اینکار از برنامه [connectBot](https://play.google.com/store/apps/details?id=org.connectbot&hl=en) استفاده میکنیم .  خب بعد از نصب برنامه بوسیله دستوری که بالاتر اشاره کردم میتونید به راحتی SSH بزنید .

![connectbot][img2]



خیلی خب ! این روش آسون ترین روش و بیشتر استفاده شده ترین روشه اما امن ترین روش نیست. جلوتر بهتون میگم چطور میتونید با استفاده از public authentication امنیت ارتباط تون با رو بیشتر کنید.

## استفاده از public authentication
خب قبل از شروع کار باید یه سری تغییرات در فایل تنظیمات ssh ایجاد کنیم فایل تنظیمات رو بوسیله ادیتور خوب nano باز میکنیم

```bash
sudo nano /etc/ssh/sshd_config
```
 این سه خط رو از حالت comment با برداشتن # بردارید .

```
    RSAAuthentication yes
    PubkeyAuthentication yes
    AuthorizedKeysFile %h/.ssh/authorized_keys
```


خب حالا میتونید شروع به تولید کلید یا همون key کنیم
برای اینکار ابتدا دستور زیر رو اجرا کنید

```bash
ssh-keygen generate
```
از شما درخواست یه passphrase میکنه که میتونید خالی بزاریدش ولی بهتره که یه پسورد برای امنیت بیشتر قرار بدید
در آخر خروجی به این شکل خواهد بود‍
‍‍‍‍‍‍‍‍‍‍‍‍‍‍‍‍‍‍‍
	```bash

	moein@moein:~$ ssh-keygen
	Generating public/private rsa key pair.
	Enter file in which to save the key (/home/moein/.ssh/id_rsa):              
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again: 
	Your identification has been saved in /home/moein/.ssh/id_rsa.
	Your public key has been saved in /home/moein/.ssh/id_rsa.pub.
	The key fingerprint is:
	SHA256:ar2ANl32ibaYW0iUWItUN6RtMuf7NfSI57ZZH75cpyE moein@moein
	The key's randomart image is:
	+---[RSA 2048]----+
	|    ..o.+        |
	|   . + * .       |
	|    o B +        |
	|     . *         |
	|      . S   .    |
	|     + * + + o   |
	|    + * * + E +.o|
	|   . o * + +.=o++|
	|      +.o ..+..+o|
	+----[SHA256]-----+
	```

خب حالا key که ساختیم رو به لیست کلید های مجاز اضافه کنیم برای اینکار ابتدا باید یک فایل بنام authorized_keys بسازیم و به آن دسترسی بدیم دستورات زیر را به ترتیب اجرا کنید

```bash
touch ~/.ssh/authorized_keys
 chmod 700 ~/.ssh

```

خب حالا محتویات فایل id_rsa.pub رو در فایل authorized_keys ذخیره میکنیم

```bash
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
```

و در نهایت فایل id_rsa رو به دستگاه اندرویدی خودتون منتقل کنید و بوسیله برنامه connectbot فایل رو import کنید, به این شکل:
``` connectBot => Manage PubKeys => Import => id_rsa File ``` اگر برای کلیدتون پسور گذاشتید باید پسوردش رو بزنید تا قفل کناره کلید باز بشه و به رنگ در بیاد

حالا میتونید مثل سابق به سرورتون ssh بزنید.

[img1]: /post-imgs/how-to-ssh/1.png
[img2]: /post-imgs/how-to-ssh/2.png