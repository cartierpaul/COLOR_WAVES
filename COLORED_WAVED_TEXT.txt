#!/bin/bash

# Text to display
text="Get the birthday started!"

# Define contrasting color pairs
colors=("1;31" "1;34" "1;32" "1;33" "1;35" "1;36" "1;91" "1;94" "1;92" "1;93" "1;95" "1;96")  # Red, Blue, Green, Yellow, Purple, Cyan, Light Red, Light Blue, Light Green, Light Yellow, Light Purple, Light Cyan

# Function to generate frames with moving colors
generate_frame() {
    local frame_number=$1
    local num_pairs=${#colors[@]}
    local frame=""
    for (( i=0; i<${#text}; i++ )); do
        local color_index=$(( (i + frame_number) % num_pairs ))
        local color=${colors[$color_index]}
        frame+="\033[${color}m${text:$i:1}"
    done
    frame+="\033[0m"
    clear 2>/dev/null
    echo -e "$frame"
}

# Number of frames
num_frames=40

trap 'exit' INT  # Terminate script when Ctrl+C is pressed

# Generate and display frames
while true; do
    for (( i=1; i<=$num_frames; i++ )); do
        frame=$(generate_frame $i)
        tput home  # Move cursor to the top
        clear 2>/dev/null
        echo -e "$frame" 2>/dev/null
        sleep 0.055  # Adjust the sleep duration to control the animation speed
    done
done