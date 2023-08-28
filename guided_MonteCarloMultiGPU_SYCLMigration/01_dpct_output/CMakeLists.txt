if(MAX_GPU)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsycl -fsycl-targets=intel_gpu_pvc -Xsycl-target-backend '-options -ze-intel-enable-auto-large-GRF-mode' -D MAX_GPU")
else()
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fsycl ")
endif()
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS}")
SET(CMAKE_EXE_LINKER_FLAGS  "-pthread ${CMAKE_EXE_LINKER_FLAGS}")
SET(CMAKE_EXE_LINKER_FLAGS  "-qmkl ${CMAKE_EXE_LINKER_FLAGS}")
include_directories(${CMAKE_SOURCE_DIR}/01_dpct_output/Common/)
include_directories(${CMAKE_SOURCE_DIR}/01_dpct_output/include/)

add_executable (01_dpct_output Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarloMultiGPU.cpp.dp.cpp Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_gold.cpp.dp.cpp Samples/5_Domain_Specific/MonteCarloMultiGPU/MonteCarlo_kernel.dp.cpp Samples/5_Domain_Specific/MonteCarloMultiGPU/multithreading.cpp)
target_link_libraries(01_dpct_output sycl)

add_custom_target (run_sm_cpu  cd ${CMAKE_SOURCE_DIR}/01_dpct_output/ && ONEAPI_DEVICE_SELECTOR=opencl:cpu ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/01_dpct_output)
add_custom_target (run_sm_gpu  cd ${CMAKE_SOURCE_DIR}/01_dpct_output/ && ONEAPI_DEVICE_SELECTOR=level_zero:gpu ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/01_dpct_output)
add_custom_target (run_sm_gpu_opencl  cd ${CMAKE_SOURCE_DIR}/01_dpct_output/ && ONEAPI_DEVICE_SELECTOR=opencl:gpu ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/01_dpct_output)

