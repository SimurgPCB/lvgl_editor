# LVGL XML Cheat Sheet

## 🚨 File Naming Rules (CRITICAL!)

### ✅ VALID Names
```
alarm.xml
screen_main.xml
my_button_v2.xml
user_profile.xml
_private.xml
button1.xml
```

### ❌ INVALID Names (Causes "Wrong Name" Error)
```
my-button.xml       ← NO hyphens!
screen.main.xml     ← NO dots (except .xml)!
my component.xml    ← NO spaces!
1_button.xml        ← NO starting with numbers!
```

**Rule:** Only letters, numbers, underscores. Must start with letter or underscore.

---

## 📂 Project Structure

```
project/
├── project.xml          # Display config
├── globals.xml         # Global resources
├── components/         # Reusable UI
│   └── my_button/
│       └── my_button.xml
├── screens/           # App screens
│   └── screen_main.xml
├── images/            # Assets
└── fonts/             # Fonts
```

---

## 🔤 Reference Syntax

```xml
<!-- Constants (from globals) -->
<view style_pad_all="#unit_md" />

<!-- Component properties (from API) -->
<lv_label text="$title" />

<!-- Data binding (from subjects) -->
<lv_slider bind_value="volume" />

<!-- Images -->
<lv_image src="#icon_home" />

<!-- Fonts -->
<lv_label style_text_font="roboto_bold_16" />
```

---

## 📦 Component Template

```xml
<component>
    <previews>
        <preview width="200" height="100" />
    </previews>

    <api>
        <prop name="text" type="string" default="Button"/>
    </api>

    <styles>
        <style name="base" bg_color="0x2196F3" radius="8" />
    </styles>

    <view extends="lv_button">
        <style name="base"/>
        <lv_label text="$text" align="center"/>
    </view>
</component>
```

---

## 📱 Screen Template

```xml
<screen>
    <styles>
        <style name="bg" bg_color="0xffffff" />
    </styles>

    <view flex_flow="column" style_pad_all="16">
        <style name="bg" />
        <!-- Content here -->
    </view>
</screen>
```

---

## 🌍 globals.xml Template

```xml
<globals name="my_app">
    <consts>
        <int name="unit_md" value="12" />
        <color name="primary" value="0x2196F3" />
    </consts>

    <subjects>
        <int name="dark_theme" value="0" help="0:light 1:dark" />
        <int name="volume" value="50" min_value="0" max_value="100" />
    </subjects>

    <styles>
        <style name="disabled" opa_layered="50%" />
    </styles>

    <images>
        <file name="icon_home" src_path="images/home.png" />
    </images>

    <fonts>
        <tiny_ttf name="roboto_16" size="16" as_file="true" 
                  src_path="fonts/Roboto.ttf" />
    </fonts>
</globals>
```

---

## 🎨 Style Properties

### Size & Position
```xml
width="100"         <!-- Pixels -->
width="100%"        <!-- Percentage -->
width="content"     <!-- Fit content -->
align="center"      <!-- Alignment -->
```

### Spacing
```xml
style_pad_all="12"               <!-- All sides -->
style_pad_hor="16"               <!-- Horizontal -->
style_pad_ver="8"                <!-- Vertical -->
style_pad_left="8"               <!-- Individual -->
style_pad_row="8"                <!-- Flex gap -->
```

### Background & Border
```xml
style_bg_color="0x2196F3"       <!-- Background -->
style_bg_opa="100%"             <!-- Opacity -->
style_radius="8"                <!-- Border radius -->
style_border_color="0x000000"   <!-- Border -->
style_border_width="1"
```

### Text
```xml
style_text_color="0x000000"     <!-- Color -->
style_text_font="roboto_16"     <!-- Font -->
style_text_align="center"       <!-- Alignment -->
```

---

## 📐 Layout

### Flex Layout
```xml
<!-- Direction -->
flex_flow="row"                 <!-- Horizontal -->
flex_flow="column"              <!-- Vertical -->
flex_flow="row_wrap"            <!-- Wrap -->

<!-- Main axis -->
style_flex_main_place="start"
style_flex_main_place="center"
style_flex_main_place="end"
style_flex_main_place="space_between"
style_flex_main_place="space_around"
style_flex_main_place="space_evenly"

<!-- Cross axis -->
style_flex_cross_place="start"
style_flex_cross_place="center"
style_flex_cross_place="end"
style_flex_cross_place="stretch"

<!-- Grow -->
flex_grow="1"                   <!-- Flex grow -->
```

### Shortcuts
```xml
<row>   <!-- flex_flow="row" -->
<column> <!-- flex_flow="column" -->
<div>    <!-- Scrollable container -->
```

---

## 🧩 Widgets

### Common Widgets
```xml
<!-- Text -->
<lv_label text="Hello" />

<!-- Button -->
<lv_button>
    <lv_label text="Click" align="center" />
</lv_button>

<!-- Image -->
<lv_image src="#icon_home" />

<!-- Slider -->
<lv_slider bind_value="volume" width="150" 
           min_value="0" max_value="100" />

<!-- Switch -->
<lv_switch bind_checked="is_on" />

<!-- Progress Bar -->
<lv_bar bind_value="progress" width="200" />

<!-- Arc (circular) -->
<lv_arc bind_value="temp" width="100" height="100" />

<!-- Checkbox -->
<lv_checkbox bind_checked="agreed">
    <lv_label text="I agree" />
</lv_checkbox>

<!-- Dropdown -->
<lv_dropdown options="A&#10;B&#10;C" />

<!-- Roller -->
<roller options="1&#10;2&#10;3" visible_rows="3" />
```

---

## 🔗 Data Binding

### Define Subject (globals.xml)
```xml
<subjects>
    <int name="volume" value="50" min_value="0" max_value="100" />
    <string name="status" value="Ready" />
    <int name="is_on" value="0" help="0:off 1:on" />
</subjects>
```

### Bind Value
```xml
<lv_label bind_text="status" />
<lv_slider bind_value="volume" />
<lv_switch bind_checked="is_on" />
```

### Bind Style
```xml
<styles>
    <style name="light" bg_color="0xffffff" />
    <style name="dark" bg_color="0x1e1e1e" />
</styles>

<view>
    <!-- Apply when dark_theme = 0 -->
    <bind_style subject="dark_theme" name="light" ref_value="0" />
    <!-- Apply when dark_theme = 1 -->
    <bind_style subject="dark_theme" name="dark" ref_value="1" />
</view>
```

### Subject Enable
```xml
<!-- Enable when is_unlocked = 1 -->
<lv_button subject_enable="is_unlocked">
    <lv_label text="Submit" />
</lv_button>
```

---

## 🎬 Animations

### Define Timeline
```xml
<animations>
    <timeline name="fade_in">
        <animation 
            target="self"              <!-- Target widget -->
            prop="opa"                 <!-- Property -->
            start="0"                  <!-- Start value -->
            end="255"                  <!-- End value -->
            duration="200"             <!-- Duration (ms) -->
            delay="0"                  <!-- Delay (ms) -->
            early_apply="true"         <!-- Apply start immediately -->
            path="linear"              <!-- Easing -->
        />
    </timeline>
</animations>
```

### Animatable Properties
```
opa              → Opacity (0-255)
translate_x      → X translation
translate_y      → Y translation
scale_x          → X scale (128 = 100%)
scale_y          → Y scale (128 = 100%)
rotate           → Rotation (degrees * 10)
width, height    → Size
```

### Easing Curves (path)
```
linear           → Linear
ease_in          → Start slow
ease_out         → End slow
ease_in_out      → Both slow
overshoot        → Overshoot
bounce           → Bounce
```

### Play Timeline
```xml
<lv_button>
    <play_timeline_event target="card" timeline="open" />
    <lv_label text="Animate" />
</lv_button>
```

---

## 🎯 Common Patterns

### Theme Switching
```xml
<styles>
    <style name="light" bg_color="0xffffff" text_color="0x000000" />
    <style name="dark" bg_color="0x1e1e1e" text_color="0xffffff" />
</styles>

<view>
    <bind_style subject="dark_theme" name="light" ref_value="0" />
    <bind_style subject="dark_theme" name="dark" ref_value="1" />
</view>
```

### Button with States
```xml
<styles>
    <style name="btn_base" bg_color="0x2196F3" />
    <style name="btn_pressed" bg_color="0x1976D2" selector="pressed" />
</styles>

<view extends="lv_button">
    <remove_style_all />
    <style name="btn_base"/>
    <style name="btn_pressed" selector="pressed" />
</view>
```

### Horizontal Layout
```xml
<row style_flex_main_place="space_between" style_pad_all="8">
    <lv_label text="Left" />
    <lv_label text="Right" />
</row>
```

### Vertical Layout
```xml
<column style_flex_cross_place="center" style_pad_all="8">
    <lv_label text="Top" />
    <lv_label text="Bottom" />
</column>
```

### Card Container
```xml
<view style_bg_color="0xffffff" 
      style_radius="12" 
      style_pad_all="16"
      style_border_width="1"
      style_border_color="0xe0e0e0">
    <!-- Card content -->
</view>
```

### Fade In Animation
```xml
<animations>
    <timeline name="appear">
        <animation target="self" prop="opa" 
                   start="0" end="255" 
                   duration="200" early_apply="true" />
    </timeline>
</animations>
```

### Slide Up Animation
```xml
<animation target="panel" prop="translate_y" 
           start="100" end="0" 
           duration="300" path="ease_out" 
           early_apply="true" />
```

---

## 🐛 Troubleshooting

| Problem | Solution |
|---------|----------|
| "Wrong Name" error | Use only letters, numbers, underscores. No hyphens! |
| Component not found | Check file/folder naming, verify XML syntax |
| Style not applied | Check style definition order, verify name spelling |
| Binding not working | Verify subject exists in globals.xml |
| Animation not playing | Check target name, verify timeline name |

---

## 📏 Property Types

### Component API
```xml
<api>
    <prop name="text" type="string" default="Text"/>
    <prop name="count" type="int" default="0" min_value="0" max_value="100"/>
    <prop name="visible" type="bool" default="true"/>
    <prop name="bg_color" type="color" default="0xffffff"/>
    <prop name="icon" type="image" default=""/>
    <prop name="font" type="font" default=""/>
</api>
```

### Using Properties
```xml
<lv_label text="$text" />
<style bg_color="$bg_color" />
<lv_image src="$icon" />
<lv_label style_text_font="$font" />
```

---

## 🔢 Constants

### Define (globals.xml)
```xml
<consts>
    <int name="unit_sm" value="8" />
    <int name="unit_md" value="16" />
    <percentage name="opa_50" value="50%" />
    <color name="primary" value="0x2196F3" />
    <string name="app_name" value="My App" />
</consts>
```

### Use
```xml
<view style_pad_all="#unit_md" 
      style_bg_color="#primary"
      style_bg_opa="#opa_50" />
```

---

## 🖼️ Images & Fonts

### Images
```xml
<!-- Define in globals.xml -->
<images>
    <file name="icon_home" src_path="images/home.png" />
</images>

<!-- Use -->
<lv_image src="#icon_home" />
```

### Fonts
```xml
<!-- Define in globals.xml -->
<fonts>
    <tiny_ttf name="roboto_16" size="16" as_file="true" 
              src_path="fonts/Roboto-Regular.ttf" />
</fonts>

<!-- Use -->
<lv_label style_text_font="roboto_16" />
```

---

## ⚡ Quick Tips

1. **Always check naming** - Use underscores, not hyphens
2. **Use constants** - For colors, spacing, opacity
3. **Bind to subjects** - For reactive UIs
4. **Define global styles** - Reuse across components
5. **Use row/column** - Easier than manual flex
6. **Add help text** - Document your API properties
7. **Preview your work** - Test in LVGL Editor
8. **Organize components** - Group in folders

---

## 📚 Resources

- **Full Docs:** `/workspace/LVGL_XML_DOCUMENTATION.md`
- **API Ref:** `/workspace/API_REFERENCE.md`
- **Turkish Guide:** `/workspace/COZUM_NAMING_HATASI.md`
- **Quick Start:** `/workspace/examples/QUICK_START_GUIDE.md`
- **Official:** https://docs.lvgl.io/master/details/xml/index.html

---

## 🎓 Your Issue: "Wrong Name" Error

### Problem
File/folder names with hyphens, dots, or special characters

### Solution
```
❌ test-deneme.xml     → ✅ test_deneme.xml
❌ my.component.xml    → ✅ my_component.xml
❌ my screen.xml       → ✅ my_screen.xml
❌ 1_button.xml        → ✅ button_1.xml
```

**Remember:** C variable naming rules apply to ALL file and folder names!
