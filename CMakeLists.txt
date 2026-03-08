cmake_minimum_required(VERSION 3.22)

project(WeirdDrums VERSION 0.1.0)

# ============================================================
# 1.  JUCE — fetch it automatically (requires internet access)
#     OR set JUCE_PATH to a local clone and use add_subdirectory
# ============================================================
include(FetchContent)

FetchContent_Declare(
    JUCE
    GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
    GIT_TAG        7.0.12          # pin to a known-good release tag
    GIT_SHALLOW    TRUE
)
FetchContent_MakeAvailable(JUCE)

# If you already have JUCE cloned locally, comment-out the block above
# and use this instead (replace the path with your actual JUCE location):
#
#   set(JUCE_PATH "/path/to/JUCE")
#   add_subdirectory(${JUCE_PATH} JUCE EXCLUDE_FROM_ALL)

# ============================================================
# 2.  Plugin target
# ============================================================
juce_add_plugin(WeirdDrums
    # --- Identity ------------------------------------------------
    COMPANY_NAME            "Daniele Filaretti"
    PLUGIN_MANUFACTURER_CODE DFil
    PLUGIN_CODE             WDrm

    # --- Formats to build ----------------------------------------
    # Remove formats you don't need
    FORMATS                 VST3 Standalone AU      # AU only builds on macOS

    # --- Plugin type ---------------------------------------------
    IS_SYNTH                TRUE
    NEEDS_MIDI_INPUT        TRUE
    NEEDS_MIDI_OUTPUT       FALSE
    IS_MIDI_EFFECT          FALSE
    EDITOR_WANTS_KEYBOARD_FOCUS FALSE

    # --- Metadata ------------------------------------------------
    PRODUCT_NAME            "WeirdDrums"
    BUNDLE_ID               "com.danielefilaretti.weirddrums"
    PLUGIN_NAME             "WeirdDrums"
    DESCRIPTION             "Open Source Drum Synth Plugin"
    VST3_CATEGORIES         "Instrument" "Synth"

    # --- VST3 copy target (optional) ----------------------------
    # COPY_PLUGIN_AFTER_BUILD TRUE   # copies the built plugin to your DAW's plugin folder
)

# ============================================================
# 3.  Source files
#     These match the files found in the repo's Source/ folder.
#     If you find additional .cpp/.h files after cloning, add
#     them here in the same way.
# ============================================================
target_sources(WeirdDrums
    PRIVATE
        Source/PluginProcessor.cpp
        Source/PluginProcessor.h
        Source/PluginEditor.cpp
        Source/PluginEditor.h

        # DSP / Synthesis
        Source/Oscillator.h
        Source/Distortion.h

        Source/BottomSectionEditor.cpp
        Source/Grid2d.cpp
        Source/LabelledRotarySlider.cpp
        Source/LabelledSlider.cpp
        Source/TitleSectionEditor.cpp

)

# ============================================================
# 4.  Compile definitions
# ============================================================
target_compile_definitions(WeirdDrums
    PUBLIC
        JUCE_WEB_BROWSER=0
        JUCE_USE_CURL=0
        JUCE_VST3_CAN_REPLACE_VST2=0
        JUCE_DISPLAY_SPLASH_SCREEN=0
)

# ============================================================
# 5.  JUCE modules
# ============================================================
target_link_libraries(WeirdDrums
    PRIVATE
        juce::juce_audio_basics
        juce::juce_audio_devices
        juce::juce_audio_formats
        juce::juce_audio_plugin_client
        juce::juce_audio_processors
        juce::juce_audio_utils
        juce::juce_core
        juce::juce_data_structures
        juce::juce_dsp
        juce::juce_events
        juce::juce_graphics
        juce::juce_gui_basics
        juce::juce_gui_extra
    PUBLIC
        juce::juce_recommended_config_flags
        juce::juce_recommended_lto_flags
        juce::juce_recommended_warning_flags
)

# ============================================================
# 6.  C++ standard
# ============================================================
target_compile_features(WeirdDrums PRIVATE cxx_std_17)
