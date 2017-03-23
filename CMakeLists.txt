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

CloneExternal ( JsonApi ${GithubRepositoryRoot}/JsonApi.git
                "-DBuildTests=OFF"
            )

CloneExternal ( WebApi ${GithubRepositoryRoot}/WebApi.git
                ""
            )

CloneExternal ( WebsocketApi ${GithubRepositoryRoot}/WebsocketApi.git
                ""
            )

CloneExternal ( ScreepsApi ${GithubRepositoryRoot}/ScreepsApi.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/JsonApi/cmake \
                "
            )
add_dependencies(ScreepsApi JsonApi)

CloneExternal ( ScreepsApiV12 ${GithubRepositoryRoot}/ScreepsApi-V12.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/JsonApi/cmake \
                -DScreepsApi_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApi/cmake \
                "
            )
add_dependencies(ScreepsApiV12 JsonApi)
add_dependencies(ScreepsApiV12 ScreepsApi)

CloneExternal ( TerminalClient ${GithubRepositoryRoot}/Screeps-TerminalClient.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/JsonApi/cmake \
                -DSimple-Web-Server_DIR=${CMAKE_SOURCE_DIR}/external/WebApi/cmake \
                -DSimple-WebSocket-Server_DIR=${CMAKE_SOURCE_DIR}/external/WebsocketApi/cmake \
                -DScreepsApi_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApi/cmake \
                -DScreepsApiV12_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApiV12/cmake \
                "
            )
add_dependencies(TerminalClient JsonApi)
add_dependencies(TerminalClient WebApi)
add_dependencies(TerminalClient WebsocketApi)
add_dependencies(TerminalClient ScreepsApi)
add_dependencies(TerminalClient ScreepsApiV12)

CloneExternal ( TerminalConsole ${GithubRepositoryRoot}/Screeps-TerminalConsole.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/JsonApi/cmake \
                -DSimple-Web-Server_DIR=${CMAKE_SOURCE_DIR}/external/WebApi/cmake \
                -DSimple-WebSocket-Server_DIR=${CMAKE_SOURCE_DIR}/external/WebsocketApi/cmake \
                -DScreepsApi_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApi/cmake \
                -DScreepsApiV12_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApiV12/cmake \
                "
            )
add_dependencies(TerminalConsole JsonApi)
add_dependencies(TerminalConsole WebApi)
add_dependencies(TerminalConsole WebsocketApi)
add_dependencies(TerminalConsole ScreepsApi)
add_dependencies(TerminalConsole ScreepsApiV12)

CloneExternal ( RoomObserver ${GithubRepositoryRoot}/Screeps-RoomObserver.git
                " \
                -Dnlohmann_json_DIR=${CMAKE_SOURCE_DIR}/external/JsonApi/cmake \
                -DSimple-Web-Server_DIR=${CMAKE_SOURCE_DIR}/external/WebApi/cmake \
                -DSimple-WebSocket-Server_DIR=${CMAKE_SOURCE_DIR}/external/WebsocketApi/cmake \
                -DScreepsApi_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApi/cmake \
                -DScreepsApiV12_DIR=${CMAKE_SOURCE_DIR}/external/ScreepsApiV12/cmake \
                "
            )
add_dependencies(RoomObserver JsonApi)
add_dependencies(RoomObserver WebApi)
add_dependencies(RoomObserver WebsocketApi)
add_dependencies(RoomObserver ScreepsApi)
add_dependencies(RoomObserver ScreepsApiV12)
