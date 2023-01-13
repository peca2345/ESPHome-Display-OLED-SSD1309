# ESPHome Display OLED SSD1309 2.54" 128x64

## Description:

- MCU: ESP8266 (Wemos D1 mini)
- Input voltage: VCC 5V / microUSB 5V
- OLED Display SSD1309 2.54" 128x64 (YY_M242_OLED))

## Schema:

## ESPHome code:

esphome:
  name: garaz-oled-display

esp8266:
  board: d1_mini

logger:
captive_portal:
api:
  encryption:
    key: "Thv1BTI7DwFl0xDjO1zqdQH+Ev1t2n+WIcbbHhOGhys="
ota:
  password: "ea9dc65662cb9e0514153bdecc6cb58c"

wifi: # MAC 2C:F4:32:76:41:44
  ssid: !secret wifi_ssid
  password: !secret wifi_password
  manual_ip:
    static_ip: 192.168.1.213
    gateway: 192.168.1.1
    subnet: 255.255.255.0  
    dns1: 192.168.1.1
    dns2: 1.1.1.1

i2c:
  sda: D1
  scl: D2
  scan: false
  # setup_priority: -100

time:
  # - platform: homeassistant
  #   id: homeassistant_time
  - platform: sntp
    id: homeassistant_time

display:
  - platform: ssd1306_i2c
    model: "SSD1306 128x64"
    update_interval: 3s
    # setup_priority: -100
    address: 0x3C
    lambda: |-

      if (id(Dilna).has_state()) {          
        it.printf(102, 0, id(font1), TextAlign::TOP_LEFT, "D=%s", id(Dilna).state.c_str()); 
       }
      if (id(Kuchyn).has_state()) {        
        it.printf(102, 10, id(font1), TextAlign::TOP_LEFT, "K=%s", id(Kuchyn).state.c_str()); 
       }
      if (id(Obyvak).has_state()) {         
        it.printf(102, 20, id(font1), TextAlign::TOP_LEFT, "O=%s", id(Obyvak).state.c_str());
       }
      if (id(Otec).has_state()) {         
        it.printf(102, 30, id(font1), TextAlign::TOP_LEFT, "Z=%s", id(Otec).state.c_str());
       }     
      if (id(Loznice).has_state()) {        
        it.printf(102, 40, id(font1), TextAlign::TOP_LEFT, "L=%s", id(Loznice).state.c_str()); 
       } 
      if (id(Termostat_target).has_state()) {
        it.printf(102, 50, id(font1), TextAlign::TOP_LEFT, "T=%s", id(Termostat_target).state.c_str()); 
       }        
      if (id(Venkovni_teplota).has_state()) {        
        it.printf(77, 50, id(font1), TextAlign::TOP_LEFT, "V=%s", id(Venkovni_teplota).state.c_str());
       } 
      if (id(Venkovni_teplota).has_state()) {        
        it.printf(77, 50, id(font1), TextAlign::TOP_LEFT, "V=%s", id(Venkovni_teplota).state.c_str());
       } 

      it.strftime(64, 32, id(font2), TextAlign::CENTER, "%H:%M", id(homeassistant_time).now()); 

      if (id(Tepelko_cena_den).has_state()) {        
        it.printf(0, 0, id(font1), TextAlign::TOP_LEFT, "C=%s Kc", id(Tepelko_cena_den).state.c_str());
       } 
      if (id(Tepelko_F3_prikon).has_state()) {        
        it.printf(0, 50, id(font1), TextAlign::TOP_LEFT, "P=%s W", id(Tepelko_F3_prikon).state.c_str());
       } 


text_sensor:
  - platform: homeassistant
    id: Dilna
    entity_id: sensor.nasi_dilna_temp_dallas
  - platform: homeassistant
    id: Kuchyn
    entity_id: sensor.nasi_kuchyn_temp_dallas
  - platform: homeassistant
    id: Obyvak
    entity_id: sensor.nasi_obyvak_temp_wemos_teplota    
  - platform: homeassistant
    id: Otec
    entity_id: sensor.nasi_otec_temp_dallas
  - platform: homeassistant
    id: Loznice
    entity_id: sensor.nasi_loznice_temp_dallas
  - platform: homeassistant
    id: Termostat_target
    entity_id: sensor.Tepelko_target_temp_HA
  - platform: homeassistant
    id: Venkovni_teplota
    entity_id: sensor.gate2_parents_temperature
  - platform: homeassistant
    id: Tepelko_cena_den
    entity_id: sensor.tepelko_cena_den_2
  - platform: homeassistant
    id: Tepelko_kwh_den
    entity_id: sensor.tepelko_energie_den_2
  - platform: homeassistant
    id: Tepelko_F3_prikon
    entity_id: sensor.shelly_tepelko_channel_c_power





font: # https://fonts.google.com/
  - file: "gfonts://PT Sans Narrow"
    id: font1
    size: 10
  - file: "gfonts://PT Sans Narrow"
    id: font2
    size: 20    
  # - file: "gfonts://Abel"
  #   id: font2
  #   size: 8   
  # - file: "gfonts://Barlow Condensed"
  #   id: font3
  #   size: 10
  # - file: "gfonts://Kdam Thmor Pro"
  #   id: font4
  #   size: 8
  # - file: "gfonts://Roboto"
  #   id: font7
  #   size: 7
  # - file: "gfonts://Roboto"
  #   id: font8
  #   size: 8
  # - file: "gfonts://Roboto"
  #   id: font9
  #   size: 10



# graph:
#   # Show bare-minimum auto-ranged graph
#   - id: single_temperature_graph
#     sensor: my_temperature
#     duration: 1h
#     width: 151
#     height: 51
#   # Show multi-trace graph
#   - id: multi_temperature_graph
#     duration: 1h
#     x_grid: 10min
#     y_grid: 1.0     # degC/div
#     width: 151
#     height: 51
#     traces:
#       - sensor: my_inside_temperature
#         line_type: DASHED
#         line_thickness: 2
#         color: my_red
#       - sensor: my_outside_temperature
#         line_type: SOLID
#         line_thickness: 3
#         color: my_blue
#       - sensor: my_beer_temperature
#         line_type: DOTTED
#         line_thickness: 2
#         color: my_green
