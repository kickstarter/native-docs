### First things first

Make sure that everything from the `oss` repo has been pushed to the `private` repo by running `git pull oss && git push private`.

### Beta Deployment

Run the command `make deploy` to deploy `private/master` to Hockey.

### App Store Deployment

1. Bump the version number in Xcode and commit (build number is set automatically).
2. Deploy `private/master` to iTunes connect by running the following command:

bash/zsh:  
`RELEASE=itunes make deploy`

fish:  
`env RELEASE=itunes make deploy`

### Common Issues

CircleCI, our CI provider, often updates Xcode and that breaks our builds and deployments. This usually involves the following changes on our side:

In `circle.yml`:
Select the new Xcode version:
```yaml
machine:
  xcode:
    version: "9.2"
```

Select the simulator version to startup:
```yaml
test:
  pre:
    - xcrun instruments -w 'iPhone 8 (11.2) [9F8B48EF-912E-499E-9874-4CCF692178B3]' || true
```

In the `Makefile`, update the simulator version used to run tests:
```bash
SCHEME ?= $(TARGET)-$(PLATFORM)
TARGET ?= Kickstarter-Framework
PLATFORM ?= iOS
OS ?= 11.2
```

In `.fastlane/FastFile` update the Xcode version used to compile for deployment:
```ruby
before_all do
  xcversion(version: "9.2")
end
```

### iTunes connect

todo
