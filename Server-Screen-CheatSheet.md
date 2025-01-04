# **Server Connection Guide**

### **Apps to Use**
- **PuTTY**  
- **MobaXterm**

---

## **Password-Based Authentication**
```bash
ssh username@server_ip
```

---

# **`screen` Command Cheatsheet**

`screen` is a tool to manage multiple terminal sessions from one window.

---

## **Getting Started**

### Install `screen`:
```bash
sudo apt update && sudo apt install screen
```

### Start a New Session:
```bash
screen -S session_name
```

---

## **Manage Sessions**

### Detach:
```bash
Ctrl + A, D
```

### List Sessions:
```bash
screen -ls
```

### Reattach:
```bash
screen -r session_name
```

### Force Reattach:
```bash
screen -d -r session_name
```

---

## **Inside a Session**

- **New Window:** `Ctrl + A, C`  
- **Next Window:** `Ctrl + A, N`  
- **Previous Window:** `Ctrl + A, P`  
- **List Windows:** `Ctrl + A, "`  
- **Split Screen:**  
  - Horizontally: `Ctrl + A, S`  
  - Vertically: `Ctrl + A, |`  
  - Switch Pane: `Ctrl + A, Tab`  
- **Close Window:** `Ctrl + A, K` → Confirm with `Y`

---

## **Exit or Kill a Session**

- **Exit Session (While Attached):**  
  ```bash
  exit
  ```
- **Kill Detached Session:**  
  ```bash
  screen -X -S session_name quit
  ```

---

## **Quick Tips**

- **Start in Detached Mode:**  
  ```bash
  screen -dmS session_name
  ```
- **Run Command in New Session:**  
  ```bash
  screen -S session_name -X command
  ```

---

## **Example Workflow**

1. Start a session:  
   ```bash
   screen -S my_session
   ```
2. Run a command:  
   ```bash
   long_running_command
   ```
3. Detach:  
   `Ctrl + A, D`  
4. Reattach later:  
   ```bash
   screen -r my_session
   
