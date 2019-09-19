## WSL/Ubuntu Profile

- Adjust profiles.json, add to 'profiles' array:

```javascript
{
  "guid": "{78e390db-1bff-4533-9d7c-20f53d8bafa1}",
  "name": "WSL",
  "colorscheme": "Campbell",
  "historySize": 9001,
  "icon": "ms-appdata:///roaming/ubuntu_32px.png",
  "snapOnInput": true,
  "cursorColor": "#FFFFFF",
  "cursorShape": "bar",
  "commandline": "ubuntu",
  "fontFace": "Ubuntu Mono",
  "fontSize": 12,
  "acrylicOpacity": 0.95,
  "useAcrylic": true,
  "closeOnExit": true,
  "padding": "0, 0, 0, 0"
}
```

- Download icon(s) from https://github.com/yanglr/WindowsDevTools/tree/master/awosomeTerminal/icons
- Save icon(s) in *%LOCALAPPDATA%\packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\RoamingState*

### Sources

https://stackoverflow.com/questions/56765067/how-do-i-get-windows-10-terminal-to-launch-wsl
