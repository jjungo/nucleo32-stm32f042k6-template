cmake_minimum_required(VERSION 3.5.0)
project(nucleo32-stm32f042k6-base C ASM)

SET(MCU_TARGET STM32F042x6)

SET(ARCH -march=armv6-m)
SET(TUNE -mtune=cortex-m0)
SET(SPECS -specs=nano.specs)
SET(WARN_FLAGS "-Wall -Wextra")
SET(LINKER_SCRIPT ${CMAKE_SOURCE_DIR}/STM32F042K6Tx_FLASH.ld)

SET(HEX_FILE ${PROJECT_NAME}.hex)
SET(BIN_FILE ${PROJECT_NAME}.bin)

SET(CMAKE_SYSTEM_NAME Generic)
SET(CMAKE_SYSTEM_VERSION 1)

SET(COMMON_FLAGS "-mthumb ${ARCH} ${TUNE} ${FLOAT-ABI} ${FPU} -ffunction-sections -fdata-sections -MMD -MP ${WARN_FLAGS} ${SPEC}")

if (CMAKE_BUILD_TYPE MATCHES Debug)
    add_definitions(-DDEBUG=1)
    SET(COMMON_FLAGS "${COMMON_FLAGS} -g -gdwarf-2")
elseif (CMAKE_BUILD_TYPE MATCHES Release)
    SET(COMMON_FLAGS "${COMMON_FLAGS} -O3")
endif ()

SET(CMAKE_C_COMPILER arm-none-eabi-gcc)
SET(CMAKE_CXX_FLAGS "${COMMON_FLAGS} -std=c++14")
SET(CMAKE_C_FLAGS "${COMMON_FLAGS} -std=c11")
SET(CMAKE_EXE_LINKER_FLAGS " -lm -lnosys -fstack-usage -Wl,-gc-sections -Xlinker -Map=${PROJECT_NAME}.map -T ${LINKER_SCRIPT}")
SET(CMAKE_ASM_FLAGS "${COMMON_FLAGS}")
SET(CMAKE_OBJCOPY arm-none-eabi-objcopy)
SET(CMAKE_SIZE arm-none-eabi-size)
SET(ROOT_SOURCES ${CMAKE_SOURCE_DIR})
SET(INCLUDES_PATH ${ROOT_SOURCES}/Inc)

add_definitions(-D${MCU_TARGET} -DUSE_HAL_DRIVER)
set(SOURCES
        ${ROOT_SOURCES}/Src/gpio.c
        ${ROOT_SOURCES}/Src/i2c.c
        ${ROOT_SOURCES}/Src/main.c
        ${ROOT_SOURCES}/Src/spi.c
        ${ROOT_SOURCES}/Src/stm32f0xx_hal_msp.c
        ${ROOT_SOURCES}/Src/stm32f0xx_it.c
        ${ROOT_SOURCES}/Src/system_stm32f0xx.c
        ${ROOT_SOURCES}/Src/usart.c
        )

set(HAL_SOURCES
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_adc_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_can.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_cec.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_comp.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_cortex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_crc.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_crc_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_dac.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_dac_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_dma.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_flash.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_flash_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_gpio.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_i2c.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_i2c_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_i2s.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_irda.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_iwdg.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_pcd.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_pcd_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_pwr.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_pwr_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_rcc.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_rcc_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_rtc.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_rtc_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_smartcard.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_smartcard_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_smbus.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_spi.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_spi_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_tim.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_tim_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_tsc.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_uart.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_uart_ex.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_usart.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_hal_wwdg.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_adc.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_comp.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_crc.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_crs.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_dac.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_dma.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_exti.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_gpio.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_i2c.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_pwr.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_rcc.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_rtc.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_spi.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_tim.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_usart.c
        ${ROOT_SOURCES}/Drivers//STM32F0xx_HAL_Driver/Src/stm32f0xx_ll_utils.c
        )


SET(VENDOR_SRC
        ${HAL_SOURCES}
        ${ROOT_SOURCES}/startup_stm32f042x6.s
        ${ROOT_SOURCES}/Src/vendor/Segger/RTT_Syscalls_GCC.c
        ${ROOT_SOURCES}/Src/vendor/Segger/SEGGER_RTT.c
        ${ROOT_SOURCES}/Src/vendor/Segger/SEGGER_RTT_printf.c
        )

set_property(SOURCE
        ${ROOT_SOURCES}/startup_stm32f042x6.s
        PROPERTY LANGUAGE C)


include_directories(${ROOT_SOURCES}
        ${ROOT_SOURCES}/Inc
        ${ROOT_SOURCES}/Drivers/STM32F0xx_HAL_Driver/Inc
        ${ROOT_SOURCES}/Drivers/CMSIS/Device/ST/STM32F0xx/Include
        ${ROOT_SOURCES}/Drivers/CMSIS/Include)


add_executable(${PROJECT_NAME}.elf ${SOURCES} ${VENDOR_SRC} ${LINKER_SCRIPT})

add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -Oihex $<TARGET_FILE:${PROJECT_NAME}.elf> ${HEX_FILE}
        COMMAND ${CMAKE_OBJCOPY} -Obinary $<TARGET_FILE:${PROJECT_NAME}.elf> ${BIN_FILE}
        COMMAND ${CMAKE_SIZE} ${PROJECT_NAME}.elf
        COMMENT "Building ${HEX_FILE} \nBuilding ${BIN_FILE}")
