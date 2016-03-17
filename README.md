fastlane iOS
============

##[Fastlane](https://github.com/fastlane/fastlane)

`Fastlane` is a tool meant to be used to streamline iOS build release process.

###Steps to execute a lane:

- Checkout this repo in your project folder.
- Open terminal and set your working directory to your project folder.
```
$cd <project path>
```
- Type fastlane <lane to be executed>


Supported Lanes
============

####testflight

Use this lane to upload your builds directly to app store. This lane uses [iOS provisioning repo](https://github.com/punchh/ios-provisioning) to store provisionings.
```
$fastlane testflight
```

####testflight_local

Use this lane to upload your builds directly to app store. This lane uses certificates and provisionings stored locally on your system.
```
$fastlane testflight_local
```

####hockey

- Use this lane to upload Adhoc builds to Hockey app.
- You will have to mention hockey api token additionally in `fastspec` file.
```
$fastlane hockey
```

####github_release

Use this lane to add a github tag once the build is approved on store.
```
$fastlane github_release
```

Fastspec
============

- `fastspec` file will contain all the required identifiers & keys in JSON format.
- Here is a sample fastspec file snipped:
```
{
	"app_identifier": "<bundle identifier>",
	"output_name" : "<.ipa name>",
	"slack_channel_url" : "<slack channel webhook url>",
	"api_token" : "<github api token of the user>",
	"git_url" : "<url of github repository storing certificates & provisionings>",
	"hockey_api_token" : "<API token for hockey app>",
	"repository_name" : "<github repository name. Example: punchh/framework-ios>"
}
```
