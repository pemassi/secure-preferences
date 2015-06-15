Secure-preferences
==================

[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-secure--preferences-brightgreen.svg?style=flat)](http://android-arsenal.com/details/1/362)

This is Android Shared preference wrapper that encrypts the values of Shared Preferences using AES 128, CBC, and PKCS5 padding with integrity checking in the form of a SHA 256 hash. Each key is stored as a one way SHA 256 hash. Both keys and values are base64 encoded before storing into prefs xml file. **By default the generated key is stored in the backing preferences file and so can be read and extracted by root user.** Recommend use the user password generated option as added in v0.1.0.

The sample app is available on [playstore](https://play.google.com/store/apps/details?id=com.securepreferences.sample)

<img src="https://raw.github.com/scottyab/secure-preferences/master/docs/images/ss_frame_secure_pref.png" height="400" alt="Sample app Screenshot" />
 

##New release v0.1.0
This release is a major refactor of the guts of secure prefs, which is *Not backwards compatible* yet with older versions. So if you have an existing app using this don't upgrade. I'll be looking to add migration into a later release.

[Full list of changes](changes.md)

#Usage

##Dependency

Maven central is the preferred way:

Note: currently v0.1.0 is dependent on snapshot of aes-crypto, this is only as I was waiting for the aes-crypto repo owner to upload to maven. Apolgies this is a bit crappy I'll sort this out for v0.1.1. 

```java
repositories {
    maven {
        url "https://oss.sonatype.org/content/repositories/snapshots"
    }
}

dependencies {
    // Secured preferences
    compile 'com.scottyab:aes-crypto:0.0.2-SNAPSHOT'
    compile('com.scottyab:secure-preferences-lib:0.1.0') {
        provided 'com.scottyab:aes-crypto:0.0.2-SNAPSHOT'
    }
}
```

###Download
Or download the release .aar or clone this repo and add the library as a Android library project/module.

#Examples
This will use the default shared pref file

```java
        SharedPreferences prefs = new SecurePreferences(context);     
```

##Custom pref file
You can define a separate file for encrypted preferences. 

```java
        SharedPreferences prefs = new SecurePreferences(context, null, "my_custom_prefs.xml");
```


##User password
Passing in a password to the SecurePreferences constructor means the key is generated at runtime and *not* stored in the backing pref file. 

```java
        SharedPreferences prefs = new SecurePreferences(context, "userpassword", "my_user_prefs.xml");
```

##Changing Password

```java
        SecurePreferences securePrefs = new SecurePreferences(context, "userpassword", "my_user_prefs.xml");

        
```


#What does the data look like?

SharedPreferences keys and values are stored as simple map in an XML file.  

##XML using Standard Android SharedPreferences


```xml
	<map>
    <int name="timeout" value="500" />
    <boolean name="is_logged_in" value="true" />
</map>
```

##XML with SecurePreferences


```xml
	<map>
    <string name="TuwbBU0IrAyL9znGBJ87uEi7pW0FwYwX8SZiiKnD2VZ7">
        pD2UhS2K2MNjWm8KzpFrag==:MWm7NgaEhvaxAvA9wASUl0HUHCVBWkn3c2T1WoSAE/g=rroijgeWEGRDFSS/hg
    </string>
    <string name="8lqCQqn73Uo84Rj">k73tlfVNYsPshll19ztma7U">
        pD2UhS2K2MNjWm8KzpFrag==:MWm7NgaEhvaxAvA9wASUl0HUHCVBWkn3c2T1WoSAE/g=:jWm8KzUl0HUHCVBWkn3c2T1WoSAE/g=
    </string>
</map>
```


###Disclaimer
By default it's not bullet proof security (in fact it's more like obfuscation of the preferences) but it's a quick win for incrementally making your android app more secure. For instance it'll stop users on rooted devices easily modifying your app's shared prefs.
*Recommend using the user password based prefs as introduced in v0.1.0.*


###Contributing 
Please do send me pull requests, but also bugs, issues and enhancement requests are welcome please add an issue.


###Licence 

Much of the original code is from Daniel Abraham article on [codeproject](http://www.codeproject.com/Articles/549119/Encryption-Wrapper-for-Android-SharedPreferences). This project was created and shared on Github with his permission. 

Apache License, Version 2.0



    Copyright (C) 2013, Daniel Abraham, Scott Alexander-Bown

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

         http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.


The sample app Lock icon for sample app licenced under Creative Commons created by Sam Smith via [thenounproject.com](http://thenounproject.com/term/lock/5704/)
