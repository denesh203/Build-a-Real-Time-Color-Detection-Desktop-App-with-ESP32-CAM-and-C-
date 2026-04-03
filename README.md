📸 ESP32-CAM Real-Time Color Detection with C#
A high-performance Windows desktop application built with C# WinForms that connects to an ESP32-CAM to perform real-time video streaming and color analysis. This project uses manual MJPEG parsing and the HSV Color Model for industrial-grade accuracy.

🚀 Key Features
Zero-Dependency Streaming: Manual MJPEG parsing using byte-markers (0xFFD8 & 0xFFD9).

High-Speed Processing: Utilizes Bitmap.LockBits for direct memory access, avoiding the overhead of GetPixel.

Advanced Color Logic: Uses the HSV (Hue, Saturation, Value) model to accurately identify colors regardless of lighting/shadows.

Industrial Ready: Designed for localized factory software monitoring where high-speed hardware interfacing is required.

🛠️ How it Works
1. Connecting to the Camera
The app establishes a long-lived HTTP connection using HttpClient. The stream is captured from:
http://{IP_ADDRESS}{STREAM_PATH} (e.g., http://192.168.1.235:81/stream)

2. Reading the MJPEG Stream
Instead of using heavy libraries, we scan the raw byte stream:

Start Marker (0xFF D8): Signals the beginning of a new JPEG frame.

End Marker (0xFF D9): Signals the frame is complete and ready for processing.

3. The Color Detection Algorithm
Once a frame is captured, the DetectColor method performs these steps:

Center Focus: Only analyzes the middle 50% of the image to ignore background noise.

Pixel Sampling: Samples every 3rd pixel to maintain a high FPS without lagging the CPU.

HSV Conversion: Converts RGB values to Hue, Saturation, and Value.

Hue: Identifies the "Identity" (Red, Green, Blue).

Saturation: Filters out greyscale or washed-out colors.

Value: Detects Black (darkness) or White (brightness).

Winner Selection: Counts the classified pixels; the category with the most hits (minimum 5% threshold) is displayed.

💻 Getting Started
Hardware Setup
Flash your ESP32-CAM with the standard CameraWebServer example.

Ensure your PC and ESP32 are on the same Wi-Fi network.

Note the IP address from the Serial Monitor.