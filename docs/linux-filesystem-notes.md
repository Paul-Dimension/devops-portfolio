Файлова система Linux - нотатки (LIN-01)

1. Структура каталогів 

У Linux немає дисків C: чи D:. Є одне дерево, корінь - /, а все інше - його гілки.

/           - Корінь файлової системи, початок усього дерева
/etc        - Конфігурація системи і сервісів: sshd_config, fstab, hosts. Коли щось не працює - дивитись сюди в першу чергу
/var/log    - Журнали: messages, secure, dnf.log
/home       - Домашні каталоги користувачів. Мій: /home/savage (~)
/tmp        - Тимчасові файли. На Fedora очищається при завантаженні
/opt        - Сторонній софт, поставлений вручну, поза пакетним менеджером
/usr        - Програми і бібліотеки: /usr/bin, /usr/lib
/bin,/sbin  - На Fedora - посилання на /usr/bin і /usr/sbin
/root       - Домашній каталог. Не плутати корнем / !!!
/boot       - Ядро і завантажувач
/dev        - Пристрої (/dev/sda, /dev/null)
/proc,/sys  - Віртуальні каталоги ядра, фізичних файлів там немає
/mnt,/media - Точки монтування

2. Навігація

- pwd       - показати де я зараз
- cd /etc   - перейти за абсолютним шляхом
- cd ssh    - перейти за відносним шляхом
- cd ..     - на рівень вгору
- cd -      - туди де був щойно
- cd ~      - або просто cd, додому
- ls -la    - довгий формат -l (права, власник, розмір), -a приховані файли (з крапки), -h розміри в kb/mb
- Tab       - доповнення імен файлів та каталогів
- Абсолютний шлях починається з / і не залежить від поточного каталогу. Відносний шлях - залежить: devops-portfolio/docs працює тільки з /home/savage, а /devops-portfolio/docs шукає прямо під коренем / і не знаходить її

3. Пошук файлів 

find /etc -maxdepth 2 -name "*.conf" 2>/dev/null | head -20
find ~ -name "*.log" -mtime -7 # змінені за останні 7 днів
find /var/log -type f -size +10M # файли більші за 10 МБ
find /etc -iname "*ssh*" # -iname: без урахування регістру
find ~ -type d -name "docs" # тільки каталоги 

    find <де> <умови>: -name — маска (* — будь-які символи),    -type f файл / -type d каталог, -size, -mtime -7, -maxdepth N — не лізти глибше N рівнів

    2>/dev/null ховає помилки доступу: частина системних файлів належить root, і це нормально

    Маска прямо в ls: ls /etc/*.conf, ls /var/log/*.log

    Де лежить команда: which python3, whereis sshd

4. Місце на диску

df -h                                 # вільне місце на кожному диску
du -sh ~/* 2>/dev/null | sort -h      # що скільки займає вдома
du -sh /var/log                       # скільки важать логи
df — про файлові системи, du — про каталоги

Мій вивід df -h (дописати після виконання):
savage@localhost:~$ df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/fedora-root   15G  3.9G   12G  26% /
devtmpfs                 3.8G     0  3.8G   0% /dev
tmpfs                    3.9G     0  3.9G   0% /dev/shm
efivarfs                 256K   25K  232K  10% /sys/firmware/efi/efivars
tmpfs                    1.6G  1.4M  1.6G   1% /run
none                     1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
none                     1.0M     0  1.0M   0% /run/credentials/systemd-resolved.service
tmpfs                    3.9G     0  3.9G   0% /tmp
/dev/vda2                2.0G  490M  1.5G  25% /boot
/dev/vda1                599M   13M  587M   3% /boot/efi
none                     1.0M     0  1.0M   0% /run/credentials/serial-getty@ttyAMA0.service
none                     1.0M     0  1.0M   0% /run/credentials/getty@tty1.service
tmpfs                    791M  4.0K  791M   1% /run/user/1000

Висновок

Сервер без документації читається за 15 хвилин: конфіги в /etc, логи в /var/log, свої файли в /home, тимчасове в /tmp
find з -maxdepth і 2>/dev/null — головний інструмент пошуку
permission denied на системних каталогах — не помилка, а права root; лікується 2>/dev/null або sudo для читання, ніколи — chmod на системних каталогах
