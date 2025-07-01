# How to Extract Deployment Targets from an Xcode `.pbxproj` File Using Bash

When working with iOS projects that use CocoaPods, it's common to need visibility into deployment targets defined across various build configurations. The `.pbxproj` file inside your `Pods.xcodeproj` can get quite complex and hard to parse manually.

This article walks through a simple yet effective Bash script that extracts all iOS deployment targets from your Xcode project's build configurations and outputs them into a readable format.

## What the Script Does

- Reads the Pods.xcodeproj/project.pbxproj file.
- Extracts the IPHONEOS_DEPLOYMENT_TARGET from all XCBuildConfiguration sections.
- Associates each deployment target with its corresponding configuration name.
- Outputs the results to a text file named checking_deployment_targets.txt.

## The Script

```bash
#!/bin/bash

PBXPROJ="ios/Pods/Pods.xcodeproj/project.pbxproj"
OUTPUT_FILE="checking_deployment_targets.txt"
TEMP_FILE="tmp_pbxproj.txt"

rm -f "$OUTPUT_FILE" "$TEMP_FILE"
touch "$OUTPUT_FILE"

# Extract the build configuration part
awk '/Begin XCBuildConfiguration section/,/End XCBuildConfiguration section/' "$PBXPROJ" > "$TEMP_FILE"

# Iterate through each line to get the config name and deployment target
config=""
ios_target=""

while IFS= read -r line; do
  if echo "$line" | grep -q "baseConfigurationReference"; then
    # Extract config name
    config=$(echo "$line" | sed -E 's/.*\/\* (.*) \*\/.*/\1/')
  elif echo "$line" | grep -q "IPHONEOS_DEPLOYMENT_TARGET"; then
    ios_target=$(echo "$line" | sed -E 's/.*= ([0-9.]+);.*/\1/')
  elif echo "$line" | grep -q "};"; then
    if [[ -n "$config" && -n "$ios_target" ]]; then
      echo "$config: $ios_target" >> "$OUTPUT_FILE"
      config=""
      ios_target=""
    fi
  fi
done < "$TEMP_FILE"

rm -f "$TEMP_FILE"

echo "✅ Done! Result saved to $OUTPUT_FILE"
```

## Breakdown

Here’s a quick breakdown of what each section does:

- File Paths: Sets variables for the .pbxproj file, output file, and temporary file.
- Cleanup: Removes any existing output and temp files to ensure a clean run.
- Filter Section: Uses awk to extract only the XCBuildConfiguration section from the project file.
- Parse Loop: Reads the temporary file line by line to capture:
  - `baseConfigurationReference` → The build configuration name.
  - `IPHONEOS_DEPLOYMENT_TARGET` → The iOS minimum version for that config.
- Output: Pairs each config with its deployment target and writes it to the result file.

## Example Output

After running the script, your `checking_deployment_targets.txt` file might look like:

```txt
React-debug.release.xcconfig: 12.0
React-runtimescheduler.debug.xcconfig: 13.0
...
```

## Use Case

This script is handy when you:

- Need to audit deployment targets across multiple build configs.
- Want to ensure consistency across debug/release/staging environments.
- Troubleshoot compatibility issues with older iOS versions.

## Bonus Tip

If you want to extend this script, you could:

- Scan other `.xcodeproj` files in your workspace.
- Validate that deployment targets meet a minimum requirement.
- Automatically update inconsistent targets using `sed`.
