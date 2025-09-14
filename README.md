# Android-Penetration-Testing-Automation

_________________________________________________________________________________________________________________________________________________

=>Tools for Android Penetration Testing Automation
1. MobSF (Mobile Security Framework) -	A comprehensive security testing framework that performs static and dynamic analysis on Android apps. Ideal for automated baseline testing.
2. Quark Engine	- Signature-based malware detection system that analyzes APK internals for suspicious behavior.
3. Drozer -	A framework for exploring an application's IPC attack surface. Requires an agent be installed on the device.
4. Objection -	A Frida-based runtime exploration toolkit. Capable of injecting hooks and inspecting app behavior without requiring access to source code.
5. Medusa	- A modular framework designed for automated dynamic analysis of mobile apps, with support for both Android and iOS.
_________________________________________________________________________________________________________________________________________________

**MobSF (Mobile Security Framework)**

1. Signer Certificate
The Signer Certificate section displays key details of the app's signing certificate, which include:
X.509 Subject: Identified as "CN=john doe".
Validity Period: Valid from May 12, 2023, until May 5, 2048.
Signature Algorithm: Uses the RSA algorithm (rsassa_pkcs1v15) with a key size of 2048 bits.
Issuer: Also "CN=john doe", indicating self-signing.
Hashes (SHA256, SHA1, MD5, SHA512): Various cryptographic hashes that help verify the certificate’s authenticity.

2. Android API
   APIs refer to the libraries (and the specific methods within those libraries) that are accessed by the application. These can be part of the Android operating system or third-party libraries included within the app. APIs enable various functionalities such as networking, cryptography, user interface customization, data storage, multimedia management, and more.
   Notable entries include 'HTTP Connection' and 'HTTPS Connection' for networking operations, with HTTP specifically categorized as insecure for server communication. We also see an entry for the Java Reflection API, which is often used by malicious actors to obfuscate method names when called.
_________________________________________________________________________________________________________________________________________________

**Quark**
Quark Engine, as stated in the official documentation, is "an open source software for automating analysis of suspicious Android application
Key Features of Quark Engine
1. Malware Scoring System	- Quark Engine assigns scores to detected behaviors based on severity and impact, providing a quantifiable measure of an app’s overall threat level.
2. Rule-Based Analysis	- The engine uses customizable JSON-based rules to detect known malicious patterns and behaviors, allowing users to update or extend detection logic.
3. Five Levels of Crime Investigation	- Quark Analyzes APKs across five distinct layers, each focusing on different behavioral aspects of the application.
4. Permission Analysis	Reviews - requested permissions to flag those that are excessive, suspicious, or indicative of malicious behavior.
5. API Calls and Tracking -	Tracks API calls, especially those accessing sensitive system resources or user data, to detect suspicious activity.
6. Dynamic Analysis -	Complements static analysis by observing app behavior at runtime, helping detect actions triggered only under specific conditions..
7. Graphical Reports	- Quark generates visual representations of behavior flows and interactions, simplifying the understanding of complex threat patterns.

**Installation & Usage**
1. python3 -m venv quark
2. source quark/bin/activate
3. pip3 install -U quark-engine
4. quark --version
5. Once the Quark Engine is installed, we will download the Quark Rules using the following commands:
6. git clone https://github.com/quark-engine/quark-rules
7.  ls -l quark-rules/rules
8.  quark -s -a myapp.apk -r quark-rules
      -s is used to show the summary report, while the options -a and -r specify the APK file and set of rules accordingly

 OR install in kali
 1. sudo apt install quark-engine
 2.  git clone https://github.com/quark-engine/quark-rules
 3.  mkdir -p /root/.quark-engine
 4.  cp -r /home/kali/Desktop/quark-rules /root/.quark-engine/quark-rules
 5.   quark -a myapp.apk -s -r /home/kali/Desktop/quark-rules/rules
 6.   chown -R $(whoami):$(whoami) /home/kali/Desktop/quark-rules (if needed for permission)
 7.   chmod -R u+rX /home/kali/Desktop/quark-rules (if needed for permission)


