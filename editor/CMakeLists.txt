file(GLOB_RECURSE EditorSource ./**.cpp)
file(GLOB_RECURSE EditorHeaders ./**.h)
file(GLOB_RECURSE EditorLuaScripts ./**.lua)
file(GLOB_RECURSE EditorJSONs ./**.json)

set_source_files_properties(${EditorLuaScripts} ${EditorJSONs}
    PROPERTIES
        HEADER_FILE_ONLY TRUE
)

add_executable(Editor ${EditorSource} ${EditorHeaders} ${EditorLuaScripts} ${EditorJSONs})
set_property(TARGET Editor PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>DLL")

target_include_directories(Editor PUBLIC ../)
target_link_libraries(Editor PUBLIC Rootex)
add_dependencies(Editor Rootex Game)

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR}
    PREFIX "Editor"
    FILES ${EditorSource} ${EditorHeaders} ${EditorLuaScripts} ${EditorJSONs}
)

add_custom_command(TARGET Editor POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        $<TARGET_FILE:alut>
        $<TARGET_FILE_DIR:Editor>)
        
add_custom_command(TARGET Editor POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${OPENALSOFT_DLL_LIBRARY}
        $<TARGET_FILE_DIR:Editor>)

set_directory_properties(PROPERTIES 
    VS_STARTUP_PROJECT Editor
)
