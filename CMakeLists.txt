cmake_minimum_required(VERSION 3.3)
project(DecentralizedPokerDeckRNG
        LANGUAGES CXX
        VERSION 1.0.0)

find_package(DecentralizedPokerUtils REQUIRED)
find_package(DecentralizedPokerDeckInterface REQUIRED)

add_library(PokerDeckRNG main.cpp)
add_library(DecentralizedPoker::DeckRNG ALIAS PokerDeckRNG)

get_target_property(LIB_TYPE PokerDeckRNG TYPE)
if (LIB_TYPE STREQUAL "SHARED_LIBRARY")
    target_compile_definitions(PokerDeckRNG PUBLIC
            $<BUILD_INTERFACE:POKERDECKRNGLIB_SHARED_EXPORTS>
            $<INSTALL_INTERFACE:POKERDECKRNGLIB_SHARED_IMPORTS>)
    set_target_properties(PokerDeckRNG PROPERTIES POSITION_INDEPENDENT_CODE ON)
endif()

target_compile_features(PokerDeckRNG
        PUBLIC cxx_auto_type cxx_lambdas)
target_link_libraries(PokerDeckRNG
        PRIVATE DecentralizedPoker::Utils
        PUBLIC DecentralizedPoker::DeckInterface)

#=============== setup installation =======================
include(GNUInstallDirs)
ApplyInstallPackagePrefix(DecentralizedPokerDeckRNG)
include(CMakePackageConfigHelpers)
write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/DecentralizedPokerDeckRNGConfigVersion.cmake
        COMPATIBILITY SameMajorVersion)
install(TARGETS PokerDeckRNG
        EXPORT PokerDeckRNGTargets
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
install(EXPORT PokerDeckRNGTargets
        FILE "DecentralizedPokerDeckRNGConfig.cmake"
        NAMESPACE DecentralizedPoker::
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/DecentralizedPokerDeckRNG)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/DecentralizedPokerDeckRNGConfigVersion.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/DecentralizedPokerDeckRNG)
