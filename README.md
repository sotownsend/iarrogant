![iArrogant: Convert .entitlements into codesignable entitlements](https://raw.githubusercontent.com/sotownsend/iarrogant/master/banner.png)

[![Gem Version](https://badge.fury.io/rb/iarrogant.svg)](http://badge.fury.io/rb/iarrogant)
[![Build Status](https://travis-ci.org/sotownsend/iarrogant.svg)](https://travis-ci.org/sotownsend/iarrogant)
[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/sotownsend/iarrogant/trend.png)](https://bitdeli.com/free "Bitdeli Badge")
[![License](http://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://github.com/sotownsend/iarrogant/blob/master/LICENSE)

# What is this?

iArrogant is a command line tool that converts a XML style plist .entitlements file into the proper XML style plist .xcent used by codesign; purpose built for our testing CI system @[Fittr](http://www.fittr.com).

Before you run your entitlements through iarrogant, it may look like this

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>com.apple.developer.healthkit</key>
	<true/>
</dict>
</plist>
```

After running through iarrogant and passing in the bundle id of `com.myapp` and team identifier of `ZMEAK3CXPR`
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>application-identifier</key>
	<string>ZMEAK3CXPR.com.myapp</string>
	<key>aps-environment</key>
	<string>production</string>
	<key>com.apple.developer.healthkit</key>
	<true/>
	<key>com.apple.developer.team-identifier</key>
	<string>ZMEAK3CXPR</string>
	<key>get-task-allow</key>
	<false/>
</dict>
</plist>
```

## Requirements

- iOS 7.0+ / Mac OS X 10.9+ (Untested)
- Xcode 6.1
- Ruby 2.1 or Higher

## Communication

- If you **found a bug**, open an issue.
- If you **have a feature request**, open an issue.
- If you **want to contribute**, submit a pull request.

## Installation

Run `sudo gem install iarrogant`

---

## Usage


```
#entitlement_file_path - The user created .entitlements file, usually has a few items put in automatically by capabilities in XCode
#team_identifier - The typically 10 capitalized alpha-numeric team identifier, e.g. LOETE2NOBU
#bundle_id - Your apps bundle id given in Xcode e.g. (com.awesome_app)
#aps_environment - Usually set to 'production'
#output_xcent_file - The output file location to save the generated entities
(sh)>iarrogant <entitlement file path> <team identifier> <bundle id> <aps environment> <output_xcent_file>
```

### Convert an entitlement file
```
(sh)>iarrogant MyApp.entitlements ABCDEFG com.my_app production MyApp.xcent
```

Now you can sucessfully codesign

```
#Codesign your .app
(sh)>codesign --force --sign <SIGNING_IDENTITY (e.g. dev certificate SHA1)> --entitlements MyApp.xcent MyApp.app

#View the embedded signed entitlements
(sh)>codesign -d --entitlements - MyApp.app
```

## FAQ

### When should I use iarrogant?

When you're putting a build server togeather for CI and need to alter the provisioning profile with an adhoc distribution.  Codesign will require a copy of the original xcent entitlements which is a modified version of the entitlements file.  iarrogant will produce the correct xcent file for codesign

### Wait, can't I just use the regular .xcent file during the build and then sign the provisioning profile?

This may work sometimes.  However, many times XCode inserts other keys, e.g. `beta-test` that will be incompatible across builds.

### What's Fittr?

Fittr is a SaaS company that focuses on providing personalized workouts and health information to individuals and corporations through phenomenal interfaces and algorithmic data-collection and processing.

* * *

### Creator

- [Seo Townsend](http://github.com/sotownsend) ([@seotownsend](https://twitter.com/seotownsend))

## License

iarrogant is released under the MIT license. See LICENSE for details.
