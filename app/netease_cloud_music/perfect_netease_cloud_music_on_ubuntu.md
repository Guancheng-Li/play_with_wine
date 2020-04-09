# Perfect Netease Cloud Music on Ubuntu

## Why not Cloud Music for Linux?
1. Bugs like cannot close, cannot launch, cannot start from paused point etc.;
2. No new features (some for VIP);
3. Update slow;
4. Latest deb need other dependencies;

## Why not Docker or AppImage
1. Need to download and setup, need to decide path and remote path may change;
2. Docker cannot support the icon on the task bar;
3. AppImage need dependencies;

## Install steps
### Install deepin wine (If wechat enterprise deepin installed, skip this step)
```
mkdir -p /tmp/cloud_music_wine
wget ftp://10.5.0.9/guangzhou_local/soft/wine/deepin-wine-ubuntu.tar.gz -O /tmp/cloud_music_wine/deepin-wine-ubuntu.tar.gz
tar -xf deepin-wine-ubuntu.tar.gz
cd deepin-wine-ubuntu
sudo ./install.sh
```

### Create a separated wine environment for cloud music
```
mkdir -p ~/.wine_cloud_music
WINEPREFIX=~/.wine_cloud_music deepin-wine winecfg
```
Please set the environment as `windowns xp`, or the display may not correct.

### Install cloud music
```
cd /tmp/cloud_music_wine
wget https://d1.music.126.net/dmusic/cloudmusicsetup2.7.1.198242.exe -O /tmp/cloud_music_wine/cloud_music_install.exe
WINEPREFIX=~/.wine_cloud_music deepin-wine ./cloud_music_install.exe
```
Please uncheck all checkboxes(except the aggrement) and prefer to install at `c:\\app\cloud_music`


### Set running environment (optional)
```
WINEPREFIX=~/.wine_cloud_music deepin-wine winecfg
```
Add `c:\\app\cloud_music\cloudmusic.exe`;
Set running environment as `windows xp` (Can skip if the default is `XP`)

### Add a script to launch
```
touch /opt/wine/apps/CloudMusic/run.sh
```
Add following content:
```
WINEPREFIX=~/.wine_cloud_music LC_ALL=zh_CN.UTF-8 deepin-wine ~/.wine_cloud_music/drive_c/app/CloudMusic/cloudmusic.exe
```

### Add commands
1. Alias
Add `alias cloudmusic="nohup /opt/wine/apps/CloudMusic/run.sh > /tmp/cloudmusic.wine.log 2>&1 &"` in `~/.bash_alias`
`source ~/.bash_rc`
2. Add `Alter + F2, insert "cloudmusic"` (Optional)
`sudo ln -s /opt/wine/apps/CloudMusic/run.sh /usr/bin/cloudmusic`

### Add Icon in StartMenu
`sudo touch /usr/share/applications/CloudMusic.desktop`
Add content below:
```
[Desktop Entry]
Encoding=UTF-8
Exec=cloudmusic
Name=CloudMusic
Name[zh_CN]=网易云音乐
Comment=Netease Cloud Music
Comment[zh_CN.UTF-8]=网易云音乐
Icon=/opt/wine/apps/CloudMusic/icon.png
Terminal=false
Type=Application
Categories=Application;Media;Music;
```
Download a icon from web and mv to `/opt/wine/apps/CloudMusic/icon.png`

### CloudMusic Settings
Open cloud music, go to the setting page:
1. Check `禁用动画效果`;
2. Check `禁用GPU加速`; (May cause display not correct)
3. Check `禁用系统缩放比`;
4. Choose `有新版本时提醒我`;

### Font display not correct
1. Install fonts like `微软雅黑（msyh）` or `宋体（simsum）` accroding to 
http://www.mustenaka.cn/index.php/2019/07/16/elementaryos5/

Install reg file you may need to try:
```
regidit font.reg
wine regedit font.reg
deepin-wine regedit font.reg
WINEPREFIX=~/.wine_cloud_music regidit font.reg
WINEPREFIX=~/.wine_cloud_music wine regedit font.reg
WINEPREFIX=~/.wine_cloud_music deepin-wine regedit font.reg
```
Sometimes not successful because you did not do the 2nd item.
The font will be able selected when setting is correct in cloud music.

2. Add environment `LC_ALL=zh_CN.UTF-8` when run the exe (Very important)
