# xcrun Cheat Sheet

`xcrun` is a command-line tool that allows you to run or locate development tools and properties. It's particularly useful for iOS development and simulator management.

## Simulator Management (simctl)

### Device Operations
```bash
# List all available simulators
xcrun simctl list

# List only booted simulators
xcrun simctl list devices | grep Booted

# Boot a specific simulator
xcrun simctl boot "iPhone 15 Pro"

# Shutdown a specific simulator
xcrun simctl shutdown "iPhone 15 Pro"

# Shutdown all simulators
xcrun simctl shutdown all

# Erase all simulators (factory reset)
xcrun simctl erase all

# Erase a specific simulator
xcrun simctl erase "iPhone 15 Pro"

# Delete a simulator
xcrun simctl delete "iPhone 15 Pro"
```

### App Management
```bash
# Install an app on simulator
xcrun simctl install booted /path/to/app.app

# Uninstall an app from simulator
xcrun simctl uninstall booted com.example.app

# Launch an app
xcrun simctl launch booted com.example.app

# Terminate an app
xcrun simctl terminate booted com.example.app

# List installed apps
xcrun simctl listapps booted
```

### File Operations
```bash
# Open simulator's data directory
xcrun simctl get_app_container booted com.example.app data

# Add photos to simulator
xcrun simctl addmedia booted /path/to/photo.jpg

# Add videos to simulator
xcrun simctl addmedia booted /path/to/video.mp4

# Push a file to simulator
xcrun simctl push booted com.example.app /path/to/file.txt

# Pull a file from simulator
xcrun simctl pull booted com.example.app /remote/path/file.txt
```

### Screenshot and Recording
```bash
# Take a screenshot
xcrun simctl io booted screenshot screenshot.png

# Record video (requires stopping manually with Ctrl+C)
xcrun simctl io booted recordVideo video.mov

# Record video with specific format
xcrun simctl io booted recordVideo --codec=h264 --display=external video.mov
```

### Device Information
```bash
# Get device UDID
xcrun simctl list devices | grep "iPhone 15 Pro"

# Get device status
xcrun simctl status_bar booted

# Set status bar overrides (time, battery, etc.)
xcrun simctl status_bar booted override --time "9:41" --batteryLevel 100
```

## Development Tools

### Device and Provisioning
```bash
# List connected devices
xcrun instruments -s devices

# Show provisioning profiles
xcrun security find-identity -v -p codesigning

# Verify code signature
xcrun codesign -dv --verbose=4 /path/to/app.app
```

### Build Tools
```bash
# Find Xcode path
xcrun --show-sdk-path

# Show SDK version
xcrun --show-sdk-version

# Find specific tool path
xcrun --find clang

# Compile with specific SDK
xcrun --sdk iphoneos clang main.c -o main
```

### Archives and Distribution
```bash
# Create an archive
xcrun xcodebuild archive -scheme MyApp -archivePath MyApp.xcarchive

# Export archive for App Store
xcrun xcodebuild -exportArchive -archivePath MyApp.xcarchive -exportPath ./export -exportOptionsPlist ExportOptions.plist

# Validate archive
xcrun altool --validate-app -f MyApp.ipa -t ios --apiKey API_KEY --apiIssuer ISSUER_ID

# Upload to App Store
xcrun altool --upload-app -f MyApp.ipa -t ios --apiKey API_KEY --apiIssuer ISSUER_ID
```

## Useful Combinations

### Quick Simulator Reset and Launch
```bash
# Reset all simulators and boot iPhone 15 Pro
xcrun simctl erase all && xcrun simctl boot "iPhone 15 Pro"
```

### Development Workflow
```bash
# Build, install, and launch app in one command
xcodebuild -scheme MyApp -destination 'platform=iOS Simulator,name=iPhone 15 Pro' && \
xcrun simctl install booted MyApp.app && \
xcrun simctl launch booted com.example.myapp
```

### Debugging Helpers
```bash
# Open simulator logs
xcrun simctl spawn booted log stream --predicate 'processImagePath endswith "MyApp"'

# Clear device logs
xcrun simctl erase booted
```

## Tips

- Use `booted` as device identifier to target the currently running simulator
- Replace device names with UDIDs for more reliable scripting
- Use `xcrun simctl help` for detailed command documentation
- Many commands support `--json` flag for machine-readable output
- Some operations require the simulator to be booted first

## Common Device Names
- iPhone 15 Pro Max
- iPhone 15 Pro
- iPhone 15 Plus
- iPhone 15
- iPhone 14 Pro Max
- iPhone 14 Pro
- iPad Pro (12.9-inch) (6th generation)
- iPad Air (5th generation)

*Use `xcrun simctl list devicetypes` to see all available device types*