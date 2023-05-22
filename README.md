# Add to the begining of yuzu.sh found in the home/Deck/emulation/tools/launchers folder (from the cemu install)

#!/bin/bash

# Fetch the latest release page
latest_page=$(curl -Ls -o /dev/null -w %{url_effective} https://github.com/pineappleEA/pineapple-src/releases/latest)

# Extract the version number
version=$(echo $latest_page | grep -oP "EA-\K\d+")

# Construct the AppImage download URL
appimage_url="https://github.com/pineappleEA/pineapple-src/releases/download/EA-${version}/Linux-Yuzu-EA-${version}.AppImage"

# Check if the flag file exists
flag_file_path="/home/deck/Applications/${version}"
if [ ! -f "$flag_file_path" ]; then
    # Download the AppImage
    wget $appimage_url

    # Move the AppImage to the desired location
    mv -f ./Linux-Yuzu-EA-${version}.AppImage /home/deck/Applications/Yuzu.AppImage

    # Make the AppImage executable
    chmod +x /home/deck/Applications/Yuzu.AppImage

    # Create a flag file with the version as the filename
    touch $flag_file_path
else
    echo "AppImage version $version already exists. Skipping download."
fi
