# coloured-room-lights
Given a base colour these scripts will fill the room lights with that colour, but randomly assign slight deviations to the hue, saturation and brightness as specified by parameters. The effect is to give everything from a single colour to something like a disco scene at the other extreme.

![IMG_5342](https://github.com/philmale/coloured-room-lights/assets/8235259/0333d708-e8e9-4499-add6-f53f0d49ae21)

# Installation

You can install the basic script simply by cutting and pasting it into the script editor in Home Assistant. 
The basic script takes parameters to specify HSB settings for the base colour, and then 'spreads' that you want to randomly apply to each parameter (+ and - around the specified value, so a hue of 150 with a spread of 10 would give you hue's ranging from 140-160).
```
  service: script.coloured_room_lights
  data:
    include: 
      - optional list of lights to include
    exclude:
      - optional list of lights to exclude
    transition: 0 or more - transition in seconds for each bulb to change
    brightness: optional 0-255 or random values will be selected
    brightness_spread: 0 or more - random spread to apply to the brightness
    saturation: 0-100 - saturation to apply, random if not provided
    saturation_spread: 0 or more - random spread to apply to the saturation
    hue: 0-360 - hue to apply, random if not provided
    hue_spread: 0 or more - random spread to apply to the hue
    colour_string: JSON string specifying multiple HS values instead of sticking to one [ [h1,s1], [h2, s2], etc ]
    room: light.group name to apply the colours to - anything from one light to hundreds
```
So for example:
```
  service: script.coloured_room_lights
  data:
    hue: 128
    hue_spread: 20
    brightness: 200
    brightness_spread: 20
    saturation_spread: 20
    room: light.office
```
Because using hue/saturation numbers is a pain, there is an additional script in the distribution called coloured-room-lights-named which takes colour names, converts them to HS pairs and then calls the coloured_room_lights script for you.
That lets you use colour names rather than HS pairs - like this:

```
  service: script.coloured_room_lights_named
  data:
    colour: "Blue"
    hue_spread: 20
    brightness: 150
    brightness_spread: 30
    room: light.office
```

In order to use this though you need to have something to convert colour names into numbers.

Luckily I've written exactly that and coloured-room-lights-named just uses the custom_template [colour_map.jinja](https://github.com/philmale/colour-map) which you just drop into your config/custom_templates directory
and restart home assistant to use. 
