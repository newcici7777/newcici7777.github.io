---
title: Avatar
date: 2026-09-01
keywords: livekit, avatar
---

#### 1. 進入livekit: <https://livekit.com/>

#### 2. 根據以下的圖片步驟，建立自己的專案，因為我有建立過，所以會直接登入，畫面跟第一次建立的人不一樣

![img]({{site.imgurl}}/livekit/livekit_cloud1.png)<br>

![img]({{site.imgurl}}/livekit/livekit_cloud2.png)<br>

![img]({{site.imgurl}}/livekit/livekit_cloud3.png)<br>

![img]({{site.imgurl}}/livekit/livekit_cloud4.png)<br>

#### 3. 建立一個目錄，用VSCode

#### 4. 在VSCode終端機輸入
```
% lk cloud auth
```

#### 5. 輸入剛才自己建的Project名`avatar_flutter`，如果只有一個Project，就直接Enter  

#### 6. 跳出視窗
![img]({{site.imgurl}}/livekit/livekit_cloud5.png)<br>

![img]({{site.imgurl}}/livekit/livekit_cloud6.png)<br>

![img]({{site.imgurl}}/livekit/livekit_cloud7.png)<br>

#### 7. VSCode終端機輸入
```
% lk app create
```
預設是Python
![img]({{site.imgurl}}/livekit/livekit_cloud8.png)<br>

我自己選Flutter
![img]({{site.imgurl}}/livekit/livekit_cloud9.png)<br>


#### 8. 輸入api key

Open AI :<https://platform.openai.com/login?next=%2Fapi-keys>

```
cd Test-app
uv sync
```

----------------------

```
lk app create
cd avatar-flutter-backen
uv sync
lk agent create
Yes  
us-east   
Yes 
```

simli: <https://docs.livekit.io/agents/models/avatar/plugins/simli/>

LiveKit DOCS > Build Agents > MODELS > simli

```
uv add "livekit-agents[simli]~=1.8"
uv sync
```

在src目錄下，有一個agent.py，尋找`from livekit.plugins`，在最後面，增加simli
```
from livekit.plugins import ai_coustics, simli
```

simli.com : <https://www.simli.com/>
1. Create Avatar
2. 拷貝Face ID

![img]({{site.imgurl}}/livekit/simli1.png)<br>

3. 拷貝API_KEY

![img]({{site.imgurl}}/livekit/simli2.png)<br>

修改`src/agent.py`的檔案內容。
介於`session = AgentSession...`與`await session.start`，貼上以下的程式碼，並且把剛才取得的faceID與api_key貼上
```
   session = AgentSession(
      # ... stt, llm, tts, etc.
   )
   # ------------------
   # 貼上以下內容
   avatar = simli.AvatarSession(
      simli_config=simli.SimliConfig(
         api_key="xxxx", # API Key
         face_id="...",  # Face ID
      ),
   )

   # Start the avatar and wait for it to join
   await avatar.start(session, room=ctx.room)

   # 結束
   # -----------------------
   await session.start(
      # ... room, agent, room_options, etc....
   )
```

在終端機執行
```
uv run src/agent.py download-files
uv run src/agent.py dev
```



到livekit console來測試

<https://cloud.livekit.io/projects/p_699b217dhjz/overview>

![img]({{site.imgurl}}/livekit/simli3.png)<br>

接下來到

```
lk agent deploy
```