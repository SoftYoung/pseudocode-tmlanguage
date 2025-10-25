# Pseudocode TextMate Grammar (pseudocode.tmLanguage.json)

This workspace contains a small VS Code language contribution that provides a TextMate grammar for `.pseudo` files. It enables syntax highlighting for:

- control keywords (if, then, else, while, for, procedure, return, ...)
- general keywords (set, display, read, prompt, call, ...)
- comments (`#` and `//`)
- strings, numbers, operators, arrays and function names

This README explains how to test the grammar locally, how to package and install the extension, and quick troubleshooting steps.

## Quick test (recommended)

1. Open the folder `syntaxes-pseudocode-tmlanguage-json` in VS Code (this folder contains `package.json`).
2. Press `F5` to start an Extension Development Host. A new VS Code window opens with the extension loaded.
3. In the Extension Development Host window open one of your `.pseudo` files (for example `../exercises/test.pseudo`).
4. If highlighting doesn't show, open the Command Palette and run `Developer: Inspect Editor Tokens and Scopes` and click a token to see the TextMate scope produced by the grammar.

If you see scopes like `source.pseudocode` and `keyword.control.pseudocode` the grammar loaded successfully.

## Manual association (workspace)

If you prefer not to load an Extension Development Host, you can force VS Code to use the grammar in this workspace by adding a workspace setting. This repository already includes `.vscode/settings.json` with:

```json
{
  "files.associations": {
    "*.pseudo": "pseudocode"
  }
}
```

After adding that, reload VS Code and open a `.pseudo` file. Then use `Developer: Inspect Editor Tokens and Scopes` to verify scopes.

## Package and install (.vsix)

To package the extension and install it into your regular VS Code (outside Extension Dev Host):

1. Install vsce (if you don't have it):

```bash
npm install -g vsce
```

2. From inside `syntaxes-pseudocode-tmlanguage-json` run:

```bash
vsce package
```

This creates a file like `syntaxes-pseudocode-tmlanguage-json-0.0.1.vsix`.

3. Install the .vsix into VS Code:

```bash
code --install-extension syntaxes-pseudocode-tmlanguage-json-0.0.1.vsix
```

4. Restart VS Code and open a `.pseudo` file.

If `vsce package` fails, ensure `node`/`npm` are installed and you're running the command from the folder that contains `package.json`.

## Diagnostic steps if you still don't see colors

1. Confirm `package.json` in this folder contains a `contributes.grammars` entry pointing to `./syntaxes/pseudocode.tmLanguage.json` and a `contributes.languages` entry with `extensions: [".pseudo"]`.
2. Open `syntaxes/pseudocode.tmLanguage.json` and verify `"scopeName": "source.pseudocode"`.
3. Use `Developer: Inspect Editor Tokens and Scopes` — if the grammar produced scopes but your theme doesn't color them, add a quick rule to your user settings:

```json
"editor.tokenColorCustomizations": {
	"textMateRules": [
		{
			"scope": "keyword.control.pseudocode",
			"settings": { "foreground": "#FF0000", "fontStyle": "bold" }
		},
		{
			"scope": "string.quoted.double.pseudocode",
			"settings": { "foreground": "#008000" }
		}
	]
}
```

If that immediately shows colors, the grammar is emitting scopes correctly and you need to add theme rules for those scopes or keep the token customizations.

## Editing the grammar

- The grammar file is `syntaxes/pseudocode.tmLanguage.json`.
- Use the `patterns` and `repository` sections to add or tweak tokens (keywords, operators, strings, comments, etc.).
- After editing the grammar, reload the Extension Development Host (close and relaunch) to pick up changes.

## Contributing

If you want improvements (additional keywords, better array/matrix handling, parameter highlighting, etc.), edit `syntaxes/pseudocode.tmLanguage.json` and test via F5.

## License

This project follows the repository license (see top-level `LICENSE`).

---

If you'd like, I can:

- run `vsce package` here and show the errors encountered (I saw the command exit 1 in your terminal); or
- add a small sample screenshot or example pseudo file with colored tokens.

Which do you want me to do next?
