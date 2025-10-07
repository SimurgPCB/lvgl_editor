# LVGL XML API Reference

## Table of Contents
1. [XML Structure](#xml-structure)
2. [Component API](#component-api)
3. [Widget Reference](#widget-reference)
4. [Style Properties](#style-properties)
5. [Layout Properties](#layout-properties)
6. [Events and Bindings](#events-and-bindings)
7. [Animation API](#animation-api)

---

## XML Structure

### Root Elements

#### `<project>`
Project configuration file

**Attributes:** None

**Children:**
- `<targets>`: Display target definitions

```xml
<project>
    <targets>
        <target name="target1">
            <display width="320" height="240" />
        </target>
    </targets>
</project>
```

#### `<globals>`
Global resources and definitions

**Attributes:**
- `name`: Project/module name (required)

**Children:**
- `<api>`: API definitions (enums, etc.)
- `<consts>`: Constants
- `<subjects>`: Data binding subjects
- `<styles>`: Global styles
- `<images>`: Image resources
- `<fonts>`: Font resources

```xml
<globals name="my_project">
    <consts>...</consts>
    <subjects>...</subjects>
    <styles>...</styles>
    <images>...</images>
    <fonts>...</fonts>
</globals>
```

#### `<component>`
Reusable component definition

**Attributes:** None

**Children:**
- `<previews>`: Preview configurations
- `<api>`: Component properties
- `<consts>`: Local constants
- `<styles>`: Component styles
- `<animations>`: Animation timelines
- `<view>`: Component structure (required)

```xml
<component>
    <previews>...</previews>
    <api>...</api>
    <styles>...</styles>
    <view>...</view>
</component>
```

#### `<screen>`
Screen definition

**Attributes:** None

**Children:**
- `<consts>`: Local constants
- `<styles>`: Screen styles
- `<animations>`: Animation timelines
- `<view>`: Screen structure (required)

```xml
<screen>
    <styles>...</styles>
    <view>...</view>
</screen>
```

---

## Component API

### Property Types

#### `<prop>`
Component property definition

**Attributes:**
- `name`: Property name (required)
- `type`: Property type (required)
- `default`: Default value (optional)
- `help`: Help text (optional)
- `min_value`: Minimum value (for int/number types)
- `max_value`: Maximum value (for int/number types)

**Available Types:**
- `string`: Text value
- `int`: Integer number
- `bool`: Boolean (true/false)
- `color`: Color value (0xRRGGBB)
- `image`: Image resource reference
- `font`: Font resource reference
- `enum`: Enumeration value

```xml
<api>
    <prop name="title" type="string" default="Title" help="Card title text"/>
    <prop name="value" type="int" default="0" min_value="0" max_value="100"/>
    <prop name="visible" type="bool" default="true"/>
    <prop name="bg_color" type="color" default="0xffffff"/>
    <prop name="icon" type="image" default=""/>
    <prop name="text_font" type="font" default=""/>
</api>
```

### Using Properties

Properties are referenced with `$` prefix:

```xml
<lv_label text="$title" />
<style bg_color="$bg_color" />
<lv_image src="$icon" />
```

---

## Widget Reference

### Layout Containers

#### `<view>`
Base container widget

**Extends:** `lv_obj`

**Attributes:**
- `extends`: Base LVGL widget type
- `name`: Widget identifier (for animations, references)
- All style properties
- All layout properties

```xml
<view extends="lv_button" flex_flow="column" style_pad_all="8">
    <!-- children -->
</view>
```

#### `<div>`
Generic scrollable container

**Attributes:**
- `width`, `height`: Dimensions
- `scrollable`: Enable scrolling (true/false)
- `scroll_snap_x`, `scroll_snap_y`: Snap to children (none/start/end/center)
- `scroll_one`: Scroll one item at a time (true/false)
- All style properties

```xml
<div width="200" height="100" scrollable="true" scroll_snap_y="center">
    <!-- children -->
</div>
```

#### `<row>`
Horizontal flex container

**Attributes:**
- All view attributes
- Automatically sets `flex_flow="row"`

```xml
<row style_flex_main_place="space_between" style_pad_all="8">
    <lv_label text="Left" />
    <lv_label text="Right" />
</row>
```

#### `<column>`
Vertical flex container

**Attributes:**
- All view attributes
- Automatically sets `flex_flow="column"`

```xml
<column style_flex_cross_place="center">
    <lv_label text="Top" />
    <lv_label text="Bottom" />
</column>
```

### LVGL Native Widgets

#### `<lv_label>`
Text display widget

**Attributes:**
- `text`: Label text
- `bind_text`: Bind to subject
- `align`: Alignment (center, top_left, bottom_right, etc.)
- All style properties

```xml
<lv_label text="Hello World" 
          style_text_font="roboto_bold_16"
          style_text_color="0x000000"
          align="center" />
```

#### `<lv_button>`
Clickable button widget

**Attributes:**
- All style properties
- Event attributes

```xml
<lv_button style_bg_color="0x2196F3" style_radius="8">
    <lv_label text="Click Me" align="center" />
</lv_button>
```

#### `<lv_image>`
Image display widget

**Attributes:**
- `src`: Image source (reference with #)
- All style properties

```xml
<lv_image src="#icon_home" width="32" height="32" />
```

#### `<lv_slider>`
Slider control widget

**Attributes:**
- `min_value`: Minimum value
- `max_value`: Maximum value
- `value`: Current value
- `bind_value`: Bind to subject
- All style properties

```xml
<lv_slider bind_value="volume" 
           width="150" 
           min_value="0" 
           max_value="100" />
```

#### `<lv_switch>`
Toggle switch widget

**Attributes:**
- `checked`: Checked state (true/false)
- `bind_checked`: Bind to subject
- All style properties

```xml
<lv_switch bind_checked="is_on" />
```

#### `<lv_checkbox>`
Checkbox widget

**Attributes:**
- `checked`: Checked state (true/false)
- `bind_checked`: Bind to subject
- All style properties

```xml
<lv_checkbox bind_checked="agreed">
    <lv_label text="I agree" />
</lv_checkbox>
```

#### `<lv_bar>`
Progress bar widget

**Attributes:**
- `min_value`: Minimum value
- `max_value`: Maximum value
- `value`: Current value
- `bind_value`: Bind to subject
- All style properties

```xml
<lv_bar bind_value="progress" 
        width="200" 
        min_value="0" 
        max_value="100" />
```

#### `<lv_arc>`
Circular arc/progress widget

**Attributes:**
- `min_value`: Minimum value
- `max_value`: Maximum value
- `value`: Current value
- `rotation`: Starting angle
- `bind_value`: Bind to subject
- All style properties

```xml
<lv_arc bind_value="temperature" 
        width="100" 
        height="100" 
        rotation="135"
        min_value="0" 
        max_value="100" />
```

#### `<lv_dropdown>`
Dropdown selection widget

**Attributes:**
- `options`: Options list (newline separated)
- `selected`: Selected index
- `bind_selected`: Bind to subject
- All style properties

```xml
<lv_dropdown options="Option 1&#10;Option 2&#10;Option 3" 
             bind_selected="selected_option" />
```

#### `<lv_roller>`
Roller selection widget

**Attributes:**
- `options`: Options list (newline separated)
- `selected`: Selected index
- `visible_rows`: Number of visible rows
- `bind_value`: Bind to subject
- `subject_enable`: Enable/disable based on subject
- All style properties

```xml
<roller options="00&#10;01&#10;02&#10;03&#10;04&#10;05" 
       bind_value="selected_hour"
       visible_rows="3" />
```

#### `<lv_textarea>`
Text input widget

**Attributes:**
- `text`: Text content
- `placeholder`: Placeholder text
- `one_line`: Single line mode (true/false)
- `bind_text`: Bind to subject
- All style properties

```xml
<lv_textarea placeholder="Enter text..." 
             one_line="true" 
             bind_text="user_input" />
```

---

## Style Properties

### Size and Position

```xml
<!-- Fixed size -->
<view width="100" height="50" />

<!-- Percentage size -->
<view width="100%" height="50%" />

<!-- Content size -->
<view width="content" height="content" />

<!-- Min/Max size -->
<view min_width="100" max_width="300" 
      min_height="50" max_height="200" />

<!-- Alignment -->
<lv_label text="Text" align="center" />
```

### Spacing

```xml
<!-- Padding all sides -->
<view style_pad_all="12" />

<!-- Padding horizontal/vertical -->
<view style_pad_hor="16" style_pad_ver="8" />

<!-- Padding individual sides -->
<view style_pad_left="8" 
      style_pad_right="8" 
      style_pad_top="4" 
      style_pad_bottom="4" />

<!-- Flex gaps -->
<row style_pad_row="8" />
<column style_pad_column="8" />
```

### Background

```xml
<!-- Background color -->
<view style_bg_color="0x2196F3" />

<!-- Background opacity -->
<view style_bg_opa="100%" />
<view style_bg_opa="50%" />

<!-- Border radius -->
<view style_radius="8" />
<view style_radius="50%" />
```

### Border

```xml
<!-- Border properties -->
<view style_border_color="0x000000" 
      style_border_width="1" 
      style_border_opa="100%" />

<!-- Border sides -->
<view style_border_side="top" />
<view style_border_side="bottom" />
<view style_border_side="left" />
<view style_border_side="right" />
<view style_border_side="all" />
```

### Text

```xml
<!-- Text properties -->
<lv_label style_text_color="0x000000" 
          style_text_font="roboto_bold_16"
          style_text_align="center" />

<!-- Text alignment -->
<lv_label style_text_align="left" />
<lv_label style_text_align="center" />
<lv_label style_text_align="right" />
```

### Opacity and Visibility

```xml
<!-- Opacity -->
<view style_opa="100%" />
<view style_opa="50%" />
<view style_opa_layered="50%" />

<!-- Visibility -->
<view>
    <visible value="true" />
</view>
```

---

## Layout Properties

### Flex Layout

```xml
<!-- Flex direction -->
<view flex_flow="row" />
<view flex_flow="column" />
<view flex_flow="row_wrap" />
<view flex_flow="column_wrap" />

<!-- Main axis alignment -->
<row style_flex_main_place="start" />
<row style_flex_main_place="end" />
<row style_flex_main_place="center" />
<row style_flex_main_place="space_between" />
<row style_flex_main_place="space_around" />
<row style_flex_main_place="space_evenly" />

<!-- Cross axis alignment -->
<row style_flex_cross_place="start" />
<row style_flex_cross_place="end" />
<row style_flex_cross_place="center" />
<row style_flex_cross_place="stretch" />

<!-- Track alignment (for wrap) -->
<row style_flex_track_place="start" />
<row style_flex_track_place="end" />
<row style_flex_track_place="center" />
<row style_flex_track_place="stretch" />
<row style_flex_track_place="space_between" />
<row style_flex_track_place="space_around" />
<row style_flex_track_place="space_evenly" />

<!-- Flex grow -->
<view flex_grow="1" />
<view flex_grow="2" />
```

### Grid Layout

```xml
<!-- Grid columns and rows -->
<view style_grid_template_cols="1fr 2fr 1fr" 
      style_grid_template_rows="auto 1fr auto" />

<!-- Grid placement -->
<view style_grid_cell_column_pos="0" 
      style_grid_cell_column_span="2"
      style_grid_cell_row_pos="0"
      style_grid_cell_row_span="1" />
```

---

## Events and Bindings

### Data Binding

#### Value Binding
```xml
<!-- Bind to subject value -->
<lv_label bind_text="status_message" />
<lv_slider bind_value="volume" />
<lv_switch bind_checked="is_on" />
```

#### Style Binding
```xml
<styles>
    <style name="style_light" bg_color="0xffffff" />
    <style name="style_dark" bg_color="0x1e1e1e" />
</styles>

<view>
    <!-- Apply style when subject equals ref_value -->
    <bind_style subject="dark_theme" name="style_light" ref_value="0" />
    <bind_style subject="dark_theme" name="style_dark" ref_value="1" />
</view>
```

#### Subject Enable/Disable
```xml
<!-- Enable widget when subject is 1, disable when 0 -->
<lv_button subject_enable="is_unlocked">
    <lv_label text="Submit" />
</lv_button>
```

### Events

#### Play Timeline Event
```xml
<lv_button>
    <play_timeline_event target="card_1" timeline="open" />
    <lv_label text="Animate" />
</lv_button>
```

---

## Animation API

### Timeline Definition

```xml
<animations>
    <timeline name="fade_in">
        <animation 
            target="self"              <!-- Target widget (or "self") -->
            prop="opa"                 <!-- Property to animate -->
            start="0"                  <!-- Start value -->
            end="255"                  <!-- End value -->
            duration="200"             <!-- Duration in ms -->
            delay="0"                  <!-- Delay in ms -->
            early_apply="true"         <!-- Apply start value before animation -->
            path="linear"              <!-- Easing curve -->
        />
    </timeline>
</animations>
```

### Animation Properties

**Target Properties:**
- `target`: Widget name or "self" (required)

**Animation Properties:**
- `prop`: Property to animate (required)
  - `opa`: Opacity (0-255)
  - `translate_x`, `translate_y`: Translation (pixels)
  - `scale_x`, `scale_y`: Scale (0-255, 128=100%)
  - `rotate`: Rotation (degrees * 10)
  - `width`, `height`: Size (pixels)

**Timing:**
- `duration`: Animation duration in ms (required)
- `delay`: Delay before start in ms (default: 0)
- `early_apply`: Apply start value immediately (true/false)

**Easing Curves (path):**
- `linear`: Linear interpolation
- `ease_in`: Start slow, end fast
- `ease_out`: Start fast, end slow
- `ease_in_out`: Start and end slow
- `overshoot`: Overshoot target
- `bounce`: Bounce effect

### Example Animations

#### Fade In
```xml
<animation target="self" prop="opa" start="0" end="255" duration="200" />
```

#### Slide Up
```xml
<animation target="panel" prop="translate_y" start="100" end="0" duration="300" />
```

#### Scale In
```xml
<animation target="button" prop="scale_x" start="0" end="128" duration="200" />
<animation target="button" prop="scale_y" start="0" end="128" duration="200" />
```

#### Rotate
```xml
<animation target="icon" prop="rotate" start="0" end="3600" duration="1000" />
```

#### Sequential Animations
```xml
<timeline name="sequence">
    <!-- First animation -->
    <animation target="label1" prop="opa" start="0" end="255" duration="200" />
    
    <!-- Second animation (starts after first) -->
    <animation target="label2" prop="opa" start="0" end="255" duration="200" delay="200" />
    
    <!-- Third animation (starts after second) -->
    <animation target="label3" prop="opa" start="0" end="255" duration="200" delay="400" />
</timeline>
```

---

## Constants and References

### Constant Reference (# prefix)

```xml
<!-- Define in globals.xml -->
<consts>
    <int name="unit_md" value="12" />
    <color name="primary" value="0x2196F3" />
</consts>

<!-- Use in components/screens -->
<view style_pad_all="#unit_md" style_bg_color="#primary" />
```

### Property Reference ($ prefix)

```xml
<!-- Define in component API -->
<api>
    <prop name="title" type="string" default="Title" />
</api>

<!-- Use in component -->
<lv_label text="$title" />
```

### Subject Reference (bind_*)

```xml
<!-- Define in globals.xml -->
<subjects>
    <int name="volume" value="50" />
</subjects>

<!-- Use in components/screens -->
<lv_slider bind_value="volume" />
```

### Image Reference (# prefix)

```xml
<!-- Define in globals.xml -->
<images>
    <file name="icon_home" src_path="images/home.png" />
</images>

<!-- Use in components/screens -->
<lv_image src="#icon_home" />
```

### Font Reference

```xml
<!-- Define in globals.xml -->
<fonts>
    <tiny_ttf name="roboto_bold_16" size="16" src_path="fonts/Roboto-Bold.ttf" />
</fonts>

<!-- Use in components/screens -->
<lv_label style_text_font="roboto_bold_16" />
```

---

## Special Elements

### Remove Default Styles

```xml
<view extends="lv_button">
    <remove_style_all />  <!-- Remove all default LVGL styles -->
    <style name="custom_style" />
</view>
```

### Visibility Control

```xml
<view>
    <visible value="true" />  <!-- or false -->
</view>
```

### Preview Configuration

```xml
<previews>
    <!-- Simple preview -->
    <preview />
    
    <!-- Custom size preview -->
    <preview width="200" height="100" />
    
    <!-- With background -->
    <preview width="300" height="200" style_bg_color="0xf0f0f0" />
    
    <!-- Multiple previews -->
    <preview width="200" height="100" style_bg_color="0xffffff" />
    <preview width="300" height="200" style_bg_color="0x000000" />
</previews>
```

---

## Common Patterns

### Pattern: Conditional Rendering Based on Subject

```xml
<view>
    <!-- Show when is_logged_in = 1 -->
    <bind_style subject="is_logged_in" name="visible_style" ref_value="1" />
    <!-- Hide when is_logged_in = 0 -->
    <bind_style subject="is_logged_in" name="hidden_style" ref_value="0" />
</view>
```

### Pattern: Multi-State Button

```xml
<styles>
    <style name="btn_normal" bg_color="0x2196F3" />
    <style name="btn_pressed" bg_color="0x1976D2" selector="pressed" />
    <style name="btn_disabled" bg_opa="50%" selector="disabled" />
</styles>

<view extends="lv_button">
    <style name="btn_normal" />
    <style name="btn_pressed" selector="pressed" />
    <style name="btn_disabled" selector="disabled" />
</view>
```

### Pattern: Responsive Layout

```xml
<row style_flex_main_place="space_between" style_pad_all="#unit_md">
    <view flex_grow="1"><!-- Content --></view>
    <view flex_grow="2"><!-- Content --></view>
    <view flex_grow="1"><!-- Content --></view>
</row>
```

### Pattern: Card with Header

```xml
<column style_bg_color="0xffffff" style_radius="12" style_pad_all="16">
    <!-- Header -->
    <row style_flex_main_place="space_between" style_pad_bottom="12">
        <lv_label text="Card Title" style_text_font="roboto_bold_16" />
        <lv_button>
            <lv_label text="×" />
        </lv_button>
    </row>
    
    <!-- Content -->
    <view>
        <!-- Card content here -->
    </view>
</column>
```

---

## Best Practices

1. **Naming:**
   - Use C variable naming rules (letters, numbers, underscores only)
   - Use descriptive names
   - Prefix screens with `screen_`

2. **Organization:**
   - Group related components in directories
   - Keep components small and focused
   - Define global resources in `globals.xml`

3. **Styling:**
   - Define reusable styles
   - Use constants for consistency
   - Leverage theme switching

4. **Performance:**
   - Reuse styles instead of duplicating
   - Use appropriate widget types
   - Optimize animations

5. **Maintainability:**
   - Document API properties with help text
   - Use meaningful constant/subject names
   - Keep XML files readable with proper indentation

---

## Quick Reference

### File Naming Rules
- ✅ letters, numbers, underscores only
- ✅ start with letter or underscore
- ❌ no hyphens, dots (except .xml), spaces

### Reference Prefixes
- `#` → Constants, images, fonts (from globals)
- `$` → Component properties (from API)
- `bind_*` → Subjects (data binding)

### Common Attributes
- `width`, `height`: Size
- `flex_flow`: Layout direction
- `style_pad_all`: Padding
- `style_bg_color`: Background
- `align`: Alignment
- `bind_value`: Value binding
- `subject_enable`: Conditional enable

### Common Widgets
- `<view>`: Container
- `<row>`, `<column>`: Flex containers
- `<lv_label>`: Text
- `<lv_button>`: Button
- `<lv_image>`: Image
- `<lv_slider>`: Slider
- `<lv_switch>`: Toggle

---

*For complete examples, see `/workspace/LVGL_XML_DOCUMENTATION.md` and `/workspace/examples/`*
