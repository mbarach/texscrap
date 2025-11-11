# texscrap

`texscrap` is a Python utility for turning snippets of LaTeX—either files (via `--file`) or equations (via `--equation`)—into tightly bound stand-alone PDFs.

Optionally, you can also create PNGs via the `--png` flag. The resultant files are written to the directory the script is called from.

The primary use case is creating image files for inclusion in a presentation or blog post. Even if you are using a Beamer class for presentations, it is often useful to include images of tables and equations rather than the actual TeX since the re-sizing options are more flexible.

## Example

To render an equation, you can run:

```bash
texscrap -e "e^{\pi i} + 1 = 0"
```

This will generate a PDF file that looks like this:

![example_image](e_pwr_pi_i_plus_1__0.png)

If you want a PNG file as well, just add the `--png` flag:

```bash
texscrap --png -e "e^{\pi i} + 1 = 0"
```

## Installation

### Prerequisites

Before installing `texscrap`, ensure you have the following system dependencies installed:

- **pdflatex** – Part of [TeX Live](http://www.tug.org/texlive/) or [MacTeX](http://www.tug.org/mactex/)
- **pdfcrop** – Usually included with TeX Live/MacTeX
- **ImageMagick** – For `convert` command

**macOS (with Homebrew):**
```bash
brew install texlive imagemagick ghostscript
```

**Ubuntu/Debian (via apt):**
```bash
sudo apt-get install texlive texlive-latex-extra imagemagick ghostscript
```

**Fedora/RHEL (via dnf):**
```bash
sudo dnf install texlive imagemagick ghostscript
```

### Install texscrap

`texscrap` requires Python 3.6+. It is recommended to use [pipx](https://pipx.pypa.io/) for installation, which provides an isolated environment and global CLI access:

**Via pipx (recommended):**
```bash
pipx install git+https://github.com/mbarach/texscrap.git
```

Or, if you have a local checkout:
```bash
pipx install /path/to/texscrap
```

**Via pip (in a virtual environment):**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install git+https://github.com/mbarach/texscrap.git
```

Or for local development (editable install):
```bash
pip install -e /path/to/texscrap
```

**Via conda (if you prefer conda environments):**
```bash
conda create -n texscrap python=3.11 -c conda-forge -y
conda activate texscrap
conda install -c conda-forge texlive imagemagick ghostscript -y
pip install git+https://github.com/mbarach/texscrap.git
```

## Usage

### Basic Commands

**Render an equation:**
```bash
texscrap -e "x^2 + y^2 = z^2"
```

**Render a LaTeX file:**
```bash
texscrap -f equation.tex
```

**Render from stdin:**
```bash
echo "e^{i\pi} + 1 = 0" | texscrap
```

**Generate PNG in addition to PDF:**
```bash
texscrap --png -e "e^{\pi i} + 1 = 0"
```

### Batch Processing

**Render multiple equations from a file:**
```bash
cat equations.txt
# x^2 + 3x
# \int x^2 dx
# \log x

cat equations.txt | xargs -I % texscrap -e "%"
```

**Render multiple LaTeX files:**
```bash
find . -name '*.tex' -print0 | xargs -0 -I % texscrap -f %
```

## Customizing LaTeX Packages

The LaTeX wrapper template is located in `texscrap/templates/latex_wrapper.tex` and can be easily modified to include additional packages or customize the rendering environment.

## Documentation

```
usage: texscrap [-h] [-f FILE] [-e EQUATION] [--png]

Runs pdflatex on a LaTeX file that lacks a header and returns a tightly
cropped PDF, and optionally, a PNG file. Can also run on a quoted LaTeX
equation

optional arguments:
  -h, --help            show this help message and exit
  -f FILE, --file FILE  File to render
  -e EQUATION, --equation EQUATION
                        Equation to render
  --png                 Create a companion PNG file.
```

## Requirements

- Python 3.6 or higher
- Jinja2 >= 2.6
- pdflatex
- pdfcrop
- ImageMagick (for PNG output)

## License

GNU General Public License v3 (GPLv3)

See LICENSE file for details.

## Author

Originally developed by John Joseph Horton.
