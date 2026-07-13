# VR-ASSM Conversational Avatar AI

## Overview
VR-ASSM is a Unity project that brings together real-time speech recognition, conversational AI, voice cloning, and avatar lip sync. The system listens to the user, understands their speech, generates a response, and speaks back using a lifelike avatar.

## Features
- **Speech-to-Text (STT):** Uses Deepgram API for accurate voice transcription.
- **Conversational AI:** Integrates Groq LLM (OpenAI-compatible) for natural dialogue.
- **Text-to-Speech (TTS):** Uses Cartesia API for voice cloning and speech synthesis.
- **Lip Sync:** Realistic mouth movement using Oculus LipSync and blend shapes.
- **Modular Pipeline:** Each component can be tested and improved independently.

## Setup Instructions
1. **Clone the repository:**  
   `git clone https://github.com/yourusername/VR-ASSM.git`
2. **Open in Unity:**  
   Use Unity 2022 or newer for best compatibility.
3. **Insert your API keys:**  
   - Deepgram (STT): In `STTHandler.cs` (via Inspector)
   - Groq (LLM): In `LLMHandler.cs` (via Inspector)
   - Cartesia (TTS): In `CartesiaTTSHandler.cs` (via Inspector)
4. **Assign avatar and blend shapes:**  
   - Set up your avatar in the Unity scene.
   - Assign blend shape indices in `OculusLipSyncBlendShape`.
5. **Build and run:**  
   Test in Play mode or build for your target platform.

## VR Office Conversation Setup
1. Add an XR rig root object to the office scene (for example, your Oculus/XR camera rig).
2. Add a `CharacterController` component to the rig root.
3. Add `VRLocomotionController` to the same rig root.
4. Assign these references in `VRLocomotionController`:
  - `Head Transform`: your VR camera transform.
  - `Seat Anchor`: an empty transform where the user should stand/sit in the office.
  - `Avatar Look Target`: avatar head/chest transform to face during conversation.
5. Add `VRConversationControls` to any active scene object (or the rig root).
6. In `VRConversationControls`, assign:
  - `Avatar AI Controller`: your existing `AvatarAIController` in scene.
  - `Locomotion Controller`: the `VRLocomotionController` you added.
7. Play in VR:
  - Left thumbstick: move.
  - Right thumbstick: snap turn.
  - Controller primary button (`A`/`X` on most devices): Start/End Call.

This keeps your existing auth, ElevenLabs, lipsync, and conversation history pipeline unchanged while adding VR-first controls.

## Desktop Fallback Controls (No Headset)
For quick testing without a VR headset:
1. Select your player root object (XR Origin or camera rig root).
2. Ensure it has a `CharacterController`.
3. Add `DesktopArrowKeyMovement`.
4. Assign `View Transform` to your active camera under the rig.

Controls:
- Up Arrow / Down Arrow: move forward/backward.
- Left Arrow / Right Arrow: rotate left/right.

This is only for desktop testing and can stay enabled until headset testing is available.

## Avatar Companion Follow (Optional)
To make the avatar move with the user:
1. Add `AvatarCompanionFollow` to the avatar root object.
2. Assign `Player Target` to your player camera transform (XR camera or desktop camera).
3. Assign `Lip Sync Blend Shape` to the existing `OculusLipSyncBlendShape` on the avatar.
4. Assign `Avatar Animator`.

Suggested values:
- `Min Distance From Player`: 1.5
- `Max Distance From Player`: 2.0
- `Preferred Distance From Player`: 1.7
- `Pause While Avatar Is Speaking`: enabled
- `Always Face Player When Close`: enabled

Animation note:
- If your animator has a bool parameter named `Walk`, leave `Drive Walk Animation` enabled.
- If not, disable `Drive Walk Animation` and the avatar will still follow without walk-state toggles.

## Architecture
```
Microphone Input
      ↓
STTHandler (Deepgram)
      ↓
LLMHandler (Groq/OpenAI)
      ↓
CartesiaTTSHandler (Cartesia TTS)
      ↓
OculusLipSyncBlendShape + Animator
      ↓
Avatar speaks and animates
```

## Challenges & Solutions
- **Voice Activity Detection (VAD):**  
  Tuned silence threshold and duration, added warm-up time, and debug logs for reliable detection.
- **TTS Speed & Clarity:**  
  Adjusted Cartesia parameters and tested voice IDs for optimal quality.
- **Animator Transitions:**  
  Controlled animation parameters via script for instant, smooth transitions.
- **LLM Output Formatting:**  
  Prepended strict language rules and sanitized output for TTS compatibility.
- **Oculus LipSync Integration:**  
  Ensured correct viseme mapping and avatar compatibility for realistic mouth movement.

## License
MIT (or specify your preferred license)

## Credits
- Deepgram (STT)
- Groq (LLM)
- Cartesia (TTS)
- Oculus LipSync
- Unity Technologies

---