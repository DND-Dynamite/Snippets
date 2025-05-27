- windows [youtube downloader][https://www.youtubedownloaderhd.com/download.html]
- ubuntu

You can use `yt-dlp` to download a YouTube playlist on Ubuntu by following these steps:

### **1. Install `yt-dlp`**

First, install `yt-dlp` if you haven't already:

```bash
sudo apt update && sudo apt install -y yt-dlp
```

If you want the latest version:

```bash
sudo apt install curl -y
sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp
```

### **2. Download a YouTube Playlist**

Run the following command, replacing `PLAYLIST_URL` with your playlist link:

```bash
yt-dlp -f bestvideo+bestaudio --merge-output-format mp4 PLAYLIST_URL
```

### **3. Download with Audio & Video Merged**

If you want only audio:

```bash
yt-dlp -f bestaudio --extract-audio --audio-format mp3 PLAYLIST_URL
```

### **4. Download with Filename Formatting**

To name files properly:

```bash
yt-dlp -o "%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s" PLAYLIST_URL
```

### **5. Resume or Update Downloads**

To resume an interrupted download:

```bash
yt-dlp -c PLAYLIST_URL
```

To update `yt-dlp`:

```bash
yt-dlp -U
```

### **6. Use clipgrab app**

Check for the same in ubuntu

