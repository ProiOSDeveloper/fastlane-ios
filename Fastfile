fastlane_version "1.66.0"

default_platform :ios

platform :ios do


lane :beta do
	xcbuild
	opt_out_usage
end


# Desc: Reading fastlane specifications from JSON file 
def readSpec (key)
  file = File.read('../fastspec')
  specs = JSON.parse(file)
  return specs[key]
end



lane :match_developer do 
  match(git_url: readSpec('git_url'),
      type: "development",
      app_identifier: readSpec('app_identifier'),
      readonly: true)
end

lane :match_adHoc do
  match(git_url: readSpec('git_url'),
      type: "adhoc",
      app_identifier: readSpec('app_identifier'),
      readonly: true)
end

lane :match_appstore do
  match(git_url: readSpec('git_url'),
      type: "appstore",
      app_identifier: readSpec('app_identifier'),
      readonly: true)
end

lane :provisioning do 
  cert(development: true, output_path: "../../provisioning")
  cert(output_path: "../../provisioning")
  sigh(development: true, output_path: "../../provisioning")
  sigh(adhoc: true, output_path: "../../provisioning", force: true)
  sigh(output_path: "../../provisioning", force: true)
end

lane :punchh_gym do 
  gym( 
    use_legacy_build_api: true,
    output_directory: "../../Archives",
    output_name: readSpec('output_name')
  )
end

lane :punchh_tag do
  add_git_tag(
    tag: prompt( text: "Enter tag title (v will be automatically prefixed):")
  )
  push_git_tags
end

lane :verify_branch do
  ensure_git_branch(
    branch: 'master'
  )
end

lane :punchh_release do
  ipaPath = "../../Archives/" + readSpec('output_name') + ".ipa"
  say ipaPath
  github_release = set_github_release(
    repository_name: readSpec('repository_name'),
    api_token: readSpec('api_token'),
    description: "Fastlane testing",
    tag_name: prompt( text: "Enter tag version (Example: 1.0.0)"),
    name: prompt( text: "Enter tag title (Example: Production_release_v1.0.0)"),
    commitish: "master",
    upload_assets: [ipaPath]
  )
end

lane :punchh_hockey do
  ipaPath = "../../Archives/" + readSpec('output_name') + ".ipa"
  hockey(
    api_token: readSpec('hockey_api_token'),
    ipa: ipaPath,
    notes: prompt( text:"Enter note for testers: ")
  )
end

lane :slack do
  slack(
  message: "New version of " + readSpec('output_name') + " available on TestFlight",
  channel: readSpec('slack_channel_url'),  # Optional, by default will post to the default channel configured for the POST URL.
  success: true,        # Optional, defaults to true.
  default_payloads: [:git_branch, :git_author], # Optional, lets you specify a whitelist of default payloads to include. Pass an empty array to suppress all the default payloads. Don't add this key, or pass nil, if you want all the default payloads. The available default payloads are: `lane`, `test_result`, `git_branch`, `git_author`, `last_git_commit_message`.
)
end



# TestFlight
lane :testflight do
  ensure_git_status_clean    
  verify_branch
  match_appstore
  punchh_gym
  punchh_tag 
  pilot  
end

lane :testflight_local do
  ensure_git_status_clean    
  verify_branch
  provisioning
  punchh_gym
  punchh_tag
  pilot  
end

lane :hockey do
  ensure_git_status_clean
  verify_branch
  match_adHoc
  punchh_gym
  punchh_tag
  punchh_hockey
end

lane :github_release do 
  ensure_git_status_clean
  verify_branch
  punchh_release 
  push_git_tags
end

after_all do |lane|
  slack
end

error do |lane, exception|
end
end
