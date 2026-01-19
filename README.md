# LAN Realtime Share

A **LAN-based realtime text and file sharing platform** developed by **Dipak Shrestha**.  

**GitHub:** [DipakShrestha-ADS](https://github.com/DipakShrestha-ADS)

---

## Overview

This application allows you to:

- Share text in real-time across devices on the same LAN
- Upload files with automatic TTL countdown (files auto-delete after expiry)
- Download or delete files in real-time
- See online/offline server status

Everything is self-contained inside the `output/` folder for easy deployment.

---

## Folder Structure

All required files are inside the `output/` folder:

```

output/
├─ lan-realtime-share.exe      <-- Windows executable
├─ static/                     <-- CSS, JS files
├─ templates/                  <-- HTML templates
├─ uploads/                    <-- File storage (auto-created / TTL)
├─ README.md                   <-- This file

````

---

## Running the Application

1. **Clone or download** the repository.
2. Navigate to the `output/` folder.
3. Ensure all folders (`static/`, `templates/`, `uploads/`) and `lan-realtime-share.exe` exist in `output/`.
4. If uploads folder is missing, create the folder named uploads
4. Run the server by double-clicking `lan-realtime-share.exe` or via command prompt:

```c
// Powershell Command
./lan-realtime-share.exe
// Command Prompt Command
lan-realtime-share.exe
````

5. Open your browser:

```c
http://localhost:9000
```

6. Other devices on the same LAN can access using your IP address:

```c
http://<YOUR_IP>:9000

// Example: http://192.168.16.0:9000
/* 
    * To find your device ip *
    1. Windows:  type ipconfig from command prompt
    2. Linux/Macos: type ifconfig from terminal
*/
```

7. Start sharing text and files in real-time instantly!

---

## Notes

* The `uploads/` folder stores temporary files. Files expire automatically based on TTL.
* Manual deletion of files also syncs immediately across all connected clients.
* The watermark shows the developer name and GitHub username for attribution.
* The server runs on **port 9000** by default.