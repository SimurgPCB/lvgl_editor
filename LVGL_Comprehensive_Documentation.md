# LVGL Comprehensive API Documentation

## Table of Contents
1. [Core Objects](#core-objects)
2. [Widgets](#widgets)
3. [Styles](#styles)
4. [Animations](#animations)
5. [Events](#events)
6. [XML Support](#xml-support)
7. [File System](#file-system)
8. [Examples and Usage](#examples-and-usage)

## Core Objects

### lv_obj_t
The base object type for all LVGL widgets.

```c
// Create a base object
lv_obj_t * obj = lv_obj_create(lv_screen_active());

// Set basic properties
lv_obj_set_size(obj, 100, 50);
lv_obj_set_pos(obj, 10, 20);
lv_obj_set_style_bg_color(obj, lv_color_hex(0xff0000), 0);
```

### Screen Management
```c
// Get active screen
lv_obj_t * screen = lv_screen_active();

// Create new screen
lv_obj_t * new_screen = lv_obj_create(NULL);

// Load screen
lv_screen_load(new_screen);
```

## Widgets

### Button (lv_button)
```c
// Create button
lv_obj_t * btn = lv_button_create(lv_screen_active());
lv_obj_set_size(btn, 100, 50);
lv_obj_set_pos(btn, 10, 10);

// Add label to button
lv_obj_t * label = lv_label_create(btn);
lv_label_set_text(label, "Click me");

// Set button style
lv_obj_set_style_bg_color(btn, lv_color_hex(0x0066cc), 0);
lv_obj_set_style_bg_color(btn, lv_color_hex(0x004499), LV_STATE_PRESSED);
```

### Label (lv_label)
```c
// Create label
lv_obj_t * label = lv_label_create(lv_screen_active());
lv_label_set_text(label, "Hello World");
lv_obj_set_pos(label, 10, 10);

// Set text color
lv_obj_set_style_text_color(label, lv_color_hex(0xff0000), 0);

// Set font
lv_obj_set_style_text_font(label, &lv_font_montserrat_14, 0);
```

### Text Area (lv_textarea)
```c
// Create text area
lv_obj_t * ta = lv_textarea_create(lv_screen_active());
lv_obj_set_size(ta, 200, 100);
lv_obj_set_pos(ta, 10, 10);

// Set placeholder text
lv_textarea_set_placeholder_text(ta, "Enter text here...");

// Set one line mode
lv_textarea_set_one_line(ta, true);
```

### Slider (lv_slider)
```c
// Create slider
lv_obj_t * slider = lv_slider_create(lv_screen_active());
lv_obj_set_size(slider, 200, 20);
lv_obj_set_pos(slider, 10, 10);

// Set range
lv_slider_set_range(slider, 0, 100);

// Set initial value
lv_slider_set_value(slider, 50, LV_ANIM_OFF);
```

### Switch (lv_switch)
```c
// Create switch
lv_obj_t * sw = lv_switch_create(lv_screen_active());
lv_obj_set_pos(sw, 10, 10);

// Set state
lv_obj_add_state(sw, LV_STATE_CHECKED);
```

### Dropdown (lv_dropdown)
```c
// Create dropdown
lv_obj_t * dd = lv_dropdown_create(lv_screen_active());
lv_obj_set_pos(dd, 10, 10);

// Set options
lv_dropdown_set_options(dd, "Option 1\nOption 2\nOption 3");

// Set selected option
lv_dropdown_set_selected(dd, 1);
```

### List (lv_list)
```c
// Create list
lv_obj_t * list = lv_list_create(lv_screen_active());
lv_obj_set_size(list, 200, 300);
lv_obj_set_pos(list, 10, 10);

// Add list items
lv_obj_t * btn1 = lv_list_add_button(list, LV_SYMBOL_FILE, "File");
lv_obj_t * btn2 = lv_list_add_button(list, LV_SYMBOL_DIRECTORY, "Directory");
```

### Chart (lv_chart)
```c
// Create chart
lv_obj_t * chart = lv_chart_create(lv_screen_active());
lv_obj_set_size(chart, 200, 150);
lv_obj_set_pos(chart, 10, 10);

// Add series
lv_chart_series_t * ser = lv_chart_add_series(chart, lv_color_hex(0xff0000), LV_CHART_AXIS_PRIMARY_Y);

// Set data
lv_chart_set_point_count(chart, 10);
lv_chart_set_next_value(chart, ser, 10);
lv_chart_set_next_value(chart, ser, 20);
```

## Styles

### Creating and Applying Styles
```c
// Create style
static lv_style_t style;
lv_style_init(&style);

// Set style properties
lv_style_set_bg_color(&style, lv_color_hex(0x0066cc));
lv_style_set_border_width(&style, 2);
lv_style_set_border_color(&style, lv_color_hex(0x000000));
lv_style_set_radius(&style, 5);

// Apply style to object
lv_obj_add_style(obj, &style, 0);
```

### State-based Styles
```c
// Normal state
lv_style_set_bg_color(&style, lv_color_hex(0x0066cc), 0);

// Pressed state
lv_style_set_bg_color(&style, lv_color_hex(0x004499), LV_STATE_PRESSED);

// Checked state
lv_style_set_bg_color(&style, lv_color_hex(0x00aa00), LV_STATE_CHECKED);
```

## Animations

### Basic Animation
```c
// Animate position
lv_anim_t a;
lv_anim_init(&a);
lv_anim_set_var(&a, obj);
lv_anim_set_values(&a, 0, 100);
lv_anim_set_time(&a, 1000);
lv_anim_set_exec_cb(&a, (lv_anim_exec_xcb_t) lv_obj_set_x);
lv_anim_start(&a);
```

### Animation with Callback
```c
// Animation with callback
static void anim_callback(lv_anim_t * a) {
    lv_obj_t * obj = (lv_obj_t *) a->var;
    lv_obj_set_x(obj, a->current_value);
}

lv_anim_init(&a);
lv_anim_set_var(&a, obj);
lv_anim_set_values(&a, 0, 100);
lv_anim_set_time(&a, 1000);
lv_anim_set_exec_cb(&a, anim_callback);
lv_anim_start(&a);
```

## Events

### Event Handling
```c
// Event callback function
static void btn_event_cb(lv_event_t * e) {
    lv_event_code_t code = lv_event_get_code(e);
    lv_obj_t * btn = lv_event_get_target(e);
    
    if(code == LV_EVENT_CLICKED) {
        printf("Button clicked!\n");
    }
}

// Add event callback
lv_obj_add_event_cb(btn, btn_event_cb, LV_EVENT_ALL, NULL);
```

### Custom Events
```c
// Send custom event
lv_event_send(obj, LV_EVENT_USER_1, NULL);

// Handle custom event
static void custom_event_cb(lv_event_t * e) {
    lv_event_code_t code = lv_event_get_code(e);
    
    if(code == LV_EVENT_USER_1) {
        printf("Custom event received!\n");
    }
}
```

## XML Support

### XML Component Registration
```c
// Register component from file
lv_xml_component_register_from_file("path/to/component.xml");

// Register component from string
const char * xml_str = "<component>...</component>";
lv_xml_component_register_from_string("my_component", xml_str);
```

### XML Component Usage
```c
// Create object from XML component
lv_obj_t * obj = lv_xml_create(lv_screen_active(), "my_component", NULL);

// Create with parameters
lv_xml_create_params_t params;
params.btn_text = "Custom Text";
lv_obj_t * obj = lv_xml_create(lv_screen_active(), "my_component", &params);
```

### XML File Structure
```xml
<component>
    <consts>
        <px name="size" value="100"/>
        <color name="orange" value="0xffa020"/>
    </consts>
    
    <api>
        <prop name="btn_text" default="Apply" type="string"/>
    </api>
    
    <styles>
        <style name="blue" bg_color="0x0000ff" radius="2"/>
        <style name="red" bg_color="0xff0000"/>
    </styles>
    
    <view extends="lv_button" width="#size" styles="blue red:pressed">
        <my_h3 text="$btn_text" align="center" color="#orange"/>
    </view>
</component>
```

## File System

### File Operations
```c
// Read file
lv_fs_file_t file;
lv_fs_res_t res = lv_fs_open(&file, "path/to/file.txt", LV_FS_MODE_RD);
if(res == LV_FS_RES_OK) {
    char buffer[256];
    uint32_t bytes_read;
    lv_fs_read(&file, buffer, 256, &bytes_read);
    lv_fs_close(&file);
}
```

### Directory Operations
```c
// Open directory
lv_fs_dir_t dir;
lv_fs_res_t res = lv_fs_dir_open(&dir, "path/to/directory");
if(res == LV_FS_RES_OK) {
    lv_fs_dir_t entry;
    while(lv_fs_dir_read(&dir, &entry) == LV_FS_RES_OK) {
        printf("File: %s\n", entry.d_name);
    }
    lv_fs_dir_close(&dir);
}
```

## Examples and Usage

### Complete Button Example
```c
#include "lvgl.h"

static void btn_event_cb(lv_event_t * e) {
    lv_event_code_t code = lv_event_get_code(e);
    if(code == LV_EVENT_CLICKED) {
        printf("Button clicked!\n");
    }
}

void create_button_example(void) {
    // Create button
    lv_obj_t * btn = lv_button_create(lv_screen_active());
    lv_obj_set_size(btn, 120, 50);
    lv_obj_set_pos(btn, 10, 10);
    
    // Add label
    lv_obj_t * label = lv_label_create(btn);
    lv_label_set_text(label, "Click Me");
    lv_obj_center(label);
    
    // Set style
    lv_obj_set_style_bg_color(btn, lv_color_hex(0x0066cc), 0);
    lv_obj_set_style_bg_color(btn, lv_color_hex(0x004499), LV_STATE_PRESSED);
    lv_obj_set_style_radius(btn, 5, 0);
    
    // Add event
    lv_obj_add_event_cb(btn, btn_event_cb, LV_EVENT_CLICKED, NULL);
}
```

### Complete Form Example
```c
void create_form_example(void) {
    // Create container
    lv_obj_t * cont = lv_obj_create(lv_screen_active());
    lv_obj_set_size(cont, 300, 200);
    lv_obj_center(cont);
    lv_obj_set_flex_flow(cont, LV_FLEX_FLOW_COLUMN);
    lv_obj_set_flex_align(cont, LV_FLEX_ALIGN_CENTER, LV_FLEX_ALIGN_CENTER, LV_FLEX_ALIGN_CENTER);
    
    // Title
    lv_obj_t * title = lv_label_create(cont);
    lv_label_set_text(title, "User Form");
    lv_obj_set_style_text_font(title, &lv_font_montserrat_16, 0);
    
    // Name input
    lv_obj_t * name_label = lv_label_create(cont);
    lv_label_set_text(name_label, "Name:");
    
    lv_obj_t * name_ta = lv_textarea_create(cont);
    lv_obj_set_size(name_ta, 200, 30);
    lv_textarea_set_placeholder_text(name_ta, "Enter your name");
    
    // Email input
    lv_obj_t * email_label = lv_label_create(cont);
    lv_label_set_text(email_label, "Email:");
    
    lv_obj_t * email_ta = lv_textarea_create(cont);
    lv_obj_set_size(email_ta, 200, 30);
    lv_textarea_set_placeholder_text(email_ta, "Enter your email");
    
    // Submit button
    lv_obj_t * submit_btn = lv_button_create(cont);
    lv_obj_set_size(submit_btn, 100, 40);
    lv_obj_t * submit_label = lv_label_create(submit_btn);
    lv_label_set_text(submit_label, "Submit");
    lv_obj_center(submit_label);
}
```

### Animation Example
```c
static void anim_x_cb(void * var, int32_t v) {
    lv_obj_set_x((lv_obj_t *)var, v);
}

void create_animation_example(void) {
    // Create object
    lv_obj_t * obj = lv_obj_create(lv_screen_active());
    lv_obj_set_size(obj, 50, 50);
    lv_obj_set_style_bg_color(obj, lv_color_hex(0xff0000), 0);
    
    // Create animation
    lv_anim_t a;
    lv_anim_init(&a);
    lv_anim_set_var(&a, obj);
    lv_anim_set_values(&a, 0, 200);
    lv_anim_set_time(&a, 2000);
    lv_anim_set_exec_cb(&a, anim_x_cb);
    lv_anim_set_repeat_count(&a, LV_ANIM_REPEAT_INFINITE);
    lv_anim_set_repeat_delay(&a, 1000);
    lv_anim_start(&a);
}
```

## Best Practices

### Memory Management
```c
// Always check for NULL when creating objects
lv_obj_t * obj = lv_obj_create(lv_screen_active());
if(obj == NULL) {
    printf("Failed to create object\n");
    return;
}

// Clean up when done
lv_obj_del(obj);
```

### Performance Optimization
```c
// Use LV_ANIM_OFF for immediate updates when animation is not needed
lv_obj_set_x(obj, 100, LV_ANIM_OFF);

// Use LV_ANIM_ON for smooth transitions
lv_obj_set_x(obj, 100, LV_ANIM_ON);
```

### Error Handling
```c
// Check return values
lv_fs_res_t res = lv_fs_open(&file, "path", LV_FS_MODE_RD);
if(res != LV_FS_RES_OK) {
    printf("Failed to open file: %d\n", res);
    return;
}
```

## File Naming Conventions for LVGL UI Editor

### Valid Names
- ✅ `testDeneme.xml`
- ✅ `myScreen.xml`
- ✅ `userInterface.xml`
- ✅ `button1.xml`
- ✅ `main_screen.xml`

### Invalid Names
- ❌ `test-deneme.xml` (contains hyphen)
- ❌ `my screen.xml` (contains space)
- ❌ `user@interface.xml` (contains special character)
- ❌ `1button.xml` (starts with number)

### Naming Rules
1. Must start with a letter (a-z, A-Z) or underscore (_)
2. Can contain letters, numbers, and underscores
3. Cannot contain hyphens (-), spaces, or special characters
4. Cannot start with a number
5. Must be a valid C variable name

This documentation provides comprehensive coverage of LVGL's APIs, functions, and components with practical examples and usage instructions.