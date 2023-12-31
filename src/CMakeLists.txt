cmake_minimum_required(VERSION 3.7)

project(TETRIS LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(SDL2 CONFIG REQUIRED)
find_package(sdl2-ttf CONFIG REQUIRED)

function(ADD_PRG)

	set(options OPTIONAL)
	set(oneValueArgs TARGET )
	set(multiValueArgs SOURCES)
	cmake_parse_arguments(ADD_PRG "${options}" "${oneValueArgs}" "${multiValueArgs}" ${ARGN} )

	add_executable(${ADD_PRG_TARGET} "")
	target_sources(${ADD_PRG_TARGET} PUBLIC ${ADD_PRG_SOURCES})
	if (${CMAKE_SYSTEM_NAME} MATCHES Linux)
		target_include_directories(${ADD_PRG_TARGET} PUBLIC /usr/include/SDL2)
		target_link_libraries(${ADD_PRG_TARGET} PUBLIC SDL2 SDL2main)
	else()
		target_link_libraries(${ADD_PRG_TARGET} PUBLIC SDL2::SDL2 SDL2::SDL2main SDL2::SDL2_ttf)
	endif()
	target_compile_definitions(${ADD_PRG_TARGET} PUBLIC _USE_MATH_DEFINES)

endfunction()

ADD_PRG(TARGET TETRIS SOURCES main.cpp Game.h Game.cpp GameCore.h GameCore.cpp Map.h Map.cpp Shape.h Shape.cpp Window.h Window.cpp Window.inl)