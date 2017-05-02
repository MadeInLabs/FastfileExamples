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
    version = get_version_name #get_version_name is a fastlane plugin, execute 'fastlane add_plugin get_version_name' to install
    release_notes = prompt(
                      text: "Release notes: ",
                      multi_line_end_keyword: "END"
                    )

    def crashlyticsDefault
        crashlytics(
                    api_token: "api_token_goes_here",
                    build_secret: "build_secret_goes_here",
                    groups: "group_alias_goes_here, group_alias_goes_here2",
                    notifications: true,
                    apk_path: lane_context[SharedValues::GRADLE_APK_OUTPUT_PATH],
                    notes: release_notes
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
        gradle(
            task: "assemble",
            flavour: "Homol", #If you have a flavour
            build_type: "Release"
        )
        crashlyticsDefault
    end

    desc "Submit a new Build to Crashlytics Beta with production API links"
    lane :prod do
        gradle(task: "clean")
        gradle(
            task: "assemble",
            flavor: "Prod", #If you have a flavour
            build_type: "Release"
        )
        crashlyticsDefault
    end

    after_all do |lane|
        # This block is called, only if the executed lane was successful
        slack(
            message: "We have a new version: ",
            payload: {
                'Version' => version,
                'Release Notes' => release_notes,
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
