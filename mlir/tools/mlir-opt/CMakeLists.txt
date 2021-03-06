set(LLVM_OPTIONAL_SOURCES
  null.cpp
)

set(LIB_LIBS
  MLIRAnalysis
  MLIRLLVMIR
  MLIROptMain
  MLIRParser
  MLIRPass
  MLIRTransforms
  MLIRSupport
)
add_llvm_library(MLIRMlirOptLib
  mlir-opt.cpp
)
target_link_libraries(MLIRMlirOptLib
  ${LIB_LIBS}
)

set(LIBS
  MLIRAnalysis
  MLIRAffineOps
  MLIRAffineToStandard
  MLIRDialect
  MLIRLoopsToGPU
  MLIRLinalgToLLVM

  MLIRLoopToStandard
  MLIREDSC
  MLIRFxpMathOps
  MLIRGPU
  MLIRGPUtoNVVMTransforms
  MLIRGPUtoROCDLTransforms
  MLIRGPUtoSPIRVTransforms
  MLIRLinalgOps
  MLIRLinalgAnalysis
  MLIRLinalgEDSC
  MLIRLinalgTransforms
  MLIRLinalgUtils
  MLIRLLVMIR
  MLIRLoopOps
  MLIRNVVMIR
  MLIROpenMP
  MLIROptMain
  MLIRParser
  MLIRPass
  MLIRQuantizerFxpMathConfig
  MLIRQuantizerSupport
  MLIRQuantizerTransforms
  MLIRQuantOps
  MLIRROCDLIR
  MLIRSPIRV
  MLIRLinalgToSPIRVTransforms
  MLIRStandardToSPIRVTransforms
  MLIRSPIRVTestPasses
  MLIRSPIRVTransforms
  MLIRStandardOps
  MLIRStandardToLLVM
  MLIRTransforms
  MLIRTransformUtils
  MLIRTestDialect
  MLIRTestIR
  MLIRTestPass
  MLIRTestTransforms
  MLIRSupport
  MLIRVectorOps
  MLIRVectorToLLVM
  MLIRVectorToLoops
)
if(MLIR_CUDA_CONVERSIONS_ENABLED)
  list(APPEND LIBS
    MLIRGPUtoCUDATransforms
  )
endif()
add_llvm_tool(mlir-opt
 mlir-opt.cpp
)
llvm_update_compile_flags(mlir-opt)
# It is necessary to use whole_archive_link to ensure that all static
# initializers are called.  However, whole_archive_link libraries cannot
# also be target_link_libraries
whole_archive_link(mlir-opt ${LIBS})
target_link_libraries(mlir-opt PRIVATE MLIRIR MLIROptMain LLVMSupport LLVMCore LLVMAsmParser)
