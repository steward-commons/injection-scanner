# Injection Scanner

A small, defensive text scanner. It looks for hidden instructions, invisible characters, and lookalike-letter tricks in a file before your agent acts on that text.

This tool is **defense only**. It is built to protect, never to attack. A match means "look at this," not "this is definitely an attack." The scanner never follows the text as an instruction, and it never changes the original file.

## What it does

Scanning uses a *copy* of the text:

1. Unicode NFKC normalization
2. Strips zero-width, invisible, and bidirectional control characters
3. Maps known confusable letters (Cyrillic, Greek, and similar) to a Latin skeleton
4. Flags instruction-like phrases such as "ignore previous instructions"
5. Flags mixed-script words (for example Latin + Cyrillic in one token)

Ordinary symbols (° © ½) and accented Latin letters in names are left alone and are not treated as mixed-script hits.

## Quick usage

```bash
python3 scan_text.py path/to/file.txt
```

Exit codes:

- `0` — no instruction-like phrases found
- `1` — one or more phrases were flagged
- `2` — the file was missing or could not be read

You can also import it in Python:

```python
from scan_text import scan_text

hits = scan_text(open("note.txt", encoding="utf-8").read())
for line_number, label, excerpt in hits:
    print(line_number, label, excerpt)
```

Needs Python 3 only. There are no third-party packages.

## Tests

From this folder:

```bash
python3 -m unittest
```

Tests use Python's built-in `unittest` library. Detection fixtures are short, clearly marked examples used only to prove a phrase is caught. They are not attack recipes.

## Limitations

- This is a simple pattern scanner, not a complete security product.
- New phrasing, new lookalike characters, or a language the pattern list does not cover can slip through.
- A clean result is not proof that a file is safe.
- A flagged result is not proof of hostile intent. Read the excerpt and use your own judgment.
- The confusable map covers frequent Latin lookalikes (Cyrillic, Greek, Armenian, and a few others). It is not an exhaustive Unicode confusables list.

## License

MIT. See `LICENSE`. Copyright 2026 Steward Commons contributors.
