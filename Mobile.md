<pre>
                        _     _ _                  
            /\/\   ___ | |__ (_) | ___             
           /    \ / _ \| '_ \| | |/ _ \            
          / /\/\ \ (_) | |_) | | |  __/            
          \/    \/\___/|_.__/|_|_|\___|            
                                                   
   ___ _                _   __ _               _   
  / __\ |__   ___  __ _| |_/ _\ |__   ___  ___| |_ 
 / /  | '_ \ / _ \/ _` | __\ \| '_ \ / _ \/ _ \ __|
/ /___| | | |  __/ (_| | |__\ \ | | |  __/  __/ |_ 
\____/|_| |_|\___|\__,_|\__\__/_| |_|\___|\___|\__|
                                                   
                                                   
</pre>

### A compilation of tools and techniques targeting Mobile applications.

## General

### SQLite

Make headers visible

`.headers ON`

List tables

`.tables`

list data in a table

`select * from table_name`

### File searching

Look for database files

`find . -name *.db`


## Android

#### Mind Map
[Android Pentesting](https://cdn6.mindmeister.com/pt/1491593727/android-pentesting?fullscreen=1#)


#### Extract APK from device via ADB

Get app names

`adb shell pm list packages | grep "<app name>"`

Get app path

`adb shell pm path com.app.example`

Pull apk from device

`adb pull /data/app/com.app.example.apk /home/user`

Enumerate content URIs

`strings classes.dex | grep "content://"`


## iOS

Get app names

`frida-ps -Uai`

Get all applications installed

`find . -name com.apple.mobile.installation.plist`

`plutil -convert xml1 <plist_file>`

Run objection

`objection --gadget <PID> explore`

`objection -g "com.example.app" explore`

Get all plist files

`find / -type f -iname "*.plist"`

Check if ASLR is enabled (PIE flag)

`otool -Vh <app_name>`

[Disable ASLR](https://github.com/thlorenz/chromium-build/blob/master/mac/change_mach_o_flags.py)

`python change_mach_o_flags.py --no-pie <binary>`

### Dump unencrypted application from device's memory

Gather info

`otool -l <app_name> | grep -A 4 LC_ENCRYPTION_INFO`

`otool -l <app_name> | grep -A 3 LC_SEGMENT | grep __TEXT -A 1`

* cryptid

If true, means that the application will be decrypted when started

* cryptoff

This is the offset of the encrypted data, where the encrypted data starts

* cryptsize

That is the size of the encrypted segment

start_address = cryptoff + vmaddr

range_to_dump = crypt size + start_address

_this should be in hex, ex: 0x2000+0x1000

Get the PID of the app

`ps -ax | grep "app_name"`

`gdb -p <PID>`

`dump memory <output_file.bin> <start_address> <range_to_dump>`

Merge the original binary with the unencrypted portion dumped

`dd bs=1 seek=<start_address> conv=notrunc if=<output_file.bin> of=<binary_name>`

To change the binary cryptid value from 1 to 0, so the system does not try to decrypt the application, open it on a hex editor and search for `21 00 00 00 14 00 00`. The cryptid value is located 8 bytes after, just change from `01` to `00`. You might have to change it in more than one field, so search the hex value multiple times to validate this.

### Objection

#### Useful Commands

Dump KeyChain Data

`ios keychain dump --json`
