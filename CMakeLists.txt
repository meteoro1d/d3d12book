cmake_minimum_required(VERSION 3.20)

project(d3d12book)

set(CMAKE_CXX_STANDARD 20)

set(COMMON_INCLUDE "Common")
set(COMMON_SRC "Common/Camera.cpp"
        "Common/d3dApp.cpp"
        "Common/d3dUtil.cpp"
        "Common/DDSTextureLoader.cpp"
        "Common/GameTimer.cpp"
        "Common/GeometryGenerator.cpp"
        "Common/MathHelper.cpp")

# Chapter1
add_executable(Chapter1 "Chapter 1 Vector Algebra/XMVECTOR/xmvec3.cpp")

# Chapter2
add_executable(Chapter2 "Chapter 2 Matrix Algebra/XMMATRIX/xmmatrix.cpp")

function(CopySubDirInSource targetName sourceDir subDir)
    file(GLOB filePaths "${CMAKE_SOURCE_DIR}/${sourceDir}/${subDir}/*")
    foreach (filePath ${filePaths})
        get_filename_component(fileName ${filePath} NAME)
        add_custom_command(TARGET "${targetName}" POST_BUILD
                COMMAND ${CMAKE_COMMAND} -E copy_if_different
                ${filePath}
                "$<TARGET_FILE_DIR:${targetName}>/${subDir}/${fileName}")
    endforeach ()
endfunction()

# add_exe function
function(AddTarget targetName sourceDir)
    aux_source_directory("${CMAKE_CURRENT_SOURCE_DIR}/${sourceDir}" sourceFiles)
    add_executable(${targetName} WIN32 "${sourceFiles}" "${COMMON_SRC}")
    target_include_directories(${targetName} INTERFACE "${sourceDir}" "${COMMON_INCLUDE}")
    target_compile_options(${targetName} PRIVATE "/permissive")
    target_compile_definitions("${targetName}" PRIVATE "_UNICODE" "UNICODE")
    set_target_properties(${targetName} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})


    CopySubDirInSource(${targetName} ${sourceDir} "Shaders")
    CopySubDirInSource(${targetName} ${sourceDir} "Models")
endfunction()


# Chapter4
AddTarget(Chapter4 "Chapter 4 Direct3D Initialization/Init Direct3D")

# Chapter6
AddTarget(Chapter6 "Chapter 6 Drawing in Direct3D/Box")

# Chapter7_1
AddTarget(Chapter7_1 "Chapter 7 Drawing in Direct3D Part II/LandAndWaves")

# Chapter7_2
AddTarget(Chapter7_2 "Chapter 7 Drawing in Direct3D Part II/Shapes")

# Chapter8_1
AddTarget(Chapter8_1 "Chapter 8 Lighting/LitColumns")

# Chapter8_2
AddTarget(Chapter8_2 "Chapter 8 Lighting/LitWaves")

# Chapter9_1
AddTarget(Chapter9_1 "Chapter 9 Texturing/Crate")

# Chapter9_2
AddTarget(Chapter9_2 "Chapter 9 Texturing/TexColumns")

# Chapter9_3
AddTarget(Chapter9_3 "Chapter 9 Texturing/TexWaves")

# Chapter10
AddTarget(Chapter10 "Chapter 10 Blending/BlendDemo")

# Chapter11
AddTarget(Chapter11 "Chapter 11 Stenciling/StencilDemo")

# Chapter12
AddTarget(Chapter12 "Chapter 12 The Geometry Shader/TreeBillboards")

# Chapter13_1
AddTarget(Chapter13_1 "Chapter 13 The Compute Shader/Blur")

# Chapter13_2
AddTarget(Chapter13_2 "Chapter 13 The Compute Shader/SobelFilter")

# Chapter13_3
AddTarget(Chapter13_3 "Chapter 13 The Compute Shader/VecAdd")

# Chapter13_4
AddTarget(Chapter13_4 "Chapter 13 The Compute Shader/WavesCS")

# Chapter14_1
AddTarget(Chapter14_1 "Chapter 14 The Tessellation Stages/BasicTessellation")

# Chapter14_2
AddTarget(Chapter14_2 "Chapter 14 The Tessellation Stages/BezierPatch")

# Chapter15
AddTarget(Chapter15 "Chapter 15 First Person Camera and Dynamic Indexing/CameraAndDynamicIndexing")

# Chapter16
AddTarget(Chapter16 "Chapter 16 Instancing and Frustum Culling/InstancingAndCulling")

# Chapter17
AddTarget(Chapter17 "Chapter 17 Picking/Picking")

# Chapter18_1
AddTarget(Chapter18_1 "Chapter 18 Cube Mapping/CubeMap")

# Chapter18_2
AddTarget(Chapter18_2 "Chapter 18 Cube Mapping/DynamicCube")

# Chapter19
AddTarget(Chapter19 "Chapter 19 Normal Mapping/NormalMap")

# Chapter20
AddTarget(Chapter20 "Chapter 20 Shadow Mapping/Shadows")

# Chapter21
AddTarget(Chapter21 "Chapter 21 Ambient Occlusion/Ssao")

# Chapter22
AddTarget(Chapter22 "Chapter 22 Quaternions/QuatDemo")

# Chapter23
AddTarget(Chapter23 "Chapter 23 Character Animation/SkinnedMesh")