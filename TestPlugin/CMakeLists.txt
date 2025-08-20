cmake_minimum_required(VERSION 3.22)

project(TestPlugin VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find JUCE
find_package(PkgConfig REQUIRED)
find_path(JUCE_MODULES_PATH
    NAMES juce_core/juce_core.h
    PATHS
        /Applications/JUCE/modules
        /usr/local/include/JUCE/modules
        /opt/homebrew/include/JUCE/modules
        ${CMAKE_PREFIX_PATH}/include/JUCE/modules
    DOC "Path to JUCE modules directory"
)

if(NOT JUCE_MODULES_PATH)
    message(FATAL_ERROR "Could not find JUCE modules. Please set JUCE_MODULES_PATH")
endif()

# Add JUCE
add_subdirectory(${JUCE_MODULES_PATH} JUCE EXCLUDE_FROM_ALL)

# Create the plugin target
juce_add_plugin(TestPlugin
    COMPANY_NAME "TestCompany"
    IS_SYNTH FALSE
    NEEDS_MIDI_INPUT TRUE
    NEEDS_MIDI_OUTPUT FALSE
    IS_MIDI_EFFECT FALSE
    EDITOR_WANTS_KEYBOARD_FOCUS FALSE
    COPY_PLUGIN_AFTER_BUILD TRUE
    PLUGIN_MANUFACTURER_CODE Test
    PLUGIN_CODE TsPl
    FORMATS AU VST3 Standalone
    PRODUCT_NAME "TestPlugin"
)

# Source files
target_sources(TestPlugin
    PRIVATE
        Source/PluginEditor.cpp
        Source/PluginEditor.h
        Source/PluginProcessor.cpp
        Source/PluginProcessor.h
)

# Preprocessor definitions
target_compile_definitions(TestPlugin
    PUBLIC
        JUCE_WEB_BROWSER=0
        JUCE_USE_CURL=0
        JUCE_VST3_CAN_REPLACE_VST2=0
        JUCE_STRICT_REFCOUNTEDPOINTER=1
)

# Link JUCE modules
target_link_libraries(TestPlugin
    PRIVATE
        juce::juce_audio_basics
        juce::juce_audio_devices
        juce::juce_audio_formats
        juce::juce_audio_plugin_client
        juce::juce_audio_processors
        juce::juce_audio_utils
        juce::juce_core
        juce::juce_data_structures
        juce::juce_events
        juce::juce_graphics
        juce::juce_gui_basics
        juce::juce_gui_extra
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags
)