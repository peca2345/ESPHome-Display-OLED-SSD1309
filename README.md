# ESPHome Display OLED SSD1309 2.54" 128x64

## Description:

- MCU: ESP8266 (Wemos D1 mini)
- Input voltage: VCC 5V / microUSB 5V
- OLED Display SSD1309 2.54" 128x64 (YY_M242_OLED))

## SHOP:
- OLED Display SSD1309 2.54" 128x64 YY_M242_OLED: [ALI]([https://www.youtube.com/watch?v=u_aKcf_F1MM](https://www.aliexpress.com/item/1005003950796751.html?spm=a2g0o.order_list.order_list_main.11.6d691802H1X2WS)) 

## Schema:

## ESPHome code:
```
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

wifi: 
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

      if (id(test1).has_state()) {          
        it.printf(102, 0, id(font1), TextAlign::TOP_LEFT, "D=%s", id(test1).state.c_str()); 
       }
      if (id(test2).has_state()) {        
        it.printf(102, 10, id(font1), TextAlign::TOP_LEFT, "K=%s", id(test2).state.c_str()); 
       }
      if (id(test3).has_state()) {         
        it.printf(102, 20, id(font1), TextAlign::TOP_LEFT, "O=%s", id(test3).state.c_str());
       }
      if (id(test4).has_state()) {         
        it.printf(102, 30, id(font1), TextAlign::TOP_LEFT, "Z=%s", id(test4).state.c_str());
       }     
      if (id(test5).has_state()) {        
        it.printf(102, 40, id(font1), TextAlign::TOP_LEFT, "L=%s", id(test5).state.c_str()); 
       } 
      if (id(test6).has_state()) {
        it.printf(102, 50, id(font1), TextAlign::TOP_LEFT, "T=%s", id(test6).state.c_str()); 
       }        
      if (id(test7).has_state()) {        
        it.printf(77, 50, id(font1), TextAlign::TOP_LEFT, "V=%s", id(test7).state.c_str());
       } 
      if (id(test8).has_state()) {        
        it.printf(77, 50, id(font1), TextAlign::TOP_LEFT, "V=%s", id(test8).state.c_str());
       } 

      it.strftime(64, 32, id(font2), TextAlign::CENTER, "%H:%M", id(homeassistant_time).now()); 

      if (id(test9).has_state()) {        
        it.printf(0, 0, id(font1), TextAlign::TOP_LEFT, "C=%s Kc", id(test9).state.c_str());
       } 
      if (id(test10).has_state()) {        
        it.printf(0, 50, id(font1), TextAlign::TOP_LEFT, "P=%s W", id(test10).state.c_str());
       } 


text_sensor:
  - platform: homeassistant
    id: test1
    entity_id: sensor.test1
  - platform: homeassistant
    id: test2
    entity_id: sensor.test2
  - platform: homeassistant
    id: test3
    entity_id: sensor.test3 
  - platform: homeassistant
    id: test4
    entity_id: sensor.test4
  - platform: homeassistant
    id: test5
    entity_id: sensor.test5
  - platform: homeassistant
    id: test6
    entity_id: sensor.test6
  - platform: homeassistant
    id: test7
    entity_id: sensor.test7
  - platform: homeassistant
    id: test8
    entity_id: sensor.test8
  - platform: homeassistant
    id: test9
    entity_id: sensor.test9
  - platform: homeassistant
    id: test10
    entity_id: sensor.test10

font: # https://fonts.google.com/
  - file: "gfonts://PT Sans Narrow"
    id: font1
    size: 10
  - file: "gfonts://PT Sans Narrow"
    id: font2
    size: 20    
```
