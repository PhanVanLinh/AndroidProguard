# AndroidProguard

    android {
        buildTypes {
            release {
                minifyEnabled false
      proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            }
        }
    }

### Shrinking, Optimizer, Obfuscator and Preverifier.

**Shrinking:**  It shrink the code to by detecting and removing unused code.
**Optimizer:**  It analyze and optimize the byte code.
**Obfusator:**  It renames the optimize classes, methods with short names. (Used to prevent Reverse Engineering).
**Preverifier:**  It converts the optimize code into optimize jars and libs.

### Proguard files:
**‘proguard-android.txt’**  file, You can find it inside in your `SDK’s folder/tools/proguard/`
**‘proguard-android-optimize.txt’**  file, Similar to **‘proguard-android.txt’**  file but have optimizations enabled
**‘proguard-rules.pro’**  file, you can find it root of the module and you can customize it

### Some keywords

**-keep**
Proguard can not detect and remove unused method correctly (example some reflection method or method invoked from xml, ...) so we need to use keep for keep that kind of methods or class
**-dontwarn**
Some class or method that don't care and it don't exist at runtime then you can use `dontwarn`

**-dontusemixedcaseclassnames**
By default, obfuscated class names can contain a mix of upper-case characters and lower-case characters. Specific this mode will make class name don't mixed case.
At first look, I will think that mixed-case class names is good for secure. Why `proguard-android.txt` disable it? I think that it reduce the confusion for developers who inspect their own compiled code.
[document for more details](https://www.guardsquare.com/en/proguard/manual/usage)
https://stackoverflow.com/questions/19587999/why-is-dontusemixedcaseclassnames-included-in-the-default-proguard-android-xml

**-verbose**
Specifies to write out some more information during processing

Another keyword is easy to understand so don't need to focus

**-assumenosideeffects**
Specifies methods that don't have any side effects (other than maybe returning a value). In the optimization step, ProGuard will then remove calls to such methods, if it can determine that the return values aren't used.

### Example
#### 1) Remove Logcat
```
-assumenosideeffects class android.util.Log {
    public static boolean isLoggable(java.lang.String, int);
    public static int d(...);
    public static int w(...);
    public static int v(...);
    public static int i(...);
    public static int e(...);
}
```