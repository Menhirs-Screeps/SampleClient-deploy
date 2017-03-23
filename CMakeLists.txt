cmake_minimum_required (VERSION 2.8.8)

project (ScreepsApi-deloy)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -g -ggdb")

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set ( LocalInstallDir ${CMAKE_SOURCE_DIR}/install )
set ( GithubRepositoryRoot https://github.com/Menhirs-Screeps)

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

CloneExternal ( Json ${GithubRepositoryRoot}/json.git
                "-DBuildTests=OFF"
            )

CloneExternal ( WebApi ${GithubRepositoryRoot}/WebApi.git
                ""
            )

CloneExternal ( ScreepsApi ${GithubRepositoryRoot}/ScreepsApi.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/Json/cmake \
                "
            )
add_dependencies(ScreepsApi Json)

CloneExternal ( ScreepsApiV12 ${GithubRepositoryRoot}/ScreepsApi-V12.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/Json/cmake \
                -DScreepsApi_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApi/cmake \
                "
            )
add_dependencies(ScreepsApiV12 Json)
add_dependencies(ScreepsApiV12 ScreepsApi)

CloneExternal ( TerminalClient ${GithubRepositoryRoot}/Screeps-TerminalClient.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/Json/cmake \
                -DSimple-Web-Server_DIR=${CMAKE_SOURCE_DIR}/external/WebApi/cmake \
                -DScreepsApi_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApi/cmake \
                -DScreepsApiV12_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApiV12/cmake \
                "
            )
add_dependencies(TerminalClient Json)
add_dependencies(TerminalClient WebApi)
add_dependencies(TerminalClient ScreepsApi)
add_dependencies(TerminalClient ScreepsApiV12)
