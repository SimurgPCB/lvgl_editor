# LVGL XML Quick Start Guide

## Creating Your First Component

### Step 1: Create Component Directory

**✅ Correct naming:**
```
components/my_button/
```

**❌ Wrong naming:**
```
components/my-button/     ← Hyphen not allowed!
components/MyButton/      ← Works but not consistent
```

### Step 2: Create Component XML File

**File: `components/my_button/my_button.xml`**

```xml
<component>
    <!-- Preview configuration -->
    <previews>
        <preview width="120" height="40" style_bg_color="0xf0f0f0" />
    </previews>

    <!-- Component API (properties) -->
    <api>
        <prop name="text" type="string" default="Button" help="Button label text"/>
        <prop name="enabled" type="bool" default="true" help="Enable or disable button"/>
    </api>

    <!-- Styles -->
    <styles>
        <style name="btn_base" 
               bg_color="0x2196F3"
               text_color="0xffffff"
               radius="8"
               pad_hor="16"
               pad_ver="8" />
        <style name="btn_pressed" 
               bg_color="0x1976D2" 
               selector="pressed" />
        <style name="btn_disabled"
               bg_opa="50%"
               selector="disabled" />
    </styles>

    <!-- Component structure -->
    <view extends="lv_button">
        <style name="btn_base"/>
        <style name="btn_pressed" selector="pressed" />
        <style name="btn_disabled" selector="disabled" />
        
        <lv_label text="$text" align="center"/>
    </view>
</component>
```

### Step 3: Create Your First Screen

**File: `screens/screen_main.xml`**

```xml
<screen>
    <!-- Screen styles -->
    <styles>
        <style name="screen_bg" 
               bg_color="0xffffff" 
               pad_all="16" />
    </styles>

    <!-- Screen layout -->
    <view flex_flow="column" style_pad_all="16">
        <style name="screen_bg" />
        
        <!-- Title -->
        <lv_label text="Welcome" 
                  style_text_font="roboto_bold_20" />
        
        <!-- Use your custom component -->
        <my_button text="Click Me" />
        <my_button text="Settings" />
        <my_button text="Exit" />
    </view>
</screen>
```

## File Naming Checklist

Before creating any file or folder:

- [ ] ✅ Name starts with letter or underscore
- [ ] ✅ Name contains only letters, numbers, underscores
- [ ] ✅ No hyphens (-)
- [ ] ✅ No dots (.) except .xml extension
- [ ] ✅ No spaces
- [ ] ✅ No special characters
- [ ] ✅ No Turkish/accented characters (ç, ğ, ı, ş, ü, ö, etc.)

## Common Patterns

### Pattern 1: Button with Icon

```xml
<component>
    <api>
        <prop name="label" type="string" default="Button"/>
        <prop name="icon" type="image" default=""/>
    </api>

    <view extends="lv_button">
        <row style_flex_cross_place="center" style_pad_all="8">
            <lv_image src="$icon" />
            <lv_label text="$label" style_pad_left="8" />
        </row>
    </view>
</component>
```

### Pattern 2: Card Container

```xml
<component>
    <styles>
        <style name="card_style"
               bg_color="0xffffff"
               radius="12"
               pad_all="16"
               border_width="1"
               border_color="0xe0e0e0" />
    </styles>

    <view flex_flow="column">
        <style name="card_style" />
        <!-- Card content goes here -->
    </view>
</component>
```

### Pattern 3: Data-Bound Value Display

```xml
<!-- In globals.xml -->
<subjects>
    <int name="temperature" value="25" />
</subjects>

<!-- In component -->
<view>
    <lv_label text="Temperature:" />
    <lv_label bind_text="temperature" style_text_font="roboto_bold_20" />
    <lv_label text="°C" />
</view>
```

### Pattern 4: Theme-Aware Component

```xml
<styles>
    <style name="light_bg" bg_color="0xffffff" text_color="0x000000" />
    <style name="dark_bg" bg_color="0x1e1e1e" text_color="0xffffff" />
</styles>

<view>
    <!-- Apply light theme when dark_theme = 0 -->
    <bind_style subject="dark_theme" name="light_bg" ref_value="0" />
    
    <!-- Apply dark theme when dark_theme = 1 -->
    <bind_style subject="dark_theme" name="dark_bg" ref_value="1" />
    
    <!-- Content -->
</view>
```

## Project Setup

### Minimal Project Structure

```
my_project/
├── project.xml           # Display configuration
├── globals.xml          # Global resources
├── components/          # Reusable components
│   └── my_button/
│       └── my_button.xml
├── screens/             # App screens
│   └── screen_main.xml
├── images/              # Image assets
└── fonts/               # Font files
```

### project.xml Template

```xml
<project>
    <targets>
        <target name="default">
            <display width="320" height="240" />
        </target>
    </targets>
</project>
```

### globals.xml Template

```xml
<globals name="my_app">
    <!-- Constants -->
    <consts>
        <int name="unit_sm" value="8" />
        <int name="unit_md" value="16" />
        <color name="primary" value="0x2196F3" />
        <color name="bg_light" value="0xffffff" />
        <color name="bg_dark" value="0x1e1e1e" />
    </consts>

    <!-- Data bindings -->
    <subjects>
        <int name="dark_theme" value="0" help="0:light 1:dark" />
    </subjects>

    <!-- Global styles -->
    <styles>
        <style name="disabled" opa_layered="50%" />
    </styles>

    <!-- Images -->
    <images>
        <!-- <file name="icon_home" src_path="images/home.png" /> -->
    </images>

    <!-- Fonts -->
    <fonts>
        <tiny_ttf name="roboto_bold_20" 
                  size="20" 
                  as_file="true" 
                  src_path="fonts/Roboto-Bold.ttf" />
    </fonts>
</globals>
```

## Testing Your Component

1. **Open LVGL Editor**
2. **Load your project** (open `project.xml`)
3. **Navigate to your component** in the file tree
4. **Check the preview** pane
5. **Test interactions** (click, hover, etc.)

## Troubleshooting

### Issue: Component not appearing
**Solution:**
- Verify XML syntax is correct
- Check file/folder naming (no hyphens!)
- Ensure component file is in correct location

### Issue: "Wrong Name" error
**Solution:**
- File or folder name has invalid characters
- Use only letters, numbers, underscores
- Change `my-component.xml` → `my_component.xml`

### Issue: Style not working
**Solution:**
- Check style is defined before use
- Verify style name spelling
- Check if selector is needed (pressed, focused, etc.)

### Issue: Data binding not updating
**Solution:**
- Verify subject exists in globals.xml
- Check binding syntax: `bind_value="subject_name"`
- Ensure subject type matches (int, string, etc.)

## Next Steps

1. ✅ Create your first component
2. ✅ Create a screen using the component
3. ✅ Add data bindings
4. ✅ Implement theme switching
5. ✅ Add animations
6. ✅ Create more complex components

## Resources

- **Full Documentation:** `/workspace/LVGL_XML_DOCUMENTATION.md`
- **Turkish Solution:** `/workspace/COZUM_NAMING_HATASI.md`
- **Examples:** `/workspace/examples/`
- **Tutorials:** `/workspace/tutorials/`
- **Official Docs:** https://docs.lvgl.io/master/details/xml/index.html

---

**Remember:** Always use valid C variable names for files and folders! 🎯
