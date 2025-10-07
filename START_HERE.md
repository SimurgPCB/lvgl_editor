# 🚀 LVGL XML Complete Documentation

## ⚠️ DO YOU HAVE "Wrong Name" ERROR?

### 👉 [READ THIS FIRST (Turkish/Türkçe)](COZUM_NAMING_HATASI.md) 👈

If you're seeing this error in LVGL Editor:
```
Error: Wrong Name
File/Directory must be a valid variable name
```

**Quick Fix:**
- ❌ DON'T use: `test-deneme.xml` (hyphen)
- ✅ DO use: `test_deneme.xml` (underscore)

**Why?** File names must follow C variable naming rules (only letters, numbers, underscores).

---

## 📚 Complete Documentation Suite

I've created comprehensive documentation for all LVGL XML APIs, functions, and components with examples and usage instructions.

### 🎯 Quick Navigation

| I want to... | Go to... |
|--------------|----------|
| **Fix "Wrong Name" error** | [COZUM_NAMING_HATASI.md](COZUM_NAMING_HATASI.md) (Turkish) |
| **Get started quickly** | [Quick Start Guide](examples/QUICK_START_GUIDE.md) |
| **Look up syntax quickly** | [Cheat Sheet](CHEAT_SHEET.md) |
| **Learn everything** | [Complete Documentation](LVGL_XML_DOCUMENTATION.md) |
| **Find specific API** | [API Reference](API_REFERENCE.md) |
| **See all resources** | [Documentation Index](DOCUMENTATION_INDEX.md) |
| **See working example** | [Example Project](example_project/) |

---

## 📖 Documentation Files

### 🔴 Critical Documents

#### 1. [COZUM_NAMING_HATASI.md](COZUM_NAMING_HATASI.md) ⭐
**Turkish guide to fix naming errors**
- Complete explanation of "Wrong Name" error
- File and folder naming rules
- Examples of correct/incorrect names
- Step-by-step solutions

#### 2. [DOCUMENTATION_INDEX.md](DOCUMENTATION_INDEX.md)
**Central hub for all documentation**
- Overview of all documents
- Quick solutions to common problems
- Learning path recommendations
- Task-based navigation

### 🟢 Core Documentation

#### 3. [LVGL_XML_DOCUMENTATION.md](LVGL_XML_DOCUMENTATION.md)
**Comprehensive guide covering everything**
- Naming conventions and rules
- Project structure
- Components API
- Screens API
- Global resources
- Data binding
- Styling
- Animations
- Complete examples and usage

#### 4. [API_REFERENCE.md](API_REFERENCE.md)
**Technical reference manual**
- Complete widget reference
- All style properties
- Layout properties
- Events and bindings
- Animation API
- Common patterns

#### 5. [CHEAT_SHEET.md](CHEAT_SHEET.md)
**Quick reference card**
- File naming rules
- Reference syntax
- Common patterns
- Quick troubleshooting
- Property tables

#### 6. [Quick Start Guide](examples/QUICK_START_GUIDE.md)
**Step-by-step beginner tutorial**
- Create first component
- Create first screen
- File naming checklist
- Common patterns
- Testing components

### 🟡 Example Project

#### 7. [Example Project](example_project/)
**Working example with correct naming**
- Complete project structure
- Custom button component
- Main screen with data binding
- Theme switching
- Demonstrates all best practices

---

## 🎯 Your Specific Issue: "Wrong Name" Error

### The Problem
When creating screens or components like `test_deneme.xml`, you get:
```
Error: Wrong Name
File/Directory must be a valid variable name
```

### The Cause
LVGL XML generates C code from your files. File and folder names become:
- C function names: `test_deneme_create()`
- C variable names: `test_deneme_obj`
- C type names: `test_deneme_t`

**C identifiers can ONLY contain:** letters, numbers, underscores

### The Solution

#### ✅ CORRECT Names (Will Work)
```
test_deneme.xml          ← Use underscore
ekran_ana.xml            ← Use underscore
my_button.xml            ← Use underscore
screen_main.xml          ← Use underscore
user_profile_card.xml    ← Use underscores
```

#### ❌ INCORRECT Names (Will Fail)
```
test-deneme.xml          ← Hyphen not allowed!
ekran.ana.xml            ← Dot not allowed!
my component.xml         ← Space not allowed!
1_button.xml             ← Can't start with number!
kullanıcı.xml            ← Turkish characters not allowed!
```

### Quick Fix Guide

1. **Check your file/folder name**
2. **Replace hyphens (-) with underscores (_)**
3. **Replace dots (.) with underscores (_)** (except .xml extension)
4. **Replace spaces with underscores (_)**
5. **Remove Turkish/special characters**
6. **Don't start with numbers**

**👉 [Full explanation in Turkish](COZUM_NAMING_HATASI.md)**

---

## 🚀 Getting Started

### Step 1: Understand Naming Rules
Read: [COZUM_NAMING_HATASI.md](COZUM_NAMING_HATASI.md) (Turkish) or [Cheat Sheet](CHEAT_SHEET.md) (English)

**Key Rule:** File and folder names must be valid C variable names
- ✅ Letters, numbers, underscores only
- ✅ Must start with letter or underscore
- ❌ No hyphens, dots, spaces, special characters

### Step 2: Learn Basic Structure
Read: [Quick Start Guide](examples/QUICK_START_GUIDE.md)

**Project Structure:**
```
my_project/
├── project.xml          # Display config
├── globals.xml          # Global resources
├── components/          # Reusable UI
│   └── my_button/
│       └── my_button.xml
└── screens/            # App screens
    └── screen_main.xml
```

### Step 3: Create Your First Component
Follow: [Quick Start Guide - Component Creation](examples/QUICK_START_GUIDE.md#step-1-create-component-directory)

```xml
<!-- File: components/my_button/my_button.xml -->
<component>
    <api>
        <prop name="text" type="string" default="Button"/>
    </api>
    
    <view extends="lv_button">
        <lv_label text="$text" align="center"/>
    </view>
</component>
```

### Step 4: Create Your First Screen
Follow: [Quick Start Guide - Screen Creation](examples/QUICK_START_GUIDE.md#step-3-create-your-first-screen)

```xml
<!-- File: screens/screen_main.xml -->
<screen>
    <view flex_flow="column" style_pad_all="16">
        <lv_label text="Hello World" />
        <my_button text="Click Me" />
    </view>
</screen>
```

### Step 5: Explore Examples
Check out: [Example Project](example_project/)

---

## 📋 Quick Reference

### Reference Syntax
```xml
<!-- Constants (from globals.xml) -->
#unit_md, #primary_color, #icon_home

<!-- Component properties (from API) -->
$title, $bg_color, $icon

<!-- Data binding (from subjects) -->
bind_value="volume"
bind_text="status"
bind_checked="is_on"
```

### Common Widgets
```xml
<lv_label text="Hello" />
<lv_button><lv_label text="Click" /></lv_button>
<lv_image src="#icon_home" />
<lv_slider bind_value="volume" />
<lv_switch bind_checked="is_on" />
```

### Layout Containers
```xml
<view flex_flow="column">...</view>
<row>...</row>    <!-- flex_flow="row" -->
<column>...</column>    <!-- flex_flow="column" -->
<div scrollable="true">...</div>
```

### Data Binding
```xml
<!-- In globals.xml -->
<subjects>
    <int name="counter" value="0" />
</subjects>

<!-- In component/screen -->
<lv_label bind_text="counter" />
```

### Theme Switching
```xml
<styles>
    <style name="light" bg_color="0xffffff" />
    <style name="dark" bg_color="0x1e1e1e" />
</styles>

<view>
    <bind_style subject="dark_theme" name="light" ref_value="0" />
    <bind_style subject="dark_theme" name="dark" ref_value="1" />
</view>
```

---

## 🛠️ Troubleshooting

### Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| "Wrong Name" error | Invalid file/folder name | Use only letters, numbers, underscores |
| Component not found | Wrong naming/location | Check naming matches folder |
| Style not applied | Style undefined or wrong selector | Define style first, check name |
| Binding not working | Subject not defined | Add to globals.xml |
| Preview not showing | XML syntax error | Validate XML structure |

### Debug Checklist
- [ ] All file/folder names use only letters, numbers, underscores?
- [ ] XML tags properly closed?
- [ ] Constants/properties/subjects defined?
- [ ] Image/font paths correct?
- [ ] Subject types match widget requirements?

---

## 📚 Learning Path

### 🟢 Beginner (Start Here)
1. ✅ Read [Turkish Solution](COZUM_NAMING_HATASI.md) or [Quick Start](examples/QUICK_START_GUIDE.md)
2. ✅ Understand file naming rules
3. ✅ Create first component
4. ✅ Create first screen
5. ✅ Study [Example Project](example_project/)

### 🟡 Intermediate
1. ✅ Read [Complete Documentation](LVGL_XML_DOCUMENTATION.md)
2. ✅ Learn data binding
3. ✅ Implement theme switching
4. ✅ Work with images and fonts
5. ✅ Create complex layouts

### 🔴 Advanced
1. ✅ Master [API Reference](API_REFERENCE.md)
2. ✅ Create complex components
3. ✅ Build animation timelines
4. ✅ Integrate with C code
5. ✅ Optimize performance

---

## 📁 File Organization

### All Documentation Files

```
/workspace/
├── START_HERE.md                    ← You are here
├── DOCUMENTATION_INDEX.md           ← Central hub
├── COZUM_NAMING_HATASI.md          ← Turkish solution (CRITICAL!)
├── LVGL_XML_DOCUMENTATION.md       ← Complete guide
├── API_REFERENCE.md                 ← Technical reference
├── CHEAT_SHEET.md                   ← Quick reference
├── example_project/                 ← Working example
│   ├── project.xml
│   ├── globals.xml
│   ├── components/
│   │   └── simple_button/
│   │       └── simple_button.xml
│   ├── screens/
│   │   └── screen_main.xml
│   └── README.md
├── examples/                        ← Production examples
│   ├── QUICK_START_GUIDE.md        ← Beginner tutorial
│   ├── globals.xml
│   ├── components/
│   └── screens/
└── tutorials/                       ← Step-by-step tutorials
    ├── 1_hello_world/
    ├── 2_new_component/
    └── ...
```

---

## 🔗 External Resources

- **Official LVGL XML Docs:** https://docs.lvgl.io/master/details/xml/index.html
- **LVGL Pro Editor:** https://pro.lvgl.io
- **Online Viewer:** https://viewer.lvgl.io
- **LVGL Main Site:** https://lvgl.io
- **LVGL GitHub:** https://github.com/lvgl/lvgl

---

## ✨ What You'll Learn

From this documentation suite, you'll learn:

1. ✅ **How to fix "Wrong Name" errors** (file naming rules)
2. ✅ **How to create reusable components** (component API)
3. ✅ **How to build screens** (screen structure)
4. ✅ **How to manage global resources** (globals.xml)
5. ✅ **How to use data binding** (subjects and bindings)
6. ✅ **How to apply styles** (style system)
7. ✅ **How to create animations** (timeline animations)
8. ✅ **How to implement themes** (style bindings)
9. ✅ **How to organize projects** (best practices)
10. ✅ **How to troubleshoot issues** (debugging guide)

---

## 🎯 Summary

### Your Issue: "Wrong Name" Error ✅ SOLVED

**Problem:** `test_deneme.xml` causes "File/Directory must be a valid variable name" error

**Solution:** File names must follow C variable naming rules

**Valid Names:**
```
✅ test_deneme.xml       (underscore)
✅ ekran_ana.xml         (underscore)
✅ my_component.xml      (underscore)
```

**Invalid Names:**
```
❌ test-deneme.xml       (hyphen)
❌ ekran.ana.xml         (dot)
❌ my component.xml      (space)
```

### 📖 Full Documentation Available

This comprehensive documentation suite includes:

1. **[Turkish Solution Guide](COZUM_NAMING_HATASI.md)** - Fix naming errors (Türkçe)
2. **[Complete Documentation](LVGL_XML_DOCUMENTATION.md)** - Everything you need to know
3. **[API Reference](API_REFERENCE.md)** - Technical reference manual
4. **[Cheat Sheet](CHEAT_SHEET.md)** - Quick lookup reference
5. **[Quick Start Guide](examples/QUICK_START_GUIDE.md)** - Beginner tutorial
6. **[Documentation Index](DOCUMENTATION_INDEX.md)** - Central navigation hub
7. **[Example Project](example_project/)** - Working example with best practices

### 🚀 Next Steps

1. **If you have naming error:** Read [COZUM_NAMING_HATASI.md](COZUM_NAMING_HATASI.md)
2. **If you're new:** Read [Quick Start Guide](examples/QUICK_START_GUIDE.md)
3. **If you want quick reference:** Use [Cheat Sheet](CHEAT_SHEET.md)
4. **If you want to learn everything:** Read [Complete Documentation](LVGL_XML_DOCUMENTATION.md)

---

## 🎉 You're Ready!

You now have complete documentation for LVGL XML including:

- ✅ Solution to your "Wrong Name" error
- ✅ Complete API and function reference
- ✅ Component and screen examples
- ✅ Usage instructions and best practices
- ✅ Quick reference materials
- ✅ Working example project

**Happy Coding! 🚀**

---

**Questions?** Check [Documentation Index](DOCUMENTATION_INDEX.md) or visit https://docs.lvgl.io
