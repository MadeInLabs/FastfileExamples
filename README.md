# FastfileExamples
Here you can see a short example of Fastfiles (fastlane file that contains the famous lanes) to Android

# Any Platform

## Type "live" the release note

In the beginning of fastlane execution, a prompt is shown, waiting the release notes of the current version be typed.  To do this, its simple:

```ruby
release_notes = prompt(
                  text: "Release notes: ",
                  multi_line_end_keyword: "END"
                )
```

When prompt receives the value of the key *multi_line_end_keyword*, it will close and save the result (without the end keyword) in release_notes variable to be used in the release.

# Android Platform

## Get version name plugin

A important thing to show in the end of the release is the **version name**. It could be setted in the build.gradle of the project and will be shown to the final user in the Fabric Beta and Google Play. 

The version name can be getted by the a fastlane plugin called *get_version_name*. So, save in a variable like this:
```ruby
version = get_version_name
```

If it isn't installed yet (you can verify executing "fastlane actions gradle" in the terminal), you can install it by executing in the terminal
```shell
$ fastlane add_plugin get_version_name
```

## Distribute by Firebase App Distribution

If you don't know [App Distribution](https://firebase.google.com/docs/app-distribution), you must. If you do, let's go: fastlane helps you, with a simple syntax, distribute the .apk to specific emails.

First you need isntall [Firebase CLI](https://firebase.google.com/docs/cli?hl=pt-br) with:

```bash
curl -sL firebase.tools | bash
```

And install the fastlane plugin:
```bash
sudo fastlane add_plugin firebase_app_distribution
```

Then, you must get your app id on Firebase Console at Settings menu that looks like "1:1111111111111:android:1aaaaaaaaaa111aaaaaaaa".

So the whole App Distribution action is:

```ruby
firebase_app_distribution(
    app: "1:...:android:...",
    testers: "tester1@company.com, tester2@company.com",
    # or
    # groups: "group-alias",
    firebase_cli_path: "/usr/local/bin/firebase",
    release_notes: $release_notes
)
```

## Gradle tasks

The Clean gradle task is a good thing to execute before any build task to avoid unnecessary errors.

```ruby
gradle(task: "clean")
```

It's easy generate the apk to release without any thing else to configure.

```ruby
gradle(task: "assembleRelease")
```

But if you want add a specific flavor, this can be done:

```ruby
gradle(
    task: "assemble",
    flavor: "Homol",
    build_type: "Release"
)
```

It can be possible replace the string *Homol* with your flavor else. Watch out with the first capitalized letter, even the flavor name in gradle were not capitalized.
