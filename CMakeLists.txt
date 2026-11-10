list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/eil-cmake-utils/cmake)
include(eil_check_idf_features)
eil_check_idf_has_esp_drivers()

set(REQUIRED_COMPONENTS log color esp_idf_lib_helpers)

if(EIL_IDF_HAS_ESP_DRIVERS)
    list(APPEND REQUIRED_COMPONENTS esp_driver_spi esp_driver_gpio)
else()
    # Use driver component as esp_driver_* are not available before 5.3
    list(APPEND REQUIRED_COMPONENTS driver)
endif()

idf_component_register(
    SRCS led_strip_spi.c led_strip_spi_sk9822.c
    INCLUDE_DIRS .
    REQUIRES ${REQUIRED_COMPONENTS}
)
target_compile_options(${COMPONENT_LIB} PRIVATE -Werror=override-init)

include(eil_ci)
