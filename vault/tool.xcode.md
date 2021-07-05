---
id: 9cdd3fac-1222-4ad2-bc4b-40e488cff588
title: Xcode
desc: ''
updated: 1625030646842
created: 1621434247250
---

# Useful keyboard shortcuts

Features | Keys |
---------|----------|
 Easily navigate between current and previous file | CTRL+CMD+LEFT/RIGHT KEY |
Clear debug console | CMD + K |

# To show duration of builds
- Run the following in your terminal
```bash
defaults write com.apple.dt.Xcode ShowBuildOperationDuration -bool YES
```

# Use iOS device beta with current Xcode version
- Through symlinking (When you have multiple Xcodes)
```bash
sudo ln -s /Applications/Xcode-beta.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport/15.0 /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport
```
References: <https://gist.github.com/steipete/d9b44d8e9f341e81414e86d7ff8fb62d>
- By downloading `DeviceSupport` from <https://github.com/iGhibli/iOS-DeviceSupport>

# Carthage
- To delete Carthage cache
```bash
rm -rf ~/Library/Caches/org.carthage.CarthageKit
```