if(IDF_VER) # ESP-IDF component (ESP32)

    if(IDF_VERSION_MAJOR GREATER_EQUAL 5)
        if(IDF_VERSION_MINOR EQUAL 2)
            list(APPEND priv_requires driver)
        endif()
        if(IDF_VERSION_MINOR EQUAL 3)
            list(APPEND priv_requires esp_driver_ledc)
            list(APPEND requires esp_driver_gpio)
        endif()
    endif()
    list(APPEND priv_requires esp_timer)

    idf_component_register(
        SRCS "src/jled_base.cpp" "src/esp32_hal.cpp"  
        REQUIRES ${requires}
        PRIV_REQUIRES ${priv_requires}
        INCLUDE_DIRS "src")

    idf_build_set_property(COMPILE_OPTIONS "-DESP32" APPEND)
    set_target_properties(${TARGET} PROPERTIES LINKER_LANGUAGE CXX)

else() # Raspberry Pi Pico
	add_library (JLed src/jled_base.cpp)
	target_include_directories (JLed PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src)
endif()
