# FastfileExamples
Here you can see a short example of Fastfiles (fastlane file that contains the famous lanes) to Android

# Android Platform

## Get version name plugin

A important thing to show in the end of the release is the **version name**. It could be setted in the build.gradle of the project and will be shown to the final user in the Fabric Beta and Google Play. 

The version name can be getted by the a fastlane plugin called *get_version_name*. So, save in a variable like this:
```ruby
version = get_version_name
```

If it isn't installed yet (you can verify executing "fastlane actions gradle" in the terminal), you can install it
by executing in the terminal
```shell
$ fastlane add_plugin get_version_name
```

## Type "live" the release note
~~TODO~~
