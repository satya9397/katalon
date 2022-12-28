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

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

# before_all do |_lane, options|
#   ENV["FASTLANE_ITUNES_TRANSPORTER_USE_SHELL_SCRIPT"]="1"
#   ENV["FASTLANE_ITUNES_TRANSPORTER_PATH"]="/Applications/Transporter.app/Contents/itms"
# end

app_testers = [
  "ganta.satyadurgaprasad@cardspal.com",
]
firebase_app_id = ENV["firebase_app_id"]
tester_udids_file = ENV["tester_udids_file"]
default_platform :ios

platform :ios do



  lane :build do
      get_certificates()
      get_provisioning_profile(adhoc: true, force: true)
      build_app(export_method: "ad-hoc", scheme: "CardsPal")
      
  end

  lane :distribute do
      #scriptfile
      download_udids
      add_new_devices
      # increment_version_number(
      #   bump_type: "minor" # Automatically increment minor version number
      # )
      build
      firebase_app_distribution(
           app: firebase_app_id,
           release_notes: "Try out this app! added groups and node modules & pod modules",
           #testers: app_testers.join(","),
           groups: "group1"
      )
      #upload_to_testflight(itc_provider: "FNKCPTJ5D5")
  end
  
  lane :scriptfile do
    # Run Calabash tests
    sh "bash ./script.sh"

    # ...
   end


  lane :download_udids do
    firebase_app_distribution_get_udids(
        app: firebase_app_id,
        output_file: tester_udids_file,
    )
  end

  lane :add_new_devices do
    register_devices(devices_file: tester_udids_file)
  end

end



  # lane :test_flight do
  #   sync_code_signing(type: "appstore")    # see code signing guide for more information
  #   build_app(scheme: "MyApp")
  #   #upload_to_testflight
  #   slack(message: "Successfully distributed a new beta build")
  # end
