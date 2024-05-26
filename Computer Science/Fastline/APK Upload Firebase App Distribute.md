
```ruby
# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
#     https://docs.fastlane.tools/plugins/available-plugins
#

# NOTE:
# create .env file at android/.env

default_platform(:android)

path_default = "build/app/outputs/flutter-apk"

platform :android do

  desc "Upload a Dev Release apk to App Distribute"
  lane :devRelease do
    puts "Prepare Build Dev"
    buildFlutterApk(prod: false, isRelease: true)
    apk_path = ""
    Dir.chdir("../..") do
        pathRoot = Dir.pwd
        apk_path = "#{pathRoot}/#{path_default}/app-dev-release.apk"
        unless File.exist?(apk_path)
            UI.user_error! "No Directory Apk"
        end
    end

    firebase_app_distribution(
       app: ENV["FIREBASE_ID"],
       firebase_cli_token: ENV["FIREBASE_TOKEN"],
       debug: true,
       android_artifact_path: apk_path,
       groups: ENV["TESTER-GROUP-NAME"]
    )
    notification(subtitle: "Finished Building", message: "APK Release Ready to in App Distribute...")
  end

  desc "Upload a Dev Debug apk to App Distribute"
  lane :devDebug do
    puts "Prepare Build Dev Debug"
    buildFlutterApk(prod: false, isRelease: false)
    apk_path = ""
    Dir.chdir("../..") do
        pathRoot = Dir.pwd
        apk_path = "#{pathRoot}/#{path_default}/app-dev-debug.apk"
        unless File.exist?(apk_path)
            UI.user_error! "No Directory Apk"
        end
    end

    firebase_app_distribution(
       app: ENV["FIREBASE_ID"],
       firebase_cli_token: ENV["FIREBASE_TOKEN"],
       debug: true,
       android_artifact_path: apk_path,
       groups: "axiapp-tester"
    )

    notification(subtitle: "Finished Building", message: "APK Debug Ready to in App Distribute...")

  end

  private_lane :buildFlutterApk do |options|
    type_file = "lib/main_dev.dart"
    build_type = "dev"
    release_type = "--debug"
    if options[:prod]
        type_file = "lib/main_prod.dart"
        build_type = "prod"
    end
    if options[:isRelease]
       release_type = "--release"
    end

    puts "Build in mode #{release_type}"

    flutter_keyword = fvmCheck()
    if flutter_keyword == "fvm"
        sh("fvm", "flutter", "pub", "get", log: false)
        sh(
            "fvm",
            "flutter",
            "build",
            "apk",
            "-t",
            type_file,
            "--flavor",
            build_type,
            release_type,
            "--no-sound-null-safety",
            )
    else
        sh("flutter", "pub", "get", log: false)
        sh(
            "flutter",
            "build",
            "apk",
            "-t",
            type_file,
            "--flavor",
            build_type,
            release_type,
            "--no-sound-null-safety",
            )
    end
  end


  def fvmCheck
    is_fvm_installed = false

    Dir.chdir("../..") do
        puts Dir.pwd
        is_fvm_installed = Dir.exist? "#{Dir.pwd}/.fvm"
    end

    if is_fvm_installed
       return "fvm"
    end

    return "flutter"
  end

  error do |exception, message|
    notification(subtitle: "Failure Build #{exception}", message: "#{message}")
  end

end
```
