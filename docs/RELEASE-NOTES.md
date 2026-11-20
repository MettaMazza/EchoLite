**Active development · 20 November 2026**

- **Apple Silicon Mac:** Qwen3.5-4B starts with Metal and four recurrent checkpoints. In the tested 8 GB M1 calculator turn, EchoLite reused 1,564 of 1,870 follow-up tokens (83.6%). The four saved checkpoints occupied 201 MiB.
- **Android:** New conversation recovers an interrupted turn when its worker has exited. Active replies are protected, and prior conversations and completed tool results remain available. The APK supports in-place updates from the previous development release.
- **Android models:** 4B remains the default. The optional 9B profile can use Adreno OpenCL on supported devices.

Download **Install.EchoLite.zip** for an Apple Silicon Mac, or **Install.EchoLite.on.Android.zip** to provision a connected ARM64 Android phone from a Mac. Existing provisioned phones can update directly with **EchoLite-Android-arm64.apk**. Model and offline reference downloads are separate from the APK.
