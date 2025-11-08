# Using `material-icon-theme` in VS Code **with your own customizations**

This guide shows you how to enable **Material Icon Theme** and then **override** specific file/folder icons with your own SVGs or color-cloned variants—without building a custom icon theme from scratch.

---

## 1) Install & activate Material Icon Theme

1. Open **Extensions** (`Ctrl/Cmd + Shift + X`) → search **Material Icon Theme** → **Install**.
2. Set it as your icon theme:

   * Command Palette → **File Icon Theme: Select File Icon Theme** → **Material Icon Theme**, or
   * Add to **Settings (JSON)**:

     ```json
     "workbench.iconTheme": "material-icon-theme"
     ```

---

## 2) Quick overrides via Settings (map names/extensions → built-in icons)

Add these to **Settings (JSON)** (`Preferences: Open Settings (JSON)`):

```json
{
  "workbench.iconTheme": "material-icon-theme",

  // Map specific filenames or globs to existing icon names
  "material-icon-theme.files.associations": {
    "**.env": "tune",
    "config.json": "settings",
    "*.mycfg": "json"
  },

  // Map folder names to existing folder icon names
  "material-icon-theme.folders.associations": {
    "src": "src",
    "scripts": "tools",
    "config": "config"
  }
}
```

**Notes**

* Use `**.ext` to apply broadly; `*.ext` can be overridden by more specific matches.
* Use exact names (e.g., `config.json`) for one-off mappings.

---

## 3) Use **your own SVG icons** (files or folders)

Material Icon Theme can load **custom SVGs** directly from your user extensions area.

### 3.1 Place your SVGs

Create a simple structure like:

```
~/.vscode/extensions/icons/
  wellbeing.svg
  productivity.svg
  folder-fleeting.svg
  folder-fleeting-open.svg   // required for folders (closed + open)
```

> Windows: `%USERPROFILE%\.vscode\extensions\icons\...`
> macOS/Linux: `~/.vscode/extensions/icons/...`

### 3.2 Reference them in Settings

**File icon example** (e.g., `wellbeing.svg` for `wellbeing.md`):

```json
"material-icon-theme.files.associations": {
  "wellbeing.md": "../../icons/wellbeing"
}
```

**Folder icon example** (requires *two* SVGs for closed/open):

```json
"material-icon-theme.folders.associations": {
  "fleeting": "../../../../icons/folder-fleeting"
}
```

**Important path tip:** values are **paths without the `.svg` extension**, and they’re **relative to the theme’s internal dist folder**, so you’ll often see `../../` or `../../../..`. If the icon doesn’t appear, try adjusting the number of `../` segments.

---

## 4) Clone existing icons with new colors (no SVGs needed)

You can **clone** a built-in icon, give it a new name and color, and then associate it with files/folders.

```json
{
  "material-icon-theme.files.customClones": [
    {
      "name": "idea-fast",
      "base": "lightbulb",
      "color": "amber-400",
      "fileNames": ["idea.txt", "idea.md"]
    }
  ],
  "material-icon-theme.folders.customClones": [
    {
      "name": "notes-fleeting",
      "base": "folder",
      "color": "light-blue-400",
      "folderNames": ["fleeting"]
    }
  ]
}
```

After cloning, you can also reference the new names (`idea-fast`, `notes-fleeting`) anywhere you’d use a built-in icon name.

---

## 5) Example: tie it all together

Let’s hook up the two custom SVGs we created earlier and a couple of practical mappings:

```json
{
  "workbench.iconTheme": "material-icon-theme",

  "material-icon-theme.files.associations": {
    "wellbeing.md": "../../icons/wellbeing",      // custom SVG
    "productivity.md": "../../icons/productivity",// custom SVG
    "**.env": "tune",                              // built-in
    "config.json": "settings"                      // built-in
  },

  "material-icon-theme.folders.associations": {
    "fleeting": "../../../../icons/folder-fleeting", // custom closed/open SVGs
    "scripts": "tools"                                // built-in
  },

  "material-icon-theme.files.customClones": [
    {
      "name": "bolt-blue",
      "base": "flash",
      "color": "blue-400",
      "fileNames": ["speed.md"]
    }
  ]
}
```

---

## 6) Troubleshooting checklist

* **Icons didn’t change?**

  * Command Palette → **Developer: Reload Window**.
  * Verify **Material Icon Theme** is the active icon theme.
  * Double-check the **path** (number of `../` segments) and **omit `.svg`** in the value.
  * Confirm the **SVGs are valid** (open them in a browser).
  * For folders, ensure you provided **both** `folder-name.svg` and `folder-name-open.svg`.

* **Which icon names exist?**

  * Search the Material Icon Theme README or browse its `icons` folder to see available names you can map/clone.

* **Want repo-wide overrides only?**

  * Put mappings in **Workspace Settings** (`.vscode/settings.json`) instead of User Settings.

---

## 7) What **not** to do

* There’s **no `extends` support** for file icon themes in VS Code. You can’t inherit another file icon theme and patch a few entries. Use the settings above or copy/modify an existing theme if you need deeper changes.

---

## 8) SVG snippets you can use (optional)

**Wellbeing leaf (gradient)**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" aria-label="wellbeing leaf sprig">
  <title>Wellbeing</title>
  <defs>
    <linearGradient id="g" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#2ecc71"/>
      <stop offset="1" stop-color="#27ae60"/>
    </linearGradient>
  </defs>
  <path d="M11.5 20.5c2-3.5 2.5-7.2 2.2-11.3" fill="none" stroke="#1e8e5a" stroke-width="1.6" stroke-linecap="round"/>
  <path d="M6.2 9.5c2.7-1.4 5.1-1.3 7.3.2-1.6 2.9-4.1 3.7-7.3-.2z" fill="url(#g)"/>
  <path d="M15.5 5.5c2.1-.3 3.9.5 5.3 2.4-2.7 1.8-4.8 1.6-6.6-.7 0 0 .6-1.4 1.3-1.7z" fill="url(#g)"/>
  <path d="M8.8 15.8c1.9-.4 3.4.1 4.6 1.6-2 .9-3.6.6-4.9-.9 0 0 .2-.5.3-.7z" fill="url(#g)"/>
</svg>
```

**Productivity bolt (flat)**

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" aria-label="productivity lightning bolt">
  <title>Productivity</title>
  <path d="M13 2L3 14h7l-1 8 10-12h-7l1-8z" fill="#FFC107"/>
</svg>
```

Place them under `~/.vscode/extensions/icons/` and reference as shown in section **3.2**.

---

## 9) Fast reference (settings keys)

* `workbench.iconTheme`
* `material-icon-theme.files.associations`
* `material-icon-theme.folders.associations`
* `material-icon-theme.files.customClones`
* `material-icon-theme.folders.customClones`
* `material-icon-theme.rootFolders.associations` (optional root folder icon)

---

That’s it! You now have a lean setup to keep Material Icon Theme’s look while sprinkling in **your own** file/folder icons where it matters. If you share your target filenames/folders, I can craft a ready-to-paste `settings.json` tailored to your project.
