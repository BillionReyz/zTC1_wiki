

**此文本适用于版本v0.x.x(即v1.0.0之前的旧版本固件)**

zTC1支持接入home assistant(以下简称为ha).

## zTC1设置

zTC1通过MQTT服务器接入ha.通过[MQTT配置](https://github.com/a2633063/zTC1/wiki/开始使用#MQTT配置)使zTC1接入ha连接的MQTT服务器.即可

注意:必须能够用app通过mqtt进行控制,之后的homeassistant接入才能成功,如果app无法通过mqtt控制,请先完成mqtt的相关配置



## home assistant设置

### ~~homeassistant的MQTT自动发现~~

ha的mqtt自动发现有很多问题,可能会出现friendlyname丢失,无法识别到设备等问题,由于是ha的原因,无法修复,所以请不要使用mqtt自动发现功能接入,后面也会将zTC1的自动发现功能删除


### home assistant手动配置

如果你不想使用ha的MQTT自动发现功能.可以手动添加实体:(以下内容中,`MAC`为设备的mac地址小写,如`123456789abc`)

>  注意:如果接入多个zTC1,请保证以下 `name`字段唯一性

```
switch:
  - platform: mqtt
    name: 'tc1_1'
    state_topic: 'homeassistant/switch/MAC/plug_0/state'
    command_topic: 'device/ztc1/set'
    payload_on: '{"mac":"MAC","plug_0":{"on":1}}'
    payload_off: '{"mac":"MAC","plug_0":{"on":0}}'
  - platform: mqtt
    name: 'tc1_2'
    state_topic: 'homeassistant/switch/MAC/plug_1/state'
    command_topic: 'device/ztc1/set'
    payload_on: '{"mac":"MAC","plug_1":{"on":1}}'
    payload_off: '{"mac":"MAC","plug_1":{"on":0}}'
  - platform: mqtt
    name: 'tc1_3'
    state_topic: 'homeassistant/switch/MAC/plug_2/state'
    command_topic: 'device/ztc1/set'
    payload_on: '{"mac":"MAC","plug_2":{"on":1}}'
    payload_off: '{"mac":"MAC","plug_2":{"on":0}}'
  - platform: mqtt
    name: 'tc1_4'
    state_topic: 'homeassistant/switch/MAC/plug_3/state'
    command_topic: 'device/ztc1/set'
    payload_on: '{"mac":"MAC","plug_3":{"on":1}}'
    payload_off: '{"mac":"MAC","plug_3":{"on":0}}'
  - platform: mqtt
    name: 'tc1_5'
    state_topic: 'homeassistant/switch/MAC/plug_4/state'
    command_topic: 'device/ztc1/set'
    payload_on: '{"mac":"MAC","plug_4":{"on":1}}'
    payload_off: '{"mac":"MAC","plug_4":{"on":0}}'
  - platform: mqtt
    name: 'tc1_6'
    state_topic: 'homeassistant/switch/MAC/plug_5/state'
    command_topic: 'device/ztc1/set'
    payload_on: '{"mac":"MAC","plug_5":{"on":1}}'
    payload_off: '{"mac":"MAC","plug_5":{"on":0}}'

sensor:
  - platform: mqtt
    name: 'tc1_power'
    state_topic: 'homeassistant/sensor/MAC/power/state'
    unit_of_measurement: 'W'
    icon: 'mdi:gauge'
    value_template: '{{ value_json.power }}'

homeassistant:
  customize:
    switch.tc1_1:
      friendly_name: TC1插槽1
    switch.tc1_2:
      friendly_name: TC1插槽2
    switch.tc1_3:
      friendly_name: TC1插槽3
    switch.tc1_4:
      friendly_name: TC1插槽4
    switch.tc1_5:
      friendly_name: TC1插槽5
    switch.tc1_6:
      friendly_name: TC1插槽6
    sensor.tc1_power:
      friendly_name: TC1功率

group:
  tc1:
    name: TC1插座
    view: no
    entities:
      - sensor.tc1_power
      - switch.tc1_1
      - switch.tc1_2
      - switch.tc1_3
      - switch.tc1_4
      - switch.tc1_5
      - switch.tc1_6
```

