# Linux Boot Process & Service Management

## فارسی

در زمان بوت، کنترل ما روی سیستم خیلی محدود عه؛ بنابراین دونستن اینکه از لحظه‌ی روشن‌شدن سیستم تا آماده‌شدن اون چه اتفاقی می‌افته، اهمیت زیادی داره.

فرایند بوت لینوکس رو می‌شه به‌صورت کلی به ترتیب زیر در نظر گرفت:

```text
1. Firmware
2. Bootloader
3. Kernel
4. init / systemd
5. System Services
```

توی این مبحث با مراحل اصلی Boot Process، نقش BIOS و UEFI، Bootloader و GRUB، Kernel، فایل‌های `initrd` و `initramfs` و همچنین `init` و `systemd` آشنا شدم.

---

# 1. مراحل بوت لینوکس

فرایند بوت لینوکس به‌صورت کلی شامل چهار مرحله‌ی اصلی عه:

1. شروع کار توسط Firmware
2. راه‌اندازی Bootloader
3. اجرای Kernel
4. شروع سرویس‌های سیستم

---

# 2. مرحله اول: Firmware

بعد از روشن‌شدن سیستم، اولین بخش قابل اجرا Firmware مادربرد عه.

سفت افزار یا Firmware معمولاً یکی از این دو مورد عه:

- BIOS
- UEFI

سفت افزار وظایف مهمی رو در ابتدای Boot Process انجام می‌ده مثل:

- انجام تست اولیه‌ی سخت‌افزار
- پیدا کردن دستگاه Boot
- اجرای Bootloader

### POST

یکی از اولین کارهای Firmware اجرای **POST (Power-On Self-Test)** عه.

تست اولیه‌ یا POST برای بررسی اولیه‌ی سخت‌افزار انجام می‌شه تا مشخص بشه اجزای ضروری سیستم در وضعیت قابل استفاده قرار دارن.

---

# 3. BIOS و UEFI

دو روش مهم برای Firmware سیستم عبارت‌اند از:

### BIOS

روش قدیمی‌تر Boot، BIOS عه.

توی سیستم‌های BIOS، اطلاعات مربوط به Boot معمولاً با **MBR (Master Boot Record)** مرتبط هستش.

البته MBR فضای محدودی برای اطلاعات Boot داره و توی بعضی سیستم‌ها ممکنه Bootloader به‌صورت چندمرحله‌ای اجرا بشه.

### UEFI

روش جدیدتر و پیشرفته‌تر Firmware، UEFI عه.

توی سیستم‌های UEFI، یه پارتیشن مخصوص به اسم ESP یا EFI System Partition هست که برای فایل‌های مربوط به Boot استفاده می‌شه.

---

# 4. مرحله دوم: Bootloader

بعد از Firmware، نوبت به Bootloader می‌رسه.

Bootloader یه نرم‌افزار سبک عه که وظیفه‌ی اصلی اون آماده‌کردن شرایط لازم برای اجرای سیستم‌عامل و بارگذاری Kernel عه.

توی سیستم‌های لینوکسی، یکی از مهم‌ترین Bootloaderها GRUB عه

### GRUB

بوت‌لودر GRUB می‌تونه یه سیستم‌عامل رو اجرا کنه، چند سیستم‌عامل رو مدیریت کنه، پارامترهای مختلفی رو زمان Boot به Kernel ارسال کنه، Kernel لینوکس رو توی حافظه بارگذاری کنه.

به همین خاطر GRUB نقش مهمی بین Firmware و Kernel داره.

---

# 5. مرحله سوم: اجرای Kernel

بعد از اجرای Bootloader، نوبت به Kernel می‌رسه.

،در این مرحله Bootloader، Kernel لینوکس رو توی حافظه قرار می‌ده و بعد کنترل سیستم رو به Kernel منتقل می‌کنه.

Kernel هسته‌ی اصلی سیستم‌عامل عه و مسئول مدیریت منابع و ارتباط سطح پایین سیستم با سخت‌افزار عه.

اما Kernel برای شروع کار ممکن عه به اطلاعات و ابزارهای اولیه‌ای نیاز داشته باشه.

این اطلاعات معمولاً از طریق فایل‌های initrd و initramfs در اختیار Kernel قرار می‌گیره:

### initrd و initramfs

فایل‌های `initrd` و `initramfs` محیط اولیه‌ای رو در اختیار Kernel قرار می‌دن تا Kernel بتونه مراحل اولیه‌ی Boot رو انجام بده.

یکی از وظایف مهم اون‌ها کمک به Kernel برای پیدا کردن و آماده‌کردن Root Filesystem عه

به‌صورت ساده:

```text
Bootloader
    ↓
Kernel
    ↓
initrd / initramfs
    ↓
Find / prepare Root Filesystem
    ↓
Continue system startup
```

---

# 6. مرحله چهارم: شروع سرویس‌های سیستم

بعد از اینکه Kernel آماده شد، نوبت به اجرای برنامه‌ی اولیه‌ی سیستم یعنی init می‌رسه.

توی سیستم‌های مدرن لینوکس، این نقش معمولاً توسط systemd .انجام می‌شه

برنامه‌ی init یا systemd مسئول راه‌اندازی سرویس‌ها، زیرسیستم‌ها و برنامه‌های ضروری سیستم عه.

---

# 7. وظایف init

یکی از مهم‌ترین وظایف init مدیریت فرایند راه‌اندازی سیستم هستش.

به‌طور کلی برنامه‌ی init وظایف زیر رو بر عهده داره:

- تعیین ترتیب اجرای سرویس‌ها
- مدیریت وابستگی بین سرویس‌ها
- راه‌اندازی سرویس‌های ضروری
- متوقف‌کردن سرویس‌ها
- ری‌استارت کردن سرویس‌ها
- فراهم‌کردن امکان مدیریت سرویس‌ها توسط Administrator

مثلا ممکنه یه سرویس فقط وقتی اجرا بشه که سرویس‌های دیگه‌ای اول فعال شده باشن.

---

# پیدا کردن init فعلی سیستم

برای پیدا کردن init فعلی می‌تونیم از دستورات پایین استفاده کنیم:

```bash
which init
readlink -f /usr/sbin/init
pstree
```

- دستور `which init` مسیر دستور `init` رو نشان می‌ده.
- دستور `readlink -f /usr/sbin/init` مسیر واقعی فایل `init` رو مشخص می‌کنه.
- دستور `pstree` ساختار سلسله‌مراتبی Processها رو نمایش می‌ده.

---

# 8. SysVinit

یکی از روش‌های قدیمی مدیریت سرویس‌ها توی لینوکس، SysVinit عه.

توی این روش، اسکریپت‌های سرویس‌ها معمولاً نوی دایرکتوری زیر قرار دارن:

```text
/etc/init.d
```

برای مدیریت سرویس‌ها می‌شه از دستوراتی مثل موارد زیر استفاده کنیم:

```bash
/etc/init.d/ntpd status
/etc/init.d/ntpd start
/etc/init.d/ntpd stop
/etc/init.d/ntpd restart
```

روش SysVinit هنوز ممکنه توی بعضی سیستم‌ها یا برای سازگاری با نرم‌افزارهای قدیمی پشتیبانی بشه، ولی توی توزیع‌های مدرن، `systemd` استاندارد رایج‌تری است.

---

# 9. systemd

استاندارد مدرن مدیریت سرویس‌ها و فرایند راه‌اندازی سیستم توی خیلی از توزیع‌های لینوکس، systemd عه.

یکی از مفاهیم اصلی `systemd`، **Unit** عه.

واحدها یا Unitها می‌تونن انواع مختلفی داشته باشن و هرکدوم بخشی از سیستم رو مدیریت کنند.

---

# مشاهده Unitها

برای دیدن Unitهای فعال می‌تونیم از دستور زیر استفاده کنیم:

```bash
systemctl list-units
```

برای دیدن Unitهای مربوط به Targetها می‌تونیم از دستور زیر استفاده کنیم:

```bash
systemctl list-units --type=target
```

برای دیدن Default Target سیستم می‌تونیم از دستور زیر استفاده کنیم:

```bash
systemctl get-default
```

---

# دیدن فایل یه Unit

برای دیدن محتوای فایل مربوط به یه Unit می‌تونیم از دستور زیر استفاده کنیم:

```bash
systemctl cat ntpd.service
```

یا:

```bash
systemctl cat graphical.target
```

این دستورات به ما کمک می‌کنن تنظیمات و اطلاعات مربوط به Unit موردنظر رو بررسی کنیم.

---

# 10. مدیریت سرویس‌ها با systemctl

ابزار اصلی برای مدیریت سرویس‌ها و Unitهای `systemd`، دستور `systemctl` عه.

### مشاهده وضعیت سرویس

برای دیدن وضعیت یه سرویس می‌تونیم از دستور پایین استفاده کنیم:

```bash
systemctl status sshd
```

### شروع سرویس

برای شروع یه سرویس می‌تونیم از دستور پایین استفاده کنیم:

```bash
systemctl start sshd
```

### متوقف‌کردن سرویس

برای متوقف‌کردن یه سرویس می‌تونیم از دستور پایین استفاده کنیم:

```bash
systemctl stop sshd
```

### راه‌اندازی دوباره سرویس

برای راه‌اندازی دوباره یه سرویس می‌تونیم از دستور پایین استفاده کنیم:

```bash
systemctl restart sshd
```

### بارگذاری دوباره تنظیمات سرویس

برای Reload کردن تنظیمات یه سرویس می‌تونیم از دستور پایین استفاده کنیم:

```bash
systemctl reload sshd
```

### بررسی فعال بودن سرویس

برای بررسی فعال بودن یه سرویس می‌تونیم از دستور پایین استفاده کنیم:

```bash
systemctl is-active sshd
```

### بررسی Failed بودن سرویس

برای بررسی Failed بودن یه سرویس می‌تونیم از دستور پایین استفاده کنیم:

```bash
systemctl is-failed sshd
```

### فعال‌کردن سرویس برای اجرای خودکار موقع Boot

برای فعال‌کردن اجرای خودکار یه سرویس موقع Boot می‌تونیم از دستور پایین استفاده کنیم:

```bash
systemctl enable sshd
```

### غیرفعال‌کردن اجرای خودکار سرویس موقع Boot

برای غیرفعال‌کردن اجرای خودکار یه سرویس موقع Boot می‌تونیم از دستور پایین استفاده کنیم:

```bash
systemctl disable sshd
```

### بارگذاری دوباره فایل‌های Unit

برای بارگذاری دوباره فایل‌های Unit توسط `systemd` می‌تونیم از دستور پایین استفاده کنیم:

```bash
systemctl daemon-reload
```

> نکته: `daemon-reload` با `reload` تفاوت داره.
>
> دستور `reload` تنظیمات یه سرویس در حال اجرا رو Reload می‌کنه، درحالی‌که دستور `daemon-reload` باعث می‌شه خود `systemd` فایل‌های Unit رو دوباره بخونه.

---

# 11. مشاهده پیام‌های Boot و Kernel

موقع Boot، Kernel پیام‌های مختلفی تولید می‌کنه.

این پیام‌ها توی ساختاری به اسم Kernel Ring Buffer نگهداری می‌شن.

برای دیدن پیام‌های Kernel می‌تونیم از دستور زیر استفاده کنیم:

```bash
dmesg
```

همچنین برای دیدن پیام‌های Kernel از طریق `systemd-journald` می‌تونیم از دستور زیر استفاده کنیم:

```bash
journalctl -k
```

برای دیدن پیام‌های مربوط به Boot فعلی می‌تونیم از دستور زیر استفاده کنیم:

```bash
journalctl -b
```

---

# 12. مشاهده لاگ‌های systemd با journalctl

ابزار اصلی برای مشاهده و بررسی لاگ‌های `systemd` و `journald`، دستور `journalctl` است.

### دیدن تمام لاگ‌ها

برای دیدن تمام لاگ‌ها می‌تونیم از دستور زیر استفاده کنیم:

```bash
journalctl
```

### نمایش لاگ‌ها بدون استفاده از Pager

برای نمایش لاگ‌ها بدون استفاده از Pager می‌تونیم از دستور زیر استفاده کنیم:

```bash
journalctl --no-pager
```

### نمایش 10 خط آخر

برای نمایش 10 خط آخر می‌تونیم از دستور زیر استفاده کنیم:

```bash
journalctl -n 10
```

### نمایش لاگ‌های مربوط به یک بازه‌ی زمانی

برای دیدن لاگ‌های یه بازه‌ی زمانی می‌تونیم از دستور زیر استفاده کنیم:

```bash
journalctl -S -1d
```

توی این مثال، لاگ‌های یک روز گذشته نمایش داده می‌شن.

### دیدن لاگ‌ها در حالت عیب‌یابی

برای دیدن لاگ‌ها توی حالت عیب‌یابی می‌تونیم از دستور زیر استفاده کنیم:

```bash
journalctl -xe
```

### دیدن لاگ‌های یک Unit مشخص

برای دیدن لاگ‌های یه Unit مشخص می‌تونیم از دستور زیر استفاده کنیم:

```bash
journalctl -u ntp
```

### دیدن لاگ‌های یک Process با PID مشخص

برای دیدن لاگ‌های یه Process با PID مشخص می‌تونیم از دستور زیر استفاده کنیم:

```bash
journalctl _PID=2929
```

---

# 13. بررسی لاگ‌های Kernel و Boot

برای بررسی مشکلات مربوط به Boot و Kernel، دستورات زیر کاربرد زیادی دارن:

```bash
dmesg
journalctl -k
journalctl -b
```

هرکدوم از این دستورها اطلاعات متفاوتی درباره‌ی وضعیت Kernel و فرایند Boot در اختیار ما قرار می‌دن.

---

# نکات مهم

- قبل از Bootloader، Firmware اجرا می‌شه.
- مسئولیت بارگذاری Kernel بر عهده‌ی Bootloader عه.
- یکی از Bootloaderهای مهم مورد استفاده توی سیستم‌های لینوکسی، `GRUB` عه.
- فایل‌های `initrd` و `initramfs` در مراحل اولیه‌ی راه‌اندازی Kernel نقش دارن.
- مسئولیت شروع فرایند راه‌اندازی سرویس‌ها بر عهده‌ی `init` عه.
- توی بسیاری از توزیع‌های مدرن، `systemd` نقش `init` رو بر عهده داره.
- روش قدیمی‌تر مدیریت سرویس‌ها در لینوکس، `SysVinit` عه.
- مفهوم اصلی `systemd` بر پایه‌ی Unitهاست.
- برای مدیریت Unitها و سرویس‌ها از `systemctl` استفاده می‌شه.
- برای دیدن لاگ‌های `systemd` و `journald` از `journalctl` استفاده می‌شه.
- برای دیدن پیام‌های Kernel و Kernel Ring Buffer از `dmesg` استفاده می‌شه.
- دستور `systemctl enable` یه سرویس رو برای اجرای خودکار موقع Boot فعال می‌کنه.
- دستور `systemctl start` یه سرویس رو همین حالا اجرا می‌کنه؛ این دو مفهوم یکسان نیستند.
- دستور `systemctl disable` اجرای خودکار سرویس توی Boot رو غیرفعال می‌کنه، اما لزوماً سرویس در حال اجرا رو متوقف نمی‌کنه.
- دستور `systemctl stop` سرویس در حال اجرا رو متوقف می‌کنه.
- دستور `systemctl reload` با دستور `systemctl daemon-reload` تفاوت داره.

---

# جمع‌بندی

فرایند بوت لینوکس رو می‌تونیم به ترتیب زیر خلاصه کنیم:

```text
1. Firmware
2. Bootloader
3. Kernel
4. init / systemd
5. Services
```

در ابتدا Firmware سخت‌افزار رو بررسی میکنه و Boot Device رو پیدا می‌کنه. بعد Bootloader، که توی سیستم‌های لینوکسی معمولاً `GRUB` عه، Kernel رو بارگذاری می‌کنه.

هسته یا Kernel با کمک `initrd` یا `initramfs` مراحل اولیه‌ی راه‌اندازی رو انجام میده و بعد از آماده‌شدن، کنترل فرایند راه‌اندازی به `init` یا در سیستم‌های مدرن به `systemd` منتقل می‌شه.

در نهایت `systemd` سرویس‌ها و Unitهای موردنیاز سیستم رو مدیریت می‌کنه.

ابزارهای مهم این مبحث عبارت‌اند از:

```text
systemctl: مدیریت سرویس‌ها و Unitها
journalctl: دیدن لاگ‌های systemd
dmesg: دیدن پیام‌های Kernel
```

---

# دستورات مهم

```bash
# Boot and Kernel information
dmesg
journalctl -k
journalctl -b

# Find init
which init
readlink -f /usr/sbin/init
pstree

# systemd Units
systemctl list-units
systemctl list-units --type=target
systemctl get-default

# Inspect Unit files
systemctl cat ntpd.service
systemctl cat graphical.target

# Service management
systemctl status sshd
systemctl start sshd
systemctl stop sshd
systemctl restart sshd
systemctl reload sshd
systemctl is-active sshd
systemctl is-failed sshd
systemctl enable sshd
systemctl disable sshd
systemctl daemon-reload

# systemd logs
journalctl
journalctl --no-pager
journalctl -n 10
journalctl -S -1d
journalctl -xe
journalctl -u ntp
journalctl _PID=1234
```

---

# Tags

`#Linux` `#LPIC1` `#BootProcess` `#BIOS` `#UEFI` `#GRUB` `#Kernel` `#Systemd` `#SysVinit` `#LinuxServices`
