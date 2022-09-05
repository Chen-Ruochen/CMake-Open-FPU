## 添加库  
在工程目录下新建文件夹cmsis_dsp  

找到CubeMX下载过的库，默认路径在```C:/Users/用户名/STM32Cube/Repository```  
找到对应MCU的库，如```STM32Cube_FW_F4_V1.27.1```  

- 进入```STM32Cube_FW_F4_V1.27.1/Drivers/CMSIS/DSP```  
将Source文件夹及Include文件夹复制到cmsis_dsp  

- 进入```STM32Cube_FW_F4_V1.27.1/Drivers/CMSIS/Lib```  
将GCC文件夹复制到cmsis_dsp  
## 修改Cmake文件  
打开工程，打开```CMakeLists_template.txt```文件  
找到如下代码：
```
#Uncomment for hardware floating point
#add_compile_definitions(ARM_MATH_CM4;ARM_MATH_MATRIX_CHECK;ARM_MATH_ROUNDING)
#add_compile_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
#add_link_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
```
将2-4行开头的#删除：
```
#Uncomment for hardware floating point
add_compile_definitions(ARM_MATH_CM4;ARM_MATH_MATRIX_CHECK;ARM_MATH_ROUNDING)
add_compile_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
add_link_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
```
在```add_executable($${PROJECT_NAME}.elf $${SOURCES} $${LINKER_SCRIPT})```下面添加代码：  
```
#Add dsp include
include_directories(cmsis_dsp/Include)
#Add dsp lib
target_link_libraries(${PROJECT_NAME}.elf ${CMAKE_SOURCE_DIR}/cmsis_dsp/GCC/libarm_cortexM4lf_math.a)
```
## 使用
添加```arm_math.h```头文件到代码中即可使用相关函数