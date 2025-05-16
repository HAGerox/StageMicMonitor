cmake_minimum_required(VERSION 3.22) # JUCE requires CMake 3.22 or higher
project(StageMicMonitor VERSION 0.1.0)

# Add JUCE as a subdirectory (assuming JUCE is in a folder named "JUCE" at the project root)
# You might need to adjust the path if your JUCE source is elsewhere or if you use git submodules
add_subdirectory(JUCE)

# Add your executable
# add_executable(StageMicMonitor_App Main.cpp) # Replace Main.cpp with your main source file(s)

# Use the juce_add_console_app function for a non-GUI application
juce_add_console_app(StageMicMonitor_App
    PRODUCT_NAME "StageMicMonitor"
    VERSION "0.1.0"
    # Add other properties as needed, e.g., COMPANY_COPYRIGHT, BUNDLE_ID for macOS
)

# target_sources(StageMicMonitor_App PRIVATE Main.cpp) # Add your source files here

# Update target_sources to point to Main.cpp within the Backend/Source directory
target_sources(StageMicMonitor_App PRIVATE Backend/Source/Main.cpp)

# Add the Backend/Source directory to the include path
target_include_directories(StageMicMonitor_App PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/Backend/Source)

# Specify the JUCE modules your project needs.
# For a basic audio backend, you'll likely need at least these:
target_link_libraries(StageMicMonitor_App PRIVATE
    juce::juce_audio_basics
    juce::juce_audio_devices
    juce::juce_core
    juce::juce_events
    # Add other modules as needed, e.g., juce::juce_audio_formats for file handling
)

# Generate the JuceHeader.h file for your target
juce_generate_juce_header(StageMicMonitor_App)

# Link against recommended JUCE warning and config flags
target_link_libraries(StageMicMonitor_App PUBLIC
    juce::juce_recommended_warning_flags
    juce::juce_recommended_config_flags
)

# If you only want to build a console application without any GUI elements,
# you can tell JUCE to not link against GUI libraries.
# This is often handled by juce_add_console_app, but if you are using add_executable directly
# and want to be explicit or if juce_add_console_app is not sufficient:
# target_compile_definitions(StageMicMonitor_App PRIVATE JUCE_AUDIOPROCESSOR_NO_GUI=1)

