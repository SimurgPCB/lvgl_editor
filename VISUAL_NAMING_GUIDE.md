# 🎨 LVGL XML Visual Naming Guide

## The "Wrong Name" Error - Visual Explanation

### What You See in LVGL Editor

```
┌─────────────────────────────────────────┐
│              ⚠️ Error                   │
├─────────────────────────────────────────┤
│                                         │
│    ❌ Wrong Name                        │
│                                         │
│    File/Directory must be a valid       │
│    variable name.                       │
│                                         │
│              [ Tamam ]                  │
└─────────────────────────────────────────┘
```

---

## Why This Happens

### LVGL XML → C Code Generation

Your XML files are converted to C code:

```
Your File Name              Generated C Code
─────────────────────────────────────────────────────
test_deneme.xml       →     lv_obj_t* test_deneme_create()
                            #define TEST_DENEME_GEN_H
                            test_deneme_obj

test-deneme.xml       →     lv_obj_t* test-deneme_create() ❌ ERROR!
                            #define TEST-DENEME-GEN-H      ❌ ERROR!
                            test-deneme_obj                ❌ ERROR!
```

**C language DOES NOT allow hyphens in identifiers!**

---

## Visual File Naming Rules

### ✅ VALID Characters

```
┌─────────────────────────────────────────────┐
│  ALLOWED CHARACTERS                         │
├─────────────────────────────────────────────┤
│                                             │
│  Letters:     a b c ... x y z               │
│               A B C ... X Y Z               │
│                                             │
│  Numbers:     0 1 2 3 4 5 6 7 8 9          │
│               (NOT at start!)               │
│                                             │
│  Underscore:  _                             │
│                                             │
└─────────────────────────────────────────────┘
```

### ❌ INVALID Characters

```
┌─────────────────────────────────────────────┐
│  NOT ALLOWED                                │
├─────────────────────────────────────────────┤
│                                             │
│  Hyphen:      -                     ❌      │
│  Dot:         .                     ❌      │
│               (except .xml extension)       │
│  Space:       [ ]                   ❌      │
│  Special:     @ # $ % ^ & * ( )    ❌      │
│  Turkish:     ç ğ ı ş ü ö          ❌      │
│                                             │
└─────────────────────────────────────────────┘
```

---

## Common Naming Mistakes → Fixes

### Example 1: Hyphen Problem

```diff
❌ BAD:
   components/
   └── test-deneme/
       └── test-deneme.xml

✅ GOOD:
   components/
   └── test_deneme/
       └── test_deneme.xml
```

**Fix:** Replace `-` with `_`

---

### Example 2: Dot Problem

```diff
❌ BAD:
   screens/
   └── screen.ana.xml

✅ GOOD:
   screens/
   └── screen_ana.xml
```

**Fix:** Replace `.` with `_` (except .xml extension)

---

### Example 3: Space Problem

```diff
❌ BAD:
   components/
   └── my component/
       └── my component.xml

✅ GOOD:
   components/
   └── my_component/
       └── my_component.xml
```

**Fix:** Replace ` ` (space) with `_`

---

### Example 4: Starting with Number

```diff
❌ BAD:
   components/
   └── 1_button/
       └── 1_button.xml

✅ GOOD:
   components/
   └── button_1/
       └── button_1.xml
```

**Fix:** Move number to end or middle

---

### Example 5: Turkish Characters

```diff
❌ BAD:
   screens/
   └── kullanıcı_profil.xml

✅ GOOD:
   screens/
   └── kullanici_profil.xml
```

**Fix:** Replace `ı` → `i`, `ü` → `u`, etc.

---

## Valid Name Patterns

### Pattern: Snake Case (Recommended)

```
✅ button_primary.xml
✅ user_profile_card.xml
✅ screen_settings.xml
✅ theme_switcher.xml
✅ nav_bar_item.xml
```

**Rule:** All lowercase, words separated by underscores

---

### Pattern: Camel Case (Works but less common)

```
✅ buttonPrimary.xml
✅ userProfileCard.xml
✅ screenSettings.xml
✅ themeSwitcher.xml
```

**Rule:** First word lowercase, capitalize subsequent words

---

### Pattern: Pascal Case (Works but less common)

```
✅ ButtonPrimary.xml
✅ UserProfileCard.xml
✅ ScreenSettings.xml
✅ ThemeSwitcher.xml
```

**Rule:** Capitalize first letter of each word

---

## Complete Project Example

### ✅ CORRECT Structure

```
my_project/                          ✅ Valid (underscore)
├── project.xml                      ✅ Valid (simple word)
├── globals.xml                      ✅ Valid (simple word)
│
├── components/                      ✅ Valid (simple word)
│   ├── simple_button/               ✅ Valid (underscore)
│   │   └── simple_button.xml        ✅ Valid (matches folder)
│   ├── user_card/                   ✅ Valid (underscore)
│   │   └── user_card.xml            ✅ Valid (matches folder)
│   └── nav_bar/                     ✅ Valid (underscore)
│       └── nav_bar.xml              ✅ Valid (matches folder)
│
├── screens/                         ✅ Valid (simple word)
│   ├── screen_main.xml              ✅ Valid (underscore)
│   ├── screen_settings.xml          ✅ Valid (underscore)
│   └── screen_profile.xml           ✅ Valid (underscore)
│
├── images/                          ✅ Valid (simple word)
│   ├── icon_home.png                ✅ Valid (underscore)
│   └── user_avatar.png              ✅ Valid (underscore)
│
└── fonts/                           ✅ Valid (simple word)
    └── roboto_regular.ttf           ✅ Valid (underscore)
```

---

### ❌ INCORRECT Structure (Will Cause Errors)

```
my-project/                          ❌ Hyphen!
├── project.xml
├── globals.xml
│
├── components/
│   ├── simple-button/               ❌ Hyphen!
│   │   └── simple-button.xml        ❌ Hyphen!
│   ├── user.card/                   ❌ Dot!
│   │   └── user.card.xml            ❌ Dot!
│   └── nav bar/                     ❌ Space!
│       └── nav bar.xml              ❌ Space!
│
├── screens/
│   ├── screen.main.xml              ❌ Dot!
│   ├── ayarlar-ekranı.xml          ❌ Hyphen + Turkish chars!
│   └── 1-profile.xml                ❌ Starts with number + hyphen!
│
└── images/
    └── icon-home.png                ❌ Hyphen!
```

---

## Real-World Examples

### Mobile App Example

```
mobile_app/
├── project.xml
├── globals.xml
├── components/
│   ├── bottom_tab_bar/
│   │   └── bottom_tab_bar.xml       ✅
│   ├── list_item/
│   │   └── list_item.xml            ✅
│   └── status_badge/
│       └── status_badge.xml         ✅
└── screens/
    ├── screen_home.xml              ✅
    ├── screen_messages.xml          ✅
    └── screen_profile.xml           ✅
```

---

### Smart Home UI Example

```
smart_home/
├── project.xml
├── globals.xml
├── components/
│   ├── device_card/
│   │   └── device_card.xml          ✅
│   ├── room_selector/
│   │   └── room_selector.xml        ✅
│   └── temperature_control/
│       └── temperature_control.xml  ✅
└── screens/
    ├── screen_dashboard.xml         ✅
    ├── screen_rooms.xml             ✅
    └── screen_devices.xml           ✅
```

---

### E-commerce App Example

```
shop_app/
├── project.xml
├── globals.xml
├── components/
│   ├── product_card/
│   │   └── product_card.xml         ✅
│   ├── cart_item/
│   │   └── cart_item.xml            ✅
│   └── category_button/
│       └── category_button.xml      ✅
└── screens/
    ├── screen_catalog.xml           ✅
    ├── screen_cart.xml              ✅
    └── screen_checkout.xml          ✅
```

---

## Turkish Users - Common Conversions

### Turkish Character Replacements

```
Turkish → English
─────────────────
ç       → c
ğ       → g
ı       → i
İ       → I
ö       → o
ş       → s
ü       → u
Ç       → C
Ğ       → G
Ö       → O
Ş       → S
Ü       → U
```

### Turkish Word Examples

```
❌ Turkish              ✅ English/Converted
──────────────────────────────────────────
kullanıcı.xml    →     kullanici.xml
ayarlar-ekranı.xml →   ayarlar_ekrani.xml
başlangıç.xml    →     baslangic.xml
düğme.xml        →     dugme.xml
menü-öğesi.xml   →     menu_ogesi.xml
```

---

## Quick Decision Tree

```
Is your filename valid?

Start here:
│
├─ Contains hyphen (-)? 
│  └─ ❌ NO → Replace with underscore (_)
│
├─ Contains dot (.) other than .xml?
│  └─ ❌ NO → Replace with underscore (_)
│
├─ Contains space ( )?
│  └─ ❌ NO → Replace with underscore (_)
│
├─ Starts with number?
│  └─ ❌ NO → Move number to end
│
├─ Contains Turkish/special chars?
│  └─ ❌ NO → Replace with English equivalents
│
└─ Only letters, numbers, underscores?
   └─ ✅ YES → Valid name!
```

---

## Testing Your Names

### Mental Check

Before creating a file/folder, ask:

1. ❓ Can I use this as a C variable name?
2. ❓ Does it contain only letters, numbers, underscores?
3. ❓ Does it start with a letter or underscore?

If **YES** to all → ✅ Valid name
If **NO** to any → ❌ Fix it first

---

### Examples to Test Yourself

**Try to identify which names are valid:**

```
1. my_button.xml           → ?
2. my-button.xml           → ?
3. button1.xml             → ?
4. 1button.xml             → ?
5. my.button.xml           → ?
6. MyButton.xml            → ?
7. my button.xml           → ?
8. _private_btn.xml        → ?
9. btn-primary-v2.xml      → ?
10. btn_primary_v2.xml     → ?
```

**Answers:**
```
1. ✅ Valid (underscore)
2. ❌ Invalid (hyphen)
3. ✅ Valid (letter then number)
4. ❌ Invalid (starts with number)
5. ❌ Invalid (dot in name)
6. ✅ Valid (PascalCase)
7. ❌ Invalid (space)
8. ✅ Valid (starts with underscore)
9. ❌ Invalid (hyphens)
10. ✅ Valid (underscores)
```

---

## Summary Checklist

Before creating any XML file or folder:

### ✅ DO:
- [ ] Use letters (a-z, A-Z)
- [ ] Use numbers (0-9) after first character
- [ ] Use underscores (_) to separate words
- [ ] Start with letter or underscore
- [ ] Use descriptive names
- [ ] Be consistent (choose one style)

### ❌ DON'T:
- [ ] Use hyphens (-)
- [ ] Use dots (.) except .xml extension
- [ ] Use spaces
- [ ] Start with numbers
- [ ] Use Turkish/accented characters
- [ ] Use special characters (@#$%^&*)

---

## Quick Reference Card

```
┌────────────────────────────────────────────┐
│  LVGL XML NAMING QUICK REFERENCE           │
├────────────────────────────────────────────┤
│                                            │
│  RULE: C Variable Naming Only              │
│                                            │
│  ✅ Allowed:                               │
│     • Letters: a-z, A-Z                    │
│     • Numbers: 0-9 (not first)             │
│     • Underscore: _                        │
│                                            │
│  ❌ Not Allowed:                           │
│     • Hyphen: -                            │
│     • Dot: . (except .xml)                 │
│     • Space:                               │
│     • Special: @#$%^&*()                   │
│     • Turkish: çğışüö                      │
│                                            │
│  Examples:                                 │
│     ✅ test_deneme.xml                     │
│     ✅ my_button_v2.xml                    │
│     ✅ screen_main.xml                     │
│     ❌ test-deneme.xml                     │
│     ❌ my.button.xml                       │
│     ❌ ekran ana.xml                       │
│                                            │
└────────────────────────────────────────────┘
```

---

## Your Specific Case

### What You Tried
```
test_deneme.xml  or  test-deneme.xml
```

### The Verdict

```
test_deneme.xml    ✅ CORRECT!
                      (underscore is valid)

test-deneme.xml    ❌ WRONG!
                      (hyphen is not valid)
```

### If Error Persists

1. Check folder name matches file name
2. Ensure no hidden characters
3. Verify XML syntax is correct
4. Try renaming both folder and file

---

**🎉 You're now an expert in LVGL XML naming!**

For more help, see:
- [Turkish Guide](COZUM_NAMING_HATASI.md)
- [Documentation Index](DOCUMENTATION_INDEX.md)
- [Cheat Sheet](CHEAT_SHEET.md)
