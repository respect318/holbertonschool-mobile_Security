# Task 3: Android Security Challenge – Revealing Hidden Functions

## Objective

Locate and invoke hidden functions in an Android application to retrieve a secret flag using dynamic analysis and reverse engineering.

## Tools Used

* Frida
* Objection
* GDB
* APKTool / JADX
* Android Studio
* Python

## Target

APK: `Apk_task3`
Goal: Find and invoke hidden function(s) to decrypt a secret flag not accessible via normal app usage.

## Step-by-Step Process

1. **APK Decompilation**

   * Decompiled using `jadx-gui` and `apktool`.
   * Discovered obfuscated methods under a utility class (e.g., `Utils`, `SecHandler`, or `FlagManager`).

2. **Exploring Hidden Code Paths**

   * Noted functions with no clear references in the app flow.
   * Observed possible base64 or hex encodings involved.

3. **Dynamic Analysis with Frida**

   * Hooked app with:

     ```bash
     frida -U -n com.example.apk_task3
     ```
   * Wrote a script to intercept and call the suspected hidden method:

     ```js
     Java.perform(function () {
         var TargetClass = Java.use("com.example.sec.Utils");
         console.log("[+] Calling hidden function...");
         var result = TargetClass.revealSecret();
         console.log("[+] Flag: " + result);
     });
     ```

4. **Objection Runtime Exploration**

   * Used commands like:

     ```bash
     android hooking watch class_method com.example.sec.Utils revealSecret
     ```
   * Observed behavior and outputs.

5. **Flag Retrieval**

   * Invoked method at runtime.
   * Extracted encoded result, reversed the encoding with a helper Python script (e.g., Base64 decoding).

6. **Decoding Output**

   * Final flag obtained after decoding and formatting.

## Output

> ⚑ Decrypted Flag: `Holbert{calling_uncalled_functions_is_now_known!}`

## Challenges Faced

* Method obfuscation increased initial complexity.
* Required careful runtime exploration to determine function signatures.
