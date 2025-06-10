# Домашнее задание к занятию «Защита хоста»

## Задание 1

1. Установите eCryptfs.

2. Добавьте пользователя cryptouser.

3. Зашифруйте домашний каталог пользователя с помощью eCryptfs.

*В качестве ответа пришлите снимки экрана домашнего каталога пользователя с исходными и зашифрованными данными.*

### Решение

1. Установите eCryptfs.

    ```
    ┌──(kali㉿kali)-[~]
    └─$ sudo apt-get update && sudo apt-get install ecryptfs-utils -y
    ```

    ![](pic/1.png)

2. Добавьте пользователя cryptouser.

    ```
    ┌──(kali㉿kali)-[~]
    └─$ sudo adduser cryptouser
    ```

    ![](pic/2.png)

3. Зашифруйте домашний каталог пользователя с помощью eCryptfs.

    ```
    ┌──(kali㉿kali)-[~]
    └─$ sudo modprobe ecryptfs                  
                    
    ┌──(kali㉿kali)-[~]
    └─$ sudo ecryptfs-migrate-home -u cryptouser
    ```

    ![](pic/3.png)

**Проверка**

![](pic/4.png)

---

## Задание 2

1. Установите поддержку LUKS.

2. Создайте небольшой раздел, например, 100 Мб.

3. Зашифруйте созданный раздел с помощью LUKS.

*В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.*

### Решение

1. Установите поддержку LUKS.

    ```
    ┌──(kali㉿kali)-[~]
    └─$ sudo apt install cryptsetup
    ```

    ![](pic/5.png)

2. Создайте небольшой раздел, например, 100 Мб.

    ```
    ┌──(kali㉿kali)-[~]
    └─$ sudo fdisk /dev/sdb
    ```

    ![](pic/6.png)

3. Зашифруйте созданный раздел с помощью LUKS. + проверка

    ```
    ┌──(kali㉿kali)-[~]
    └─$ sudo cryptsetup -y -v --type luks2 luksFormat /dev/sdb1

    ┌──(kali㉿kali)-[~]
    └─$ sudo cryptsetup luksOpen /dev/sdb1 disk

    ┌──(kali㉿kali)-[~]
    └─$ ls /dev/mapper/disk

    ┌──(kali㉿kali)-[~]
    └─$ sudo mkfs.ext4 /dev/mapper/disk

    ┌──(kali㉿kali)-[~]
    └─$ mkdir .secret

    ┌──(kali㉿kali)-[~]
    └─$ sudo mount /dev/mapper/disk .secret/

    ┌──(kali㉿kali)-[~]
    └─$ sudo umount .secret

    ┌──(kali㉿kali)-[~]
    └─$ sudo cryptsetup luksClose disk
    ```

    ![](pic/7.png)

---