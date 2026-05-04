# 📚 Metadata-Based File Renamer

Rename files using embedded metadata (Title + Author) with intelligent fallbacks.

Supports formats like:
- PDF
- EPUB
- CBZ / CBR
- Images (where metadata exists)
- Anything supported by ExifTool

---

## ⚙️ How it works

The script follows a layered strategy:

1. Extract metadata using ExifTool
2. If file is a PDF → refine using pdfinfo
3. Clean metadata:
   - Remove ( … ) and [ … ]
4. If metadata is missing:
   - Fallback to filename
   - Remove bracketed text
   - Drop last word (to avoid partial/corrupt suffixes)
5. Rename to:

Title - Author.ext

---

## 🚀 Installation

### 1. Save Script

chmod +x rename.sh

---

### 2. Install Dependencies

#### 🟢 Ubuntu / Debian

sudo apt update && sudo apt install -y libimage-exiftool-perl poppler-utils file

---

#### 🔴 Red Hat / Fedora / CentOS

sudo dnf install -y perl-Image-ExifTool poppler-utils file

---

#### ⚪ Arch Linux

sudo pacman -S exiftool poppler file

---

## 🧪 Usage

./rename.sh <file>

Example:

./rename.sh "Deep Learning (Draft v2).pdf"

---

## 📌 Example Transformation

Input:

Deep Learning (Ian Goodfellow) draft v2.pdf

Possible Output:

Deep Learning - Ian Goodfellow.pdf

Fallback case (no metadata):

Some Random File (v3 final).epub
→ Some Random File - Unknown.epub

---

## ⚠️ Warning (Read This Before You Batch Anything)

This script is heuristic-based, not perfect.

Things that can go wrong:

- Metadata may be:
  - Missing
  - Incorrect ("Microsoft Word - document1")
  - Misleading
- The fallback rule:
  removes the last word from filename

  This can destroy valid titles, e.g.:

  "One Piece Vol 01.cbz"
  → "One Piece Vol"

- Author may become "Unknown"
- Duplicate filenames will abort rename
- CBZ/CBR files often have no usable metadata

---

## 🧠 Recommended Safe Usage

Before trusting it blindly:

echo "./rename.sh \"file.pdf\""

Or manually test on a few files first.

If you want to batch:

find . -type f -print0 | xargs -0 -I{} ./rename.sh "{}"

But honestly — don’t batch until you trust your rules.

---

## 🧩 Limitations

- Does not parse ComicInfo.xml inside CBZ/CBR
- No metadata quality scoring
- No interactive confirmation
- No dry-run mode (yet)

---

## 🔧 Future Improvements

- Add --dry-run
- Add interactive rename approval
- Parse CBZ metadata (ComicInfo.xml)
- Reject low-quality metadata automatically
- Smarter fallback than “drop last word”

---

## 🧠 Final Thought

This script is not a “renamer”.

It’s a decision engine pretending to be one.

And right now, its decisions are:
- deterministic
- simple
- occasionally wrong

Which is exactly where things start getting interesting.