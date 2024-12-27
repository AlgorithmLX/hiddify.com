---
title: نحوه فعال‌سازی Swap برای یک VPS لینوکسی
---


# نحوه ساخت Swap برای یک VPS لینوکسی بدون ایجاد اختلال در عملکرد

در این مقاله، نحوه اضافه کردن حافظه Swap به یک سرور مجازی (VPS) که در حال حاضر زیر بار است و فاقد Swap می‌باشد، توضیح داده می‌شود. مراحل زیر را با دقت انجام دهید تا مشکلی برای سرور پیش نیاید.

---

## مرحله ۱: بررسی وضعیت فعلی Swap  
ابتدا وضعیت فعلی حافظه Swap را بررسی کنید:  
<div style="text-align: left; direction: ltr;">
  
```bash
sudo swapon --show
```

</div>  

اگر خروجی خالی بود، یعنی سرور Swap فعال ندارد.

---

## مرحله ۲: بررسی فضای دیسک  
برای ایجاد Swap، نیاز به فضای آزاد روی دیسک داریم. با دستور زیر فضای آزاد را بررسی کنید:  
<div style="text-align: left; direction: ltr;">

```bash
df -h
```

</div>

---

## مرحله ۳: ایجاد فایل Swap  
یک فایل جدید برای Swap ایجاد کنید. در این مثال، یک Swap با حجم ۲ گیگابایت ساخته می‌شود:  
<div style="text-align: left; direction: ltr;">

```bash
sudo fallocate -l 2G /swapfile
```

</div>  

اگر دستور بالا در سیستم شما پشتیبانی نمی‌شود، از دستور زیر استفاده کنید:  
<div style="text-align: left; direction: ltr;">

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=2048
```

</div>

---

## مرحله ۴: تنظیم مجوزهای فایل Swap  
برای افزایش امنیت، دسترسی به فایل Swap را محدود کنید:  
<div style="text-align: left; direction: ltr;">

```bash
sudo chmod 600 /swapfile
```

</div>

---

## مرحله ۵: تنظیم فایل به عنوان Swap  
با دستور زیر فایل ایجادشده را به عنوان Swap تنظیم کنید:  
<div style="text-align: left; direction: ltr;">

```bash
sudo mkswap /swapfile
```

</div>

---

## مرحله ۶: فعال‌سازی Swap  
Swap را فعال کنید:  
<div style="text-align: left; direction: ltr;">

```bash
sudo swapon /swapfile
```

</div>

حالا دوباره وضعیت Swap را بررسی کنید تا از فعال‌سازی آن مطمئن شوید:  
<div style="text-align: left; direction: ltr;">

```bash
sudo swapon --show
```

</div>

---

## مرحله ۷: دائمی‌سازی Swap  
برای اطمینان از فعال ماندن Swap پس از ری‌استارت سرور، فایل Swap را به فایل `fstab` اضافه کنید:  
<div style="text-align: left; direction: ltr;">

```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

</div>

---

## نکات پایانی  
- **اندازه مناسب Swap**: بسته به نیاز سرور و منابع موجود، اندازه Swap را انتخاب کنید.  
- **پایش Swap**: پس از ایجاد Swap، با استفاده از ابزارهایی مانند `htop` یا `free` وضعیت آن را زیر نظر بگیرید.  

امیدواریم این راهنما برای شما مفید بوده باشد! 😊
