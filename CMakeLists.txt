SET(CMAKE_SYSTEM_NAME Generic)
SET(CMAKE_SYSTEM_VERSION 1)
cmake_minimum_required(VERSION 3.7)

# specify the cross compiler
SET(CMAKE_C_COMPILER_WORKS 1)
SET(CMAKE_C_COMPILER arm-none-eabi-gcc)
SET(CMAKE_CXX_COMPILER_WORKS 1)
SET(CMAKE_CXX_COMPILER arm-none-eabi-g++)
set(AS arm-none-eabi-as)
set(AR arm-none-eabi-ar)
set(OBJCOPY arm-none-eabi-objcopy)
set(OBJDUMP arm-none-eabi-objdump)
set(SIZE arm-none-eabi-size)


SET(LINKER_SCRIPT ${CMAKE_SOURCE_DIR}/ble_app_blinky_gcc_nrf52.ld)

#Uncomment for hardware floating point
#SET(FPU_FLAGS "-mfloat-abi=hard -mfpu=fpv4-sp-d16")
#add_definitions(-DARM_MATH_CM4 -DARM_MATH_MATRIX_CHECK -DARM_MATH_ROUNDING -D__FPU_PRESENT=1 )

#Uncomment for software floating point
#SET(FPU_FLAGS "-mfloat-abi=soft")

SET(COMMON_FLAGS "-mcpu=cortex-m4 ${FPU_FLAGS} -mthumb -mthumb-interwork -ffunction-sections -fdata-sections -g -fno-common -fmessage-length=0 ${linkerFlags}")

SET(CMAKE_CXX_FLAGS_INIT "${COMMON_FLAGS} -std=c++11")
SET(CMAKE_C_FLAGS_INIT "${COMMON_FLAGS} -std=gnu99")
SET(CMAKE_EXE_LINKER_FLAGS_INIT "-Wl,-gc-sections,--print-memory-usage -T ${LINKER_SCRIPT}")

PROJECT(ble_app_blinky C CXX ASM)
set(CMAKE_CXX_STANDARD 11)

#add_definitions(-DARM_MATH_CM4 -DARM_MATH_MATRIX_CHECK -DARM_MATH_ROUNDING -D__FPU_PRESENT=1 )
add_definitions(-DNRF52832_XXAB -DBOARD_PCA10040)

SET(SDK_ROOT C:/private/pca10040/nRF5_SDK_15.2.0_9412b96)

file(GLOB SOURCES
        src/main.c
        ${SDK_ROOT}/modules/nrfx/mdk/gcc_startup_nrf52.S
        ${SDK_ROOT}/components/libraries/log/src/nrf_log_backend_rtt.c
        ${SDK_ROOT}/components/libraries/log/src/nrf_log_backend_serial.c
        ${SDK_ROOT}/components/libraries/log/src/nrf_log_backend_uart.c
        ${SDK_ROOT}/components/libraries/log/src/nrf_log_default_backends.c
        ${SDK_ROOT}/components/libraries/log/src/nrf_log_frontend.c
        ${SDK_ROOT}/components/libraries/log/src/nrf_log_str_formatter.c
        ${SDK_ROOT}/components/libraries/button/app_button.c
        ${SDK_ROOT}/components/libraries/util/app_error.c
        ${SDK_ROOT}/components/libraries/util/app_error_handler_gcc.c
        ${SDK_ROOT}/components/libraries/util/app_error_weak.c
        ${SDK_ROOT}/components/libraries/scheduler/app_scheduler.c
        ${SDK_ROOT}/components/libraries/timer/app_timer.c
        ${SDK_ROOT}/components/libraries/util/app_util_platform.c
        ${SDK_ROOT}/components/libraries/hardfault/hardfault_implementation.c
        ${SDK_ROOT}/components/libraries/util/nrf_assert.c
        ${SDK_ROOT}/components/libraries/atomic_fifo/nrf_atfifo.c
        ${SDK_ROOT}/components/libraries/atomic_flags/nrf_atflags.c
        ${SDK_ROOT}/components/libraries/atomic/nrf_atomic.c
        ${SDK_ROOT}/components/libraries/balloc/nrf_balloc.c
        ${SDK_ROOT}/external/fprintf/nrf_fprintf.c
        ${SDK_ROOT}/external/fprintf/nrf_fprintf_format.c
        ${SDK_ROOT}/components/libraries/memobj/nrf_memobj.c
        ${SDK_ROOT}/components/libraries/pwr_mgmt/nrf_pwr_mgmt.c
        ${SDK_ROOT}/components/libraries/ringbuf/nrf_ringbuf.c
        ${SDK_ROOT}/components/libraries/experimental_section_vars/nrf_section_iter.c
        ${SDK_ROOT}/components/libraries/strerror/nrf_strerror.c
        ${SDK_ROOT}/modules/nrfx/mdk/system_nrf52.c
        ${SDK_ROOT}/components/boards/boards.c
        ${SDK_ROOT}/integration/nrfx/legacy/nrf_drv_clock.c
        ${SDK_ROOT}/integration/nrfx/legacy/nrf_drv_uart.c
        ${SDK_ROOT}/modules/nrfx/drivers/src/nrfx_clock.c
        ${SDK_ROOT}/modules/nrfx/drivers/src/nrfx_gpiote.c
        ${SDK_ROOT}/modules/nrfx/drivers/src/nrfx_power_clock.c
        ${SDK_ROOT}/modules/nrfx/drivers/src/prs/nrfx_prs.c
        ${SDK_ROOT}/modules/nrfx/drivers/src/nrfx_uart.c
        ${SDK_ROOT}/modules/nrfx/drivers/src/nrfx_uarte.c
        ${SDK_ROOT}/external/segger_rtt/SEGGER_RTT.c
        ${SDK_ROOT}/external/segger_rtt/SEGGER_RTT_Syscalls_GCC.c
        ${SDK_ROOT}/external/segger_rtt/SEGGER_RTT_printf.c
        ${SDK_ROOT}/components/ble/common/ble_advdata.c
        ${SDK_ROOT}/components/ble/common/ble_conn_params.c
        ${SDK_ROOT}/components/ble/common/ble_conn_state.c
        ${SDK_ROOT}/components/ble/common/ble_srv_common.c
        ${SDK_ROOT}/components/ble/nrf_ble_gatt/nrf_ble_gatt.c
        ${SDK_ROOT}/components/ble/nrf_ble_qwr/nrf_ble_qwr.c
        ${SDK_ROOT}/external/utf_converter/utf.c
        ${SDK_ROOT}/components/ble/ble_services/ble_lbs/ble_lbs.c
        ${SDK_ROOT}/components/softdevice/common/nrf_sdh.c
        ${SDK_ROOT}/components/softdevice/common/nrf_sdh_ble.c
        ${SDK_ROOT}/components/softdevice/common/nrf_sdh_soc.c
        )

include_directories(
        inc
        ${SDK_ROOT}/components/nfc/ndef/generic/message
        ${SDK_ROOT}/components/nfc/t2t_lib
        ${SDK_ROOT}/components/nfc/t4t_parser/hl_detection_procedure
        ${SDK_ROOT}/components/ble/ble_services/ble_ancs_c
        ${SDK_ROOT}/components/ble/ble_services/ble_ias_c
        ${SDK_ROOT}/components/libraries/pwm
        ${SDK_ROOT}/components/softdevice/s132/headers/nrf52
        ${SDK_ROOT}/components/libraries/usbd/class/cdc/acm
        ${SDK_ROOT}/components/libraries/usbd/class/hid/generic
        ${SDK_ROOT}/components/libraries/usbd/class/msc
        ${SDK_ROOT}/components/libraries/usbd/class/hid
        ${SDK_ROOT}/modules/nrfx/hal
        ${SDK_ROOT}/components/nfc/ndef/conn_hand_parser/le_oob_rec_parser
        ${SDK_ROOT}/components/libraries/log
        ${SDK_ROOT}/components/ble/ble_services/ble_gls
        ${SDK_ROOT}/components/libraries/fstorage
        ${SDK_ROOT}/components/nfc/ndef/text
        ${SDK_ROOT}/components/libraries/mutex
        ${SDK_ROOT}/components/libraries/gpiote
        ${SDK_ROOT}/components/libraries/bootloader/ble_dfu
        ${SDK_ROOT}/components/nfc/ndef/connection_handover/common
        ${SDK_ROOT}/components/boards
        ${SDK_ROOT}/components/nfc/ndef/generic/record
        ${SDK_ROOT}/components/nfc/t4t_parser/cc_file
        ${SDK_ROOT}/components/ble/ble_advertising
        ${SDK_ROOT}/external/utf_converter
        ${SDK_ROOT}/components/ble/ble_services/ble_bas_c
        ${SDK_ROOT}/modules/nrfx/drivers/include
        ${SDK_ROOT}/components/libraries/experimental_task_manager
        ${SDK_ROOT}/components/ble/ble_services/ble_hrs_c
        ${SDK_ROOT}/components/nfc/ndef/connection_handover/le_oob_rec
        ${SDK_ROOT}/components/libraries/queue
        ${SDK_ROOT}/components/libraries/pwr_mgmt
        ${SDK_ROOT}/components/ble/ble_dtm
        ${SDK_ROOT}/components/toolchain/cmsis/include
        ${SDK_ROOT}/components/ble/ble_services/ble_rscs_c
        ${SDK_ROOT}/components/ble/common
        ${SDK_ROOT}/components/ble/ble_services/ble_lls
        ${SDK_ROOT}/components/nfc/ndef/connection_handover/ac_rec
        ${SDK_ROOT}/components/ble/ble_services/ble_bas
        ${SDK_ROOT}/components/libraries/mpu
        ${SDK_ROOT}/components/libraries/experimental_section_vars
        ${SDK_ROOT}/components/softdevice/s132/headers
        ${SDK_ROOT}/components/ble/ble_services/ble_ans_c
        ${SDK_ROOT}/components/libraries/slip
        ${SDK_ROOT}/components/libraries/delay
        ${SDK_ROOT}/components/libraries/mem_manager
        ${SDK_ROOT}/components/libraries/csense_drv
        ${SDK_ROOT}/components/libraries/memobj
        ${SDK_ROOT}/components/ble/ble_services/ble_nus_c
        ${SDK_ROOT}/components/softdevice/common
        ${SDK_ROOT}/components/ble/ble_services/ble_ias
        ${SDK_ROOT}/components/libraries/usbd/class/hid/mouse
        ${SDK_ROOT}/components/libraries/low_power_pwm
        ${SDK_ROOT}/components/nfc/ndef/conn_hand_parser/ble_oob_advdata_parser
        ${SDK_ROOT}/components/ble/ble_services/ble_dfu
        ${SDK_ROOT}/external/fprintf
        ${SDK_ROOT}/components/libraries/svc
        ${SDK_ROOT}/components/libraries/atomic
        ${SDK_ROOT}/components
        ${SDK_ROOT}/components/libraries/scheduler
        ${SDK_ROOT}/components/libraries/cli
        ${SDK_ROOT}/components/ble/ble_services/ble_lbs
        ${SDK_ROOT}/components/ble/ble_services/ble_hts
        ${SDK_ROOT}/components/libraries/crc16
        ${SDK_ROOT}/components/nfc/t4t_parser/apdu
        ${SDK_ROOT}/components/libraries/util
        ${SDK_ROOT}/components/libraries/usbd/class/cdc
        ${SDK_ROOT}/components/libraries/csense
        ${SDK_ROOT}/components/libraries/balloc
        ${SDK_ROOT}/components/libraries/ecc
        ${SDK_ROOT}/components/libraries/hardfault
        ${SDK_ROOT}/components/ble/ble_services/ble_cscs
        ${SDK_ROOT}/components/libraries/hci
        ${SDK_ROOT}/components/libraries/timer
        ${SDK_ROOT}/integration/nrfx
        ${SDK_ROOT}/components/nfc/t4t_parser/tlv
        ${SDK_ROOT}/components/libraries/sortlist
        ${SDK_ROOT}/components/libraries/spi_mngr
        ${SDK_ROOT}/components/libraries/led_softblink
        ${SDK_ROOT}/components/nfc/ndef/conn_hand_parser
        ${SDK_ROOT}/components/libraries/sdcard
        ${SDK_ROOT}/components/nfc/ndef/parser/record
        ${SDK_ROOT}/modules/nrfx/mdk
        ${SDK_ROOT}/components/ble/ble_services/ble_cts_c
        ${SDK_ROOT}/components/ble/ble_services/ble_nus
        ${SDK_ROOT}/components/libraries/twi_mngr
        ${SDK_ROOT}/components/ble/ble_services/ble_hids
        ${SDK_ROOT}/components/libraries/strerror
        ${SDK_ROOT}/components/libraries/crc32
        ${SDK_ROOT}/components/nfc/ndef/connection_handover/ble_oob_advdata
        ${SDK_ROOT}/components/nfc/t2t_parser
        ${SDK_ROOT}/components/nfc/ndef/connection_handover/ble_pair_msg
        ${SDK_ROOT}/components/libraries/usbd/class/audio
        ${SDK_ROOT}/components/nfc/t4t_lib/hal_t4t
        ${SDK_ROOT}/components/nfc/t4t_lib
        ${SDK_ROOT}/components/ble/peer_manager
        ${SDK_ROOT}/components/drivers_nrf/usbd
        ${SDK_ROOT}/components/libraries/ringbuf
        ${SDK_ROOT}/components/ble/ble_services/ble_tps
        ${SDK_ROOT}/components/nfc/ndef/parser/message
        ${SDK_ROOT}/components/ble/ble_services/ble_dis
        ${SDK_ROOT}/components/nfc/ndef/uri
        ${SDK_ROOT}/components/ble/nrf_ble_gatt
        ${SDK_ROOT}/components/ble/nrf_ble_qwr
        ${SDK_ROOT}/components/libraries/gfx
        ${SDK_ROOT}/components/libraries/button
        ${SDK_ROOT}/modules/nrfx
        ${SDK_ROOT}/components/libraries/twi_sensor
        ${SDK_ROOT}/integration/nrfx/legacy
        ${SDK_ROOT}/components/libraries/usbd/class/hid/kbd
        ${SDK_ROOT}/components/nfc/ndef/connection_handover/ep_oob_rec
        ${SDK_ROOT}/external/segger_rtt
        ${SDK_ROOT}/components/libraries/atomic_fifo
        ${SDK_ROOT}/components/ble/ble_services/ble_lbs_c
        ${SDK_ROOT}/components/nfc/ndef/connection_handover/ble_pair_lib
        ${SDK_ROOT}/components/libraries/crypto
        ${SDK_ROOT}/components/ble/ble_racp
        ${SDK_ROOT}/components/libraries/fds
        ${SDK_ROOT}/components/nfc/ndef/launchapp
        ${SDK_ROOT}/components/libraries/atomic_flags
        ${SDK_ROOT}/components/ble/ble_services/ble_hrs
        ${SDK_ROOT}/components/ble/ble_services/ble_rscs
        ${SDK_ROOT}/components/nfc/ndef/connection_handover/hs_rec
        ${SDK_ROOT}/components/nfc/t2t_lib/hal_t2t
        ${SDK_ROOT}/components/libraries/usbd
        ${SDK_ROOT}/components/nfc/ndef/conn_hand_parser/ac_rec_parser
        ${SDK_ROOT}/components/libraries/stack_guard
        ${SDK_ROOT}/components/libraries/log/src
)

add_executable(${PROJECT_NAME}.elf ${SOURCES} ${LINKER_SCRIPT})
set_target_properties(${PROJECT_NAME}.elf PROPERTIES LINKER_LANGUAGE CXX)

set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -Wl,-Map=${PROJECT_BINARY_DIR}/${PROJECT_NAME}.map -L ${SDK_ROOT}/modules/nrfx/mdk")

set(HEX_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.hex)
set(BIN_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.bin)

add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -Oihex $<TARGET_FILE:${PROJECT_NAME}.elf> ${HEX_FILE}
        COMMAND ${CMAKE_OBJCOPY} -Obinary $<TARGET_FILE:${PROJECT_NAME}.elf> ${BIN_FILE}
        COMMENT "Building ${HEX_FILE}
Building ${BIN_FILE}")

add_custom_target(UPLOAD
        arm-none-eabi-gdb -iex "target remote tcp:127.0.0.1:3333"
        -iex "monitor program $<TARGET_FILE:${PROJECT_NAME}.elf>"
        -iex "monitor reset init"
        -iex "disconnect" -iex "quit")