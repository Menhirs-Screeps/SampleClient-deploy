cmake_minimum_required (VERSION 2.8.8)

project (ScreepsApi-deloy)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -g -ggdb")

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set ( LocalInstallDir ${CMAKE_SOURCE_DIR}/install )

include (ExternalProject)
function ( CloneExternal name repository args )
    ExternalProject_Add(${name}
        GIT_REPOSITORY ${repository}
        GIT_TAG "master"
        SOURCE_DIR "${CMAKE_SOURCE_DIR}/external/${name}"
        CMAKE_ARGS ${args} -DCMAKE_INSTALL_PREFIX=${LocalInstallDir}
    )
endfunction ()

#                -Dnlohmann_json_DIR=${LocalInstallDir}/cmake
#                -DScreepsApi_DIR=${LocalInstallDir}/cmake
#                -DSimple-Web-Server_DIR=${LocalInstallDir}/cmake 

CloneExternal ( Json https://github.com/Lyon1-Asterix/json.git
                "-DBuildTests=OFF"
            )

CloneExternal ( SimpleWebServer https://github.com/Lyon1-Asterix/Simple-Web-Server.git
                ""
            )

CloneExternal ( ScreepsApi https://github.com/Lyon1-Asterix/ScreepsApi.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/Json/cmake \
                "
            )
add_dependencies(ScreepsApi Json)

CloneExternal ( ScreepsApiV12 https://github.com/Lyon1-Asterix/ScreepsApi-V12.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/Json/cmake \
                -DScreepsApi_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApi/cmake \
                "
            )
add_dependencies(ScreepsApiV12 Json)
add_dependencies(ScreepsApiV12 ScreepsApi)

CloneExternal ( ScreepsSimpleClient https://github.com/Lyon1-Asterix/Screeps-SampleClient.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/Json/cmake \
                -DSimple-Web-Server_DIR=${CMAKE_SOURCE_DIR}/external/SimpleWebServer/cmake \
                -DScreepsApi_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApi/cmake \
                -DScreepsApiV12_DIR=${LocalInstallDir}/cmake \
                "
            )
add_dependencies(ScreepsSimpleClient Json)
add_dependencies(ScreepsSimpleClient SimpleWebServer)
add_dependencies(ScreepsSimpleClient ScreepsApi)
add_dependencies(ScreepsSimpleClient ScreepsApiV12)
