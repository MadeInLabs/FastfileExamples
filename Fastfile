# Customise this file, documentation can be found here:
# https://github.com/fastlane/fastlane/tree/master/fastlane/docs
# All available actions: https://docs.fastlane.tools/actions
# can also be listed using the `fastlane actions` command

# Change the syntax highlighting to Ruby
# All lines starting with a # are ignored when running `fastlane`

# If you want to automatically update fastlane if a new version is available:
# update_fastlane

# This is the minimum version number required.
# Update this, if you use features of a newer version
fastlane_version "2.14.2"

default_platform :android

platform :android do
    $version = get_version_name #get_version_name is a fastlane plugin, execute '[sudo] fastlane add_plugin get_version_name' to install
    $release_notes = prompt(
                      text: "Release notes: ",
                      multi_line_end_keyword: "END"
                    )
    #Install firebase plugin for fastlane 'sudo fastlane add_plugin firebase_app_distribution'
    def firebaseAppDistributionDefault
        firebase_app_distribution(
            app: app_id_goes_here, #Something like "1:###:android:###"
            groups: "madeinweb-e-mobile", #This group must be created at App Distribution at Side Menu
            firebase_cli_path: "/usr/local/bin/firebase", #This path may be changed. You need to execute install Firebase CLI. For Mac users, this command was tested: 'curl -sL firebase.tools | bash'
            release_notes: $release_notes
        )
    end

    before_all do
        # Slack hook url
        ENV["SLACK_URL"] = "https://hooks.slack.com/services/your_slack_hook_url_complete_goes_here"
    end

    desc "Runs all the tests"
    lane :test do
        gradle(task: "test")
        gradle(task: "connectedAndroidTest")
    end

    desc "Submit a new Build to Crashlytics Beta with homologation API links"
    lane :homol do
        gradle(task: "clean")
        gradle(task: "test")
        gradle(task: "connectedAndroidTest")
        gradle(
            task: "assemble",
            flavor: "Homol", #If you have a flavor
            build_type: "Release"
        )
        firebaseAppDistributionDefault
    end

    desc "Submit a new Build to Crashlytics Beta with production API links"
    lane :prod do
        gradle(task: "clean")
        gradle(task: "test")
        gradle(task: "connectedAndroidTest")
        gradle(
            task: "assemble",
            flavor: "Prod", #If you have a flavor
            build_type: "Release"
        )
        firebaseAppDistributionDefault
    end

    after_all do |lane|
        # This block is called, only if the executed lane was successful
        slack(
            message: "We have a new version of [Project name] to Android: ",
            payload: {
                'Version' => $version,
                'Release Notes' => $release_notes,
                'Flavor' => lane_context[SharedValues::GRADLE_FLAVOR],
            },
            default_payloads: [],
          )
    end

    error do |lane, exception|
        # slack(
        #   message: exception.message,
        #   success: false
        # )
    end
end

# More information about multiple platforms in fastlane: https://github.com/fastlane/fastlane/blob/master/fastlane/docs/Platforms.md
# All available actions: https://docs.fastlane.tools/actions

# fastlane reports which actions are used
# No personal data is sent or shared. Learn more at https://github.com/fastlane/enhancer
