# OGC.Engineering
## ogc_deploy_thermal_inspection_tool_nrtps
Developer contact - dustin < at > ogc.engineering

---
### Location:

- Deployment using NRTPS OS:
    - location: deployments/ogc_deploy_thermal_inspection_tool_nrtps/
    - repo: https://github.com/OGC-dustin/ogc_deploy_thermal_inspection_tool_nrtps

---
### Deployment Options:
- at compile time, default options within repositories that are not redefined in deployment will provide warnings

---
### Resources:
- Software - Applications:
    - ogc_app_thermal_inspection_tool - directs system functionality
- Software - Libraries:
    - ogc_lib_os_nrtps - library providing the non real time polled scheduler ( nrtps ) operating system ( os ) functionality
    - ogc_lib_sd_card - libraries to handle sd card memory devices
    - ogc_lib_indicator_status_single - library to handle single color status indicators ( individual RGB Colors )
    - ogc_lib_indicator_status_multi - library to handle multi color status indicators ( mix of RGB to make specific colors )
    - ogc_lib_rgb_color_mixing - library handling simple and advanced color mixing calculations ( reference library to mix colors )
    - ogc_lib_encoder_dual_phase - library to track direction and position of a two phase encoder
- Firmware - Drivers:
    - ogc_drv_oled_ssd1351 - driver for SSD1351 RGB OLED
    - ogc_drv_ov7670 - driver for color camera sensor
    - ogc_drv_neopixel - driver to NeoPixel LEDs
    - ogc_drv_mlx90640 - driver for the MLX90640 IR Camera
    - ogc_drv_garmin_lidar_lite_v3 - driver for the Garmin Lidar Lite v3
- Firmware - Hal:
    - ogc_fw_thermal_inspection_tool - provides base level functionality and hardware abstraction to easy to use functions
- Firmware - CSP:
    - ogc_csp_ti_tm4c123g - provides chip level definitions and support drivers specific to the microprocessor family
- Hardware - Description:
    - ogc_hw_thermal_inspection_tool - provides hardware definitions, planned module definitions and pin assignment definitions
