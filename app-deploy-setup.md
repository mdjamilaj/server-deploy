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
```
