cmake_minimum_required(VERSION 3.23.3)

# project(COM LANGUAGES CXX)

# find_package(CSerialPort)
# if (CSerialPort_FOUND)
# 	include_directories(${CSerialPort_INCLUDE_DIR})
# 	add_executable( ${PROJECT_NAME} main.cpp)
# 	target_link_libraries (${PROJECT_NAME} ${CSerialPort_LIBRARY})
# else()
# 	message(STATUS "Not found system CSerialPort")
# endif ()

project(COM LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


# add by itas109
set(CSerialPortRootPath "${PROJECT_SOURCE_DIR}/CSerialPort")
include_directories(${CSerialPortRootPath}/include)
list(APPEND CSerialPortSourceFiles ${CSerialPortRootPath}/src/SerialPort.cpp ${CSerialPortRootPath}/src/SerialPortBase.cpp ${CSerialPortRootPath}/src/SerialPortInfo.cpp ${CSerialPortRootPath}/src/SerialPortInfoBase.cpp)
if (WIN32)
    list(APPEND CSerialPortSourceFiles ${CSerialPortRootPath}/src/SerialPortInfoWinBase.cpp ${CSerialPortRootPath}/src/SerialPortWinBase.cpp)
else (UNIX)
    list(APPEND CSerialPortSourceFiles ${CSerialPortRootPath}/src/SerialPortInfoUnixBase.cpp ${CSerialPortRootPath}/src/SerialPortUnixBase.cpp)
endif()
# end by itas109

add_executable(${PROJECT_NAME}
    main.cpp
    ${CSerialPortSourceFiles} # add by itas109
)

# add by itas109
if (WIN32)
        # for function availableFriendlyPorts
        target_link_libraries( ${PROJECT_NAME} setupapi)
elseif(APPLE)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(FOUNDATION_LIBRARY Foundation)
    target_link_libraries( ${PROJECT_NAME} ${FOUNDATION_LIBRARY} ${IOKIT_LIBRARY})
elseif(UNIX)
        target_link_libraries( ${PROJECT_NAME} pthread)
endif ()
# end by itas109




#cmake -DCMAKE_PREFIX_PATH="/Users/honey/Desktop/COM/CSerialPort/bin/install" ..