---
title: TryHackMe Brute It
date: 2026-07-08
slug: thm-room-bruteit
description: "write-up to try hack me room: brute it"
updated: 2026-07-08T12:10:39+08:00
---

[Try Hack Me: Brute It](https://tryhackme.com/room/bruteit)

一個簡單又有成就感的 challenge 。

它明確的說明了： `Learn how to brute, hash cracking and escalate privileges in this box!` 讓我們知道這個 room 著重於哪些技巧


## Task2: Recon
`nmap` 可以看到 `ssh` and `http` 是開著的，打開 http page 是 apache 的 default page.

利用 `gobuster` enumerate web content , 發現 `admin` 目錄，是一個登入頁面。檢查 source code 發現了登入帳號 `admin` 以及一個名字 `john`

Web flag 也在這裡揭露。

## Task3: Getting a shell
在這個情境下，有了 username 我們可以直接 brute-forcing 找出 password.

登入後是一段給 `john` 的筆記，附帶一個 RSA key，試著用它來連線到 ssh ，發現它有 passphrase 保護，直接來著手破解。

1. 用 `ssh2john` 轉成 hash
	- `ssh2john id_rsa`
2. 用 `hashcat` 配合 rockyou 來猜出密碼
	- `hashcat -m 22931 -a 0 ./john.hash path/to/rockyou.txt`

踩到一個坑是 `ssh2john` 輸出的格式開頭是 `id_rsa:$sshng$1$16$E32C4...` ，跟 hashcat 預期的不同，這裡我手動移除了 `id_rsa` 的 prefix.

操作正確的話，很快就可以拿到 passphrase. 並以 `john` 登入取得 user flag(user.txt)

## Task4: Privilege Escalation

第一個 task 是 `What is the root's password?` 這裡我沒什麼頭緒。

檢查 `sudo -l` ，發現 `john` 有 `sudo cat` 的權限，檢查 [GTFOBins](https://gtfobins.org/) 沒發現什麼有用的手段。

第二個 task 是 `root.txt` ，是恰到好處的提示，我想到 CTF 有把資訊放在 `/root/root.txt`. 的慣例，試了 `sudo cat /root/root.txt` 就成功拿到 flag 了。

那 root's password 要怎麼取得？考慮這個 room 的主題， `sudo cat /root/shadow`  就可以拿到 hash ，然後丟進 hashcat ，喝杯咖啡，感受安全性的脆弱☕。

## Retrospective
這是在學完 `john the ripper`, `hashcat` 之後想練習找到的 room。雖說提示很多，需要的技巧也不難，但算是我第一個沒有在外部輔助下完成的 room，寫個 write-up 以資記念。

其中比較有卡住的是 Task 4 中的 `What is the root's password?` 它有個提示是利用 GTFOBins ，但我在上面找不到關於 `cat` 的有用資訊， 之前在找 `vim` 的 
`:!bash` 也找不到，也許是還不夠悉熟用錯方法了。但反過來說自己從有限的知識中找到可以利用的手段還滿有成就感的。
