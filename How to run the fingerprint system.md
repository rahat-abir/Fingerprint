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

## 3️⃣ Start the Fingerprint Bridge (.NET Service)

In the terminal, run:

```bash
cd zkfinger-bridge\ZkFingerBridge
dotnet run
```
Wait until you see that the bridge service is running successfully.

⚠️ Important: Keep this terminal open.
The bridge service must remain running for fingerprint capture to work.

4️⃣ Start Docker Containers

Open another terminal inside the web folder.

Run:
```bash
docker compose up
```
Wait for Docker to:

Build containers (first time only)

Start Apache

Start MySQL

Install dependencies (if configured)

⚠️ The first run may take a few minutes.

5️⃣ Open the Application

Once everything is running, open your browser and go to:
```
http://localhost:7000/dashboard
```
If everything is set up correctly, the dashboard should load successfully.
