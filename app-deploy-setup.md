- [ ] Open your project's pubspec.yaml file and add the following lines under the dev_dependencies section:
```
flutter_launcher_icons: "^0.10.0"
```

- [ ] Open your project's pubspec.yaml file and add the following lines under the dev_dependencies section:
```
flutter_icons:
  android: true
  ios: true
  image_path: "icons/icon.png"
```

- [ ] Save the file, and then run the following command in your terminal or command prompt:

```
flutter pub run flutter_launcher_icons:main
```

```
flutter pub global activate rename
flutter pub global run rename --bundleId com.gamesbazarbd.teammahal
flutter pub global run rename --appname "Games Bazar BD"
flutter build appbundle
```
- [ ] Create an upload keystore
* cd to go Under the bin folder 'C:\Program Files (x86)\Java\jre-1.8\bin'
```
./keytool -genkey -v -keystore ./upload-keystore.jks -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias upload
```
*after create the file cut and paste 'project/android/app'

- [ ] Create a file named [project]/android/key.properties
```
storePassword=<password-from-previous-step>
keyPassword=<password-from-previous-step>
keyAlias=upload
storeFile=<keystore-file-location>
```

- [ ] Configure gradle to use your upload key when building your app in release mode by editing the [project]/android/app/build.gradle file.

*Add the keystore information from your properties file before the android block:
```
def keystoreProperties = new Properties()
def keystorePropertiesFile = rootProject.file('key.properties')
if (keystorePropertiesFile.exists()) {
    keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
}

 android {
         ...
   }
```

*And Replace the 'buildTypes{}' function to paste under code:
```
signingConfigs {
      release {
          keyAlias keystoreProperties['keyAlias']
          keyPassword keystoreProperties['keyPassword']
          storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
          storePassword keystoreProperties['storePassword']
      }
  }
  buildTypes {
      release {
          signingConfig signingConfigs.release
      }
  }
```

- [ ] Re Run the below command:
```
flutter build appbundle
```

* build\app\outputs\bundle\release\app-release.aab
