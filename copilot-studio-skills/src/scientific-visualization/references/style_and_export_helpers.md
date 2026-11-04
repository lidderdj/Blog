# Style and Export Helpers

Self-contained Python helpers used by the examples in SKILL.md. They need only matplotlib and NumPy. Paste the block into a notebook or script (or run it in a code interpreter, if one is available) before running an example that calls these functions.

## Helper Code

```python
import matplotlib as mpl
import matplotlib.pyplot as plt

# ---------- Colorblind-safe palettes ----------
OKABE_ITO = {
    'orange': '#E69F00', 'sky_blue': '#56B4E9', 'bluish_green': '#009E73',
    'yellow': '#F0E442', 'blue': '#0072B2', 'vermillion': '#D55E00',
    'reddish_purple': '#CC79A7', 'black': '#000000',
}
OKABE_ITO_LIST = list(OKABE_ITO.values())
WONG = ['#000000', '#E69F00', '#56B4E9', '#009E73', '#F0E442', '#0072B2', '#D55E00', '#CC79A7']
TOL_BRIGHT = ['#4477AA', '#EE6677', '#228833', '#CCBB44', '#66CCEE', '#AA3377', '#BBBBBB']
TOL_MUTED = ['#332288', '#88CCEE', '#44AA99', '#117733', '#999933', '#DDCC77', '#CC6677', '#882255', '#AA4499']
TOL_HIGH_CONTRAST = ['#004488', '#DDAA33', '#BB5566']

PALETTES = {
    'okabe_ito': OKABE_ITO_LIST, 'wong': WONG, 'tol_bright': TOL_BRIGHT,
    'tol_muted': TOL_MUTED, 'tol_high_contrast': TOL_HIGH_CONTRAST,
}

def apply_palette(palette_name='okabe_ito'):
    """Set the default color cycle to a colorblind-safe palette."""
    colors = PALETTES[palette_name]
    plt.rcParams['axes.prop_cycle'] = plt.cycler(color=colors)
    return colors

set_color_palette = apply_palette  # alias

# ---------- Publication styles ----------
def _base_style():
    return {
        'figure.dpi': 100, 'figure.facecolor': 'white',
        'figure.constrained_layout.use': True,
        'font.size': 8, 'font.family': 'sans-serif',
        'font.sans-serif': ['Arial', 'Helvetica', 'DejaVu Sans'],
        'axes.linewidth': 0.5, 'axes.labelsize': 9, 'axes.titlesize': 9,
        'axes.spines.top': False, 'axes.spines.right': False,
        'axes.edgecolor': 'black', 'axes.labelcolor': 'black',
        'axes.axisbelow': True, 'axes.grid': False,
        'axes.prop_cycle': mpl.cycler(color=OKABE_ITO_LIST),
        'xtick.major.size': 3, 'xtick.minor.size': 2,
        'xtick.major.width': 0.5, 'xtick.minor.width': 0.5,
        'xtick.labelsize': 7, 'xtick.direction': 'out',
        'ytick.major.size': 3, 'ytick.minor.size': 2,
        'ytick.major.width': 0.5, 'ytick.minor.width': 0.5,
        'ytick.labelsize': 7, 'ytick.direction': 'out',
        'lines.linewidth': 1.5, 'lines.markersize': 4, 'lines.markeredgewidth': 0.5,
        'legend.fontsize': 7, 'legend.frameon': False,
        'savefig.dpi': 300, 'savefig.format': 'pdf', 'savefig.bbox': 'tight',
        'savefig.pad_inches': 0.05, 'savefig.facecolor': 'white',
        'pdf.fonttype': 42, 'ps.fonttype': 42,  # embed TrueType fonts
        'image.cmap': 'viridis',
    }

_STYLE_OVERRIDES = {
    'default': {},
    'nature': {'font.size': 7, 'axes.labelsize': 8, 'axes.titlesize': 8,
               'xtick.labelsize': 6, 'ytick.labelsize': 6, 'legend.fontsize': 6,
               'savefig.dpi': 600},
    'science': {'font.size': 7, 'axes.labelsize': 8, 'xtick.labelsize': 6,
                'ytick.labelsize': 6, 'legend.fontsize': 6, 'savefig.dpi': 600},
    'cell': {'font.size': 8, 'axes.labelsize': 9, 'xtick.labelsize': 7,
             'ytick.labelsize': 7, 'legend.fontsize': 7, 'savefig.dpi': 600},
    'minimal': {'axes.linewidth': 0.8, 'xtick.major.width': 0.8,
                'ytick.major.width': 0.8, 'lines.linewidth': 2},
    'presentation': {'font.size': 14, 'axes.labelsize': 16, 'axes.titlesize': 18,
                     'xtick.labelsize': 12, 'ytick.labelsize': 12, 'legend.fontsize': 12,
                     'axes.linewidth': 1.5, 'lines.linewidth': 2.5, 'lines.markersize': 8},
}

def apply_publication_style(style_name='default'):
    """Apply a preset: default, nature, science, cell, minimal, presentation."""
    style = _base_style()
    style.update(_STYLE_OVERRIDES[style_name])
    plt.rcParams.update(style)

# ---------- Journal specifications ----------
# Column widths and max height in mm
JOURNAL_SIZES = {
    'nature':  {'single': 89,   'double': 183, 'max_height': 247, 'style': 'nature'},
    'science': {'single': 55,   'double': 175, 'max_height': 233, 'style': 'science'},
    'cell':    {'single': 85,   'double': 178, 'max_height': 230, 'style': 'cell'},
    'plos':    {'single': 83,   'double': 173, 'max_height': 233, 'style': 'default'},
    'acs':     {'single': 82.5, 'double': 178, 'max_height': 247, 'style': 'default'},
    'ieee':    {'single': 89,   'double': 182, 'max_height': 247, 'style': 'default'},
}

# Export formats and DPI by figure type
JOURNAL_EXPORT = {
    'nature':  {'line_art': (['pdf', 'eps'], 1000), 'photo': (['tiff'], 300), 'combination': (['pdf'], 600)},
    'science': {'line_art': (['eps', 'pdf'], 1000), 'photo': (['tiff'], 300), 'combination': (['eps'], 600)},
    'cell':    {'line_art': (['pdf', 'eps'], 1000), 'photo': (['tiff'], 300), 'combination': (['pdf'], 600)},
    'plos':    {'line_art': (['pdf', 'eps'], 600),  'photo': (['tiff', 'png'], 300), 'combination': (['tiff'], 300)},
    'acs':     {'line_art': (['tiff', 'pdf'], 600), 'photo': (['tiff'], 300), 'combination': (['tiff'], 600)},
    'ieee':    {'line_art': (['pdf', 'eps'], 600),  'photo': (['tiff'], 300), 'combination': (['pdf'], 300)},
}

def configure_for_journal(journal, figure_width='single'):
    """Apply the journal's style and set the default figure size (4:3 aspect)."""
    spec = JOURNAL_SIZES[journal.lower()]
    apply_publication_style(spec['style'])
    width_in = spec[figure_width] / 25.4
    plt.rcParams['figure.figsize'] = (width_in, width_in * 0.75)

def check_figure_size(fig, journal='nature', tolerance_mm=5):
    """Report figure size in mm and whether it fits the journal's column widths."""
    spec = JOURNAL_SIZES.get(journal.lower(), JOURNAL_SIZES['nature'])
    w_in, h_in = fig.get_size_inches()
    w_mm, h_mm = w_in * 25.4, h_in * 25.4
    column = next((c for c in ('single', 'double') if abs(w_mm - spec[c]) < tolerance_mm), None)
    result = {
        'width_mm': round(w_mm, 1), 'height_mm': round(h_mm, 1),
        'column_type': column, 'width_ok': column is not None,
        'height_ok': h_mm <= spec['max_height'],
        'recommended_mm': {'single': spec['single'], 'double': spec['double'],
                           'max_height': spec['max_height']},
    }
    result['compliant'] = result['width_ok'] and result['height_ok']
    print(result)
    return result

# ---------- Export ----------
def save_publication_figure(fig, filename, formats=('pdf', 'png'), dpi=300,
                            transparent=False, **kwargs):
    """Save a figure in several formats with publication settings."""
    saved = []
    for fmt in formats:
        out = f"{filename}.{fmt}"
        fig.savefig(out, format=fmt,
                    dpi=min(dpi, 300) if fmt in ('pdf', 'eps', 'svg') else dpi,
                    bbox_inches='tight', pad_inches=0.1,
                    facecolor='none' if transparent else 'white',
                    edgecolor='none', transparent=transparent, **kwargs)
        saved.append(out)
    return saved

def save_for_journal(fig, filename, journal, figure_type='combination'):
    """Save with a journal's formats and DPI: figure_type is line_art, photo, or combination."""
    formats, dpi = JOURNAL_EXPORT[journal.lower()][figure_type]
    return save_publication_figure(fig, filename, formats=formats, dpi=dpi)
```

Always confirm current requirements in the journal's author guidelines; see `journal_requirements.md`.

## Other Color Definitions

```python
# Sequential colormaps (perceptually uniform first)
SEQUENTIAL_COLORMAPS = ['viridis', 'plasma', 'inferno', 'magma', 'cividis',
                        'YlOrRd', 'YlGnBu', 'Blues', 'Greens', 'Purples']

# Diverging colormaps that are safe for colorblind viewers
DIVERGING_COLORMAPS_SAFE = ['RdYlBu', 'RdBu', 'PuOr', 'BrBG', 'PRGn', 'PiYG']  # PRGn, PiYG: use with caution
# Avoid (red-green)
DIVERGING_COLORMAPS_AVOID = ['RdGn', 'RdYlGn']

# Fluorophore channels: colorblind-friendly alternatives to blue/green/red
FLUOROPHORES_ACCESSIBLE = {'Channel1': '#0072B2', 'Channel2': '#E69F00',
                           'Channel3': '#D55E00', 'Channel4': '#CC79A7',
                           'Channel5': '#F0E442'}

# DNA bases, colorblind-friendly
DNA_BASES_ACCESSIBLE = {'A': '#009E73', 'C': '#0072B2', 'G': '#E69F00', 'T': '#D55E00'}
```

## Matplotlib Style Files

To use a style file, save one of the blocks below as a `.mplstyle` file and load it with `plt.style.use('path/to/file.mplstyle')`.

### publication.mplstyle

```ini
# Publication-quality matplotlib style
# Usage: plt.style.use('publication.mplstyle')
#
# This style provides clean, professional formatting suitable
# for most scientific journals

# Figure properties
figure.dpi: 100
figure.facecolor: white
figure.autolayout: False
figure.constrained_layout.use: True
figure.figsize: 3.5, 2.5

# Font properties
font.size: 8
font.family: sans-serif
font.sans-serif: Arial, Helvetica, DejaVu Sans

# Axes properties
axes.linewidth: 0.5
axes.labelsize: 9
axes.titlesize: 9
axes.labelweight: normal
axes.spines.top: False
axes.spines.right: False
axes.spines.left: True
axes.spines.bottom: True
axes.edgecolor: black
axes.labelcolor: black
axes.axisbelow: True
axes.grid: False
axes.prop_cycle: cycler('color', ['E69F00', '56B4E9', '009E73', 'F0E442', '0072B2', 'D55E00', 'CC79A7', '000000'])

# Tick properties
xtick.major.size: 3
xtick.minor.size: 2
xtick.major.width: 0.5
xtick.minor.width: 0.5
xtick.labelsize: 7
xtick.direction: out
ytick.major.size: 3
ytick.minor.size: 2
ytick.major.width: 0.5
ytick.minor.width: 0.5
ytick.labelsize: 7
ytick.direction: out

# Line properties
lines.linewidth: 1.5
lines.markersize: 4
lines.markeredgewidth: 0.5

# Legend properties
legend.fontsize: 7
legend.frameon: False
legend.loc: best

# Save properties
savefig.dpi: 300
savefig.format: pdf
savefig.bbox: tight
savefig.pad_inches: 0.05
savefig.transparent: False
savefig.facecolor: white

# Image properties
image.cmap: viridis
image.aspect: auto
```

### nature.mplstyle

```ini
# Nature journal style
# Usage: plt.style.use('nature.mplstyle')
#
# Optimized for Nature journal specifications:
# - Single column: 89 mm
# - Double column: 183 mm
# - High resolution requirements

# Figure properties
figure.dpi: 100
figure.facecolor: white
figure.constrained_layout.use: True
figure.figsize: 3.5, 2.625  # 89 mm single column, 3:4 aspect

# Font properties (Nature prefers smaller fonts)
font.size: 7
font.family: sans-serif
font.sans-serif: Arial, Helvetica

# Axes properties
axes.linewidth: 0.5
axes.labelsize: 8
axes.titlesize: 8
axes.labelweight: normal
axes.spines.top: False
axes.spines.right: False
axes.edgecolor: black
axes.axisbelow: True
axes.grid: False
axes.prop_cycle: cycler('color', ['E69F00', '56B4E9', '009E73', 'F0E442', '0072B2', 'D55E00', 'CC79A7'])

# Tick properties
xtick.major.size: 2.5
xtick.minor.size: 1.5
xtick.major.width: 0.5
xtick.minor.width: 0.4
xtick.labelsize: 6
xtick.direction: out
ytick.major.size: 2.5
ytick.minor.size: 1.5
ytick.major.width: 0.5
ytick.minor.width: 0.4
ytick.labelsize: 6
ytick.direction: out

# Line properties
lines.linewidth: 1.2
lines.markersize: 3
lines.markeredgewidth: 0.4

# Legend properties
legend.fontsize: 6
legend.frameon: False

# Save properties (Nature requirements)
savefig.dpi: 600  # 1000 for line art, 600 for combination
savefig.format: pdf
savefig.bbox: tight
savefig.pad_inches: 0.05
savefig.facecolor: white

# Image properties
image.cmap: viridis
```

### presentation.mplstyle

```ini
# Presentation/Poster style
# Usage: plt.style.use('presentation.mplstyle')
#
# Larger fonts and thicker lines for presentations,
# posters, and projected displays

# Figure properties
figure.dpi: 100
figure.facecolor: white
figure.constrained_layout.use: True
figure.figsize: 8, 6

# Font properties (larger for visibility)
font.size: 14
font.family: sans-serif
font.sans-serif: Arial, Helvetica, Calibri

# Axes properties
axes.linewidth: 1.5
axes.labelsize: 16
axes.titlesize: 18
axes.labelweight: normal
axes.spines.top: False
axes.spines.right: False
axes.edgecolor: black
axes.axisbelow: True
axes.grid: False
axes.prop_cycle: cycler('color', ['E69F00', '56B4E9', '009E73', 'F0E442', '0072B2', 'D55E00', 'CC79A7'])

# Tick properties
xtick.major.size: 6
xtick.minor.size: 4
xtick.major.width: 1.5
xtick.minor.width: 1.0
xtick.labelsize: 12
xtick.direction: out
ytick.major.size: 6
ytick.minor.size: 4
ytick.major.width: 1.5
ytick.minor.width: 1.0
ytick.labelsize: 12
ytick.direction: out

# Line properties
lines.linewidth: 2.5
lines.markersize: 8
lines.markeredgewidth: 1.0

# Legend properties
legend.fontsize: 12
legend.frameon: False

# Save properties
savefig.dpi: 300
savefig.format: png
savefig.bbox: tight
savefig.pad_inches: 0.1
savefig.facecolor: white

# Image properties
image.cmap: viridis
```
