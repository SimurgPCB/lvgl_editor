# LVGL XML Documentation

## Table of Contents
1. [Overview](#overview)
2. [Naming Conventions](#naming-conventions)
3. [Project Structure](#project-structure)
4. [XML File Types](#xml-file-types)
5. [API Reference](#api-reference)
6. [Common Issues and Solutions](#common-issues-and-solutions)
7. [Examples](#examples)
8. [Best Practices](#best-practices)

## Overview

LVGL XML provides a declarative way to create user interfaces using XML markup. This system allows you to define screens, components, styles, and resources in a structured format that gets compiled to C code.

## Naming Conventions

### ⚠️ Critical: File and Variable Naming Rules

The "Wrong Name" error occurs when file names don't follow C variable naming conventions. **All XML file names must be valid C identifiers.**

#### Valid Naming Rules:
- ✅ Use only letters, numbers, and underscores
- ✅ Start with a letter or underscore
- ✅ Use snake_case convention
- ✅ Examples: `my_screen.xml`, `button_component.xml`, `main_view.xml`

#### Invalid Naming (Causes "Wrong Name" Error):
- ❌ `test_deneme.xml` - Contains non-ASCII characters (Turkish 'ğ' equivalent)
- ❌ `my-screen.xml` - Contains hyphens
- ❌ `123screen.xml` - Starts with number
- ❌ `my screen.xml` - Contains spaces
- ❌ `test@home.xml` - Contains special characters

#### Solution for Turkish Characters:
Replace Turkish characters with ASCII equivalents:
- `ğ` → `g`
- `ş` → `s` 
- `ç` → `c`
- `ı` → `i`
- `ö` → `o`
- `ü` → `u`

**Example Fix:**
- ❌ `test_deneme.xml` → ✅ `test_deneme.xml` (if no Turkish chars)
- ❌ `ekran_görünümü.xml` → ✅ `ekran_gorunumu.xml`

## Project Structure

```
project/
├── project.xml          # Main project configuration
├── globals.xml          # Global resources (styles, colors, fonts, images)
├── screens/            # Screen definitions
│   ├── screen_name.xml
│   └── ...
├── components/         # Reusable components
│   ├── component_name.xml
│   └── ...
├── widgets/           # Custom widgets
├── fonts/            # Font files
├── images/           # Image assets
└── preview-bin/      # Generated preview files
```

## XML File Types

### 1. Project Configuration (`project.xml`)

Defines display targets and project settings.

```xml
<project>
    <targets>
        <target name="target1">
            <display width="320" height="240"/>
        </target>
    </targets>
</project>
```

### 2. Global Resources (`globals.xml`)

Contains shared resources like styles, colors, fonts, and images.

```xml
<globals name="project_name">
    <api>
        <!-- Enum definitions -->
    </api>
    
    <consts>
        <!-- Constants -->
        <int name="unit_sm" value="6" />
        <color name="primary_color" value="0x0066cc" />
    </consts>
    
    <styles>
        <!-- Global styles -->
        <style name="button_style" bg_color="#primary_color" />
    </styles>
    
    <subjects>
        <!-- Data binding subjects -->
        <int name="counter" value="0" />
    </subjects>
    
    <images>
        <!-- Image resources -->
        <file name="icon_home" src_path="images/home.png" />
    </images>
    
    <fonts>
        <!-- Font resources -->
        <tiny_ttf name="font_medium" size="16" src_path="fonts/arial.ttf" />
    </fonts>
</globals>
```

### 3. Screen Definitions (`screens/screen_name.xml`)

Define complete screens/pages.

```xml
<screen>
    <styles>
        <!-- Screen-specific styles -->
        <style name="bg_style" bg_color="0xffffff" />
    </styles>
    
    <view>
        <!-- Screen content -->
        <style name="bg_style" />
        
        <lv_button align="center">
            <lv_label text="Hello World" />
        </lv_button>
    </view>
</screen>
```

### 4. Component Definitions (`components/component_name.xml`)

Define reusable UI components.

```xml
<component>
    <api>
        <!-- Component properties -->
        <prop name="button_text" type="string" default="Click me" />
        <prop name="is_enabled" type="bool" default="true" />
    </api>
    
    <styles>
        <!-- Component styles -->
        <style name="base_style" bg_color="0x0066cc" />
    </styles>
    
    <view extends="lv_button">
        <!-- Component structure -->
        <style name="base_style" />
        <lv_label text="$button_text" align="center" />
    </view>
</component>
```

## API Reference

### Core Elements

#### `<project>`
Main project container.

**Attributes:** None
**Children:** `<targets>`

#### `<targets>`
Container for display targets.

**Children:** `<target>`

#### `<target>`
Display target definition.

**Attributes:**
- `name` (string): Target identifier

**Children:** `<display>`

#### `<display>`
Display configuration.

**Attributes:**
- `width` (number): Display width in pixels
- `height` (number): Display height in pixels

### Global Resources

#### `<globals>`
Global resources container.

**Attributes:**
- `name` (string): Project name (must be valid C identifier)

**Children:** `<api>`, `<consts>`, `<styles>`, `<subjects>`, `<images>`, `<fonts>`

#### `<consts>`
Constants definition container.

**Children:** `<int>`, `<color>`, `<percentage>`

#### `<int>`
Integer constant.

**Attributes:**
- `name` (string): Constant name
- `value` (number): Integer value

#### `<color>`
Color constant.

**Attributes:**
- `name` (string): Color name
- `value` (hex): Color value (e.g., "0xff0000")

#### `<percentage>`
Percentage constant.

**Attributes:**
- `name` (string): Percentage name
- `value` (string): Percentage value (e.g., "50%")

### Styles

#### `<styles>`
Styles container.

**Children:** `<style>`

#### `<style>`
Style definition.

**Attributes:**
- `name` (string): Style name
- Style properties (various): Any LVGL style property

**Common Style Properties:**
- `width`, `height`: Dimensions
- `bg_color`, `bg_opa`: Background
- `border_width`, `border_color`: Border
- `pad_all`, `pad_top`, `pad_bottom`, `pad_left`, `pad_right`: Padding
- `margin_all`, `margin_top`, `margin_bottom`, `margin_left`, `margin_right`: Margin
- `radius`: Border radius
- `text_color`, `text_font`: Text styling

### UI Elements

#### `<view>`
Container element (maps to `lv_obj`).

**Attributes:**
- `extends` (string): Base widget type (default: "lv_obj")
- Layout and style attributes

#### `<lv_button>`
Button widget.

**Attributes:**
- Standard widget attributes
- `align`: Alignment (e.g., "center", "top_left")

#### `<lv_label>`
Label widget.

**Attributes:**
- `text` (string): Label text
- `align`: Text alignment

#### `<lv_image>`
Image widget.

**Attributes:**
- `src` (string): Image source (reference to image in globals)

### Layout Elements

#### `<row>`
Horizontal layout container.

**Attributes:**
- Layout and style attributes

#### `<column>`
Vertical layout container.

**Attributes:**
- Layout and style attributes

#### `<div>`
Generic container with flex layout.

**Attributes:**
- `flex_flow`: Flex direction ("row", "column", "row_wrap", "column_wrap")
- `flex_grow`: Flex grow factor
- Layout and style attributes

### Data Binding

#### `<subjects>`
Data subjects container (for data binding).

**Children:** `<int>`, `<string>`, `<bool>`

#### `<bind_style>`
Conditional style binding.

**Attributes:**
- `subject` (string): Subject name to bind to
- `name` (string): Style name to apply
- `ref_value`: Reference value for condition

### Events

#### `<play_timeline_event>`
Timeline animation event.

**Attributes:**
- `target` (string): Animation target
- `timeline` (string): Timeline name

### Component API

#### `<api>`
Component API definition.

**Children:** `<prop>`, `<enumdefs>`

#### `<prop>`
Component property.

**Attributes:**
- `name` (string): Property name
- `type` (string): Property type ("string", "int", "bool", "color")
- `default`: Default value

## Common Issues and Solutions

### 1. "Wrong Name" Error

**Problem:** File names contain invalid characters for C identifiers.

**Solution:**
```bash
# Invalid
test_deneme.xml
my-component.xml
123screen.xml

# Valid
test_deneme.xml  # If no Turkish chars
my_component.xml
screen_123.xml
```

### 2. XML Parsing Errors

**Problem:** Malformed XML structure.

**Solution:**
- Ensure all tags are properly closed
- Check for proper nesting
- Validate XML syntax

### 3. Missing Resources

**Problem:** Referenced styles, images, or fonts not found.

**Solution:**
- Ensure all resources are defined in `globals.xml`
- Check resource names match exactly (case-sensitive)
- Verify file paths for images and fonts

### 4. Build Errors

**Problem:** Generated C code doesn't compile.

**Solution:**
- Check for naming conflicts
- Ensure all dependencies are included
- Verify LVGL configuration

## Examples

### Example 1: Simple Screen

**File:** `screens/welcome_screen.xml`

```xml
<screen>
    <styles>
        <style name="bg_style" 
               bg_color="0xf0f0f0" 
               pad_all="20" />
        <style name="title_style" 
               text_color="0x333333" 
               text_font="font_large" />
    </styles>
    
    <view>
        <style name="bg_style" />
        
        <column align="center" width="100%" height="100%">
            <lv_label text="Welcome!" align="center">
                <style name="title_style" />
            </lv_label>
            
            <lv_button align="center" style_margin_top="20">
                <lv_label text="Get Started" />
            </lv_button>
        </column>
    </view>
</screen>
```

### Example 2: Reusable Button Component

**File:** `components/custom_button.xml`

```xml
<component>
    <api>
        <prop name="button_text" type="string" default="Button" />
        <prop name="button_color" type="color" default="0x0066cc" />
        <prop name="is_primary" type="bool" default="false" />
    </api>
    
    <styles>
        <style name="base_style"
               bg_opa="100%"
               width="content"
               height="content"
               radius="8"
               pad_hor="16"
               pad_ver="8"
               text_color="0xffffff" />
        
        <style name="primary_style"
               bg_color="0x0066cc" />
               
        <style name="secondary_style"
               bg_color="0x666666" />
               
        <style name="pressed_style"
               bg_opa="80%" />
    </styles>
    
    <view extends="lv_button">
        <remove_style_all />
        <style name="base_style" />
        
        <!-- Conditional styling based on is_primary prop -->
        <bind_style subject="is_primary" name="primary_style" ref_value="true" />
        <bind_style subject="is_primary" name="secondary_style" ref_value="false" />
        
        <style name="pressed_style" selector="pressed" />
        
        <lv_label text="$button_text" align="center" />
    </view>
</component>
```

### Example 3: Using the Custom Button

**File:** `screens/button_demo.xml`

```xml
<screen>
    <view style_pad_all="20">
        <column width="100%" style_flex_cross_place="center">
            <!-- Use the custom button component -->
            <custom_button 
                button_text="Primary Button" 
                is_primary="true" />
                
            <custom_button 
                button_text="Secondary Button" 
                is_primary="false" 
                style_margin_top="10" />
        </column>
    </view>
</screen>
```

### Example 4: Data Binding Example

**File:** `globals.xml` (partial)

```xml
<globals name="counter_app">
    <subjects>
        <int name="counter_value" value="0" />
        <bool name="is_positive" value="false" />
    </subjects>
    
    <styles>
        <style name="positive_style" text_color="0x00aa00" />
        <style name="negative_style" text_color="0xaa0000" />
        <style name="zero_style" text_color="0x666666" />
    </styles>
</globals>
```

**File:** `screens/counter_screen.xml`

```xml
<screen>
    <view style_pad_all="20">
        <column align="center" width="100%">
            <lv_label text="Counter: $counter_value" align="center">
                <!-- Conditional styling based on counter value -->
                <bind_style subject="counter_value" name="positive_style" condition=">" ref_value="0" />
                <bind_style subject="counter_value" name="negative_style" condition="<" ref_value="0" />
                <bind_style subject="counter_value" name="zero_style" condition="==" ref_value="0" />
            </lv_label>
            
            <row style_margin_top="20">
                <lv_button>
                    <lv_label text="-" />
                    <!-- Event to decrease counter -->
                    <change_subject_event subject="counter_value" operation="add" value="-1" />
                </lv_button>
                
                <lv_button style_margin_left="10">
                    <lv_label text="+" />
                    <!-- Event to increase counter -->
                    <change_subject_event subject="counter_value" operation="add" value="1" />
                </lv_button>
            </row>
        </column>
    </view>
</screen>
```

## Best Practices

### 1. File Organization
- Use descriptive, valid C identifier names
- Organize components by functionality
- Keep related screens in subdirectories if needed

### 2. Naming Conventions
- Use snake_case for all names
- Prefix component names with purpose (e.g., `button_`, `card_`, `modal_`)
- Use descriptive names for styles and subjects

### 3. Style Management
- Define common styles in `globals.xml`
- Use consistent naming for similar styles
- Leverage style inheritance and composition

### 4. Component Design
- Keep components focused and reusable
- Use meaningful property names
- Provide sensible defaults
- Document component APIs

### 5. Data Binding
- Use descriptive subject names
- Keep subject scope minimal
- Consider performance implications of frequent updates

### 6. Performance
- Minimize deep nesting
- Use appropriate widget types
- Optimize image and font resources
- Consider memory constraints

### 7. Maintenance
- Comment complex logic
- Use consistent formatting
- Version control all XML files
- Test on target hardware regularly

## Troubleshooting Checklist

When encountering issues:

1. ✅ Check file names are valid C identifiers
2. ✅ Verify XML syntax is correct
3. ✅ Ensure all referenced resources exist
4. ✅ Check for naming conflicts
5. ✅ Validate property names and values
6. ✅ Review generated C code for errors
7. ✅ Test with minimal examples first
8. ✅ Check LVGL version compatibility

## Additional Resources

- [LVGL Official Documentation](https://docs.lvgl.io/)
- [LVGL XML Reference](https://docs.lvgl.io/master/details/xml/)
- [LVGL GitHub Repository](https://github.com/lvgl/lvgl)
- [LVGL Forum](https://forum.lvgl.io/)

---

*This documentation covers LVGL XML v1.0.0-rc2. Features and syntax may change in future versions.*