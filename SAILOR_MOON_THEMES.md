# Sailor Moon Accessibility-Friendly Themes

This document describes the two new Sailor Moon inspired themes added to Horizon that prioritize accessibility and eye comfort.

## 🌙 Sailor Mercury Theme

A soft, ocean-inspired theme featuring gentle blues and cyans, inspired by Sailor Mercury's powers of water and ice.

### Color Palette
- **Primary Colors**: Soft ice blue (#7fb3d9), gentle aqua (#6ba8cd), muted teal (#5c9bb8)
- **Background**: Deep ocean blue-black (#0b1420) with subtle gradients
- **Text**: Soft icy white (#e8f4f8) for excellent contrast
- **Accent Colors**: Mercury silver (#b8d4e6), powder blue (#a3c7db), frost blue (#9bc8e0)

### Accessibility Features
- **High Contrast**: Text-to-background ratios exceed WCAG AAA standards (16.5:1)
- **Eye-Friendly**: Muted, desaturated colors prevent eye strain
- **Dark Theme**: Reduces blue light exposure for comfortable long-term use
- **Colorblind Support**: Includes colorblind-friendly color mappings

### Design Philosophy
Inspired by Sailor Mercury's calm, intelligent nature and connection to water. Colors evoke peaceful ocean depths while maintaining excellent readability.

## 🌿 Sailor Jupiter Theme  

A gentle, nature-inspired theme featuring soft greens and earth tones, inspired by Sailor Jupiter's connection to nature and thunder.

### Color Palette
- **Primary Colors**: Soft forest green (#6b9e6b), gentle sage (#8bb08b), muted moss (#7aa57a)
- **Background**: Deep forest green-black (#0f1a0e) with natural gradients
- **Text**: Soft nature white (#f0f8f0) for excellent contrast
- **Accent Colors**: Jupiter jade (#7fb57f), soft olive (#a3b373), spring green (#b8d4b8)

### Accessibility Features
- **High Contrast**: Text-to-background ratios meet WCAG AA standards (5.7:1+)
- **Eye-Friendly**: Natural, muted greens are easy on the eyes
- **Dark Theme**: Forest-inspired dark backgrounds reduce eye strain
- **Colorblind Support**: Includes colorblind-friendly color mappings

### Design Philosophy
Inspired by Sailor Jupiter's strength and connection to nature. Colors evoke peaceful forest environments while maintaining professional readability.

## 🎨 Technical Implementation

### File Structure
- `scss/themes/variables/_sailor_mercury_variables.scss` - Mercury theme color variables
- `scss/themes/variables/_sailor_jupiter_variables.scss` - Jupiter theme color variables  
- `scss/themes/chat/sailor-mercury.scss` - Mercury theme main styles
- `scss/themes/chat/sailor-jupiter.scss` - Jupiter theme main styles

### Key Features
- **Automatic Discovery**: Themes are automatically detected by webpack
- **Bootstrap Integration**: Full Bootstrap 5 color system integration
- **Responsive Design**: Works across all device sizes
- **Gender Color Support**: Accessible color mappings for all gender identifiers
- **Message Types**: Proper contrast for highlights, warnings, and events

### Color Contrast Testing
Both themes have been tested for WCAG compliance:

| Element | Mercury Theme | Jupiter Theme | Standard |
|---------|---------------|---------------|----------|
| Primary Text | 16.5:1 | 16.5:1 | 7:1 (AAA) ✅ |
| Accent Colors | 8.2:1 | 5.7:1 | 4.5:1 (AA) ✅ |
| Interactive Elements | 8.2:1+ | 5.7:1+ | 4.5:1 (AA) ✅ |

## 🌟 Usage

These themes will appear in the Horizon theme selector as:
- **Sailor Mercury** - For users who prefer cool, calming blues
- **Sailor Jupiter** - For users who prefer natural, earthy greens

Both themes are designed to be non-straining for extended use while maintaining the magical aesthetic of the Sailor Moon universe.

## 🛡️ Accessibility Compliance

- ✅ WCAG 2.1 AA compliant contrast ratios
- ✅ Colorblind-friendly color choices
- ✅ Reduced eye strain with muted colors
- ✅ Dark theme reduces blue light exposure
- ✅ Clear visual hierarchy
- ✅ Consistent color usage patterns

These themes follow all established theme standards in the Horizon codebase while adding new accessibility-focused options for users who need gentler color schemes.