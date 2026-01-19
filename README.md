# LAN Realtime Share

A **LAN-based realtime text and file sharing platform** developed by **Dipak Shrestha**.  

**GitHub:** [DipakShrestha-ADS](https://github.com/DipakShrestha-ADS)

> Built with **Go** for speed, simplicity, and cross-platform support.  

---

---

## Overview

LAN LIVE SHARE is a **local server application** that allows you to:

- Share **text messages** in real-time with other devices on your LAN
- Upload and share **files** with automatic countdown timers
- Download or delete shared files instantly
- View **online/offline server status**
- All features work **locally on your network**, no internet required
- Works on Windows, Linux, and Mac

Everything is self-contained inside the `output/` folder.  

> Note: Since this is a **local server**, everything you share stays **within your LAN**, so it’s **safe and private**.  

> **Hint:** The server runs entirely on your machine; other devices just connect via your LAN IP.
---

## How It Works

1. When you run the server executable, it starts a **local web server** on your machine.
2. Other devices on the same **LAN network** can access it using your machine's **IP address** and port `9000`.
3. Anything you **share — text or files — is synced in real-time** across all connected devices on the network.
4. Files are **stored temporarily** (with countdown timers) and automatically deleted after expiry or when manually removed.

---

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

## Running the Application || Getting Started

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

## Quick Example (Go-powered!)

The server is built in Go. Starting it is as simple as:

```go
package main

func main() {
    cleanUploads()
	startCleanup()
	
	os.MkdirAll("uploads", 0755)
    http.HandleFunc("/", indexHandler)
	http.HandleFunc("/ws", wsHandler)
	http.HandleFunc("/upload", uploadHandler)
	http.HandleFunc("/files", listFilesHandler)
	http.HandleFunc("/delete", deleteFileHandler)

	http.Handle("/files/", http.StripPrefix("/files/", http.FileServer(http.Dir("uploads"))))
	http.Handle("/static/", http.StripPrefix("/static/", http.FileServer(http.Dir("static"))))
    // Start the LAN Live Share server on port 9000
    startServer(":9000")
}
```

---

## LAN LIVE SHARE – API Routes

This LAN Live Share server exposes the following **HTTP endpoints** for text and file sharing.
All routes are accessible locally via `http://localhost:9000` or your LAN IP `http://<YOUR_IP>:9000`.

| Route                | Method | Description                                                                                        |
| -------------------- | ------ | -------------------------------------------------------------------------------------------------- |
| `/`                  | GET    | Serves the main web interface for text and file sharing.                                           |
| `/ws`                | GET    | WebSocket endpoint for **real-time text messages**. All connected clients sync instantly.          |
| `/upload`            | POST   | Upload a file to the server. The uploaded file will appear in the file list for all clients.       |
| `/files`             | GET    | Returns the list of **all uploaded files** with countdown timers (remaining time before deletion). |
| `/delete`            | POST   | Delete a specific file from the server. The deletion is **synced across all clients**.             |
| `/files/<filename>`  | GET    | Download a shared file by its name.                                                                |
| `/static/<filepath>` | GET    | Serves static assets (CSS, JS, images) used by the web interface.                                  |

---

### Example Usage

**Upload a file:**

```bash
curl -X POST -F "file=@example.txt" http://localhost:9000/upload
```

**Delete a file:**

```bash
curl -X POST -d "filename=example.txt" http://localhost:9000/delete
```

**List files:**

```bash
curl http://localhost:9000/files
```

---

### Info for API Routes

* All routes are **local**, so only devices on your **LAN** can access them.
* `/ws` is used internally by the web interface for **real-time syncing** but can also be accessed by custom clients.
* File uploads are **temporary**: each file has a countdown timer and is **automatically deleted** after expiry.
* `/files` and `/delete` endpoints **sync changes across all connected clients in real-time**.

---

## Notes

* The `uploads/` folder stores temporary files. Files expire automatically based on TTL.
* Manual deletion of files also syncs immediately across all connected clients.
* The watermark shows the developer name and GitHub username for attribution.
* The server runs on **port 9000** by default.