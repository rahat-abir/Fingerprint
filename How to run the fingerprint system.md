# 🖐️ How to Run the Fingerprint Project

---

## 1️⃣ Open the Project Folder

- Navigate to the **main project folder**.
- Open the **`web`** folder.

---

## 2️⃣ Open Terminal in the Web Folder

- Right-click inside an empty space in the `web` folder.
- Click **"Open in Terminal"**
<img width="1918" height="851" alt="image" src="https://github.com/user-attachments/assets/8fd8ccc8-ba05-48d8-8c03-847d1b8c5139" />

---
## 2️⃣.2️⃣ Make sure to connect the fingerprint device.
## 3️⃣ Start the Fingerprint Bridge (.NET Service)

In the terminal, run:

```bash
cd zkfinger-bridge\ZkFingerBridge
dotnet run
```
Wait until you see that the bridge service is running successfully.

It will look like this,
<img width="1899" height="421" alt="image" src="https://github.com/user-attachments/assets/38aa4645-137e-4a02-bbca-2b91c878aaae" />


⚠️ Important: Keep this terminal open.
The bridge service must remain running for fingerprint capture to work.
## 4️⃣ Open docker desktop app.
## 4️⃣.1️⃣ Start Docker Containers

Open another terminal inside the web folder.

Run:
```bash
docker compose up
```
Wait for Docker run.

⚠️ The first run may take a few minutes.

It will look like this
<img width="1919" height="854" alt="image" src="https://github.com/user-attachments/assets/ac4ece43-3647-4989-928f-640ef127125c" />


## 5️⃣ Open the Application

Once everything is running, open your browser and go to:
```
http://localhost:7000/dashboard
```
If everything is set up correctly, the dashboard should load successfully.
