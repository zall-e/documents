# راهنمای کامل راه‌اندازی Tor + Privoxy در ویندوز


## 1. مقدمه
- تور(Tor) ابزاری برای ناشناس‌سازی ترافیک اینترنت است و Privoxy به‌عنوان یک پراکسی محلی عمل می‌کند تا ترافیک سیستم را از طریق Tor عبور دهد. با ترکیب این دو، می‌توان تمام ترافیک سیستم یا مرورگر را از طریق شبکه Tor هدایت کرد تا IP شما تغییر کرده و حریم خصوصی حفظ شود.

## 2. دانلود و نصب

### 2.1 دانلود Tor
- وب‌سایت رسمی: [https://www.torproject.org/download/tor/](https://www.torproject.org/download/tor/)
- دانلود نسخه Tor Expert Bundle
- استخراج فایل ZIP در مسیر دلخواه مثلا `C:\Tor`

### 2.2 دانلود Privoxy
- دانلود: [https://www.privoxy.org](https://www.privoxy.org/sf-download-mirror/Win32/)
- نصب یا استخراج نسخه ویندوز در مسیر مثلا `C:\Privoxy`

## 3. پیکربندی

### 3.1 فایل torrc.txt
- داخل پوشه tor، فایلی به اسم torrc (یا torrc.sample) وجود داره. اگه torrc نبود، یه فایل متنی جدید به اسم torrc بساز.
- فایل torrc رو با Notepad باز کن و این خطوط رو اضافه کن 👇
```
SocksPort 9050
ControlPort 9051
CookieAuthentication 0
```

### 3.2 فایل config.txt برای Privoxy
- بعد از نصب، فایل config.txt رو باز کن
- مسیر متداولی که احتمالا اونجا قرار داره: C:\Program Files (x86)\Privoxy
- توی فایل دنبال خطی بگرد که با forward-socks5 شروع میشه. اگه نبود، اضافه کن👇
- فایل نوت بوک در حالت ادمین باز کنید.
```
forward-socks5t   /   127.0.0.1:9050 .
```

## 4. اجرای Tor و Privoxy
دستورات در حالت ادمین اجرا کنید.
```bash
cd C:\Tor
tor.exe -f torrc.txt

cd "C:\Program Files (x86)\Privoxy"
privoxy.exe config.txt
```

## 5. تنظیم سیستم برای استفاده از پروکسی
- مسیر: **Settings → Network & Internet → Proxy**
- فعال کردن **Use a proxy server**
- Address: `127.0.0.1`
- Port: `8118`

## 6. اتصال کل سیستم با Proxifier
- نصب Proxifier
- افزودن Proxy با Address `127.0.0.1` و Port `9050`
- ساخت Rule برای هدایت کل ترافیک
- فعال‌سازی **Global Mode**

## 7. اتصال مرورگر با FoxyProxy
- نصب افزونه FoxyProxy روی Firefox یا Chrome
- ساخت پروفایل جدید:
  - Proxy Type: HTTP
  - IP: `127.0.0.1`
  - Port: `8118`
- فعال‌سازی پروفایل

## 8. راه‌اندازی خودکار با Batch Script
### start_tor_privoxy.bat
```bat
@echo off
cd /d "C:\Tor"
start "" /B tor.exe -f torrc.txt
timeout /t 8 /nobreak >nul
cd /d "C:\Program Files (x86)\Privoxy"
start "" /B privoxy.exe config.txt
exit
```

### اضافه کردن به Startup
- ساخت Shortcut از Batch
- مسیر: `shell:startup`
- قرار دادن شورتکات

## 9. تست اتصال با Python
```python
import requests

proxies = {
    'http': 'http://127.0.0.1:8118',
    'https': 'http://127.0.0.1:8118'
}

r = requests.get('https://check.torproject.org', proxies=proxies)
print(r.text)
```

## 10. خطاهای رایج
| خطا | دلیل احتمالی | راه‌حل |
|-----|--------------|--------|
| Port 8118 در حال استفاده است | اجرای چندباره Privoxy | بستن پردازش قبلی با `tasklist` و `taskkill` |
| default.filter پیدا نشد | فایل پیش‌فرض Privoxy وجود ندارد | مسیر را اصلاح یا فایل را اضافه کنید |
| عدم دسترسی به logfile | ویندوز اجازه نوشتن ندارد | اجرای Privoxy با Admin یا تغییر مسیر فایل |
| Invalid header received | تنظیمات پراکسی اشتباه در مرورگر یا سیستم | بررسی مجدد Proxy در مرورگر یا Windows |


لینک GPT:

https://chatgpt.com/share/68e0f260-dc18-8010-96c2-fe0373e5f9bb
