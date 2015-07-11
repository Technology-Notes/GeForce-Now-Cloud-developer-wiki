### As a developer do I need to configure resolution and FPS?
No, Resolution and FPS is selected by onboarding team at NVIDIA
### Why do I get this message: "Failed to create the message bus"?
This error message is reported by the simulator, primarily due to the fact that the communication port is already in-use (e.g., by another running instance of the simulator, or by the NVStreaming service running on the machine). 

To resolve this issue, make sure you have stopped all the NVIDIA streaming services running on your machine, also make sure that no other instance of the simulator is running.
### What audio is supported by GFN?
Stereo only
### What controls are supported? 
GFN supports X-Input along with keyboard and mouse.
### Do I need to change the in-game buttons display?
Generally you do not need to change in-game buttons, it should just work using X-input.
### Does GFN support access to the external network?
Yes
### Are players in GFN games able to play with other players of the normal PC games?
Yes
### How does Authentication and Federation work?
NVIDIA supports OAuth 2.0.

See: [Account Federation Flow](https://github.com/camify/GFN-Link/wiki/Account-Federation-Flow)
### What version of Windows does GFN run on?
Windows Server 12 SP 1
### What version of DirectX?
DirectX 11
### What if we need other libraries to be installed with our game?
The NVIDIA Onboarding team will take care of this for you.