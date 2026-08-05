# Search Verticals — Executive Ribbon v2 Button Style

Use this configuration in the **PnP Modern Search Verticals** web part to visually pair the vertical buttons with `ExecutiveRibbon-v2.html`.

> Target: PnP Modern Search v4.22.x  
> Scope: Native Search Verticals styling — no custom SPFx extension required.

## Recommended vertical configuration

Use short labels so the row stays compact. Apply Fluent UI icons where they make the category faster to scan.

| Vertical | Suggested Fluent UI icon | Example value |
|---|---|---|
| All Prompts | Sparkle | `all` |
| Mission | Bullseye | `mission` |
| Writing | Edit | `writing` |
| Analysis | AnalyticsView | `analysis` |
| Briefing | Presentation | `briefing` |
| Reference | Library | `reference` |

Keep your existing vertical names, values, query templates, and result connections if they are already working. The names above are examples, not required replacements.

## Native v2-matched style settings

Open the Search Verticals property pane and expand **Vertical styling**.

| Setting | Value | Purpose |
|---|---:|---|
| Vertical background color | `#F5F9FF` | Matches the pale blue v2 surface |
| Vertical mouse-over color | `#EAF4FF` | Matches v2 metadata-chip hover depth |
| Vertical border color | `#0078D4` | Uses the primary v2 blue |
| Vertical border thickness | `2` px | Gives each vertical a deliberate button frame |
| Vertical font size | `14` px | Compact, readable executive-gallery scale |

For the web part title:

| Setting | Value |
|---|---:|
| Title font | `Segoe UI` |
| Title font size | `16` px |
| Title font color | `#17365D` |

Suggested title: **Explore the Executive AI Playbook**

## Theme colors

The native selected state follows the SharePoint theme. For the closest match to Executive Ribbon v2, use these design tokens when your page/theme configuration allows them:

- Primary / selected: `#0078D4`
- Deep navy: `#17365D`
- Hover surface: `#EAF4FF`
- Cyan accent: `#00A6A6`
- Violet accent: `#6B5BD2`
- Text: `#172033`
- Border: `#CFDAE7`

## Recommended layout behavior

- Keep the verticals directly above the Search Results web part.
- Use icons consistently: either every vertical gets one or none do.
- Keep labels to one or two words when possible.
- Preserve the existing Search Results connection and `{verticals.value}` query logic.
- Let the selected vertical use the strongest blue; inactive verticals should remain pale.
- Avoid hyperlink mode for filter-like verticals. Use it only when a button intentionally navigates elsewhere.
- If a vertical is a hyperlink, choose the opening behavior deliberately so users do not lose the current sorted/filtered gallery.

## Validation checklist

1. Confirm each button changes the connected Search Results web part.
2. Confirm the active vertical is visibly distinct from hover.
3. Verify button labels do not wrap at the normal page width.
4. Test keyboard focus and selection.
5. Test hover and selected contrast on the actual SharePoint section background.
6. Confirm the vertical row remains usable on tablet and narrow widths.
7. Confirm changing verticals resets refiners only where expected.

## Native limitation

The standard Search Verticals web part does **not** use the custom Handlebars layout system available in Search Results. Its native property pane supports background, hover, border, border thickness, font size, title styling, icons, and link behavior.

Therefore, these Executive Ribbon v2 effects are not available through a verticals HTML file alone:

- multicolor gradient button fills
- glowing cyan/violet rails
- custom border radius
- layered shadows
- custom hover lift animation
- per-button color variants

Those effects require a separately developed SPFx extension or a custom verticals web part. The native recipe above intentionally stays supportable and paste-free while matching the v2 palette and visual weight as closely as the standard controls permit.
