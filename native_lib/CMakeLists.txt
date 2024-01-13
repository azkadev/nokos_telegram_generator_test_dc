cmake_minimum_required(VERSION 3.16)

project(nokos_telegram_generator_test_dc_library VERSION 0.0.1 LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 20)
 
message(STATUS "START COMPILE PACKAGE_TEMPLATE")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")

set(nokos_telegram_generator_test_dc_shared_dir "lib")
set(nokos_telegram_generator_test_dc_exe_dir "bin")
set(prefix_shared_library "lib")
 

if (ANDROID)
 set(package_shared_extension "_${ANDROID_ARCH_NAME}.so")
 set(package_exe_extension "")
 set(package_name "nokos_telegram_generator_test_dc_${ANDROID_ARCH_NAME}")
elseif (WIN32)
 set(package_shared_extension ".dll")
 set(package_exe_extension ".exe") 
 set(package_name "nokos_telegram_generator_test_dc")
elseif (APPLE)
 set(package_shared_extension ".dylib")
 set(package_exe_extension "") 
 set(package_name "nokos_telegram_generator_test_dc")
else()
 set(package_shared_extension ".so")
 set(package_exe_extension "") 
 set(package_name "nokos_telegram_generator_test_dc")
endif()


add_library(nokos_telegram_generator_test_dc_shared SHARED nokos_telegram_generator_test_dc.cpp)
add_executable(nokos_telegram_generator_test_dc_exe nokos_telegram_generator_test_dc.cpp)

set_target_properties(nokos_telegram_generator_test_dc_shared PROPERTIES 
  LIBRARY_OUTPUT_DIRECTORY "${nokos_telegram_generator_test_dc_shared_dir}"
  PREFIX "${prefix_shared_library}"
  OUTPUT_NAME "${package_name}"
)

set_target_properties(nokos_telegram_generator_test_dc_exe PROPERTIES 
  RUNTIME_OUTPUT_DIRECTORY "${nokos_telegram_generator_test_dc_exe_dir}"
  OUTPUT_NAME "${package_name}"
)

if (WIN32)
 set_target_properties(nokos_telegram_generator_test_dc_shared PROPERTIES
  WINDOWS_EXPORT_ALL_SYMBOLS ON
)
endif (WIN32)
 
add_custom_command(
  TARGET nokos_telegram_generator_test_dc_shared POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${nokos_telegram_generator_test_dc_shared_dir}/${prefix_shared_library}nokos_telegram_generator_test_dc${package_shared_extension}"
)

add_custom_command(
  TARGET nokos_telegram_generator_test_dc_exe POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${nokos_telegram_generator_test_dc_exe_dir}/nokos_telegram_generator_test_dc${package_exe_extension}" 
)

 