# LiteStream Studio

LiteStream is a lightweight, OBS-inspired capture studio with a dual-canvas **Preview / Program** workflow. Edits are staged in Preview; only the CUT button publishes a snapshot to Program. Recording and streaming use Program, so later Preview edits do not silently alter the output already on air.

## Features

- Screen/window/tab capture, camera, microphone, local images, color backdrops, and editable text.
- Drag and resize text overlays in Preview; adjust their wording, font, size, alignment, color, bold/italic style, and backplate.
- Source Fit controls for Default, Fit to screen, Fill screen, Stretch, Original size, and 50–200% scale.
- Import `.pptx` slide decks and navigate slides in Preview. The lightweight browser parser renders text, basic shapes, and embedded images; advanced PowerPoint effects, fonts, transitions, audio/video, and complex charts may not be reproduced exactly.
- Search for Bible editions listed as Public Domain, CC0, GPL, or Creative Commons Attribution/ShareAlike without NC/ND restrictions in the GetBible catalog; download an edition for local/offline passage lookup and add passages to Preview as editable text. Check source licensing and attribution before publication.
- Program Projector mirrors the Program canvas in a separate window. Detect/select a display where supported, or move the window to a connected screen and fullscreen it manually.
- Local WebM recording and a native RTMP sender for one Restream-compatible ingest. Configure destinations at your restream provider; LiteStream sends one upstream Program feed.

## Important: RTMP requirements

The native RTMP command uses **FFmpeg** installed on the machine and available as `ffmpeg` on `PATH`. If it is installed elsewhere, set `LITESTREAM_FFMPEG` to its executable path before launching LiteStream. The current installer does not bundle FFmpeg, so package and license the binary separately if distributing it. RTMP is not available in the plain browser preview. Enter the restream service's RTMP/RTMPS server URL and stream key in the app; keep that key private.

The native FFmpeg integration is source-implemented but has not been compiled or verified in this workspace because Rust/Cargo and a Windows build toolchain are unavailable here. Validate it against your selected restream provider before a live event.

## Run the UI in a browser

For a quick UI preview, serve `ui/` from a local HTTP server or open `ui/index.html` in a current browser. Screen, camera, and microphone permissions generally require a secure context (`https://` or `localhost`). Internet access is needed to search/download Bible editions; imported texts are stored locally by the browser/webview. Screen sharing is started only after the user chooses the capture action.

## Build the Windows installer

The configured bundle target is a Windows NSIS installer. Build on Windows or in a correctly configured Windows environment:

1. Install Node.js LTS, Rust (MSVC), Microsoft C++ Build Tools, and the WebView2 Runtime.
2. Install FFmpeg separately if you need RTMP output and make it available on `PATH` or set `LITESTREAM_FFMPEG`.
3. From this folder run:

   ```powershell
   npm install
   npm run dev
   npm run build
   ```

The installer is placed under `src-tauri/target/release/bundle/nsis/`.

## Compatibility scope

A Windows `.exe` does not run on every device. Tauri builds must be packaged separately for Windows, macOS, and Linux; capture support depends on each operating system and webview. Mobile/tablet support and device-specific capture have not been validated. Screen capture, camera access, MediaRecorder codecs, monitor enumeration, and audio routing vary by platform. Program Projector currently mirrors video, not audio. Test the actual target device and driver combinations before distribution. Recordings are WebM, with codecs determined by the platform runtime.

## Privacy and security

Capture permissions are requested after the user starts a source. Media composition and recording are local. When broadcasting, Program media is sent to the RTMP/RTMPS endpoint and key supplied by the user. Bible catalog/text requests go to GetBible; downloaded editions remain in local browser/webview storage. Do not share stream keys.
