# LVGL XML Comprehensive Documentation

## Table of Contents
1. [Naming Conventions & Rules](#naming-conventions--rules)
2. [Project Structure](#project-structure)
3. [Components API](#components-api)
4. [Screens API](#screens-api)
5. [Global Resources](#global-resources)
6. [Data Binding](#data-binding)
7. [Styling](#styling)
8. [Animations](#animations)
9. [Examples and Usage](#examples-and-usage)

---

## Naming Conventions & Rules

### ⚠️ CRITICAL: File and Directory Naming Rules

**The "Wrong Name" error occurs when file or directory names don't follow C variable naming conventions.**

#### ✅ Valid Names (C Variable Rules):
- Must start with a letter (a-z, A-Z) or underscore (_)
- Can contain letters, numbers, and underscores only
- **NO hyphens (-), dots (.), or special characters**
- **NO spaces**

#### ✅ CORRECT Examples:
```
alarm.xml              ✓
theme_switcher.xml     ✓
screen_hello_world.xml ✓
my_component.xml       ✓
button1.xml            ✓
_private_comp.xml      ✓
```

#### ❌ INCORRECT Examples (Will cause "Wrong Name" error):
```
test-deneme.xml        ✗ (hyphen not allowed)
my.component.xml       ✗ (dots not allowed except .xml extension)
1_button.xml           ✗ (cannot start with number)
my component.xml       ✗ (spaces not allowed)
button-1.xml           ✗ (hyphen not allowed)
```

### Why This Matters
LVGL XML generates C code from your XML files. The file and directory names become:
- Function names
- Variable names  
- Type names
- Include guard names

**Solution for "test_deneme.xml":**
- Use `test_deneme.xml` (with underscore) ✓
- NOT `test-deneme.xml` (with hyphen) ✗

---

## Project Structure

### Standard Project Layout
```
your_project/
├── project.xml          # Project configuration
├── globals.xml          # Global resources (styles, images, fonts, subjects)
├── components/          # Reusable components
│   ├── basic/
│   │   ├── button/
│   │   │   ├── button.xml
│   │   │   ├── button_gen.c
│   │   │   └── button_gen.h
│   │   └── ...
│   └── cards/
│       └── ...
├── screens/            # Application screens
│   ├── screen_main.xml
│   ├── screen_settings.xml
│   └── ...
├── widgets/            # Custom widgets (C-based)
├── images/             # Image assets
├── fonts/              # Font files
└── preview-bin/        # Preview build output
```

### project.xml
Defines display targets and project settings:

```xml
<project>
    <targets>
        <target name="target1">
            <display width="320" height="240" />
        </target>
        <target name="full_view">
            <display width="480" height="480" />
        </target>
    </targets>
</project>
```

**Properties:**
- `target.name`: Target identifier (used in build system)
- `display.width`: Display width in pixels
- `display.height`: Display height in pixels

---

## Components API

### Component Structure

Components are reusable UI elements with custom APIs. They are defined in `<component>` tags.

#### Basic Component Template
```xml
<component>
    <previews>
        <preview width="200" height="100" style_bg_color="0xffffff" />
    </previews>

    <api>
        <prop name="label" type="string" default="Button" help="Button text"/>
        <prop name="enabled" type="bool" default="true" help="Enable/disable button"/>
        <prop name="icon" type="image" default="" help="Button icon"/>
    </api>

    <consts>
        <int name="local_const" value="42" />
    </consts>

    <styles>
        <style name="style_base" 
               bg_color="0x2196F3" 
               text_color="0xffffff" 
               radius="8" />
    </styles>

    <view extends="lv_button">
        <style name="style_base"/>
        <lv_label text="$label" align="center"/>
    </view>
</component>
```

### Component API Properties

API properties define the component's interface:

| Type | Description | Example |
|------|-------------|---------|
| `string` | Text value | `<prop name="title" type="string" default="Hello"/>` |
| `int` | Integer number | `<prop name="value" type="int" default="0" min_value="0" max_value="100"/>` |
| `bool` | Boolean (true/false) | `<prop name="visible" type="bool" default="true"/>` |
| `color` | Color value | `<prop name="bg_color" type="color" default="0xff0000"/>` |
| `image` | Image resource | `<prop name="icon" type="image" default=""/>` |
| `font` | Font resource | `<prop name="text_font" type="font" default=""/>` |
| `enum` | Enumeration | Defined in globals.xml |

### Using Component Properties

Properties are referenced with `$` prefix:

```xml
<view extends="lv_button">
    <lv_label text="$label" />              <!-- String property -->
    <visible value="$visible" />            <!-- Bool property -->
    <style bg_color="$bg_color" />          <!-- Color property -->
    <image src="$icon" />                   <!-- Image property -->
</view>
```

---

## Screens API

Screens are top-level UI containers. They are defined in `<screen>` tags.

### Basic Screen Template
```xml
<screen>
    <styles>
        <style name="style_light" bg_color="#accent2_50_light" />
        <style name="style_dark" bg_color="#accent2_50_dark" />
    </styles>

    <view flex_flow="column">
        <bind_style subject="dark_theme" name="style_light" ref_value="0" />
        <bind_style subject="dark_theme" name="style_dark" ref_value="1" />

        <!-- Screen content -->
        <lv_label text="Hello World" align="center"/>
        
        <!-- Using components -->
        <button label="Click Me" />
    </view>
</screen>
```

### Screen Elements

Screens can contain:
- **LVGL widgets**: `lv_label`, `lv_button`, `lv_image`, etc.
- **Custom components**: Your reusable components
- **Layout containers**: `div`, `row`, `column`, `view`
- **Data bindings**: Dynamic value updates
- **Style bindings**: Theme switching

---

## Global Resources

### globals.xml Structure

Global resources are shared across all components and screens:

```xml
<globals name="project_name">
    <api>
        <!-- Enum definitions -->
        <enumdefs>
            <enumdef name="my_enum">
                <value name="OPTION_A" value="0"/>
                <value name="OPTION_B" value="1"/>
            </enumdef>
        </enumdefs>
    </api>

    <consts>
        <!-- Constants -->
        <int name="unit_sm" value="6" />
        <int name="unit_md" value="12" />
        <percentage name="opa_muted" value="20%" />
        <color name="primary_color" value="0x2196F3" />
        <string name="app_name" value="My App" />
    </consts>

    <styles>
        <!-- Global styles -->
        <style name="style_disabled" opa_layered="60%" />
        <style name="style_reset" 
               width="content" 
               height="content" 
               bg_opa="0" />
    </styles>

    <subjects>
        <!-- Data binding subjects -->
        <int name="dark_theme" value="0" help="0:light 1:dark" />
        <int name="volume" value="50" min_value="0" max_value="100" />
        <string name="user_name" value="John" />
    </subjects>

    <images>
        <!-- Image resources -->
        <file name="icon_home" src_path="images/icon_home.png" />
        <file name="logo" src_path="images/logo.png" />
    </images>

    <fonts>
        <!-- Font resources -->
        <tiny_ttf name="my_font_16" 
                  size="16" 
                  as_file="true" 
                  src_path="fonts/Roboto-Regular.ttf" />
    </fonts>
</globals>
```

### Constants Reference

Constants are referenced with `#` prefix:

```xml
<view style_pad_all="#unit_md">           <!-- Integer constant -->
    <lv_label style_text_opa="#opa_muted" <!-- Percentage constant -->
              style_bg_color="#primary_color" /> <!-- Color constant -->
</view>
```

### Image Resources

```xml
<images>
    <file name="icon_play" src_path="images/icon_play.png" />
    <file name="icon_pause" src_path="images/icon_pause.png" />
</images>

<!-- Usage: -->
<lv_image src="#icon_play" />
```

### Font Resources

```xml
<fonts>
    <tiny_ttf name="roboto_regular_14" 
              size="14" 
              as_file="true" 
              src_path="fonts/Roboto-Regular.ttf" />
    <tiny_ttf name="roboto_bold_20" 
              size="20" 
              as_file="true" 
              src_path="fonts/Roboto-Bold.ttf" />
</fonts>

<!-- Usage: -->
<lv_label text="Hello" style_text_font="roboto_bold_20" />
```

---

## Data Binding

### Subjects (Data Sources)

Subjects are global data that can be bound to UI elements:

```xml
<subjects>
    <int name="temperature" value="25" min_value="-50" max_value="50" />
    <int name="is_on" value="1" help="0:off 1:on" />
    <string name="status_text" value="Ready" />
</subjects>
```

### Binding Values

```xml
<!-- Bind label text to subject -->
<lv_label bind_text="status_text" />

<!-- Bind slider value to subject -->
<lv_slider bind_value="temperature" />

<!-- Bind switch checked state to subject -->
<lv_switch bind_checked="is_on" />

<!-- Bind bar value to subject -->
<lv_bar bind_value="temperature" />
```

### Conditional Styling with Bindings

```xml
<styles>
    <style name="style_off" bg_color="0xff0000" />
    <style name="style_on" bg_color="0x00ff00" />
</styles>

<view>
    <!-- Apply style_off when is_on = 0 -->
    <bind_style subject="is_on" name="style_off" ref_value="0" />
    
    <!-- Apply style_on when is_on = 1 -->
    <bind_style subject="is_on" name="style_on" ref_value="1" />
</view>
```

### Subject-Based Enable/Disable

```xml
<lv_button subject_enable="is_on">
    <!-- Button enabled when is_on = 1, disabled when = 0 -->
    <lv_label text="Click Me" />
</lv_button>
```

---

## Styling

### Style Definition

Styles are defined in `<styles>` sections:

```xml
<styles>
    <style name="style_primary"
           bg_color="0x2196F3"
           bg_opa="100%"
           text_color="0xffffff"
           radius="8"
           pad_all="12" />
    
    <style name="style_pressed"
           bg_color="0x1976D2"
           selector="pressed" />
</styles>
```

### Common Style Properties

#### Layout & Size
- `width`, `height`: Size in pixels or "content", "100%"
- `min_width`, `min_height`: Minimum dimensions
- `max_width`, `max_height`: Maximum dimensions
- `align`: Alignment (center, top_left, bottom_right, etc.)

#### Spacing
- `pad_all`: Padding on all sides
- `pad_hor`: Horizontal padding (left + right)
- `pad_ver`: Vertical padding (top + bottom)
- `pad_left`, `pad_right`, `pad_top`, `pad_bottom`: Individual padding
- `pad_row`, `pad_column`: Gap between flex children

#### Background
- `bg_color`: Background color
- `bg_opa`: Background opacity (0-100% or 0-255)
- `radius`: Border radius

#### Border
- `border_color`: Border color
- `border_width`: Border width
- `border_opa`: Border opacity
- `border_side`: Which sides (top, bottom, left, right, all)

#### Text
- `text_color`: Text color
- `text_font`: Font name (from globals)
- `text_align`: Text alignment

#### Flex Layout
- `flex_flow`: row, column, row_wrap, column_wrap
- `flex_grow`: Flex grow factor
- `flex_main_place`: start, end, center, space_between, space_around, space_evenly
- `flex_cross_place`: start, end, center, stretch
- `flex_track_place`: start, end, center, stretch, space_between, space_around, space_evenly

### Applying Styles

```xml
<!-- Inline style -->
<view>
    <style bg_color="0xff0000" radius="8" />
</view>

<!-- Named style -->
<view>
    <style name="style_primary" />
</view>

<!-- With selector (state-based) -->
<lv_button>
    <style name="style_base" />
    <style name="style_pressed" selector="pressed" />
    <style name="style_focused" selector="focused" />
</lv_button>

<!-- Shorthand inline properties -->
<view style_bg_color="0xff0000" style_radius="8" style_pad_all="12" />
```

---

## Animations

### Timeline Definitions

Animations are organized into timelines:

```xml
<animations>
    <timeline name="open">
        <animation target="self" 
                   prop="opa" 
                   start="0" 
                   end="255" 
                   duration="200" 
                   early_apply="true" />
        
        <animation target="label_1" 
                   prop="translate_y" 
                   start="-40" 
                   end="0" 
                   duration="200" 
                   delay="100" 
                   early_apply="true" />
        
        <animation target="button_1" 
                   prop="opa" 
                   start="0" 
                   end="255" 
                   duration="300" 
                   delay="200" 
                   path="bounce" />
    </timeline>
    
    <timeline name="close">
        <animation target="self" 
                   prop="opa" 
                   start="255" 
                   end="0" 
                   duration="200" />
    </timeline>
</animations>
```

### Animation Properties

| Property | Description | Example |
|----------|-------------|---------|
| `target` | Target widget name or "self" | `target="button_1"` |
| `prop` | Property to animate | `prop="opa"`, `prop="translate_x"` |
| `start` | Starting value | `start="0"` |
| `end` | Ending value | `end="255"` |
| `duration` | Duration in ms | `duration="200"` |
| `delay` | Delay before start in ms | `delay="100"` |
| `early_apply` | Apply start value immediately | `early_apply="true"` |
| `path` | Easing curve | `path="ease_in"`, `path="bounce"` |

### Animatable Properties

- `opa`: Opacity (0-255)
- `translate_x`, `translate_y`: Translation
- `scale_x`, `scale_y`: Scaling
- `rotate`: Rotation (degrees)
- `width`, `height`: Size
- And more LVGL properties...

### Playing Animations

```xml
<lv_button>
    <play_timeline_event target="card_1" timeline="open" />
    <lv_label text="Animate" />
</lv_button>
```

---

## Examples and Usage

### Example 1: Simple Button Component

**File: `components/my_button/my_button.xml`**

```xml
<component>
    <previews>
        <preview style_pad_all="8" />
    </previews>

    <api>
        <prop name="label" type="string" default="Click Me" help="Button text"/>
        <prop name="bg_color" type="color" default="0x2196F3" help="Background color"/>
    </api>

    <styles>
        <style name="style_base" 
               bg_color="$bg_color"
               text_color="0xffffff"
               radius="8"
               pad_hor="20"
               pad_ver="12" />
        <style name="style_pressed" 
               bg_opa="80%" 
               selector="pressed" />
    </styles>

    <view extends="lv_button">
        <style name="style_base"/>
        <style name="style_pressed" selector="pressed" />
        <lv_label text="$label" align="center"/>
    </view>
</component>
```

**Usage in screen:**
```xml
<screen>
    <view>
        <my_button label="Submit" bg_color="0x4CAF50" />
        <my_button label="Cancel" bg_color="0xf44336" />
    </view>
</screen>
```

### Example 2: Card with Data Binding

**File: `components/status_card/status_card.xml`**

```xml
<component>
    <api>
        <prop name="title" type="string" default="Status"/>
    </api>

    <styles>
        <style name="card_base"
               bg_color="0xffffff"
               radius="12"
               pad_all="16" />
        <style name="card_active"
               bg_color="0xe3f2fd" />
    </styles>

    <view>
        <style name="card_base" />
        <!-- Conditional styling based on subject -->
        <bind_style subject="is_active" name="card_active" ref_value="1" />
        
        <lv_label text="$title" style_text_font="roboto_bold_16" />
        <lv_label bind_text="status_message" style_text_font="roboto_regular_14" />
    </view>
</component>
```

### Example 3: Settings Screen with Theme Switching

**File: `screens/screen_settings.xml`**

```xml
<screen>
    <styles>
        <style name="style_light" 
               bg_color="0xffffff" 
               text_color="0x000000" />
        <style name="style_dark" 
               bg_color="0x1e1e1e" 
               text_color="0xffffff" />
    </styles>

    <view flex_flow="column" style_pad_all="16">
        <!-- Apply theme-based styles -->
        <bind_style subject="dark_theme" name="style_light" ref_value="0" />
        <bind_style subject="dark_theme" name="style_dark" ref_value="1" />

        <lv_label text="Settings" style_text_font="roboto_bold_20" />
        
        <row style_flex_main_place="space_between" style_pad_ver="12">
            <lv_label text="Dark Theme" />
            <lv_switch bind_checked="dark_theme" />
        </row>

        <row style_flex_main_place="space_between" style_pad_ver="12">
            <lv_label text="Volume" />
            <lv_slider bind_value="volume" width="150" />
        </row>
    </view>
</screen>
```

### Example 4: Animated Card

**File: `components/animated_card/animated_card.xml`**

```xml
<component>
    <api>
        <prop name="title" type="string" default="Card"/>
    </api>

    <animations>
        <timeline name="appear">
            <animation target="self" 
                       prop="opa" 
                       start="0" 
                       end="255" 
                       duration="300" 
                       early_apply="true" />
            <animation target="self" 
                       prop="translate_y" 
                       start="20" 
                       end="0" 
                       duration="300" 
                       path="ease_out" 
                       early_apply="true" />
        </timeline>
    </animations>

    <styles>
        <style name="card" 
               bg_color="0xffffff" 
               radius="12" 
               pad_all="16" />
    </styles>

    <view>
        <style name="card" />
        <lv_label text="$title" />
    </view>
</component>
```

### Example 5: Complete globals.xml

**File: `globals.xml`**

```xml
<globals name="my_app">
    <consts>
        <!-- Spacing units -->
        <int name="unit_xs" value="4" />
        <int name="unit_sm" value="8" />
        <int name="unit_md" value="12" />
        <int name="unit_lg" value="16" />
        
        <!-- Colors -->
        <color name="primary" value="0x2196F3" />
        <color name="secondary" value="0x4CAF50" />
        <color name="danger" value="0xf44336" />
        <color name="bg_light" value="0xffffff" />
        <color name="bg_dark" value="0x1e1e1e" />
    </consts>

    <subjects>
        <!-- Theme -->
        <int name="dark_theme" value="0" help="0:light 1:dark" />
        
        <!-- App state -->
        <int name="is_active" value="0" />
        <string name="status_message" value="Ready" />
        <int name="volume" value="50" min_value="0" max_value="100" />
    </subjects>

    <styles>
        <style name="style_disabled" opa_layered="50%" />
    </styles>

    <images>
        <file name="icon_settings" src_path="images/settings.png" />
        <file name="icon_home" src_path="images/home.png" />
    </images>

    <fonts>
        <tiny_ttf name="roboto_regular_14" 
                  size="14" 
                  as_file="true" 
                  src_path="fonts/Roboto-Regular.ttf" />
        <tiny_ttf name="roboto_bold_16" 
                  size="16" 
                  as_file="true" 
                  src_path="fonts/Roboto-Bold.ttf" />
        <tiny_ttf name="roboto_bold_20" 
                  size="20" 
                  as_file="true" 
                  src_path="fonts/Roboto-Bold.ttf" />
    </fonts>
</globals>
```

---

## Widget Reference

### Layout Containers

#### view
Base container widget
```xml
<view flex_flow="column" style_pad_all="8">
    <!-- children -->
</view>
```

#### div
Generic container (scrollable)
```xml
<div width="200" height="100" scrollable="true">
    <!-- children -->
</div>
```

#### row
Horizontal flex container
```xml
<row style_flex_main_place="space_between">
    <lv_label text="Left" />
    <lv_label text="Right" />
</row>
```

#### column
Vertical flex container
```xml
<column style_flex_cross_place="center">
    <lv_label text="Top" />
    <lv_label text="Bottom" />
</column>
```

### LVGL Native Widgets

#### lv_label
Text display
```xml
<lv_label text="Hello World" 
          style_text_font="roboto_bold_16"
          style_text_color="0x000000" />
```

#### lv_button
Clickable button
```xml
<lv_button>
    <lv_label text="Click" align="center" />
</lv_button>
```

#### lv_image
Image display
```xml
<lv_image src="#icon_home" />
```

#### lv_slider
Slider control
```xml
<lv_slider bind_value="volume" 
           width="150" 
           min_value="0" 
           max_value="100" />
```

#### lv_switch
Toggle switch
```xml
<lv_switch bind_checked="is_on" />
```

#### lv_checkbox
Checkbox
```xml
<lv_checkbox bind_checked="agreed">
    <lv_label text="I agree" />
</lv_checkbox>
```

#### lv_bar
Progress bar
```xml
<lv_bar bind_value="progress" 
        width="200" 
        min_value="0" 
        max_value="100" />
```

#### lv_arc
Circular progress/control
```xml
<lv_arc bind_value="temperature" 
        width="100" 
        height="100" 
        min_value="0" 
        max_value="100" />
```

---

## Best Practices

### 1. Naming Conventions
✅ **DO:**
- Use descriptive, meaningful names
- Follow C variable naming rules (letters, numbers, underscores only)
- Use snake_case for consistency: `user_profile_card.xml`
- Prefix screens with `screen_`: `screen_main.xml`

❌ **DON'T:**
- Use hyphens: `user-profile.xml`
- Use spaces: `user profile.xml`
- Start with numbers: `1_screen.xml`

### 2. Component Organization
- Group related components in directories
- Keep components small and focused
- Use clear, documented API properties

### 3. Styling
- Define global styles in `globals.xml`
- Use constants for colors, spacing, and sizes
- Leverage theme switching with subject bindings

### 4. Data Binding
- Define subjects in `globals.xml`
- Use meaningful subject names
- Add help text to document subject purpose

### 5. Performance
- Reuse styles instead of duplicating
- Use appropriate widget types
- Optimize animations (shorter durations, fewer properties)

---

## Troubleshooting

### "Wrong Name" Error
**Problem:** File or directory name contains invalid characters

**Solution:** Rename to follow C variable naming rules:
- ✅ `my_component.xml`
- ❌ `my-component.xml`

### Component Not Found
**Problem:** Component not showing up

**Solution:**
- Check file name matches directory name
- Verify component is in correct directory
- Ensure XML is valid

### Style Not Applied
**Problem:** Style not visible

**Solution:**
- Check style name spelling
- Verify style is defined before use
- Check selector (pressed, focused, etc.) is correct

### Subject Binding Not Working
**Problem:** UI not updating with subject changes

**Solution:**
- Verify subject is defined in globals.xml
- Check binding syntax: `bind_value="subject_name"`
- Ensure subject name spelling is correct

---

## Quick Reference Card

### File Naming Rules
```
✅ alarm.xml
✅ screen_settings.xml
✅ user_card_v2.xml
❌ my-component.xml
❌ screen.settings.xml
❌ 1_button.xml
```

### Reference Syntax
```xml
<!-- Constants (from globals) -->
<view style_pad_all="#unit_md" />

<!-- Component properties -->
<lv_label text="$label" />

<!-- Subjects (data binding) -->
<lv_label bind_text="status" />

<!-- Images -->
<lv_image src="#icon_home" />
```

### Common Patterns
```xml
<!-- Flex row with spacing -->
<row style_flex_main_place="space_between" style_pad_all="8">
    <lv_label text="Left" />
    <lv_label text="Right" />
</row>

<!-- Themed component -->
<view>
    <bind_style subject="dark_theme" name="style_light" ref_value="0" />
    <bind_style subject="dark_theme" name="style_dark" ref_value="1" />
</view>

<!-- Enabled/disabled by subject -->
<lv_button subject_enable="is_unlocked">
    <lv_label text="Submit" />
</lv_button>
```

---

## Additional Resources

- **Official Documentation:** https://docs.lvgl.io/master/details/xml/index.html
- **LVGL Pro Editor:** https://pro.lvgl.io
- **Online Viewer:** https://viewer.lvgl.io
- **Examples:** See `/workspace/examples` and `/workspace/tutorials`

---

## Summary

**Key Takeaways:**

1. **Naming is Critical:** Always use C variable naming rules (letters, numbers, underscores only)
2. **Component-Oriented:** Build reusable components with clear APIs
3. **Data Binding:** Use subjects for reactive UIs
4. **Theming:** Leverage style bindings for theme switching
5. **Animations:** Organize animations into timelines
6. **Global Resources:** Centralize styles, images, fonts in `globals.xml`

**Your Issue Solution:**
The "Wrong Name" error occurs when file/directory names violate C variable naming rules. Use underscores (`_`) instead of hyphens (`-`) or dots (`.`) in names.

✅ Correct: `test_deneme.xml`
❌ Wrong: `test-deneme.xml` or `test.deneme.xml`
