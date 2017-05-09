### First things first

Make sure that everything from the `oss` repo has been pushed to the `private` repo by running `git pull oss && git push private`.

### Beta Deployment

Run the command `make deploy` to deploy `private/master` to Hockey.

### App Store Deployment

1. Bump the version number in Xcode and commit (build number is set automatically).
2. Run the command `env RELEASE=itunes make deploy` to deploy `private/master` to iTunes connect.

### iTunes connect

todo
