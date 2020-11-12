# Support plugins.
set(LLVM_NO_DEAD_STRIP 1)

set(LLVM_LINK_COMPONENTS
  ${LLVM_TARGETS_TO_BUILD}
  Analysis
  AsmParser
  AsmPrinter
  BitReader
  CBackendCodeGen
  CBackendInfo
  CodeGen
  Core
  IRReader
  MC
  ScalarOpts
  SelectionDAG
  Support
  Target
  )

if (NOT USE_SYSTEM_LLVM)
  add_llvm_tool(llvm-cbe
    llvm-cbe.cpp
    )
  export_executable_symbols(llvm-cbe)
endif()

if(USE_SYSTEM_LLVM)
  add_executable(llvm-cbe
    llvm-cbe.cpp
    )
  if(WIN32)
    llvm_map_components_to_libnames(llvm_libs
      ${LLVM_LINK_COMPONENTS}
      )
    target_link_libraries(llvm-cbe ${llvm_libs})
  else()
    llvm_map_components_to_libnames(llvm_libs
      CBackendCodeGen
      CBackendInfo
      )
    target_link_libraries(llvm-cbe LLVM ${llvm_libs})
  endif()
endif()
