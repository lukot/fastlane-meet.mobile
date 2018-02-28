# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

default_platform(:android)

platform :android do
  desc "Runs all the tests"
  lane :test do
    gradle(task: "test")
  end

  desc "Does the awesome release"
  lane :do_the_freakin_release do
    gradle(task: "test")
    
    increment_version_name(bump_type: "minor",
                         app_project_dir: 'app')
    increment_version_code(app_project_dir: 'app')
    
	commit_android_version_bump
	
	changelog_from_git_commits
    
    ver_code = get_version_code(app_project_dir: 'app')
    ver_name = get_version_name(app_project_dir: 'app')
    tag_name = "v#{ver_name}.#{ver_code}"

    gradle(task: "assembleDebug assembleAndroidTest")
    capture_android_screenshots

    gradle(task: "clean assembleRelease")
    upload_to_play_store

    add_git_tag(tag: tag_name)
  end

  desc "Does the initial release"
  lane :initial_release do
    gradle(task: "test")

    changelog_from_git_commits(
        pretty: "- (%ae) %s",
        date_format: "short",
        match_lightweight_tag: false,
        merge_commit_filtering: "exclude_merges")

    ver_code = get_version_code(app_project_dir: 'app')
    ver_name = get_version_name(app_project_dir: 'app')
    tag_name = "v#{ver_name}.#{ver_code}"

    gradle(task: "assembleDebug")
    gradle(task: "assembleAndroidTest")
    capture_android_screenshots
    gradle(task: "assembleRelease")
    upload_to_play_store(apk: "app/build/outputs/apk/release/app-release.apk",
                         skip_upload_apk: false,
                         validate_only: false)
    add_git_tag(tag: tag_name)
  end
end
