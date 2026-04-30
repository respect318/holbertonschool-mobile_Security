Task 1: Hooking Native Functions in Android – Report

Objective
Perform dynamic analysis on an Android app using Frida to intercept and extract a hidden decrypted flag processed by a native JNI function.

Tools Used
Frida
ADB (Android Debug Bridge)

Objection
Android Studio

Target
APK: Apk_task1 Native Library: libnative-lib.so Function Hooked: getSecretMessage

Step-by-Step Process
App Installation & Launch
Installed and launched the app on emulator/device.
Observed UI and interaction but found no visible flag.

Library Identification
Used adb shell and frida-trace to confirm presence of libnative-lib.so.
Frida Setup
Attached to running app with:
frida -U -n com.example.apk_task1

Hooking JNI Function
Hooked native method using:
Interceptor.attach(Module.findExportByName("libnative-lib.so", "Java_com_example_MainActivity_getSecretMessage"), { onLeave: function (retval) { console.log("[+] Flag:", retval.readUtf8String()); } });

Flag Extraction

Relaunched app and triggered relevant functionality.
Frida intercepted the return value and printed the decrypted flag to the console.

Output
⚑ Decrypted Flag: Holberton{native_hooking_is_no_different_at_all}

Notes
ADB logcat was also used to verify logs from native side.
Objection provided an alternative interface to enumerate classes and methods.


OR XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXORI
Installed Apk_task1 on an emulator. Explored the user interface – the app had no obvious flag or output. Used adb logcat to view logs, nothing significant was output. 2. Identifying the Native Library

Decompiled the APK using APKTool and jadx. Located native library: libnative-lib.so. 3. Listing Exported Functions

Run: frida -U -n com.example.apk_task1 -e 'Module.enumerateExportsSync("libnative-lib.so").forEach(e => console.log(e.name))'
