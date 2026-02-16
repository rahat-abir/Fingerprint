# Fingerprint project setup

Download docker desktop. [Download Docker Desktop](https://www.docker.com/products/docker-desktop/)
<img width="1094" height="631" alt="image" src="https://github.com/user-attachments/assets/1e86ce4a-f245-4bff-bf7a-5a07c24fbe1f" />

---

## 1.1 install docker desktop.

- it will ask for a restart and install subsystems for linux.  
- if the installation is finished, docker desktop should look like this.
<img width="1013" height="568" alt="image" src="https://github.com/user-attachments/assets/2a2fa25b-cefb-4f30-912f-392ee7c644b1" />

---

## Install Fingerprint SDK

https://www.zkteco.com/en/SDK  
or click [Download ZKT_SDK](https://www.zkteco.com/en/SDK)

- Download the Windows ZTK SDK, and install it.  
- unzip the downloaded file, run setup.exe
  <img width="1328" height="616" alt="image" src="https://github.com/user-attachments/assets/30684422-aaee-4efe-a156-c391ee5ca063" />

- next, next, next and finish install.

---

## Install dotnet

open windows powershell and RUN:

```
winget install Microsoft.DotNet.SDK.8
```

---

## Install git bash for windows. (install link)

Open a new folder -> in that new folder, right click -> open git bash
<img width="1011" height="530" alt="image" src="https://github.com/user-attachments/assets/1aad2653-00e4-4ca7-843c-43d8bfd07723" />

After opening the git bash paste this command line.

```
git clone https://rahatalfaz@bitbucket.org/synchrotechit/attendancedeviceintegration.git
```

A successful cloning will look like this.
<img width="1429" height="459" alt="image" src="https://github.com/user-attachments/assets/5a89e41d-6ab5-44cc-9403-1d91af111c72" />

---

Git status (check if you are in the right branch) (for developers)

---

Run docker desktop and then run this command in the git bash.

1. 
```
docker exec -it jambura_app composer install
```

2. 
```
docker compose up –build
```

---

Open powershell run this
```
cd C:\fingerprint\attendancedeviceintegration\web\zkfinger-bridge\ZkFingerBridge
dotnet run
```

---

Go to:

http://localhost:7000/login  
or click here

---

Before login run this.

```
MSYS_NO_PATHCONV=1 MSYS2_ARG_CONV_EXCL="*" bash app-run.sh run migration
```
```
MSYS_NO_PATHCONV=1 MSYS2_ARG_CONV_EXCL="*" bash app-run.sh add super-user
```

---

username : demo@ahmed.com  
pass: MyStrongPass123  

---

Login and use the system.
