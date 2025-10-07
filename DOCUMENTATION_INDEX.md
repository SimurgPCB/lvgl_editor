# LVGL XML Documentation Index

## 📚 Complete Documentation Suite

This comprehensive documentation covers all aspects of LVGL XML development, with special focus on solving common issues like the "Wrong Name" error.

---

## 🚀 Start Here

### For Beginners
1. **[Turkish Solution Guide](COZUM_NAMING_HATASI.md)** ⭐ START HERE IF YOU HAVE "Wrong Name" ERROR
   - Complete explanation in Turkish
   - Solves the file/folder naming issue
   - Common mistakes and fixes

2. **[Quick Start Guide](examples/QUICK_START_GUIDE.md)**
   - Step-by-step first component
   - First screen creation
   - File naming checklist

3. **[Cheat Sheet](CHEAT_SHEET.md)**
   - Quick reference for syntax
   - Common patterns
   - Troubleshooting tips

### For Advanced Users
4. **[Complete Documentation](LVGL_XML_DOCUMENTATION.md)**
   - In-depth guide to all features
   - Component architecture
   - Data binding and animations
   - Best practices

5. **[API Reference](API_REFERENCE.md)**
   - Complete widget reference
   - All style properties
   - Event and binding API
   - Animation API

---

## 🎯 Quick Solutions

### "Wrong Name" Error? → [Read This](COZUM_NAMING_HATASI.md)

**The Problem:** Your file or folder name contains invalid characters.

**The Solution:** Use only letters, numbers, and underscores.

```
❌ test-deneme.xml     (hyphen not allowed)
✅ test_deneme.xml     (underscore is OK)

❌ ekran.ana.xml       (dots not allowed)  
✅ ekran_ana.xml       (underscore is OK)
```

**Why?** LVGL generates C code from XML. File names become C function/variable names, which can only contain letters, numbers, and underscores.

---

## 📖 Documentation Files

### Core Documentation

| File | Description | Best For |
|------|-------------|----------|
| [COZUM_NAMING_HATASI.md](COZUM_NAMING_HATASI.md) | Turkish guide to fix naming error | Users with "Wrong Name" error |
| [LVGL_XML_DOCUMENTATION.md](LVGL_XML_DOCUMENTATION.md) | Complete comprehensive guide | Learning all features |
| [API_REFERENCE.md](API_REFERENCE.md) | Technical API reference | Looking up specific properties |
| [CHEAT_SHEET.md](CHEAT_SHEET.md) | Quick reference card | Quick lookups while coding |
| [QUICK_START_GUIDE.md](examples/QUICK_START_GUIDE.md) | Beginner tutorial | First-time users |

### Example Projects

| Path | Description |
|------|-------------|
| `/workspace/examples/` | Production examples with components and screens |
| `/workspace/tutorials/1_hello_world/` | Simple hello world tutorial |
| `/workspace/tutorials/2_new_component/` | Component creation tutorial |
| `/workspace/tutorials/3_assets/` | Working with images and fonts |
| `/workspace/tutorials/4_screens/` | Screen management |
| `/workspace/tutorials/5_layouts/` | Layout techniques |
| `/workspace/tutorials/6_data_binding/` | Data binding examples |
| `/workspace/tutorials/7_translations/` | Internationalization |
| `/workspace/tutorials/8_animations/` | Animation examples |

---

## 🔑 Key Concepts

### File Naming Rules (CRITICAL!)

✅ **ALLOWED:**
- Letters: `a-z`, `A-Z`
- Numbers: `0-9` (not at start)
- Underscores: `_`

❌ **NOT ALLOWED:**
- Hyphens: `-`
- Dots: `.` (except `.xml` extension)
- Spaces: ` `
- Special characters: `@`, `#`, `$`, `%`, etc.
- Turkish/accented characters: `ç`, `ğ`, `ı`, `ş`, `ü`, `ö`

### Reference Syntax

```xml
<!-- Constants (from globals) -->
#unit_md, #primary_color, #icon_home

<!-- Component properties (from API) -->
$title, $bg_color, $icon

<!-- Data binding (from subjects) -->
bind_value="volume"
bind_text="status"
bind_checked="is_on"
```

### Project Structure

```
project/
├── project.xml          # Display configuration
├── globals.xml          # Global resources (colors, fonts, subjects)
├── components/          # Reusable UI components
│   └── my_button/
│       └── my_button.xml
├── screens/            # Application screens
│   └── screen_main.xml
├── images/             # Image assets
├── fonts/              # Font files
└── widgets/            # Custom C widgets
```

---

## 📋 Common Tasks

### Task: Create a New Component

1. Create folder: `components/my_component/` ✅ Use underscores!
2. Create file: `components/my_component/my_component.xml`
3. Define component structure:

```xml
<component>
    <previews>
        <preview width="200" height="100" />
    </previews>

    <api>
        <prop name="text" type="string" default="Hello"/>
    </api>

    <view>
        <lv_label text="$text" />
    </view>
</component>
```

**See:** [Quick Start Guide](examples/QUICK_START_GUIDE.md#step-1-create-component-directory)

### Task: Create a New Screen

1. Create file: `screens/screen_myscreen.xml` ✅ Valid name!
2. Define screen structure:

```xml
<screen>
    <view flex_flow="column" style_pad_all="16">
        <lv_label text="My Screen" />
        <!-- Add components here -->
    </view>
</screen>
```

**See:** [Quick Start Guide](examples/QUICK_START_GUIDE.md#step-3-create-your-first-screen)

### Task: Add Data Binding

1. Define subject in `globals.xml`:

```xml
<subjects>
    <int name="counter" value="0" min_value="0" max_value="100" />
</subjects>
```

2. Bind to widget:

```xml
<lv_label bind_text="counter" />
<lv_slider bind_value="counter" />
```

**See:** [Documentation - Data Binding](LVGL_XML_DOCUMENTATION.md#data-binding)

### Task: Add Theme Switching

1. Define theme subject and styles:

```xml
<!-- In globals.xml -->
<subjects>
    <int name="dark_theme" value="0" help="0:light 1:dark" />
</subjects>

<!-- In component/screen -->
<styles>
    <style name="light" bg_color="0xffffff" text_color="0x000000" />
    <style name="dark" bg_color="0x1e1e1e" text_color="0xffffff" />
</styles>

<view>
    <bind_style subject="dark_theme" name="light" ref_value="0" />
    <bind_style subject="dark_theme" name="dark" ref_value="1" />
</view>
```

**See:** [Cheat Sheet - Theme Switching](CHEAT_SHEET.md#theme-switching)

### Task: Add Animation

1. Define timeline:

```xml
<animations>
    <timeline name="fade_in">
        <animation target="self" prop="opa" 
                   start="0" end="255" 
                   duration="200" early_apply="true" />
    </timeline>
</animations>
```

2. Trigger animation:

```xml
<lv_button>
    <play_timeline_event target="card" timeline="fade_in" />
    <lv_label text="Animate" />
</lv_button>
```

**See:** [API Reference - Animation](API_REFERENCE.md#animation-api)

---

## 🛠️ Troubleshooting

### Common Errors

| Error | Cause | Solution | Reference |
|-------|-------|----------|-----------|
| "Wrong Name" | Invalid file/folder name | Use only letters, numbers, underscores | [Turkish Guide](COZUM_NAMING_HATASI.md) |
| Component not found | Wrong file structure | Check naming and location | [Quick Start](examples/QUICK_START_GUIDE.md) |
| Style not applied | Style not defined or wrong name | Define style first, check spelling | [Cheat Sheet](CHEAT_SHEET.md#troubleshooting) |
| Binding not working | Subject not defined | Add to globals.xml | [Documentation](LVGL_XML_DOCUMENTATION.md#data-binding) |
| Preview not showing | XML syntax error | Check XML structure | [API Reference](API_REFERENCE.md) |

### Debug Checklist

- [ ] File/folder names: Only letters, numbers, underscores?
- [ ] XML syntax: All tags properly closed?
- [ ] References: Constants/properties/subjects defined?
- [ ] Paths: Images/fonts exist at specified paths?
- [ ] Bindings: Subject types match widget requirements?

---

## 📚 Learning Path

### Level 1: Beginner
1. Read [Turkish Solution](COZUM_NAMING_HATASI.md) or [Quick Start](examples/QUICK_START_GUIDE.md)
2. Learn file naming rules
3. Create first component
4. Create first screen

### Level 2: Intermediate
1. Study [Complete Documentation](LVGL_XML_DOCUMENTATION.md)
2. Learn data binding
3. Implement theme switching
4. Work with images and fonts

### Level 3: Advanced
1. Master [API Reference](API_REFERENCE.md)
2. Create complex components
3. Build animation timelines
4. Integrate with C code

---

## 🎯 Your Specific Issue

### Problem: "File/Directory must be a valid variable name"

When trying to create `test_deneme.xml` or similar files, you get this error.

### Root Cause
LVGL XML generates C code. File and folder names become:
- C function names
- C variable names
- C type names

C identifiers can only contain: **letters, numbers, underscores**

### Solution

**If you used:** `test-deneme.xml` ❌
```
The hyphen (-) is not allowed in C variable names
```

**Use instead:** `test_deneme.xml` ✅
```
Underscores are valid in C variable names
```

### Examples of Name Corrections

| Your Name (Wrong) | Corrected Name | Issue |
|-------------------|----------------|-------|
| `test-deneme.xml` | `test_deneme.xml` | Hyphen → Underscore |
| `ekran.ana.xml` | `ekran_ana.xml` | Dot → Underscore |
| `my component.xml` | `my_component.xml` | Space → Underscore |
| `1_buton.xml` | `buton_1.xml` | Can't start with number |
| `kullanıcı.xml` | `kullanici.xml` | No Turkish characters |

### Detailed Explanation
👉 See [COZUM_NAMING_HATASI.md](COZUM_NAMING_HATASI.md) for complete Turkish explanation

---

## 🔗 External Resources

- **Official LVGL XML Documentation:** https://docs.lvgl.io/master/details/xml/index.html
- **LVGL Pro Editor:** https://pro.lvgl.io
- **Online Viewer:** https://viewer.lvgl.io
- **LVGL Main Site:** https://lvgl.io
- **LVGL GitHub:** https://github.com/lvgl/lvgl

---

## 📝 Quick Reference Tables

### Widget Summary

| Widget | Purpose | Key Attributes |
|--------|---------|----------------|
| `<view>` | Container | `flex_flow`, `extends` |
| `<row>` | Horizontal layout | Auto `flex_flow="row"` |
| `<column>` | Vertical layout | Auto `flex_flow="column"` |
| `<lv_label>` | Text display | `text`, `bind_text` |
| `<lv_button>` | Clickable button | Events |
| `<lv_image>` | Image display | `src` |
| `<lv_slider>` | Slider control | `bind_value`, `min_value`, `max_value` |
| `<lv_switch>` | Toggle switch | `bind_checked` |
| `<lv_bar>` | Progress bar | `bind_value` |
| `<lv_arc>` | Circular control | `bind_value`, `rotation` |

### Style Property Summary

| Category | Properties |
|----------|-----------|
| Size | `width`, `height`, `min_width`, `max_width` |
| Spacing | `style_pad_all`, `style_pad_hor`, `style_pad_ver` |
| Background | `style_bg_color`, `style_bg_opa`, `style_radius` |
| Border | `style_border_color`, `style_border_width` |
| Text | `style_text_color`, `style_text_font`, `style_text_align` |
| Flex | `flex_flow`, `flex_grow`, `style_flex_main_place` |

### Reference Prefix Summary

| Prefix | Usage | Example |
|--------|-------|---------|
| `#` | Constants, images, fonts | `#unit_md`, `#icon_home` |
| `$` | Component properties | `$title`, `$bg_color` |
| `bind_*` | Data binding | `bind_value="volume"` |

---

## ✅ Success Checklist

Before you start:
- [ ] I understand file naming rules (letters, numbers, underscores only)
- [ ] I know the difference between #, $, and bind_* references
- [ ] I've reviewed the project structure
- [ ] I have the documentation bookmarked

For each component:
- [ ] File/folder names are valid C identifiers
- [ ] Component has a preview
- [ ] API properties are documented with help text
- [ ] Styles are defined before use
- [ ] XML syntax is correct

For each screen:
- [ ] Screen file is in screens/ directory
- [ ] Uses valid naming (e.g., screen_main.xml)
- [ ] References to components are correct
- [ ] Data bindings are properly set up

---

## 🎉 Summary

This documentation suite provides:

1. ✅ **Solution to "Wrong Name" error** ([Turkish](COZUM_NAMING_HATASI.md))
2. ✅ **Complete feature guide** ([Documentation](LVGL_XML_DOCUMENTATION.md))
3. ✅ **Technical reference** ([API Reference](API_REFERENCE.md))
4. ✅ **Quick lookup** ([Cheat Sheet](CHEAT_SHEET.md))
5. ✅ **Getting started** ([Quick Start](examples/QUICK_START_GUIDE.md))
6. ✅ **Real examples** (See `/workspace/examples/` and `/workspace/tutorials/`)

**Start with the Turkish guide if you have the naming error, then proceed to other docs as needed.**

---

**Happy Coding! 🚀**

*For questions or issues, refer to the troubleshooting sections in each document or visit https://docs.lvgl.io*
