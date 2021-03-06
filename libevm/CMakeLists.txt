
set(SOURCES
	ExtVMFace.cpp
	VM.cpp
	VMOpt.cpp
	VMCalls.cpp
	VMFactory.cpp
)

if (EVMJIT)
	list(APPEND SOURCES
		JitVM.cpp
		SmartVM.cpp
	)
endif()

file(GLOB HEADERS "*.h")

add_library(evm ${SOURCES} ${HEADERS})

find_package(Eth)

target_include_directories(evm PRIVATE ..)
target_link_libraries(evm ${Eth_ETHCORE_LIBRARIES})
target_link_libraries(evm ${Eth_EVMCORE_LIBRARIES})

if (EVMJIT)
	target_include_directories(evm PRIVATE ../evmjit/include)
	target_link_libraries(evm ${Eth_EVMJIT_LIBRARIES})
endif()

install(TARGETS evm RUNTIME DESTINATION bin ARCHIVE DESTINATION lib LIBRARY DESTINATION lib)
