### First things first

Make sure that everything from the `oss` repo has been pushed to the `private` repo by running `git pull oss && git push private`.

### Beta Deployment

Run the command `make deploy` to deploy `private/master` to Hockey.

### App Store Deployment

1. Bump the version number in Xcode and commit (build number is set automatically).
1. Deploy `private/master` to iTunes connect by running the following command:

bash/zsh:  
`RELEASE=itunes make deploy`

fish:  
`env RELEASE=itunes make deploy`

### Common Issues

CircleCI, our CI provider, often updates Xcode and that breaks our builds and deployments. This usually involves the following changes on our side:

In `circle.yml`:
Select the new Xcode version:
```yaml
xcode_version: &xcode_version 9.3
...
base_job: &base_job
  macos:
    xcode: "9.3.0"
```

Select the simulator version to startup:
```yaml
preload_simulator: &preload_simulator xcrun instruments -w 'iPhone 8 (11.3) [5FEDB80B-4688-4334-8B56-77D413CF00DC]' || true
```

### Fastlane match

We use `fastlane match` to manage our certificates and provisioning profiles. CircleCI 2.0 also requires that we use `match`. All that you need to do is run `bundle exec fastlane match_all` in the project folder, however if you have not run this before then follow these steps:

1. run `bundle install`
1. run `bundle exec fastlane match_all`
1. If prompted for Xcode version to select, enter the current version (this is passed by an environment var on CircleCI)
1. When asked for the passphrase to decode the certificates, use `Fastlane match passphrase` found in 1Password in the Native Squad vault
1. You will also be prompted for the password for `appledev@kickstarter.com`, this is in the vault too.

**Note**: This should be run any time you have trouble with codesigning or if any of the provisioning profiles change, e.g. if a new device is added.
