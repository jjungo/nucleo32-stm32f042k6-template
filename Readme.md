A simple template for the STM32F042K6 dev kit.

### build

    mkdir build && cd build
    cmake ..
    make

## flash

     st-flash write build/nucleo32-stm32f042k6-base.bin 0x8000000
