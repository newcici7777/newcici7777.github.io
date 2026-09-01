---
title: livekit
date: 2026-09-01
keywords: livekit 
---
```
brew install uv
source $HOME/.local/bin/env
uv --version
```

```
uv python install 3.11
rm -rf .venv
uv venv --python 3.11
uv add "livekit-agents[silero,turn-detector]~=1.3" "livekit-plugins-noise-cancellation~=0.2" "python-dotenv"
```

進入Livekit cloud  
Livekit cloud: <https://cloud.livekit.io/projects/p_tn5apg35ao3/overview>

進入Settings > API keys

![img]({{site.imgurl}}/livekit/livekit1.png)<br>

建立專案名稱

複製API KEY<br>
![img]({{site.imgurl}}/livekit/livekit2.png)<br>

建立.env檔案，並把上方的API KEY 內容貼入
![img]({{site.imgurl}}/livekit/livekit3.png)<br>
