# Executive AI Playbook — expandable prompt template

Template: [ExecutiveMultiView-v2-Expandable.html](./ExecutiveMultiView-v2-Expandable.html)

Target: PnP Modern Search v4.22.0. This is a separate candidate version; earlier templates remain available.

## What this version does

- Retains Ribbon, Tiles, Compact, and Spotlight views.
- Adds View full prompt / Hide full prompt to every record using native details/summary.
- Displays selectable full prompt text with line breaks preserved.
- Retains selected filters, results count, sorting, previews, original-record links, and pagination.
- Uses manual copying. A one-click Copy prompt component is not included.

The four visual layouts share a revised card structure. This is not a byte-for-byte CSS patch of v1.

## Configure Prompt Body before installing

1. Identify the source Prompt Body column and its crawled property. Do not assume its internal name or managed-property name.
2. Map the crawled property to a suitable retrievable managed property in the applicable Search Schema. Follow the existing site's schema conventions.
3. Reindex the source list if the mapping changed, and allow the search index to refresh.
4. Add the verified managed property to the Search Results web part's Selected Properties.
5. In layout slot configuration, add a custom slot named **PromptBody** and map it to that managed property.
6. Inspect Search Debug for a known record. Verify that the returned value contains the entire prompt, including its ending, line breaks, and placeholders. A nonempty but truncated result is not sufficient.

The template reads:
```handlebars
{{slot item @root.slots.PromptBody}}
```

If custom slot configuration is unavailable in the installed build, replace every PromptBody slot expression with the verified direct property binding, such as `item.YourVerifiedManagedProperty`, including the corresponding if conditions. The example is a placeholder, not a property to create.

## Existing field bindings

| Data | Binding |
| --- | --- |
| Title | item.Title |
| Original record | item.Path |
| Preview | item.PreviewUrl |
| Short description | item.SCUsageNotes |
| Mission category | item.SCMissionCategory |
| Response format | item.SCResponseFormat |
| Created | item.Created |
| Full prompt | PromptBody slot mapped to a verified managed property |

Path must resolve to the intended original record, normally the list item's display form. Prompt Body and Usage Notes are different fields.

## Install

1. Save the current working web-part template and configuration for rollback.
2. Download the HTML from GitHub's Raw/Download option.
3. Edit the PnP Search Results web part and open its custom results-template editor.
4. Replace the template with the complete HTML, including its supplied data-content wrapper.
5. Alternatively, place the HTML in a SharePoint library readable by the intended audience and configure its external template URL.
6. Save and publish the test page. Validate there before replacing the production gallery.

Do not use the GitHub file-view page URL as the external template URL. Use GitHub to transfer the file rather than emailing HTML attachments.

## Validate in the installed tenant

- Confirm each record has its own title, description, and prompt.
- Expand and collapse records with mouse, touch, and keyboard.
- Verify all four views and their radio controls work after save/publish.
- Copy a long prompt and compare it with the source, especially the last paragraph and placeholders.
- Check records with no prompt: the template should show its unavailable-message and retain Open original.
- Check filters, sorting, previews, paging, long titles, and phone layouts.
- Expect the selected view and expanded records to reset when PnP rerenders results.
- Use only one instance per page: radio names and IDs are shared in this version.

Local checks completed: four expansion blocks and balanced Handlebars block nesting. A full Handlebars parser was unavailable locally. Live PnP rendering, sanitization, keyboard behavior, and actual data mappings have not been verified.

## Plain text and rich text

The full prompt uses escaped double braces and CSS white-space: pre-wrap. This expects plain-text Prompt Body values and preserves literal prompt markup.

If Search returns rich-text HTML, tags may display literally. Do not switch blindly to triple braces: first choose a verified rich-text rendering or HTML-to-plain-text conversion strategy and test copied output. Usage Notes retains the existing rich-text binding from the supplied source.

## Lessons applied from the earlier MEDLOG work

Validate the chain from source column to crawled property, managed property, Selected Properties, and layout binding before debugging CSS. Keep the explicit data.items/item loop scope. Template changes cannot repair a missing Search value. These are implementation principles; this package does not claim a prior MEDLOG clipboard component was found.

## One-click Copy prompt — extension specification

Handlebars renders markup; the PnP template sanitizer blocks inline JavaScript. Adding onclick or a script tag to the template is not a supported clipboard implementation.

Implement a custom PnP web component in an SPFx extensibility library:

1. Register the component through getCustomWebComponents().
2. Pass the complete prompt as an escaped component attribute or another supported property-binding mechanism.
3. Render a real keyboard-accessible Copy prompt button inside the component.
4. Invoke navigator.clipboard.writeText from the user's click and handle rejected requests.
5. Show Copied only after the write succeeds; announce feedback through an aria-live region.
6. If copying fails, retain selectable text and tell the user to copy manually.
7. Copy only the prompt, not the record title, chips, or UI instructions.
8. Verify exact whitespace, quotation marks, Unicode, HTML-like prompt text, multiple records, and large prompts in MED365.
9. Package and deploy the library through the applicable app catalog, then configure the Search Results web part to load it.

This document describes the extension contract; no component has been deployed or connected by this upload.

References:
- [PnP templating and sanitization](https://microsoft-search.github.io/pnp-modern-search/extensibility/templating/)
- [PnP custom web components](https://microsoft-search.github.io/pnp-modern-search/extensibility/custom_web_component/)
