# Color Theory

## Overview

Each color has its own meaning.

In the RGB model, colors begin as black, and change as different levels of red, green, and blue are added.  
Each red, green, and blue value is a number from 0 to 255. 0 means that there's 0% of that color, and is black. 255 means that there's 100% of that color.

Your eyes will see three things in each color:

1. What color (its hue or name),
2. Values: its lightness or darkness
3. Intensity: its brightness or dullness

Each color has all three qualities: hue, value, intensity. For example, a color can be describe as having green (hue), medium (value), and bright (intensity).

## Value/Lightness

**Value** or Lightness describes how light or dark a color is. When a color has white added to it, it is a **tint** and is lighter in value. When a color has black added to it, it is a **shade** and is darker in value.

Thường value of a color có 3 kiểu là: light, medium, dark.

Tone is a hue or mixture of pure colors to which only pure gray is added (equal amounts of black and white). Adding gray to a color will make the intensity much duller. Beware of mixing too much gray into a hue as it can become over-dulled and virtually impossible to restore the brilliance.

## Intensity/Chroma/Saturation

**Intensity** (also called chroma or saturation) is the brightness or dullness of a color. A color as we see it on a color wheel is at full intensity (bright). When we mix it with gray, black, or white, it becomes dull (or lose intensity). Colors also lose intensity when mixed with their complement (the opposite color on the wheel). For example, adding a little green to bright red will make the red duller.

white là `#fff` hay rgb(255,255,255)

## Hue

**Hue** is what color it is. The name of the color such as yellow, red, blue, etc. Hue refers to the origin of the colors we can see. Primary and Secondary colors (Yellow, Orange, Red, Violet, Blue, and Green) are considered hues; however, tertiary colors (mixed colors where neither color is dominant) would also be considered hues.

Colors can be warm or cool. The warm hues are the ones seen in the sun or fire: yellow, orange, red. Cool hues—greens and blues—are found in the restful elements of nature, such as the sky, water, and grass.

A **color wheel** is a circle where similar colors, or hues, are near each other, and different ones are further apart. For example, pure red is between the hues rose and orange.

### Primary colors

The three primary colors of light are: Red, Green, Blue (RGB). They belong to the Additive Color Model. They can be combined in different proportions to make all other colors

The artist color wheel (based in blue, red, and yellow) predates modern science and was discovered by Newton’s prism experiments. We DO, however, still use the RBY model for mixing paints, and it is the most common color wheel students will typically find in art stores.

## CSS rgb() and hexadecimal color

There are 3 main categories of color: primary, secondary, tertiary color

### Primary color

In the additive RGB color model, primary colors are colors that, when combined, create pure white. But for this to happen, each color needs to be at its highest intensity.

Red, green, blue (255) là 3 primary color. 3 màu này combine `255,255,255` tạo ra white color. You cannot mix any other color to create the 3 primary colors.  
`rgb(0,0,0)` is the black color.

### Secondary color

Secondary colors are the colors you get when you equally mixing primary colors. Example:

- yellow (255,255,0)
- cyan (0,255,255)
- magenta (255,0,255)

### Tertiary colors

Tertiary colors are created by combining a primary with a nearby secondary color. Example: orange (255,127,0). Orange = red (255,0,0) + yellow (255,255,0). Ta làm toán đơn giản (255+255)/2 = 255; (255+0)/2 = 127.5 round to 127; (0+0)/2 = 0

- spring green (0,255,127) = cyan + green
- violet (127,0,255) = magenta + blue
- chartreuse green (127,255,0) = (green + yellow)
- azure (0,127,255) = (blue + cyan)
- rose (255,0,127) = (red + magenta)

A very common way to apply color to an element with CSS is with hexadecimal or hex values. While hex values sound complicated, they're really just another form of RGB values.

Hex color values start with a # character and take six characters from 0-9 and A-F. The first pair of characters represent red, the second pair represent green, and the third pair represent blue. For example, `#4B5320`.

## CSS HSL color

The HSL color model, or hue, saturation, and lightness, is another way to represent colors.

The CSS hsl function accepts 3 values: a number from 0 to 360 for hue, a percentage from 0 to 100 for saturation, and a percentage from 0 to 100 for lightness.

If you imagine a color wheel, the hue red is at 0 degrees, green is at 120 degrees, and blue is at 240 degrees.

Saturation is the intensity of a color from 0%, or gray, to 100% for pure color. You must add the percent sign % to the saturation and lightness values.

Lightness is how bright a color appears, from 0%, or complete black, to 100%, complete white, with 50% being neutral.

## Color rules

60-30-10 = 60% primary color, 30% secondary color, 10% accent color.

Always considering contrast and readability

## Color Schemes/color groups

Monochromatic color schemes are made up of one hue in different values and intensities. Its just one color, different tint and shade

Analogous color schemes are three to five hues that are next to each other on the color wheel. These schemes always have one color in common. Red is the common hue in the example shown here (orange, red-orange, and red).

Complementary color: Colors that are opposite on the color wheel. These compementary color pairs have the strongest hue contrast.

If two complementary colors are combined, they produce gray. But when they are placed side-by-side, these colors produce strong visual contrast and appear brighter.

## Prioritize your website NEUTRAL color, not primary color

Many people choose colors in this manner:

- Primary colors – usually their logo or main brand color.
- Secondary colors – usually accent colors or secondary brand colors.
- Neutral colors – usually considered “the background color” in web design.

I do things a bit differently:

- Primary color (for me) – is the background color.
- Accent color – is the logo or brand color.
- Neutral color – used as a (subtle) secondary background color or secondary accent color.

[Bài tham khảo](https://wpjohnny.com/prioritize-neutral-color-not-primary-color/#:~:text=Primary%20colors%20%E2%80%93%20usually%20their%20logo,background%20color%E2%80%9D%20in%20web%20design.)

Accent Color refers to a color used to emphasize key parts of the UI, such as the active tab, focused input texts, checked boxes, etc.

## References

