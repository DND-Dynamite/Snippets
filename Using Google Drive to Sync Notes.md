Great choice! You can **automate syncing your Obsidian vault using a cloud provider** (like Google Drive or Dropbox) with a **CLI-based approach on Windows**. Here's a guide to automate it using **Google Drive** with **rclone**, which is a powerful command-line sync tool.

---

## ✅ **Step-by-Step: Automate Obsidian Sync via CLI on Windows (Google Drive + rclone)**

---

### 🛠️ **Tools Needed**

- **[rclone](https://rclone.org/downloads/)** – a command-line tool to sync files with many cloud providers
    
- Optional: **Task Scheduler** (to run the sync on a schedule)
    

---

### 🔧 Step 1: Install rclone

1. Download and install rclone from: [https://rclone.org/downloads/](https://rclone.org/downloads/)
    
2. Open **Command Prompt** or **PowerShell**.
    
3. Run the config command:
    
    ```sh
    rclone config
    ```
    

---

### 🔧 Step 2: Configure Google Drive Remote

Follow the prompts:

1. Type `n` to create a new remote.
    
2. Name it something like `gdrive`.
    
3. Choose `13` for **Google Drive**.
    
4. Follow the OAuth prompts (it will open your browser).
    
5. Allow rclone access to your Google account.
    
6. Finish config with default settings.
    

📂 This will let you sync to a folder like `gdrive:/ObsidianVault`.

---

### 🗂️ Step 3: Organize Your Vault

Let’s say your local vault is in:

```plaintext
C:\Users\Aniruddha\Documents\Obsidian
```

And you want to sync it to Google Drive at:

```plaintext
gdrive:/ObsidianVault
```

---

### 🔁 Step 4: Create a Sync Script

Open Notepad, and save this as a `.bat` file, like `sync_obsidian.bat`:

```bat
@echo off
rclone sync "C:\Users\Aniruddha\Documents\Obsidian" gdrive:/ObsidianVault --progress
```

You can also add `--dry-run` first to test it safely.

> 🧠 You can reverse the direction if you want to pull from cloud:

```bat
rclone sync gdrive:/ObsidianVault "C:\Users\Aniruddha\Documents\Obsidian" --progress
```

---

### ⏰ Step 5: Automate with Task Scheduler (Optional)

1. Open **Task Scheduler**.
    
2. Create a new task.
    
3. Under the **Action** tab, add:
    
    - **Action**: Start a program
        
    - **Program/script**: `sync_obsidian.bat` (full path)
        
4. Set a **Trigger** (e.g., every 15 minutes or at logon).
    
5. Save the task.
    

Now it syncs automatically!

---

### ✅ Access from Mobile

- Install **Google Drive app**.
    
- Use **a local sync app** like **FolderSync (Android)** or **FileBrowser Pro (iOS)** to mirror `ObsidianVault` to local storage.
    
- Point the **Obsidian mobile app** to the local synced folder.
    

---

## 🚀 Bonus: Two-Way Sync

rclone doesn’t support two-way sync **natively**, but you can:

- Run two syncs (push + pull) OR
    
- Use `rclone bisync` (experimental):
    
    ```sh
    rclone bisync "C:\Users\Aniruddha\Documents\Obsidian" gdrive:/ObsidianVault
    ```
    

---

## ✅ Summary

| Step | Description                            |
| ---- | -------------------------------------- |
| 1    | Install `rclone`                       |
| 2    | Configure Google Drive remote          |
| 3    | Create sync `.bat` script              |
| 4    | Run it manually or with Task Scheduler |
| 5    | Access synced files from mobile        |

---





