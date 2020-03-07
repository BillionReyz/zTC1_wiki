zTC1支持接入home assistant(以下简称为ha).

## zTC1设置

zTC1通过MQTT服务器接入ha.通过[MQTT配置](https://github.com/a2633063/zTC1/wiki/开始使用#MQTT配置)使zTC1接入ha连接的MQTT服务器.即可

注意:必须能够用app通过mqtt进行控制,之后的homeassistant接入才能成功,如果app无法通过mqtt控制,请先完成mqtt的相关配置



## home assistant设置

注意: 不建议为了此排插入坑hass,使用hass需要很多时间专门来学习相关内容.本人没有精力教授hass相关配置.仅提供配置文件.请自行学习相关配置方式.

### ~~homeassistant的MQTT自动发现~~

ha的mqtt自动发现有很多问题,可能会出现friendlyname丢失,无法识别到设备等问题,由于是ha的原因,无法修复,所以请不要使用mqtt自动发现功能接入,1.0.2及以上版本不再支持mqtt自动发现功能

### home assistant手动配置

注意:

​	由于hass配置文件的关系,此配置文件会导致hass大量的错误提示,但是不影响控制.介意的不要使用.

  

以下内容中,请将`MACMAC`替换为你的排插的mac地址,不带冒号,全部小写,如`123456789abc`

(mac地址可以在app设备设置页面中点击mac地址直接复制)

>  注意:如果接入多个zTC1,请保证以下 `name`字段唯一性

```
switch:
  - platform: mqtt
    name: "ztc1_1_MACMAC"
    state_topic: "device/ztc1/MACMAC/state"
    command_topic: "device/ztc1/MACMAC/set"
    payload_on: "{\"mac\":\"MACMAC\",\"plug_0\":{\"on\":1}}"
    payload_off: "{\"mac\":\"MACMAC\",\"plug_0\":{\"on\":0}}"
    value_template: "{{ value_json.plug_0.on }}"
    state_on: "1"
    state_off: "0"
  - platform: mqtt
    name: "ztc1_2_MACMAC"
    state_topic: "device/ztc1/MACMAC/state"
    command_topic: "device/ztc1/MACMAC/set"
    payload_on: "{\"mac\":\"MACMAC\",\"plug_1\":{\"on\":1}}"
    payload_off: "{\"mac\":\"MACMAC\",\"plug_1\":{\"on\":0}}"
    value_template: "{{ value_json.plug_1.on }}"
    state_on: "1"
    state_off: "0"
  - platform: mqtt
    name: "ztc1_3_MACMAC"
    state_topic: "device/ztc1/MACMAC/state"
    command_topic: "device/ztc1/MACMAC/set"
    payload_on: "{\"mac\":\"MACMAC\",\"plug_2\":{\"on\":1}}"
    payload_off: "{\"mac\":\"MACMAC\",\"plug_2\":{\"on\":0}}"
    value_template: "{{ value_json.plug_2.on }}"
    state_on: "1"
    state_off: "0"
  - platform: mqtt
    name: "ztc1_4_MACMAC"
    state_topic: "device/ztc1/MACMAC/state"
    command_topic: "device/ztc1/MACMAC/set"
    payload_on: "{\"mac\":\"MACMAC\",\"plug_3\":{\"on\":1}}"
    payload_off: "{\"mac\":\"MACMAC\",\"plug_3\":{\"on\":0}}"
    value_template: "{{ value_json.plug_3.on }}"
    state_on: "1"
    state_off: "0"
  - platform: mqtt
    name: "ztc1_5_MACMAC"
    state_topic: "device/ztc1/MACMAC/state"
    command_topic: "device/ztc1/MACMAC/set"
    payload_on: "{\"mac\":\"MACMAC\",\"plug_4\":{\"on\":1}}"
    payload_off: "{\"mac\":\"MACMAC\",\"plug_4\":{\"on\":0}}"
    value_template: "{{ value_json.plug_4.on }}"
    state_on: "1"
    state_off: "0"
  - platform: mqtt
    name: "ztc1_6_MACMAC"
    state_topic: "device/ztc1/MACMAC/state"
    command_topic: "device/ztc1/MACMAC/set"
    payload_on: "{\"mac\":\"MACMAC\",\"plug_5\":{\"on\":1}}"
    payload_off: "{\"mac\":\"MACMAC\",\"plug_5\":{\"on\":0}}"
    value_template: "{{ value_json.plug_5.on }}"
    state_on: "1"
    state_off: "0"

  - platform: mqtt
    name: "ztc1_power_MACMAC"
    state_topic: "device/ztc1/MACMAC/sensor"
    unit_of_measurement: 'W'
    icon: mdi:gauge
    value_template: "{{ value_json.power }}"
  - platform: mqtt
    name: "ztc1_time_MACMAC"
    state_topic: "device/ztc1/MACMAC/sensor"
    #unit_of_measurement: '秒'
    icon: mdi:gauge
    #value_template: '{{ value_json.total_time }}'
    value_template: >-
      {% set time = value_json.total_time %}
      {% set minutes = ((time % 3600) / 60) | int %}
      {% set hours = ((time % 86400) / 3600) | int %}
      {% set days = (time / 86400) | int %}
      {%- if time < 60 -%}
        <1分钟
      {%- else -%}
        {%- if days > 0 -%}
            {{ days }}天
        {%- endif -%}
        {%- if hours > 0 -%}
            {{ hours }}小时
        {%- endif -%}
        {%- if minutes > 0 -%}
            {{ minutes }}分钟
        {%- endif -%}
      {%- endif -%}
    
homeassistant:
  customize:
    switch.ztc1_1_MACMAC:
      friendly_name: zTC1插槽1
    switch.ztc1_2_MACMAC:
      friendly_name: zTC1插槽2
    switch.ztc1_3_MACMAC:
      friendly_name: zTC1插槽3
    switch.ztc1_4_MACMAC:
      friendly_name: zTC1插槽4
    switch.ztc1_5_MACMAC:
      friendly_name: zTC1插槽5
    switch.ztc1_6_MACMAC:
      friendly_name: zTC1插槽6
    sensor.ztc1_power_MACMAC:
      friendly_name: zTC1功率
    sensor.ztc1_time_MACMAC:
      friendly_name: zTC1运行时间
```

