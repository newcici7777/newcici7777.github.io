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
uv add "livekit-plugins-openai"
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

```
uv run agent.py download-files
```

```
uv run agent.py console
```
{% highlight python linenos %}
import logging

from dotenv import load_dotenv
from livekit import agents
from livekit.agents import Agent, AgentServer, AgentSession, JobContext, room_io
#from livekit.plugins import noise_cancellation, silero

load_dotenv()


# Define your agent's behavior by extending the Agent class
class Assistant(Agent):
    def __init__(self) -> None:
        super().__init__(
            instructions="You are a helpful voice AI assistant.",  # System prompt for the LLM
        )


server = AgentServer()


# The entrypoint function runs when a participant joins the room
@server.rtc_session()
async def entrypoint(ctx: JobContext):
    # Configure the voice pipeline with STT, LLM, TTS, and VAD providers
    session = AgentSession(
        stt="assemblyai/universal-streaming:en",  # Speech-to-text provider
        llm="openai/gpt-4.1-mini",                # Language model for responses
        tts="cartesia/sonic-3",                   # Text-to-speech voice
        #vad=silero.VAD.load(),                    # Voice activity detection
    )

    # Start the session with noise cancellation enabled
    await session.start(
        agent=Assistant(),
        room=ctx.room,
        room_options=room_io.RoomOptions(
            audio_input=room_io.AudioInputOptions(
#                noise_cancellation=noise_cancellation.BVC(),  # Background voice cancellation
            ),
        ),
    )


if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    agents.cli.run_app(server)

{% endhighlight %}

----------------------------------

backup tts
{% highlight python linenos %}
import logging

from dotenv import load_dotenv
from livekit import agents
from livekit.agents import Agent, AgentServer, AgentSession, JobContext, room_io
#from livekit.plugins import noise_cancellation, silero
from livekit.agents import llm, stt, tts, inference

load_dotenv()


# Define your agent's behavior by extending the Agent class
class Assistant(Agent):
    def __init__(self) -> None:
        super().__init__(
            instructions="You are a helpful voice AI assistant, keep replies under 3 sentences.",  # System prompt for the LLM
        )


server = AgentServer()


# The entrypoint function runs when a participant joins the room
@server.rtc_session()
async def entrypoint(ctx: JobContext):
    # Configure the voice pipeline with STT, LLM, TTS, and VAD providers
    # session = AgentSession(
    #     stt="assemblyai/universal-streaming:en",  # Speech-to-text provider
    #     llm="openai/gpt-4.1-mini",                # Language model for responses
    #     tts="cartesia/sonic-3",                   # Text-to-speech voice
    #     #vad=silero.VAD.load(),                    # Voice activity detection
    # )
    session = AgentSession(
        # LLM with fallback: OpenAI primary, Gemini backup
        llm=llm.FallbackAdapter(
            [
                inference.LLM(model="openai/gpt-4.1-mini"),
                inference.LLM(model="google/gemini-2.5-flash"),
            ]
        ),
        # STT with fallback: AssemblyAI primary, Deepgram backup
        stt=stt.FallbackAdapter(
            [
                inference.STT.from_model_string("assemblyai/universal-streaming:en"),
                inference.STT.from_model_string("deepgram/nova-3"),
            ]
        ),
        # TTS with fallback: Cartesia primary, Inworld backup
        tts=tts.FallbackAdapter(
            [
                inference.TTS.from_model_string("cartesia/sonic-3:9626c31c-bec5-4cca-baa8-f8ba9e84c8bc"),
                inference.TTS.from_model_string("inworld/inworld-tts-1"),
            ]
        ),
        #vad=vad,
        #turn_detection=MultilingualModel(),
    )


    # Start the session with noise cancellation enabled
    await session.start(
        agent=Assistant(),
        room=ctx.room,
        room_options=room_io.RoomOptions(
            audio_input=room_io.AudioInputOptions(
#                noise_cancellation=noise_cancellation.BVC(),  # Background voice cancellation
            ),
        ),
    )


if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    agents.cli.run_app(server)

{% endhighlight %}

-------------------------------
加上log

{% highlight python linenos %}
import logging

from dotenv import load_dotenv
from livekit import agents
from livekit.agents import Agent, AgentServer, AgentSession, JobContext, room_io
#from livekit.plugins import noise_cancellation, silero
from livekit.agents import llm, stt, tts, inference
from livekit.agents import AgentStateChangedEvent, MetricsCollectedEvent, metrics
import time
logger = logging.getLogger(__name__)
load_dotenv()


# Define your agent's behavior by extending the Agent class
class Assistant(Agent):
    def __init__(self) -> None:
        super().__init__(
            instructions="You are a helpful voice AI assistant, keep replies under 3 sentences.",  # System prompt for the LLM
        )


server = AgentServer()


# The entrypoint function runs when a participant joins the room
@server.rtc_session()
async def entrypoint(ctx: JobContext):
    # Configure the voice pipeline with STT, LLM, TTS, and VAD providers
    # session = AgentSession(
    #     stt="assemblyai/universal-streaming:en",  # Speech-to-text provider
    #     llm="openai/gpt-4.1-mini",                # Language model for responses
    #     tts="cartesia/sonic-3",                   # Text-to-speech voice
    #     #vad=silero.VAD.load(),                    # Voice activity detection
    # )
    session = AgentSession(
        # LLM with fallback: OpenAI primary, Gemini backup
        llm=llm.FallbackAdapter(
            [
                inference.LLM(model="openai/gpt-4.1-mini"),
                inference.LLM(model="google/gemini-2.5-flash"),
            ]
        ),
        # STT with fallback: AssemblyAI primary, Deepgram backup
        stt=stt.FallbackAdapter(
            [
                inference.STT.from_model_string("assemblyai/universal-streaming:en"),
                inference.STT.from_model_string("deepgram/nova-3"),
            ]
        ),
        # TTS with fallback: Cartesia primary, Inworld backup
        tts=tts.FallbackAdapter(
            [
                inference.TTS.from_model_string("cartesia/sonic-3:9626c31c-bec5-4cca-baa8-f8ba9e84c8bc"),
                inference.TTS.from_model_string("inworld/inworld-tts-1"),
            ]
        ),
        #vad=vad,
        #turn_detection=MultilingualModel(),
        preemptive_generation=True,
    )
    #####################################
    # Aggregate data across all conversation turns
    usage_collector = metrics.UsageCollector()

    # Track End of Utterance timing (when turn detector decides user finished speaking)
    last_eou_metrics: metrics.EOUMetrics | None = None

    @session.on("metrics_collected")
    def _on_metrics_collected(ev: MetricsCollectedEvent):
        nonlocal last_eou_metrics
        # Capture EOU metrics for TTFA calculation
        if ev.metrics.type == "eou_metrics":
            last_eou_metrics = ev.metrics

        # Log each metric as it arrives and add to usage collector
        metrics.log_metrics(ev.metrics)
        usage_collector.collect(ev.metrics)


    async def log_usage():
        # Print per-session summary (tokens, audio duration, costs)
        summary = usage_collector.get_summary()
        logger.info("Usage summary: %s", summary)


    # Fire log_usage when worker shuts down
    ctx.add_shutdown_callback(log_usage)

    @session.on("agent_state_changed")
    def _on_agent_state_changed(ev: AgentStateChangedEvent):
        if ev.new_state == "speaking":
            if last_eou_metrics:
                # Calculate time since user finished speaking
                elapsed = time.time() - last_eou_metrics.timestamp
                logger.info(f"Time to first audio: {elapsed:.3f}s")

    #######################################
    # Start the session with noise cancellation enabled
    await session.start(
        agent=Assistant(),
        room=ctx.room,
        room_options=room_io.RoomOptions(
            audio_input=room_io.AudioInputOptions(
#                noise_cancellation=noise_cancellation.BVC(),  # Background voice cancellation
            ),
        ),
    )


if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    agents.cli.run_app(server)

{% endhighlight %}


-----------------------------

以下是不知何時冒出來的內容

步驟一：打開另一個終端機或 VS Code 視窗，啟動你的 Python Agent，讓它上線等待。

步驟二：回到你目前的 Flutter 專案，執行 flutter run 把前端介面打開。

步驟三：當你在 Flutter App 點擊連線或進入房間時，Python Agent 就會被觸發並加入同一個房間，開始和你對話並傳送 Avatar 畫面！


```
/etc/zshrc:7: command not found: locale
cici@liyutingdeMacBook-Pro livekit-flutter % lk cloud auth
Saved CLI config to [/Users/cici/.livekit/cli-config.yaml]
Device [avatar_flutter]
Requesting verification token...
Please confirm access by visiting:

   https://cloud.livekit.io/cli/confirm-auth?t=69b68b26-6e96-473b-b7ad-a93ba99f9ec4
Authenticated project [avatar_flutter]
Saved CLI config to [/Users/cici/.livekit/cli-config.yaml]
cici@liyutingdeMacBook-Pro livekit-flutter % lk app create
Using project [avatar-flutter]
Cloning template...
Instantiating environment...
Installing template...
⚠  Installation failed — dependencies were NOT installed
┃ task "install" not found
┃ Fix your toolchain, then re-run the install step manually in ./avatar-flutter.
Cleaning up...

Your Flutter voice assistant is ready to go!

To give it a try:
    cd /Users/cici/Desktop/Livekit/livekit-flutter/avatar-flutter
    flutter pub get
    flutter run

For more help view your project's README.md file or join our Slack community at https://livekit.io/join-slack.
cici@liyutingdeMacBook-Pro livekit-flutter % ls
avatar-flutter
cici@liyutingdeMacBook-Pro livekit-flutter % cd avatar-flutter 
cici@liyutingdeMacBook-Pro avatar-flutter % ls
README.md               devtools_options.yaml   pubspec.yaml
analysis_options.yaml   ios                     test
android                 lib                     web
assets                  macos
cici@liyutingdeMacBook-Pro avatar-flutter % flutter pub get
Resolving dependencies... 
The current Dart SDK version is 3.2.6.

Because voice_assistant requires SDK version ^3.10.0, version solving failed.
cici@liyutingdeMacBook-Pro avatar-flutter % flutter upgrade
Unable to upgrade Flutter: Your Flutter checkout is currently not on a release branch.
Use "flutter channel" to switch to an official channel, and retry. Alternatively,
re-install Flutter by going to https://flutter.dev/docs/get-started/install.
cici@liyutingdeMacBook-Pro avatar-flutter % flutter --version
Flutter 3.16.9 • channel [user-branch] • unknown source
Framework • revision 41456452f2 (2 years, 8 months ago) • 2024-01-25 10:06:23 -0800
Engine • revision f40e976bed
Tools • Dart 3.2.6 • DevTools 2.28.5
cici@liyutingdeMacBook-Pro avatar-flutter % flutter pub get
Downloading package sky_engine...                                1,595ms
Downloading flutter_patched_sdk tools...                         1,817ms
Downloading flutter_patched_sdk_product tools...                 1,880ms
Downloading darwin-x64 tools...                                    16.2s
Downloading darwin-x64/font-subset tools...                      1,511ms
Resolving dependencies... 
The current Dart SDK version is 3.4.0.

Because voice_assistant requires SDK version ^3.10.0, version solving failed.
cici@liyutingdeMacBook-Pro avatar-flutter % flutter pub get
Resolving dependencies... 
The current Flutter SDK version is 3.22.0.

Because voice_assistant requires Flutter SDK version
  >=3.38.0, version solving failed.
cici@liyutingdeMacBook-Pro avatar-flutter % flutter pub get
Resolving dependencies... (1.0s)
The current Dart SDK version is 3.4.0.

Because voice_assistant depends on flutter_lints >=5.0.0
  which requires SDK version >=3.5.0 <4.0.0, version solving
  failed.
cici@liyutingdeMacBook-Pro avatar-flutter % flutter pub get
Resolving dependencies... (1.1s)
The current Dart SDK version is 3.4.0.

Because voice_assistant depends on livekit_components >=1.0.1
  which requires SDK version >=3.5.1 <4.0.0, version solving
  failed.
cici@liyutingdeMacBook-Pro avatar-flutter % flutter pub get
Resolving dependencies... (1.4s)
The current Dart SDK version is 3.4.0.

Because voice_assistant depends on livekit_components >=1.0.1
  which requires SDK version >=3.5.1 <4.0.0, version solving
  failed.
cici@liyutingdeMacBook-Pro avatar-flutter % flutter pub get
Error detected in pubspec.yaml:
Error on line 42, column 3: Duplicate mapping key.
   ╷
42 │   livekit_client: ^2.5.0
   │   ^^^^^^^^^^^^^^
   ╵
Please correct the pubspec.yaml file at
/Users/cici/Desktop/Livekit/livekit-flutter/avatar-flutter/pubs
pec.yaml
cici@liyutingdeMacBook-Pro avatar-flutter % flutter pub get
Resolving dependencies... (1.2s)
The current Dart SDK version is 3.4.0.

Because voice_assistant depends on livekit_client >=2.4.1
  which requires SDK version >=3.6.0 <4.0.0, version solving
  failed.


You can try the following suggestion to make the pubspec resolve:
* Consider downgrading your constraint on livekit_client: flutter pub add livekit_client:^2.3.6
cici@liyutingdeMacBook-Pro avatar-flutter % flutter pub get
Resolving dependencies... (1.6s)
Downloading packages... (25.6s)
+ args 2.7.0
+ async 2.11.0 (2.13.1 available)
+ boolean_selector 2.1.1 (2.1.2 available)
+ characters 1.3.0 (1.4.1 available)
+ chat_bubbles 1.9.2 (1.10.1 available)
+ clock 1.1.1 (1.1.3 available)
+ collection 1.18.0 (1.19.1 available)
+ connectivity_plus 6.1.5 (7.3.1 available)
+ connectivity_plus_platform_interface 2.1.0
+ crypto 3.0.7
+ cupertino_icons 1.0.8 (1.0.9 available)
+ dart_webrtc 1.8.2
+ dbus 0.7.12 (0.8.0 available)
+ device_info_plus 11.3.0 (13.2.0 available)
+ device_info_plus_platform_interface 7.0.2 (8.1.0 available)
+ fake_async 1.3.1 (1.3.3 available)
+ ffi 2.1.3 (2.2.0 available)
+ file 7.0.1
+ fixnum 1.1.1
+ flutter 0.0.0 from sdk flutter
+ flutter_dotenv 6.0.1
+ flutter_lints 4.0.0 (6.0.0 available)
+ flutter_sficon 1.3.0
+ flutter_test 0.0.0 from sdk flutter
+ flutter_web_plugins 0.0.0 from sdk flutter
+ flutter_webrtc 0.12.12+hotfix.1 (1.6.2+hotfix.3 available)
+ http 1.6.0
+ http_parser 4.0.2 (4.1.2 available)
+ intl 0.20.2 (0.20.3 available)
+ js 0.7.1 (0.7.2 available)
+ leak_tracker 10.0.4 (11.0.2 available)
+ leak_tracker_flutter_testing 3.0.3 (3.0.10 available)
+ leak_tracker_testing 3.0.1 (3.0.2 available)
+ lints 4.0.0 (6.1.0 available)
+ livekit_client 2.3.6 (2.13.0 available)
+ logging 1.3.0
+ matcher 0.12.16+1 (0.12.20 available)
+ material_color_utilities 0.8.0 (0.13.1 available)
+ meta 1.12.0 (1.19.0 available)
+ nested 1.0.0
+ nm 0.5.0 (0.6.0 available)
+ path 1.9.0 (1.9.1 available)
+ path_provider 2.1.5 (2.1.6 available)
+ path_provider_android 2.2.10 (2.3.1 available)
+ path_provider_foundation 2.4.1 (2.6.0 available)
+ path_provider_linux 2.2.1 (2.2.2 available)
+ path_provider_platform_interface 2.1.2 (2.1.3 available)
+ path_provider_windows 2.3.0
+ petitparser 6.0.2 (7.0.2 available)
+ platform 3.1.6 (3.2.0 available)
+ platform_detect 2.1.0 (2.1.6 available)
+ plugin_platform_interface 2.1.8
+ protobuf 3.1.0 (6.1.0 available)
+ provider 6.1.5+1
+ pub_semver 2.2.1
+ sdp_transform 0.3.2
+ shimmer 3.0.0 (4.0.0 available)
+ sky_engine 0.0.99 from sdk flutter
+ source_span 1.10.0 (1.10.2 available)
+ stack_trace 1.11.1 (1.12.2 available)
+ stream_channel 2.1.2 (2.1.4 available)
+ string_scanner 1.2.0 (1.4.1 available)
+ synchronized 3.1.0+1 (3.4.2 available)
+ term_glyph 1.2.1 (1.2.2 available)
+ test_api 0.7.0 (0.7.14 available)
+ typed_data 1.3.2 (1.4.0 available)
+ url_launcher 6.3.1 (6.3.2 available)
+ url_launcher_android 6.3.9 (6.3.33 available)
+ url_launcher_ios 6.3.3 (6.4.2 available)
+ url_launcher_linux 3.2.1 (3.2.3 available)
+ url_launcher_macos 3.2.2 (3.2.6 available)
+ url_launcher_platform_interface 2.3.2
+ url_launcher_web 2.3.3 (2.4.3 available)
+ url_launcher_windows 3.1.4 (3.1.6 available)
+ uuid 4.6.0
+ vector_math 2.1.4 (2.4.3 available)
+ vm_service 14.2.1 (15.3.0 available)
+ web 1.1.1
+ webrtc_interface 1.5.1
+ win32 5.5.4 (6.4.0 available)
+ win32_registry 1.1.5 (3.0.3 available)
+ xdg_directories 1.1.0
+ xml 6.5.0 (7.0.1 available)
Changed 83 dependencies!
58 packages have newer versions incompatible with dependency constraints.
Try `flutter pub outdated` for more information.
cici@liyutingdeMacBook-Pro avatar-flutter % 
```

------------------
```
name: voice_assistant
description: "A sample AI Voice Assistant app built on LiveKit Agents"
# The following line prevents the package from being accidentally published to
# pub.dev using `flutter pub publish`. This is preferred for private packages.
publish_to: 'none' # Remove this line if you wish to publish to pub.dev

# The following defines the version and build number for your application.
# A version number is three numbers separated by dots, like 1.2.43
# followed by an optional build number separated by a +.
# Both the version and the builder number may be overridden in flutter
# build by specifying --build-name and --build-number, respectively.
# In Android, build-name is used as versionName while build-number used as versionCode.
# Read more about Android versioning at https://developer.android.com/studio/publish/versioning
# In iOS, build-name is used as CFBundleShortVersionString while build-number is used as CFBundleVersion.
# Read more about iOS versioning at
# https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html
# In Windows, build-name is used as the major, minor, and patch parts
# of the product and file versions while build-number is used as the build suffix.
version: 1.0.0+14

environment:
  sdk: ">=3.4.0 <4.0.0"
  # sdk: ^3.10.0
  flutter: ">=3.22.0"

# Dependencies specify other packages that your package needs in order to work.
# To automatically upgrade your package dependencies to the latest versions
# consider running `flutter pub upgrade --major-versions`. Alternatively,
# dependencies can be manually updated by changing the version numbers below to
# the latest version available on pub.dev. To see which dependencies have newer
# versions available, run `flutter pub outdated`.
dependencies:
  flutter:
    sdk: flutter
  #livekit_components: ^1.3.1

  # The following adds the Cupertino Icons font to your application.
  # Use with the CupertinoIcons class for iOS style icons.
  cupertino_icons: ^1.0.8
  chat_bubbles: ^1.6.0
  livekit_client: ^2.3.6
  flutter_dotenv: ^6.0.0
  http: ^1.3.0
  provider: ^6.1.2
  url_launcher: ^6.3.1
  shimmer: ^3.0.0
  uuid: ^4.5.1
  flutter_sficon: ^1.2.0
  intl: ^0.20.0
  logging: ^1.3.0

dev_dependencies:
  flutter_test:
    sdk: flutter

  # The "flutter_lints" package below contains a set of recommended lints to
  # encourage good coding practices. The lint set provided by the package is
  # activated in the `analysis_options.yaml` file located at the root of your
  # package. See that file for information about deactivating specific lint
  # rules and activating additional ones.
  flutter_lints: ^4.0.0

# For information on the generic Dart part of this file, see the
# following page: https://dart.dev/tools/pub/pubspec

# The following section is specific to Flutter packages.
flutter:

  # The following line ensures that the Material Icons font is
  # included with your application, so that you can use the icons in
  # the material Icons class.
  uses-material-design: true

  # To add assets to your application, add an assets section, like this:
  # The assets/ directory also picks up the optional assets/.env file when
  # present (declared file assets must exist, directory contents may vary).
  assets:
    - assets/
  #   - images/a_dot_burr.jpeg
  #   - images/a_dot_ham.jpeg

  # An image asset can refer to one or more resolution-specific "variants", see
  # https://flutter.dev/to/resolution-aware-images

  # For details regarding adding assets from package dependencies, see
  # https://flutter.dev/to/asset-from-package

  # To add custom fonts to your application, add a fonts section here,
  # in this "flutter" section. Each entry in this list should have a
  # "family" key with the font family name, and a "fonts" key with a
  # list giving the asset and other descriptors for the font. For
  # example:
  # fonts:
  #   - family: Schyler
  #     fonts:
  #       - asset: fonts/Schyler-Regular.ttf
  #       - asset: fonts/Schyler-Italic.ttf
  #         style: italic
  #   - family: Trajan Pro
  #     fonts:
  #       - asset: fonts/TrajanPro.ttf
  #       - asset: fonts/TrajanPro_Bold.ttf
  #         weight: 700
  #
  # For details regarding fonts from package dependencies,
  # see https://flutter.dev/to/font-from-package

  ```


