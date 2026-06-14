# generate-resume-kami

Generate professional, print-ready HTML resumes using the [Kami](https://github.com/tw93/Kami) design system constraints. Supports both English and Chinese output.

## Thanks

This skill is built on top of the design constraints from **[Kami](https://github.com/tw93/Kami)** by [@tw93](https://github.com/tw93) — an open-source design system for professional documents. Kami provides the typography, color palette, and layout rules that make AI-generated documents look polished and consistent.

Kami is part of tw93's AI tool trilogy: [Kaku](https://github.com/anthropics/claude-code/tree/main/.kaku) (code generation), Kami (documents), and Waza (habit drilling).

## What it does

- Pulls your personal data (work experience, education, skills, certs) from ud tasks
- Generates A4-formatted HTML resumes following Kami's editorial design spec
- Outputs both English and Chinese versions with proper CJK typography
- Print to PDF directly from browser (Cmd+P / Ctrl+P)

## Design highlights (from Kami)

- Warm parchment canvas (`#f5f4ed`), never pure white
- Ink blue accent (`#1B365D`) — the only accent color
- Charter serif font (EN) / TsangerJinKai02 (ZH)
- Weight-based hierarchy (400/500 only), no fake bold
- Clean section dividers, no card styling or hard shadows

## Install

```bash
ud market install skill generate-resume-kami
```

## Usage

```
/generate-resume-kami        # both EN and ZH
/generate-resume-kami en     # English only
/generate-resume-kami zh     # Chinese only
```

## Output

Files are written to `tmp/`:
- `tmp/resume-<name>.html` (English)
- `tmp/resume-<name>-zh.html` (Chinese)

Open in browser and print to PDF.

## License

This skill is MIT licensed. The Kami design system is also MIT licensed — see [tw93/Kami](https://github.com/tw93/Kami) for details. Note: TsangerJinKai02 font requires a commercial license from tsanger.cn for non-personal use.
