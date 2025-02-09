# ChronoWall
A simple script to automatically change wallpapers based on the time of day. Supports multiple OS with customizable themes for morning, afternoon, evening, and night


## Features
- Automatically updates wallpaper at predefined times.
- Works on macOS, Linux, and Windows.
- Customizable time ranges and wallpapers.

---

## Installation and Setup

### macOS
#### 1. Create the script
Create a script file at `~/wallpaper_changer.sh`:

```bash
#!/bin/zsh
hour=$(date +%H)
case $hour in
  05|06|07|08) wallpaper="dawn.jpg" ;;
  09|10|11)    wallpaper="morning.jpg" ;;
  12|13|14|15) wallpaper="afternoon.jpg" ;;
  16|17|18)    wallpaper="dusk.jpg" ;;
  19|20|21|22) wallpaper="night.jpg" ;;
  *)           wallpaper="midnight.jpg" ;;
esac

osascript -e "tell application \"System Events\" to tell every desktop to set picture to \"/Users/YOUR_USERNAME/Pictures/Wallpaper/$wallpaper\""
```

Replace `YOUR_USERNAME` with your actual macOS username.

Make the script executable:
```bash
chmod +x ~/wallpaper_changer.sh
```

#### 2. Automate with `launchctl`
Create a plist file at `~/Library/LaunchAgents/local.wallpaper.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>local.wallpaper</string>
    <key>ProgramArguments</key>
    <array>
        <string>/bin/zsh</string>
        <string>-c</string>
        <string>/Users/YOUR_USERNAME/wallpaper_changer.sh</string>
    </array>
    <key>StartCalendarInterval</key>
    <array>
        <dict>
            <key>Minute</key>
            <integer>0</integer>  <!-- Runs every hour at :00 -->
        </dict>
    </array>
</dict>
</plist>
```

Load the agent:
```bash
launchctl load ~/Library/LaunchAgents/local.wallpaper.plist
```
Check if it's running:
```bash
launchctl list | grep local.wallpaper
```

---

### Linux
#### 1. Create the script
Create a file at `~/wallpaper_changer.sh`:
```bash
#!/bin/bash
hour=$(date +%H)
case $hour in
  05|06|07|08) wallpaper="dawn.jpg" ;;
  09|10|11)    wallpaper="morning.jpg" ;;
  12|13|14|15) wallpaper="afternoon.jpg" ;;
  16|17|18)    wallpaper="dusk.jpg" ;;
  19|20|21|22) wallpaper="night.jpg" ;;
  *)           wallpaper="midnight.jpg" ;;
esac

gsettings set org.gnome.desktop.background picture-uri "file:///home/YOUR_USERNAME/Pictures/Wallpaper/$wallpaper"
```

Make it executable:
```bash
chmod +x ~/wallpaper_changer.sh
```

#### 2. Automate with `cron`
Open the crontab:
```bash
crontab -e
```
Add the following line to run the script every hour:
```bash
0 * * * * /home/YOUR_USERNAME/wallpaper_changer.sh
```
Save and exit.

---

### Windows
#### 1. Create the script
Create a PowerShell script `C:\Users\YOUR_USERNAME\wallpaper_changer.ps1`:
```powershell
$hour = (Get-Date).Hour
$wallpaperPath = "C:\Users\YOUR_USERNAME\Pictures\Wallpaper\"

switch ($hour) {
    {$_ -in 5..8}  { $wallpaper = "dawn.jpg" }
    {$_ -in 9..11} { $wallpaper = "morning.jpg" }
    {$_ -in 12..15} { $wallpaper = "afternoon.jpg" }
    {$_ -in 16..18} { $wallpaper = "dusk.jpg" }
    {$_ -in 19..22} { $wallpaper = "night.jpg" }
    default { $wallpaper = "midnight.jpg" }
}

$fullPath = "$wallpaperPath$wallpaper"
SystemParametersInfo(20, 0, $fullPath, 3)
```

#### 2. Automate with Task Scheduler
1. Open **Task Scheduler** and click **Create Basic Task**.
2. Name it "WallpaperChanger" and click **Next**.
3. Select **Daily**, click **Next**, and set it to run every hour.
4. Choose **Start a Program**.
5. Set **Program/script** to `powershell.exe`.
6. In **Add arguments**, enter:
   ```powershell
   -ExecutionPolicy Bypass -File "C:\Users\YOUR_USERNAME\wallpaper_changer.ps1"
   ```
7. Click **Finish**.

Your wallpaper will now change every hour based on the time of day.

---

## Customization
- Modify time ranges and image filenames in the scripts.
- Place your images in the specified directory (`~/Pictures/Wallpaper/` or `C:\Users\YOUR_USERNAME\Pictures\Wallpaper\`).

Free to use and modify!


