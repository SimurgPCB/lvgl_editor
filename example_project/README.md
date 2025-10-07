# Example LVGL XML Project

This is a simple example project demonstrating correct LVGL XML structure and naming conventions.

## ✅ Correct Naming Examples

All files and folders in this project follow C variable naming rules:

### Project Structure
```
example_project/
├── project.xml                              ✅ Valid name
├── globals.xml                              ✅ Valid name
├── components/
│   └── simple_button/                       ✅ Valid folder name (underscore)
│       └── simple_button.xml                ✅ Valid file name (matches folder)
├── screens/
│   └── screen_main.xml                      ✅ Valid screen name
├── images/                                  ✅ Valid folder name
├── fonts/                                   ✅ Valid folder name
└── README.md
```

### Why These Names Work

- `simple_button` ✅ - Uses underscore to separate words
- `screen_main` ✅ - Uses underscore, starts with letter
- `project.xml` ✅ - Simple word with .xml extension

### ❌ Names That Would Fail

```
components/
  simple-button/        ❌ Hyphen not allowed
  simple.button/        ❌ Dot not allowed
  1_button/             ❌ Can't start with number
  simple button/        ❌ Space not allowed
```

## Project Features

### 1. Custom Button Component
- Reusable button with customizable label and color
- Three states: normal, pressed, disabled
- API properties for easy configuration

### 2. Main Screen
- Displays status text
- Shows counter value
- Uses custom button components
- Theme switching capability

### 3. Global Resources
- Spacing units (unit_xs, unit_sm, unit_md, etc.)
- Color palette (primary, secondary, danger)
- Theme subject for light/dark mode
- Data subjects (counter, status_text)

### 4. Data Binding
- Status text bound to subject
- Counter bound to subject
- Theme switching with style bindings

## How to Use

### 1. Open in LVGL Editor
1. Download LVGL Pro Editor from https://pro.lvgl.io
2. Open this project folder
3. Navigate to `screens/screen_main.xml` to see the main screen

### 2. Preview the UI
- The preview pane shows the UI in real-time
- Toggle the dark theme switch to see theme changes
- Click buttons to see pressed states

### 3. Modify and Extend
- Edit `components/simple_button/simple_button.xml` to customize the button
- Edit `screens/screen_main.xml` to change the layout
- Add new components in the `components/` folder
- Add new screens in the `screens/` folder

## Key Concepts Demonstrated

### 1. Component Creation
```xml
<component>
    <api>
        <prop name="label" type="string" default="Button"/>
    </api>
    <view extends="lv_button">
        <lv_label text="$label" />
    </view>
</component>
```

### 2. Screen Creation
```xml
<screen>
    <view flex_flow="column">
        <lv_label text="Hello" />
        <simple_button label="Click Me" />
    </view>
</screen>
```

### 3. Using Constants
```xml
<!-- Define in globals.xml -->
<consts>
    <int name="unit_md" value="12" />
</consts>

<!-- Use with # prefix -->
<view style_pad_all="#unit_md" />
```

### 4. Data Binding
```xml
<!-- Define subject in globals.xml -->
<subjects>
    <string name="status_text" value="Ready" />
</subjects>

<!-- Bind to widget -->
<lv_label bind_text="status_text" />
```

### 5. Theme Switching
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

## Common Patterns Used

### Pattern 1: Reusable Button
The `simple_button` component demonstrates:
- Custom API properties
- Style states (normal, pressed, disabled)
- Removing default LVGL styles
- Using component properties with $ prefix

### Pattern 2: Flexible Layout
The main screen demonstrates:
- Column layout with flex
- Using spacing constants
- Responsive spacing

### Pattern 3: Data-Driven UI
The screen shows:
- Text binding to subjects
- Switch binding to subjects
- Real-time UI updates

## Next Steps

1. **Add More Components**
   - Create `components/card/card.xml`
   - Create `components/input_field/input_field.xml`

2. **Add More Screens**
   - Create `screens/screen_settings.xml`
   - Create `screens/screen_about.xml`

3. **Add Assets**
   - Add images to `images/` folder
   - Add fonts to `fonts/` folder
   - Reference in `globals.xml`

4. **Add Animations**
   - Add timeline animations to components
   - Create screen transitions

## Troubleshooting

### Issue: Component not showing
- Verify component file name matches folder name
- Check XML syntax is correct
- Ensure preview is defined

### Issue: Binding not working
- Check subject is defined in globals.xml
- Verify subject name spelling
- Ensure subject type matches (int, string, etc.)

### Issue: Style not applied
- Verify style is defined before use
- Check style name spelling
- Ensure constants are defined in globals.xml

## Resources

- **Documentation Index**: `/workspace/DOCUMENTATION_INDEX.md`
- **Full Documentation**: `/workspace/LVGL_XML_DOCUMENTATION.md`
- **API Reference**: `/workspace/API_REFERENCE.md`
- **Cheat Sheet**: `/workspace/CHEAT_SHEET.md`
- **Turkish Guide**: `/workspace/COZUM_NAMING_HATASI.md`
- **Official Docs**: https://docs.lvgl.io/master/details/xml/index.html

## License

This example project is provided for educational purposes.
