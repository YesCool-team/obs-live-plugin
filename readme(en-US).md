## Overview
First, thank you for using YesCool! OBS Plugin😄.

This plugin runs on Windows x64 platform and relies on OBS Studio to run,compatible with OBS version 30 and above. It supports camera control (zoom, focus,presets), pan-tilt control (360° rotation), MIDI keyboard (normal keys, joystick, T-bar, knob, slide bar, etc. 
Support key up, key down, long pressed, light on/off control), automation program (trigger conditions, trigger actions),All scenes preview and API services.

## Authorization
YesCool! plug-in takes a lot of effort to develop and maintain, so it can only be used with a small license fee. Click "Authorization" in the software interface and follow the screen prompts to complete the authorization.

## PTZ Control
Includes camera control and pan-tilt control. 
When you switch OBS scene sources, YesCool! ptz manager automatically identifies the source type and displays the PTZ control if it is a video/camera. For a newly added or first identified scene source, you need to make some simple Settings,including the camera IP, port, protocol (VISCA, PELCO, Network), and then you can control the camera and pan-tilt.

## MIDI Keyboard
Professional director keyboard supporting MIDI protocol, when you connect to the computer via USB interface, you can create personalized keyboard commands.
YesCool! plug-in allows you to create multiple keyboard schemes and switch them freely. Each key set instructions and parameters can be.

## All-scenes preview
Display the preview of all scenes, detect the media sounds in each scene in real time, and prominently display the status.

## Images slide preview/switch
Display all the images slide within the current preview scene. Double-click to directly switch to the slide images.

## Audio effects
YesCool! plugin includes global functions such as audio fade-in and fade-out, and volume gain. After selecting "Use Background Music" in the automation program, this function will be automatically enabled.

## Automation programs
If you want to implement some automatic operations in the OBS Studio, such as switching scenes, playing media, calling presets, layout adjustment, etc., you can use YesCool! plug-in's the automatic programs.
A program allows to add a number of different combinations of trigger conditions and trigger actions, set up a number of different programs to form a program group, to achieve more powerful and flexible automatic lives.

### Trigger contidions 
- It supports the composition of logical AND, OR.
- Supports comparison operations equal(=), lessThan(<), greaterThan(>), not equal(<>), greaterEqual(>=), lessEqual(<=).
- Direct triggering is supported.
- It supports the judgment of the visibility, size, position,rot angle, media state, scene and other items of the selected condition item.

### Trigger actions 
When the trigger conditions is successful, the actions will triggered.
- Allows multiple actions to be executed together.
- Support scene, input source, preset, media and other operations.
- When controlling the position, size and angle of the scene item, it supports continuous action, which can be changed by 
- incremental or absolute value, so that simple actions can be formed.

## API services
YesCool! plug-in has a built-in API service and supports both TCP and UDP protocols. The API service allows you to implement more advanced extensions. The API service has the following features:
1. Concise API instruction format.
2. API directives can be bind to a MIDI keyboard.
3. Freedom to control the API service is enabled or disabled.
4. Service requests/responses using UTF8.

### API Reuqest 
When communicating with the API services, the instruction and parameter are generated into a JSON format string, the JSON format is {"cmd":"command name","param1":"parameter 1","param2": parameter 2}, and then the JSON string is encoded by UTF8 to byte sequence, and then Base64 encoded into parentheses. You will get the transferred content of the API service, which is JSON=>UTF8=>(base64). 
For example: suppose you want to pop up a message box saying "Hello,World! , then you perform the following steps to generate the transfer content:
1. Generate the JSON string: {"cmd":"msgbox","param1":"Hello,World!"}
2. The above JSON string is encoded as a byte array using UTF8 and then converted to base64: eyJjbWQiOiJtc2dib3giLCJwYXJhbTEiOiJIZWxsbyxXb3JsZCEifQ==
3. Place the resulting base64 code in parentheses: (eyJjbWQiOiJtc2dib3giLCJwYXJhbTEiOiJIZWxsbyxXb3JsZCEifQ==)
4. Just send the result from step 3 to the API services.

### API Response 
After receiving the request, the API services parses the instruction content and executes it, and returns the execution result. 
The result is plain text, which returns OK if successful or an error message otherwise.

### Directives 
The following are the service instructions supported by the current version and the instruction parameter requirements in the format: Instruction name(parameter 1,[parameter 2]). If a parameter is marked with square rackets, it is an optional parameter, and if a directive has no parameter list, it is not required.

### System directives
- OpenUrl(param1):Opens the URL specified by param1.
- GetUrl(param1):Sends an HTTP GET request to the URL specified by param1.
- PostUrl(param1):Sends an HTTP POST request to the URL specified by param1.
- PutUrl(param1):Sends an HTTP PUT request to the URL specified by param1.
- DeleteUrl(param1):Sends an HTTP DELETE request to the URL specified by param1.
- OpenFile(param1):Opens the file specified by param1.
- OpenFolder(param1):Opens the folder specified by param1.
- Hotkey(param1):Triggers the hotkey combination specified by param1 (Authorization is required), with each key linked by + and multiple combinations separated by a comma.The following examples are all valid:
	- Ctrl+A,Ctrl+Alt+F1 (Two copies of the composite keys),
	- Ctrl+A (A composite keys),
	- Escape (Single key)
- MsgBox(param1):Displays the message contents specified by param1 as a message box.
- Toast(param1):Displays the message content specified by param1 as a tooltip.
- Exit:Quit OBS Studio.

### PTZ directives
Only control the current selected scene item, PTZ all directives need to be authorized after effective!!
- ZoomStop:Stop the zoom action.
- ZoomIn:Zoom in (to bring the target closer).
- ZoomOut:Zoom out(to push the target away).
- FocusStop:Stop focusing.
- FocusFar:Far focusing.
- FocusNear:Near focusing.
- FocusAuto:Auto focusing.

- PTZLeft(param1,[param2])、PTZLeftUp(param1,[param2])、PTZRight(param1,[param2])、PTZRightUp(param1,[param2])、PTZUp(param1,[param2])、PTZDown(param1,[param2])、PTZLeftDown(param1,[param2])、PTZRightDown(param1,[param2]):Turn the pan-tilt in the specified direction. param1 is expected to be a number; it determines the rotation speed. param2, which requires a number, specifies the maximum allowed speed value.
- PTZStop:Make the pan-tilt stop rotating.
- GotoHome：Bring the pan-tilt back to the original position.
- CallPreset(param1):param1 is expected to be a number, start at 0, and call a preset for the specified sequence number.
- SetPreset(param1):param1 is expected to be a number that starts at 0 and set a preset for the specified sequence number.

### OBS directives 
- ToggleRecording:Start/stop recording(Authorization is required).
- StartRecord:Start recording(Authorization is required).
- StopRecord:Stop recording(Authorization is required).

- ToggleLiving:Start/stop living(Authorization is required).
- StartLive:Start living(Authorization is required).
- StopLive:Stop living(Authorization is required).

- ToggleStudioMode:Turn studio mode on/off(Authorization is required).
- OpenStudioMode:Turn studio mode on(Authorization is required).
- CloseStudioMode:Turn studio mode off(Authorization is required).

- ToggleVirtualCam:Turn virtual camera on/off(Authorization is required).
- OpenVirtualCam:Turn virtual camera on(Authorization is required).
- CloseVirtualCam:Turn virtual camera off(Authorization is required).

- ToggleFullscreen:Turn full screen view on/off(Authorization is required).
- OpenFullscreen:Turn full screen view on(Authorization is required).
- CloseFullscreen:Turn full screen view off(Authorization is required).

- Preview(param1):Make the scene switch to the preview view specified by param1(Authorization is required), param1 can be a number number or the name of the scene.
- Output(param1):Make the scene switch to the output view specified by param1(Authorization is required), param1 can be a number number or the name of the scene.
- CutDirect:Directly switch the currently preview scene to the output view(Authorization is required).
- AutoDirect:Transitions the previewed scene to the output view based on the last used transition effect(Authorization is required).
- Transaction(param1):Transitions the preview scene to the output view using the transition effect specified by param1(Authorization is required).
- T-Bar(param1):Control the change amplitude of OBS scene switching (Authorization is required), param1 is required to be a number.

- Select(param1):Select a item in the preview scene(Authorization is required), param1 can be a numeric or the name of the scene items.
- Volume(param1,[param2]):Adjust the volume of the selected item for the preview scene(Authorization is required)，param1 is the volume value, and param2 is the maximum volume.If there is no selected item for the preview scene, the execution fails.
- ToggleMuted:Mute/unmute the media of the selected item for the preview scene(Authorization is required).
- Muted:Mute the media of the selected item for the preview scene(Authorization is required).
- UnMuted:Unmute the media of the selected item for the preview scene(Authorization is required).

- PlayMedia:Play the media of the selected item of the preview scene(Authorization is required).
- PauseMedia:Pause playing the media of the selected item of the preview scene(Authorization is required).
- PlayPauseMedia:Pause/Resume the media of the selected item of the preview scene(Authorization is required).
- RestartMedia:Replay the media of the selected item of the preview scene(Authorization is required).
- StopMedia:Stop the media of the selected item of the preview scene(Authorization is required).
- PreviousMedia:If the media is a playlist, switch to the previous media playback(Authorization is required).
- NextMedia:If the media is a playlist, switch to the next media playback(Authorization is required).

- ViewRecords:View media files recorded by OBS Studio.
- Screenshot:Screenshot of the current output of OBS Studio.

## End
Thank you for used.
YesCool! plug-in will continue to add new features, in the validity of the license, free use.