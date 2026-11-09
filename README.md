# Beluga toolbar clipping evidence

Screenshots for the toolbar flex-basis fix.

- `reported-clipping.png`: the original user-provided screenshot showing a clipped Create PR button.
- `before-native-toolbar.png`: a crop of the installed Beluga 3.0.0 native app at a 1394 × 860 point window size, with the meeting-minute-harmony workspace open. The workspace More button is clipped next to Search.

- `after-native-toolbar.png`: the same workspace in a separately identified native verification build, also at 1394 × 860 points, with the same sidebar and inspector widths. The workspace More button and divider are fully visible. The inspector tab and notification count differ between captures.
- `after-narrow-toolbar.png`: the fixed native verification build at 1100 × 860 points. The branch pill folds away and Create PR, its dropdown, More, Search, notifications, and the inspector toggle remain visible.

The local verification build uses release `v3.0.0-mac` plus the exact toolbar CSS patch proposed against upstream `3fb2e4d`. Toolbar.tsx and the original Toolbar.module.css are identical at those two revisions. This keeps the verification frontend compatible with the installed daemon while its existing task runs. Both the branch dropdown and the workspace More menu were opened and visually inspected; no menu action was executed.

Images are cropped to the toolbar to omit unrelated conversations. Screenshot pixels are not repainted or synthesized.
