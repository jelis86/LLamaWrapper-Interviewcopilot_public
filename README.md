<img width="850" height="360" alt="image" src="https://github.com/user-attachments/assets/1fdc25aa-6f44-4003-883d-c880d176d9b5" />

<img width="850" height="360" alt="image" src="https://github.com/user-attachments/assets/79655dbf-1d54-4135-baf2-d65b73792367" />

<img width="786" height="493" alt="image" src="https://github.com/user-attachments/assets/5a326e39-b380-4629-9a55-c51d529b2c72" />

# LLamaWrapper & AI Interview Co-pilot

> **Public repository scope:** This repository contains project documentation and screenshots only. Source code, build files, and runnable binaries are not included in this checkout. Implementation details below describe the project and cannot be independently verified from this repository.

A real-time **AI Interview Co-pilot** desktop application built with a native C++ LLM inference engine (`llama.cpp`) and a transparent C# WPF overlay UI.

Designed specifically for technical interviews, this tool captures live interviewer audio, transcribes it via low-latency streaming STT, and generates real-time, context-aware response hints based on candidate background. LLM inference runs locally; audio is sent to Google Speech-to-Text for transcription.

---

## 🌟 Key Features

* **Local LLM Inference:** Powered by a native C++ DLL wrapper (`LlamaWrapper`) over `llama.cpp` for local GPU-accelerated LLM execution (e.g., quantized `Gemma-3-12b-it`).
* **Interoperability (C++/C#):** Seamless P/Invoke integration with callback delegates for real-time token streaming from C++ Native code directly to the C# WPF UI.
* **Real-time Audio & STT Pipeline:** Intercepts system loopback audio (WASAPI/NAudio) and processes live English audio streams using Google Speech-to-Text gRPC API.
* **Smart Auto-Reconnect:** Features an automatic session manager for Google STT gRPC to prevent the 5-minute stream timeout limit during lengthy interviews.
* **Webcam-Friendly Transparent Overlay:** Always-on-top, semi-transparent WPF floating window placed directly below the camera lens to maintain natural eye contact.
* **Data Flow & Privacy:** The described design keeps resume context and LLM inference local. Audio is transmitted to Google Speech-to-Text, so the complete application is not offline or entirely local. Audio may itself contain personal information.

---

## 🏗️ Architecture Overview

```text
 [ Interviewer Audio (Zoom/Teams) ]
                │
                ▼ (WASAPI Loopback Audio Capture)
 [ C# WPF App (InterviewCopilot) ]
                │
                ▼ (gRPC Streaming API)
 [ Google Speech-to-Text ] ───► [ Real-time Question Text ]
                                         │
                                         ▼
 [ C# NativeInference (P/Invoke) ] ◄─────┘ (System Prompt + Candidate Resume)
                │
                ▼ (Native C-Style Callback Delegate)
 [ C++ Engine (LLamaWrapper.dll) ] ───► [ llama.cpp / compatible GGUF model ]
                │
                ▼ (Token-by-Token Streaming)
 [ WPF Overlay Window UI ] ───► Real-time Candidate Hints (Keywords + Opening)


```

## Implementation Components (Not Included Here)
LLamaWrapper/: C++ Native Shared Library (.dll) wrapping llama.cpp core inference. Exposes C-style export interfaces for C# P/Invoke.

InterviewCopilot/: C# .NET 8 WPF Desktop Application containing the transparent overlay UI, WASAPI audio loopback manager, and Google STT gRPC integration.

LLamaTestConsole/: C++ console test harness for validating native LLM model loading and memory management before linking with C#.

## Availability and evaluation

This checkout cannot be built or launched. Running the described system requires the private WPF and native wrapper sources, a compatible GGUF model, the corresponding GPU runtime, and Google Speech-to-Text credentials and connectivity. Model names above are examples, not a verified compatibility list. No reproducible latency or memory benchmark is included.
