# This file is a merged representation of the entire TreeSheets source-code, combined into a single document.

## Purpose
- This file contains a packed representation of the entire TreeSheets repository's contents.
- It is designed to be easily consumable by AI systems for analysis, code review, or other automated processes.

## File Format
- The content is organized as follows:
  1. This summary section
  2. Repository information
  3. Directory structure
  4. Repository files
  5. Multiple file entries, each consisting of:
    a. A header with the file path (## File: path/to/file)
    b. The full contents of the file in a code block

## Usage Guidelines
- This file should be treated as read-only. Any changes should be made to the original repository files, not this packed version.
- When processing this file, use the file path to distinguish between different files in the repository.

## Notes
- Binary files are not included in this packed representation. Please refer to the Repository Structure section for a complete list of file paths, including binary files.
- Line numbers have been added to the beginning of each line.
- Content has been formatted for parsing in markdown style.

# Directory Structure
```
.github/
  workflows/
    build.yml
platform/
  linux/
    com.strlen.TreeSheets.svg
    com.strlen.TreeSheets.xml
src/
  cell.h
  document.h
  evaluator.h
  events.h
  genpot.bat
  grid.h
  image.h
  lobster_impl.cpp
  main.cpp
  pot_update.sh
  script_interface.h
  selection.h
  stdafx.cpp
  stdafx.h
  system.h
  text.h
  threadpool.h
  tools.h
  treesheets_impl.h
  tsapp.h
  tscanvas.h
  tsframe.h
  wxtools.h
.clang-format
.gitattributes
CMakeLists.txt
README.md
```

# Files

## File: .github/workflows/build.yml
```yaml
name: CI

on:
  push:
    branches:
      - master
  pull_request:
    branches:
      - master

jobs:
  build-linux:
    name: Build Linux (${{ matrix.arch }})
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        include:
          - arch: x64
            os: ubuntu-latest
          - arch: arm64
            os: ubuntu-24.04-arm
    permissions:
      contents: write
    steps:
    - uses: actions/checkout@v6
    - name: cache build
      uses: actions/cache@v5
      with:
        path: _build/_deps
        key: ${{ runner.os }}-${{ matrix.arch }}-cmake-02-build-${{ hashFiles('CMakeLists.txt') }}
        restore-keys: ${{ runner.os }}-${{ matrix.arch }}-cmake-02-build
    - name: apt update
      run: sudo apt-get -o Acquire::Retries=3 update
    - name: install opengl
      run: sudo apt-get -o Acquire::Retries=3 install mesa-common-dev libgl1-mesa-dev libgl1 libglx-mesa0 libxext-dev
    - name: install gtk
      run: sudo apt-get -o Acquire::Retries=3 install libgtk-3-dev
    - name: cmake
      run: |
        cmake -S . -B _build \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCPACK_PACKAGING_INSTALL_PREFIX=/usr \
        -DCMAKE_BUILD_TYPE=Release \
        -DwxBUILD_SHARED=OFF \
        -DwxBUILD_INSTALL=OFF \
        -DwxUSE_SYS_LIBS=OFF \
        -DTREESHEETS_VERSION=${{ github.run_number }}
    - name: build and package TreeSheets
      run: cmake --build _build --target package -j4
    - name: Remove epoch from .deb filename
      run: |
        shopt -s extglob
        for file in _build/treesheets_*:*.deb; do mv -v "${file}" "${file/_+([[:digit:]]):/_}"; done
    - name: Create release
      if: github.event_name == 'push'
      uses: ncipollo/release-action@v1
      with:
        tag: ${{ github.run_number }}
        allowUpdates: true
        omitBody: true
        commit: ${{ github.sha }}
        artifacts: "_build/treesheets_*.deb"
    - name: Upload artifacts
      if: github.event_name == 'pull_request'
      uses: actions/upload-artifact@v4
      with:
        name: linux-builds-${{ matrix.arch }}
        path: _build/treesheets_*.deb
    - name: Remove CPack artifact
      run: rm -f _build/treesheets_*.deb
```

## File: platform/linux/com.strlen.TreeSheets.svg
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!-- Created with Inkscape (http://www.inkscape.org/) -->

<svg
   xmlns:osb="http://www.openswatchbook.org/uri/2009/osb"
   xmlns:dc="http://purl.org/dc/elements/1.1/"
   xmlns:cc="http://creativecommons.org/ns#"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:svg="http://www.w3.org/2000/svg"
   xmlns="http://www.w3.org/2000/svg"
   xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd"
   xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
   width="680"
   height="680"
   viewBox="0 0 179.91666 179.91667"
   version="1.1"
   id="svg8"
   inkscape:version="0.92.5 (2060ec1f9f, 2020-04-08)"
   sodipodi:docname="treesheets.svg"
   inkscape:export-filename="/home/matthew/Desktop/treesheets.png"
   inkscape:export-xdpi="96"
   inkscape:export-ydpi="96">
  <defs
     id="defs2">
    <linearGradient
       id="linearGradient901"
       osb:paint="solid">
      <stop
         style="stop-color:#000000;stop-opacity:1;"
         offset="0"
         id="stop899" />
    </linearGradient>
  </defs>
  <sodipodi:namedview
     id="base"
     pagecolor="#ffffff"
     bordercolor="#666666"
     borderopacity="1.0"
     inkscape:pageopacity="0.0"
     inkscape:pageshadow="2"
     inkscape:zoom="0.733172"
     inkscape:cx="322.37837"
     inkscape:cy="271.15816"
     inkscape:document-units="px"
     inkscape:current-layer="layer1"
     showgrid="false"
     inkscape:window-width="1920"
     inkscape:window-height="1014"
     inkscape:window-x="0"
     inkscape:window-y="0"
     inkscape:window-maximized="1"
     units="px" />
  <metadata
     id="metadata5">
    <rdf:RDF>
      <cc:Work
         rdf:about="">
        <dc:format>image/svg+xml</dc:format>
        <dc:type
           rdf:resource="http://purl.org/dc/dcmitype/StillImage" />
        <dc:title></dc:title>
      </cc:Work>
    </rdf:RDF>
  </metadata>
  <g
     inkscape:label="Layer 1"
     inkscape:groupmode="layer"
     id="layer1"
     transform="translate(-19.704056,-40.32725)">
    <g
       id="g23"
       transform="matrix(1.0592611,0,0,1.0770814,-1.1013222,-16.908959)">
      <rect
         y="59.368668"
         x="25.867487"
         height="154.58766"
         width="157.39896"
         id="rect75-3"
         style="fill:#ffffff;fill-opacity:1;stroke:#767676;stroke-width:12.45077991;stroke-miterlimit:4;stroke-dasharray:none;stroke-opacity:1" />
      <path
         inkscape:connector-curvature="0"
         id="path15"
         d="M 37.041672,132.95834 H 170.08929"
         style="fill:none;stroke:#6d6d6d;stroke-width:3.06500006;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-dasharray:12.26, 12.26;stroke-dashoffset:0;stroke-opacity:1" />
      <path
         inkscape:connector-curvature="0"
         id="path15-8"
         d="M 36.588093,163.95238 H 169.63572"
         style="fill:none;stroke:#6d6d6d;stroke-width:3.06500006;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-dasharray:12.26000013, 12.26000013;stroke-dashoffset:0;stroke-opacity:1" />
      <path
         inkscape:connector-curvature="0"
         id="path15-5"
         d="M 129.26786,203.41308 V 70.36547"
         style="fill:none;stroke:#6d6d6d;stroke-width:3.06500006;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-dasharray:12.26000013, 12.26000013;stroke-dashoffset:0;stroke-opacity:1" />
      <g
         id="flowRoot913"
         style="font-style:normal;font-variant:normal;font-weight:bold;font-stretch:normal;font-size:74.66666412px;line-height:1.25;font-family:'Libris ADF Std';-inkscape-font-specification:'Libris ADF Std, Bold';font-variant-ligatures:normal;font-variant-caps:normal;font-variant-numeric:normal;font-feature-settings:normal;text-align:start;letter-spacing:0px;word-spacing:0px;writing-mode:lr-tb;text-anchor:start;fill:#000000;fill-opacity:1;stroke:none;stroke-opacity:1"
         transform="matrix(0.76673,0,0,0.76673,893.21672,-3.6280039)"
         aria-label="TS">
        <path
           id="path825"
           style="font-style:normal;font-variant:normal;font-weight:bold;font-stretch:normal;font-size:74.66666412px;font-family:'Libris ADF Std';-inkscape-font-specification:'Libris ADF Std, Bold';font-variant-ligatures:normal;font-variant-caps:normal;font-variant-numeric:normal;font-feature-settings:normal;text-align:start;writing-mode:lr-tb;text-anchor:start;fill:#000000;fill-opacity:1;stroke:none;stroke-opacity:1"
           d="m -1078.6855,155.60379 c 0,-15.38133 -0.075,-31.35999 -0.075,-46.74133 h 15.1574 l 2.3893,-6.79466 h -41.664 l -2.9867,6.79466 h 17.7707 c 0,8.43734 0.1493,15.456 0.1493,21.056 0,21.87733 -0.7466,25.16267 -2.3146,27.104 z"
           inkscape:connector-curvature="0" />
        <path
           id="path827"
           style="font-style:normal;font-variant:normal;font-weight:bold;font-stretch:normal;font-size:74.66666412px;font-family:'Libris ADF Std';-inkscape-font-specification:'Libris ADF Std, Bold';font-variant-ligatures:normal;font-variant-caps:normal;font-variant-numeric:normal;font-feature-settings:normal;text-align:start;writing-mode:lr-tb;text-anchor:start;fill:#000000;fill-opacity:1;stroke:none;stroke-opacity:1"
           d="m -1064.2003,150.07846 c 2.688,4.40533 11.2747,6.12267 15.9787,7.69067 12.2453,0 18.6667,-10.37867 18.6667,-18.592 0,-3.808 -1.792,-7.24267 -4.928,-9.856 -2.0907,-1.64267 -8.6614,-2.83733 -12.32,-4.85333 -3.8827,-2.09067 -6.1227,-5.67467 -6.1227,-9.25867 0,-2.61333 0.5973,-4.85333 1.6427,-6.34667 0.896,-0.14933 1.7173,-0.224 2.24,-0.224 3.2853,0 8.96,1.86667 11.7226,5.74934 l 7.9894,-5.00267 c -2.7627,-4.40533 -7.5414,-6.72 -11.9467,-6.72 -3.5093,0 -10.0053,1.49333 -12.3947,3.36 -4.928,4.032 -10.4533,8.88533 -10.4533,14.26133 0,3.50934 1.1947,6.64534 3.808,9.10934 h -0.075 c 2.3894,2.464 8.8854,3.65866 11.3494,4.85333 l 1.8666,0.896 c 4.6294,2.24 8.736,4.256 8.736,8.96 0,5.00266 -1.344,7.24266 -2.6133,7.24266 -3.8827,0 -10.1547,-2.016 -12.8427,-6.64533 z"
           inkscape:connector-curvature="0" />
      </g>
    </g>
  </g>
</svg>
```

## File: platform/linux/com.strlen.TreeSheets.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<mime-info xmlns="http://www.freedesktop.org/standards/shared-mime-info">
  <mime-type type="application/x-treesheets">
    <comment xml:lang="en">TreeSheets document</comment>
    <icon name="com.strlen.TreeSheets"/>
    <glob pattern="*.cts"/>
    <magic priority="50">
      <match type="string" value="TSFF" offset="0"/>
    </magic>
    <sub-class-of type="application/octet-stream"/>
  </mime-type>
</mime-info>
```

## File: src/cell.h
```c
/* The evaluation types for a cell.
CT_DATA: "Data"
CT_CODE: "Operation"
CT_VARD: "Variable Assign"
CT_VARU: "Variable Read"
CT_VIEWH: "Horizontal View"
CT_VIEWV: "Vertical View"
*/
enum { CT_DATA = 0, CT_CODE, CT_VARD, CT_VIEWH, CT_VARU, CT_VIEWV };

/* The drawstyles for a cell:

*/
enum { DS_GRID, DS_BLOBSHIER, DS_BLOBLINE };

/**
    The Cell structure represents the editable cells in the sheet.

    They are mutable structures containing a text and grid object. Along with
    formatting information.
*/
struct Cell {
    Cell *parent;
    int sx {0};
    int sy {0};
    int ox {0};
    int oy {0};
    int minx {0};
    int miny {0};
    int ycenteroff {0};
    int txs {0};
    int tys {0};
    int celltype;
    Text text;
    Grid *grid;
    uint cellcolor {g_cellcolor_default};
    uint actualcellcolor {g_cellcolor_default};
    uint textcolor {g_textcolor_default};
    bool tiny {false};
    bool verticaltextandgrid {true};
    wxUint8 drawstyle {DS_GRID};
    wxString note;

    Cell(Cell *_p = nullptr, const Cell *_clonefrom = nullptr, int _ct = CT_DATA,
         Grid *_g = nullptr)
        : parent(_p), celltype(_ct), grid(_g) {
        text.cell = this;
        if (_g) _g->cell = this;
        if (_p) {
            text.relsize = _p->text.relsize;
            verticaltextandgrid = _p->verticaltextandgrid;
        }
        if (_clonefrom) CloneStyleFrom(_clonefrom);
    }

    ~Cell() { DELETEP(grid); }
    void Clear() {
        DELETEP(grid);
        text.t.Clear();
        text.image = nullptr;
        Reset();
    }

    bool HasText() const { return !text.t.empty(); }
    bool HasTextSize() const { return HasText() || text.relsize; }
    bool HasTextState() const { return HasTextSize() || text.image; }
    bool HasHeader() const { return HasText() || text.image; }
    bool HasContent() const { return HasHeader() || grid; }
    bool GridShown(Document *doc) const {
        return grid && (!grid->folded || this == doc->currentdrawroot);
    }
    int MinRelsize()  // the smallest relsize is actually the biggest text
    {
        int rs = INT_MAX;
        if (grid) {
            rs = grid->MinRelsize(rs);
        } else if (HasText()) {
            // the "else" causes oversized titles but a readable grid when you zoom, if only
            // the grid has been shrunk
            rs = text.MinRelsize(rs);
        }
        return rs;
    }

    size_t EstimatedMemoryUse() {
        return sizeof(Cell) + text.EstimatedMemoryUse() + (grid ? grid->EstimatedMemoryUse() : 0);
    }

    void Layout(Document *doc, wxReadOnlyDC &dc, int depth, int maxcolwidth, bool forcetiny) {
        tiny = text.filtered && !grid || forcetiny ||
               doc->PickFont(dc, depth, text.relsize, text.stylebits);
        int ixs = 0, iys = 0;
        if (!tiny) sys->ImageSize(text.DisplayImage(), ixs, iys);
        int leftoffset = 0;
        if (!HasText()) {
            if (!ixs || !iys) {
                sx = sy = tiny ? 1 : dc.GetCharHeight();
            } else {
                leftoffset = dc.GetCharHeight();
            }
        } else {
            text.TextSize(dc, sx, sy, tiny, leftoffset, maxcolwidth);
        }
        if (ixs && iys) {
            sx += ixs + 2;
            sy = max(iys + 2, sy);
        }
        text.extent = sx + depth * dc.GetCharHeight();
        txs = sx;
        tys = sy;
        if (GridShown(doc)) {
            if (HasHeader()) {
                if (verticaltextandgrid) {
                    int osx = sx;
                    if (drawstyle == DS_BLOBLINE && !tiny) sy += 4;
                    grid->Layout(doc, dc, depth, sx, sy, leftoffset, sy, tiny || forcetiny);
                    sx = max(sx, osx);
                } else {
                    int osy = sy;
                    if (drawstyle == DS_BLOBLINE && !tiny) sx += 18;
                    grid->Layout(doc, dc, depth, sx, sy, sx, 0, tiny || forcetiny);
                    sy = max(sy, osy);
                }
            } else
                tiny = grid->Layout(doc, dc, depth, sx, sy, 0, 0, forcetiny);
        }
        ycenteroff = !verticaltextandgrid ? (sy - tys) / 2 : 0;
        if (!tiny) {
            sx += g_margin_extra * 2;
            sy += g_margin_extra * 2;
        }
    }

    void Render(Document *doc, int bx, int by, wxDC &dc, int depth, int ml, int mr, int mt, int mb,
                int maxcolwidth, int cell_margin) {
        // Choose color from celltype (program operations)
        switch (celltype) {
            case CT_VARD: actualcellcolor = 0xFF8080; break;
            case CT_VARU: actualcellcolor = 0xFFA0A0; break;
            case CT_VIEWH:
            case CT_VIEWV: actualcellcolor = 0x80FF80; break;
            case CT_CODE: actualcellcolor = 0x8080FF; break;
            default: actualcellcolor = cellcolor; break;
        }
        uint parentcolor = doc->Background();
        if (parent && this != doc->currentdrawroot) {
            Cell *p = parent;
            while (p && p->drawstyle == DS_BLOBLINE)
                p = p == doc->currentdrawroot ? nullptr : p->parent;
            if (p) parentcolor = p->actualcellcolor;
        }

        if (sys->darkennonmatchingcells && !text.IsInSearch()) {
            auto cp = (uchar *)&actualcellcolor;
            loop(i, 4) cp[i] = cp[i] * 800 / 1000;
        }

        if (drawstyle == DS_GRID && actualcellcolor != parentcolor) {
            DrawRectangle(dc, actualcellcolor, bx - ml, by - mt, sx + ml + mr, sy + mt + mb);
        }
        if (drawstyle != DS_GRID && HasContent() && !tiny) {
            if (actualcellcolor == parentcolor) {
                auto cp = (uchar *)&actualcellcolor;
                loop(i, 4) cp[i] = cp[i] * 850 / 1000;
            }
            dc.SetBrush(wxBrush(LightColor(actualcellcolor)));
            dc.SetPen(wxPen(LightColor(actualcellcolor)));

            if (drawstyle == DS_BLOBSHIER)
                dc.DrawRoundedRectangle(bx - cell_margin, by - cell_margin, minx + cell_margin * 2,
                                        miny + cell_margin * 2, sys->roundness);
            else if (HasHeader())
                dc.DrawRoundedRectangle(bx - cell_margin + g_margin_extra / 2,
                                        by - cell_margin + ycenteroff + g_margin_extra / 2,
                                        txs + cell_margin * 2 + g_margin_extra,
                                        tys + cell_margin * 2 + g_margin_extra, sys->roundness);
            // FIXME: this half a g_margin_extra is a bit of hack
        }
        dc.SetTextBackground(LightColor(actualcellcolor));
        int xoff = verticaltextandgrid ? 0 : text.extent - depth * dc.GetCharHeight();
        int yoff = text.Render(doc, bx, by + ycenteroff, depth, dc, xoff, maxcolwidth);
        yoff = verticaltextandgrid ? yoff : 0;
        if (GridShown(doc)) grid->Render(doc, bx, by, dc, depth, sx - xoff, sy - yoff, xoff, yoff);

        if (!note.IsEmpty() && !tiny && this != doc->currentdrawroot) {
            wxPoint points[3];
            int size = 6;
            int right = bx + sx + mr;
            int top = by - mt;
            points[0] = wxPoint(right, top);
            points[1] = wxPoint(right, top + size);
            points[2] = wxPoint(right - size, top);
            dc.SetBrush(wxBrush(LightColor(textcolor)));
            dc.SetPen(wxPen(LightColor(textcolor)));
            dc.DrawPolygon(3, points);
        }
    }

    void CloneStyleFrom(Cell const *o) {
        cellcolor = o->cellcolor;
        textcolor = o->textcolor;
        verticaltextandgrid = o->verticaltextandgrid;
        drawstyle = o->drawstyle;
        text.stylebits = o->text.stylebits;
    }

    unique_ptr<Cell> Clone(Cell *_parent) const {
        unique_ptr<Cell> c = make_unique<Cell>(_parent, this, celltype,
                                               grid ? new Grid(grid->xs, grid->ys) : nullptr);
        c->text = text;
        c->text.cell = c.get();
        c->note = note;
        if (grid) { grid->Clone(c->grid); }
        return c;
    }

    bool IsInside(int x, int y) const { return x >= 0 && y >= 0 && x < sx && y < sy; }
    int GetX(Document *doc) const { return ox + (parent ? parent->GetX(doc) : doc->hierarchysize); }
    int GetY(Document *doc) const { return oy + (parent ? parent->GetY(doc) : doc->hierarchysize); }
    int Depth() const { return parent ? parent->Depth() + 1 : 0; }
    Cell *Parent(int i) { return i ? parent->Parent(i - 1) : this; }
    Cell *SetParent(Cell *g) {
        parent = g;
        return this;
    }
    bool IsParentOf(const Cell *c) {
        return c->parent == this || (c->parent && IsParentOf(c->parent));
    }

    wxString ToText(int indent, const Selection &sel, int format, Document *doc, bool inheritstyle,
                    Cell *root) {
        wxString str = text.ToText(indent, sel, format);
        if ((format == A_EXPHTMLT || format == A_EXPHTMLTI || format == A_EXPHTMLTE) &&
            (text.stylebits & (STYLE_UNDERLINE | STYLE_STRIKETHRU)) && this != root &&
            !str.IsEmpty()) {
            wxString spanstyle = "text-decoration:";
            spanstyle += (text.stylebits & STYLE_UNDERLINE) ? " underline" : "";
            spanstyle += (text.stylebits & STYLE_STRIKETHRU) ? " line-through" : "";
            spanstyle += ";";
            str.Prepend("<span style=\"" + spanstyle + "\">");
            str.Append("</span>");
        }
        if (format == A_EXPCSV) {
            if (grid) return grid->ToText(indent, sel, format, doc, inheritstyle, root);
            str.Replace("\"", "\"\"");
            return "\"" + str + "\"";
        }
        if (sel.cursor != sel.cursorend) return str;
        str.Append(LINE_SEPARATOR);
        if (grid) str.Append(grid->ToText(indent, sel, format, doc, inheritstyle, root));
        if (format == A_EXPXML) {
            str.Prepend(">");
            if (text.relsize) {
                str.Prepend("\"");
                str.Prepend(wxString() << -text.relsize);
                str.Prepend(" relsize=\"");
            }
            if (text.stylebits) {
                str.Prepend("\"");
                str.Prepend(wxString() << text.stylebits);
                str.Prepend(" stylebits=\"");
            }
            if (cellcolor != 0xFFFFFF) {
                str.Prepend("\"");
                str.Prepend(wxString::Format("0x%06X", cellcolor));
                str.Prepend(" colorbg=\"");
            }
            if (textcolor != 0x000000) {
                str.Prepend("\"");
                str.Prepend(wxString::Format("0x%06X", textcolor));
                str.Prepend(" colorfg=\"");
            }
            if (celltype != CT_DATA) {
                str.Prepend("\"");
                str.Prepend(wxString() << celltype);
                str.Prepend(" type=\"");
            }
            str.Prepend("<cell");
            str.Append(' ', indent);
            str.Append("</cell>\n");
        } else if ((format == A_EXPHTMLT || format == A_EXPHTMLTI || format == A_EXPHTMLTE) &&
                   this != root) {
            wxString style;
            if (!inheritstyle || !parent ||
                (text.stylebits & STYLE_BOLD) != (parent->text.stylebits & STYLE_BOLD))
                style +=
                    text.stylebits & STYLE_BOLD ? "font-weight: bold;" : "font-weight: normal;";
            if (!inheritstyle || !parent ||
                (text.stylebits & STYLE_ITALIC) != (parent->text.stylebits & STYLE_ITALIC))
                style +=
                    text.stylebits & STYLE_ITALIC ? "font-style: italic;" : "font-style: normal;";
            if (!inheritstyle || !parent ||
                (text.stylebits & STYLE_FIXED) != (parent->text.stylebits & STYLE_FIXED)) {
                style += "font-family: '";
                style += text.stylebits & STYLE_FIXED ? sys->defaultfixedfont + "', monospace;"
                                                      : sys->defaultfont + "', sans-serif;";
            }
            if (!inheritstyle || cellcolor != (parent ? parent->cellcolor : doc->Background()))
                style += wxString::Format("background-color: #%06X;", SwapColor(cellcolor));
            auto exporttextcolor = IsTag(doc) ? doc->tags[text.t] : textcolor;
            auto parenttextcolor =
                parent ? parent->IsTag(doc) ? doc->tags[parent->text.t] : parent->textcolor
                       : 0x000000;
            if (!inheritstyle || exporttextcolor != parenttextcolor)
                style += wxString::Format("color: #%06X;", SwapColor(exporttextcolor));
            str.Prepend(style.IsEmpty() ? wxString("<td>")
                                        : wxString("<td style=\"") + style + wxString("\">"));
            str.Append(' ', indent);
            str.Append("</td>\n");
        } else if (format == A_EXPHTMLB && (text.t.Len() || grid) && this != root) {
            str.Prepend("<li>");
            str.Append(' ', indent);
            str.Append("</li>\n");
        } else if (format == A_EXPHTMLO && text.t.Len()) {
            wxString h = wxString("h") + wxChar('0' + indent / 2) + ">";
            str.Prepend("<" + h);
            str.Append(' ', indent);
            str.Append("</" + h + "\n");
        }
        str.Pad(indent, ' ', false);
        return str;
    }

    void RelSize(int dir, int zoomdepth) {
        text.RelSize(dir, zoomdepth);
        if (grid) grid->RelSize(dir, zoomdepth);
    }

    void Reset() { ox = oy = sx = sy = minx = miny = ycenteroff = 0; }
    void ResetChildren() {
        Reset();
        if (grid) grid->ResetChildren();
    }

    void ResetLayout() {
        Reset();
        if (parent) parent->ResetLayout();
    }

    void LazyLayout(Document *doc, wxReadOnlyDC &dc, int depth, int maxcolwidth, bool forcetiny) {
        if (sx == 0) {
            Layout(doc, dc, depth, maxcolwidth, forcetiny);
            minx = sx;
            miny = sy;
        } else {
            sx = minx;
            sy = miny;
        }
    }

    void AddUndo(Document *doc) {
        ResetLayout();
        doc->AddUndo(this);
    }

    void Save(wxDataOutputStream &dos, Cell *ocs) const {
        dos.Write8(celltype);
        dos.Write32(cellcolor);
        dos.Write32(textcolor);
        dos.Write8(drawstyle);
        dos.WriteString(note);
        uint cellflags = this == ocs ? TS_SELECTION_MASK : 0;
        if (HasTextState()) {
            cellflags |= grid ? TS_BOTH : TS_TEXT;
            dos.Write8(cellflags);
            text.Save(dos);
            if (grid) grid->Save(dos, ocs);
        } else if (grid) {
            cellflags |= TS_GRID;
            dos.Write8(cellflags);
            grid->Save(dos, ocs);
        } else {
            cellflags |= TS_NEITHER;
            dos.Write8(cellflags);
        }
    }

    Grid *AddGrid(int x = 1, int y = 1) {
        if (!grid) {
            grid = new Grid(x, y, this);
            grid->InitCells(this);
            if (parent) grid->CloneStyleFrom(parent->grid);
        }
        return grid;
    }

    Cell *LoadGrid(wxDataInputStream &dis, int &numcells, int &textbytes, Cell *&ics) {
        int xs = dis.Read32();
        auto g = new Grid(xs, dis.Read32());
        grid = g;
        g->cell = this;
        if (!g->LoadContents(dis, numcells, textbytes, ics)) return nullptr;
        return this;
    }

    static Cell *LoadWhich(wxDataInputStream &dis, Cell *_p, int &numcells, int &textbytes, Cell *&ics) {
        auto c = new Cell(_p, nullptr, dis.Read8());
        numcells++;
        if (sys->versionlastloaded >= 8) {
            c->cellcolor = dis.Read32() & 0xFFFFFF;
            c->textcolor = dis.Read32() & 0xFFFFFF;
        }
        if (sys->versionlastloaded >= 15) c->drawstyle = dis.Read8();
        if (sys->versionlastloaded >= 25) c->note = dis.ReadString();
        int ts = dis.Read8();
        if (ts & TS_SELECTION_MASK) {
            ics = c;
            ts &= ~TS_SELECTION_MASK;
        }
        switch (ts) {
            case TS_BOTH:
            case TS_TEXT:
                c->text.Load(dis);
                textbytes += c->text.t.Len();
                if (ts == TS_TEXT) return c;
            case TS_GRID: return c->LoadGrid(dis, numcells, textbytes, ics);
            case TS_NEITHER: return c;
            default: return nullptr;
        }
    }

    unique_ptr<Cell> Eval(auto &ev) const {
        // Evaluates the internal grid if it exists, otherwise, evaluate the text.
        return grid ? grid->Eval(ev) : text.Eval(ev);
    }

    void Paste(Document *document, const Cell *original, Selection &selection) {
        parent->AddUndo(document);
        ResetLayout();
        if (!HasText() || !selection.TextEdit()) { note = original->note; }
        if (original->HasText()) {
            if (!HasText() || !selection.TextEdit()) {
                cellcolor = original->cellcolor;
                textcolor = original->textcolor;
                text.stylebits = original->text.stylebits;
            }
            text.Insert(document, original->text.t, selection, false);
        }
        if (original->text.image) text.image = original->text.image;
        if (original->grid) {
            auto gridclone = new Grid(original->grid->xs, original->grid->ys);
            gridclone->cell = this;
            original->grid->Clone(gridclone);
            // Note: deleting grid may invalidate c if its a child of grid, so clear it.
            original = nullptr;
            DELETEP(grid);  // FIXME: could merge instead?
            grid = gridclone;
            if (!HasText())
                grid->MergeWithParent(parent->grid, selection, document);  // deletes grid/this.
        }
    }

    Cell *FindNextSearchMatch(const wxString &s, Cell *best, Cell *selected, bool &lastwasselected,
                              bool reverse) {
        if (reverse && grid)
            best = grid->FindNextSearchMatch(s, best, selected, lastwasselected, reverse);
        if ((sys->casesensitivesearch ? text.t.Find(s) : text.t.Lower().Find(s)) >= 0) {
            if (lastwasselected) best = this;
            lastwasselected = false;
        }
        if (selected == this) lastwasselected = true;
        if (!reverse && grid)
            best = grid->FindNextSearchMatch(s, best, selected, lastwasselected, reverse);
        return best;
    }

    Cell *FindNextFilterMatch(Cell *best, Cell *selected, bool &lastwasselected) {
        if (!text.filtered) {
            if (lastwasselected) best = this;
            lastwasselected = false;
        }
        if (selected == this) lastwasselected = true;
        if (grid) best = grid->FindNextFilterMatch(best, selected, lastwasselected);
        return best;
    }

    Cell *FindLink(const Selection &sel, Cell *link, Cell *best, bool &lastthis, bool &stylematch,
                   bool forward, bool image) {
        if (grid) best = grid->FindLink(sel, link, best, lastthis, stylematch, forward, image);
        if (link == this) {
            lastthis = true;
            return best;
        }
        if (image ? link->text.image == text.image
                  : link->text.ToText(0, sel, A_EXPTEXT) == text.t) {
            if (link->text.stylebits != text.stylebits || link->cellcolor != cellcolor ||
                link->textcolor != textcolor) {
                if (!stylematch) best = nullptr;
                stylematch = true;
            } else if (stylematch) {
                return best;
            }
            if (!best || lastthis) {
                lastthis = false;
                return this;
            }
        }
        return best;
    }

    void FindReplaceAll(const wxString &s, const wxString &ls) {
        if (grid) grid->FindReplaceAll(s, ls);
        text.ReplaceStr(s, ls);
    }

    Cell *FindExact(const wxString &s) {
        return text.t == s ? this : (grid ? grid->FindExact(s) : nullptr);
    }

    void ImageRefCount(bool includefolded) {
        if (grid) grid->ImageRefCount(includefolded);
        if (text.image) text.image->trefc++;
    }

    void ColorChange(Document *doc, int which, uint color) {
        switch (which) {
            case A_CELLCOLOR: cellcolor = color; break;
            case A_TEXTCOLOR:
                if (IsTag(doc)) {
                    doc->tags[text.t] = color;
                } else {
                    textcolor = color;
                }
                break;
            case A_BORDCOLOR:
                if (parent && parent->grid) parent->grid->bordercolor = color;
                break;
        }
        text.WasEdited();
    }

    void SetGridTextLayout(int ds, bool vert, bool noset) {
        if (!noset) verticaltextandgrid = vert;
        if (ds != -1) drawstyle = ds;
        if (grid) grid->SetGridTextLayout(ds, vert, noset, grid->SelectAll());
    }

    bool IsTag(Document *doc) { return doc->tags.contains(text.t); }
    void MaxDepthLeaves(int curdepth, int &maxdepth, int &leaves) {
        if (curdepth > maxdepth) maxdepth = curdepth;
        if (grid)
            grid->MaxDepthLeaves(curdepth + 1, maxdepth, leaves);
        else
            leaves++;
    }

    int ColWidth() {
        return parent ? parent->grid->colwidths[parent->grid->FindCell(this).x]
                      : sys->defaultmaxcolwidth;
    }

    void CollectCells(auto &itercells, bool recurse = true) {
        itercells.push_back(this);
        if (grid && recurse) grid->CollectCells(itercells);
    }

    Cell *Graph() {
        auto n = text.GetNum();
        text.t.Clear();
        text.t.Append(L'|', n);
        return this;
    }
};
```

## File: src/document.h
```c
struct UndoItem {
    vector<Selection> path;
    vector<Selection> selpath;
    Selection sel;
    unique_ptr<Cell> clone;
    size_t estimated_size {0};
    uintptr_t cloned_from;  // May be dead.
    int generation {0};
};

struct Document {
    TSCanvas *canvas {nullptr};
    unique_ptr<Cell> root {nullptr};
    Selection prev;
    Selection hover;
    Selection selected;
    Selection begindrag;
    int isctrlshiftdrag;
    int scrollx;
    int scrolly;
    int maxx;
    int maxy;
    int centerx {0};
    int centery {0};
    int layoutxs;
    int layoutys;
    int hierarchysize;
    int fgutter {6};
    int lasttextsize;
    int laststylebits;
    Cell *currentdrawroot;  // for use during Render() calls
    vector<unique_ptr<UndoItem>> undolist;
    vector<unique_ptr<UndoItem>> redolist;
    vector<Selection> drawpath;
    int pathscalebias {0};
    wxString filename {""};
    long lastmodsinceautosave {0};
    long undolistsizeatfullsave {0};
    long lastsave {wxGetLocalTime()};
    bool modified {false};
    bool tmpsavesuccess {true};
    wxDataObjectComposite *dndobjc {new wxDataObjectComposite()};
    wxTextDataObject *dndobjt {new wxTextDataObject()};
    wxBitmapDataObject *dndobji {new wxBitmapDataObject()};
    wxFileDataObject *dndobjf {new wxFileDataObject()};

    struct Printout : wxPrintout {
        Document *doc;
        Printout(Document *d) : wxPrintout("printout"), doc(d) {}

        bool OnPrintPage(int page) {
            auto dc = GetDC();
            if (!dc) return false;
            doc->Print(*dc, *this);
            return true;
        }

        bool OnBeginDocument(int startPage, int endPage) {
            return wxPrintout::OnBeginDocument(startPage, endPage);
        }

        void GetPageInfo(int *minPage, int *maxPage, int *selPageFrom, int *selPageTo) {
            *minPage = 1;
            *maxPage = 1;
            *selPageFrom = 1;
            *selPageTo = 1;
        }

        bool HasPage(int pageNum) { return pageNum == 1; }
    };

    bool while_printing {false};
    wxPrintData printData;
    wxPageSetupDialogData pageSetupData;
    uint printscale {0};
    bool scaledviewingmode {false};
    bool paintscrolltoselection {true};
    double currentviewscale {1.0};
    bool searchfilter {false};
    int editfilter {0};
    wxDateTime lastmodificationtime;
    map<wxString, uint> tags;
    vector<Cell *> itercells;

    #define loopcellsin(par, c) \
        CollectCells(par);      \
        loopv(_i, itercells) for (auto c = itercells[_i]; c; c = nullptr)
    #define loopallcells(c)     \
        CollectCells(root.get()); \
        for (auto c : itercells)
    #define loopallcellssel(c, rec) \
        CollectCellsSel(rec);     \
        for (auto c : itercells)

    Document() {
        ResetFont();
        pageSetupData = printData;
        pageSetupData.SetMarginTopLeft(wxPoint(15, 15));
        pageSetupData.SetMarginBottomRight(wxPoint(15, 15));
        dndobjc->Add(dndobjt);
        dndobjc->Add(dndobji);
        dndobjc->Add(dndobjf);
    }

    uint Background() { return root ? root->cellcolor : 0xFFFFFF; }

    void InitCellSelect(Cell *initialselected, int xsize, int ysize) {
        if (!initialselected) {
            SetSelect(Selection(root->grid, 0, 0, 1, 1));
            return;
        }
        SetSelect(initialselected->parent->grid->FindCell(initialselected));
        selected.xs = xsize;
        selected.ys = ysize;
        sys->frame->UpdateStatus(selected, true);
    }

    void InitWith(unique_ptr<Cell> root, const wxString &filename, Cell *initialselected, int xsize, int ysize) {
        this->root = std::move(root);
        InitCellSelect(initialselected, xsize, ysize);
        ChangeFileName(filename, false);
    }

    void UpdateFileName(int page = -1) {
        sys->frame->SetPageTitle(filename, modified ? (lastmodsinceautosave ? "*" : "+") : "",
                                 page);
    }

    void ChangeFileName(const wxString &newfilename, bool checkext) {
        filename = newfilename;
        if (checkext) {
            wxFileName wxfn(filename);
            if (!wxfn.HasExt()) filename.Append(".cts");
        }
        UpdateFileName();
    }

    wxString SaveDB(bool *success, bool istempfile = false, int page = -1) {
        if (filename.empty()) return _("Save cancelled.");
        Cell *ocs = selected.xs == 0 && selected.ys == 0
                        ? nullptr
                        : selected.grid->C(selected.x, selected.y).get();
        auto start_saving_time = wxGetLocalTimeMillis();

        auto targetfilename = istempfile ? sys->TmpName(filename) : filename;
        auto savefilename = sys->NewName(targetfilename);

        {  // limit destructors
            wxBusyCursor wait;
            wxFFileOutputStream fos(savefilename);
            if (!fos.IsOk()) {
                if (!istempfile)
                    wxMessageBox(
                        _("Error writing TreeSheets file! (try saving under new filename)."),
                        savefilename.wx_str(), wxOK, sys->frame);
                return _("Error writing to file.");
            }

            wxDataOutputStream sos(fos);
            fos.Write("TSFF", 4);
            char vers = TS_VERSION;
            fos.Write(&vers, 1);
            sos.Write8(selected.xs);
            sos.Write8(selected.ys);
            sos.Write8(ocs ? drawpath.size() : 0);  // zoom level
            RefreshImageRefCount(true);
            int realindex = 0;
            loopv(i, sys->imagelist) {
                if (auto &image = *sys->imagelist[i]; image.trefc) {
                    fos.PutC(image.type);
                    sos.WriteDouble(image.display_scale);
                    wxInt64 imagelen(image.data.size());
                    sos.Write64(imagelen);
                    fos.Write(image.data.data(), imagelen);
                    image.savedindex = realindex++;
                }
            }

            fos.Write("D", 1);
            wxZlibOutputStream zos(fos, 9);
            if (!zos.IsOk()) return _("Zlib error while writing file.");
            wxDataOutputStream dos(zos);
            root->Save(dos, ocs);
            for (auto &[tag, color] : tags) {
                dos.WriteString(tag);
                dos.Write32(color);
            }
            dos.WriteString(wxEmptyString);
        }

        if (!istempfile && sys->makebaks && ::wxFileExists(filename)) {
            ::wxRemoveFile(sys->BakName(filename));
            ::wxRenameFile(filename, sys->BakName(filename));
        }

        if (!::wxRenameFile(savefilename, targetfilename, true)) {
            return _("Error renaming temporary file.");
        }

        lastmodsinceautosave = 0;
        lastsave = wxGetLocalTime();
        auto end_saving_time = wxGetLocalTimeMillis();

        if (!istempfile) {
            undolistsizeatfullsave = undolist.size();
            modified = false;
            tmpsavesuccess = true;
            sys->FileUsed(filename, this);
            if (::wxFileExists(sys->TmpName(filename))) ::wxRemoveFile(sys->TmpName(filename));
        }
        if (sys->autohtmlexport) {
            ExportFile(sys->ExtName(filename, ".html"),
                       sys->autohtmlexport == A_AUTOEXPORT_HTML_WITH_IMAGES - A_AUTOEXPORT_HTML_NONE
                           ? A_EXPHTMLTE
                           : A_EXPHTMLT,
                       false);
        }
        UpdateFileName(page);
        if (success) *success = true;
        return wxString::Format(_("Saved %s successfully (in %lld milliseconds)."), filename,
                                end_saving_time - start_saving_time);
    }

    void DrawSelect(wxDC &dc, Selection &s) {
        if (!s.grid) return;
        ResetFont();
        s.grid->DrawSelect(this, dc, s);
    }

    void UpdateHover(wxReadOnlyDC &dc, int mx, int my) {
        ResetFont();
        int x, y;
        canvas->CalcUnscrolledPosition(mx, my, &x, &y);
        prev = hover;
        hover = Selection();
        auto drawroot = WalkPath(drawpath);
        if (drawroot->grid)
            drawroot->grid->FindXY(
                this, x / currentviewscale - centerx / currentviewscale - hierarchysize,
                y / currentviewscale - centery / currentviewscale - hierarchysize, dc);
    }

    void ScrollIfSelectionOutOfView(const Selection &sel, int sx, int sy, int mx, int my) {
        if (!scaledviewingmode) {
            // required, since sizes of things may have been reset by the last editing operation
            int canvasw, canvash;
            canvas->GetClientSize(&canvasw, &canvash);
            if ((layoutys > canvash || layoutxs > canvasw) && sel.grid) {
                wxRect r = sel.grid->GetRect(this, sel);
                if (r.y < sy || r.y + r.height > my || r.x < sx || r.x + r.width > mx) {
                    canvas->Scroll(r.width > canvasw || r.x < sx ? r.x
                                   : r.x + r.width > mx             ? r.x + r.width - canvasw
                                                                      : sx,
                                   r.height > canvash || r.y < sy ? r.y
                                   : r.y + r.height > my             ? r.y + r.height - canvash
                                                                       : sy);
                }
            }
        }
    }

    void ScrollOrZoom(bool zoomiftiny = false) {
        if (!selected.grid) return;
        auto drawroot = WalkPath(drawpath);
        // If we jumped to a cell which may be insided a folded cell, we have to unfold it
        // because the rest of the code doesn't deal with a selection that is invisible :)
        for (auto cg = selected.grid->cell; cg; cg = cg->parent) {
            // Unless we're under the drawroot, no need to unfold further.
            if (cg == drawroot) break;
            if (cg->grid->folded) {
                cg->grid->folded = false;
                cg->ResetLayout();
                cg->ResetChildren();
            }
        }
        for (auto cg = selected.grid->cell; cg; cg = cg->parent)
            if (cg == drawroot) {
                if (zoomiftiny) ZoomTiny();
                paintscrolltoselection = true;
                canvas->Refresh();
                return;
            }
        Zoom(-100, false);
        if (zoomiftiny) ZoomTiny();
    }

    void ZoomTiny() {
        if (auto c = selected.GetCell(); c && c->tiny) {
            Zoom(1);  // seems to leave selection box in a weird location?
            if (selected.GetCell() != c) ZoomTiny();
        }
    }

    void ResetCursor() {
        if (selected.grid) selected.SetCursorEdit(this, selected.TextEdit());
    }

    void SetSelect(const Selection &sel = Selection()) {
        selected = sel;
        begindrag = sel;
    }

    void SelectUp() {
        if (!isctrlshiftdrag || isctrlshiftdrag == 3 || begindrag.EqLoc(selected)) return;
        auto cell = selected.GetCell();
        if (!cell) return;
        auto targetcell = begindrag.ThinExpand(this);
        selected = begindrag;
        if (targetcell) {
            auto is_parent = targetcell->IsParentOf(cell);
            auto targetcell_parent = targetcell->parent;  // targetcell may be deleted.
            targetcell->Paste(this, cell, begindrag);
            // If is_parent, cell has been deleted already.
            if (isctrlshiftdrag == 1 && !is_parent) {
                cell->parent->AddUndo(this);
                Selection cellselection = cell->parent->grid->FindCell(cell);
                cell->parent->grid->MultiCellDeleteSub(this, cellselection);
            }
            hover = targetcell_parent ? targetcell_parent->grid->FindCell(targetcell) : Selection();
            SetSelect(hover);
            wxInfoDC dc(canvas);
            Layout(dc);
        }
    }

    void DoubleClick() {
        SetSelect(hover);
        if (selected.Thin() && selected.grid) {
            selected.SelAll();
        } else if (Cell *c = selected.GetCell()) {
            selected.EnterEditOnly(this);
            c->text.SelectWord(selected);
            begindrag = selected;
        }
    }

    void Drop() {
        switch (dndobjc->GetReceivedFormat().GetType()) {
            case wxDF_BITMAP: PasteOrDrop(*dndobji); break;
            case wxDF_FILENAME: PasteOrDrop(*dndobjf); break;
            case wxDF_TEXT:
            case wxDF_UNICODETEXT: PasteOrDrop(*dndobjt);
            default:;
        }
    }

    auto CopyEntireCells(wxString &s, int action) {
        sys->clipboardcopy = s;
        auto html =
            selected.grid->ConvertToText(selected, 0, action == A_COPYWI ? A_EXPHTMLTI : A_EXPHTMLT,
                                         this, false, currentdrawroot);
        return new wxHTMLDataObject(html);
    }

    void Copy(int action) {
        auto c = selected.GetCell();
        sys->clipboardcopy = wxEmptyString;

        switch (action) {
            case A_DRAGANDDROP: {
                sys->cellclipboard = c ? c->Clone(nullptr) : selected.grid->CloneSel(selected);
                wxDataObjectComposite dragdata;
                if (c && !c->text.t && c->text.image) {
                    auto image = c->text.image;
                    if (!image->data.empty()) {
                        auto &[it, mime] = imagetypes.at(image->type);
                        auto bitmap = ConvertBufferToWxBitmap(image->data, it);
                        dragdata.Add(new wxBitmapDataObject(bitmap));
                    }
                } else {
                    auto s = selected.grid->ConvertToText(selected, 0, A_EXPTEXT, this, false,
                                                          currentdrawroot);
                    dragdata.Add(new wxTextDataObject(s));
                    if (!selected.TextEdit()) {
                        auto htmlobj = CopyEntireCells(s, wxID_COPY);
                        dragdata.Add(htmlobj);
                    }
                }
                wxDropSource dragsource(dragdata, canvas);
                dragsource.DoDragDrop(true);
                break;
            }
            case A_COPYCT: {
                sys->cellclipboard = nullptr;
                auto clipboardtextdata = new wxDataObjectComposite();
                wxString s = "";
                loopallcellssel(c, true) if (c->text.t.Len()) s += c->text.t + " ";
                if (!selected.TextEdit()) sys->clipboardcopy = s;
                clipboardtextdata->Add(new wxTextDataObject(s));
                if (wxTheClipboard->Open()) {
                    wxTheClipboard->SetData(clipboardtextdata);
                    wxTheClipboard->Close();
                }
                break;
            }
            case wxID_COPY:
            case A_COPYWI:
            default: {
                sys->cellclipboard = c ? c->Clone(nullptr) : selected.grid->CloneSel(selected);
                if (c && !c->text.t && c->text.image) {
                    auto image = c->text.image;
                    if (!image->data.empty() && wxTheClipboard->Open()) {
                        auto &[it, mime] = imagetypes.at(image->type);
                        auto bitmap = ConvertBufferToWxBitmap(image->data, it);
                        wxTheClipboard->SetData(new wxBitmapDataObject(bitmap));
                        wxTheClipboard->Close();
                    }
                } else {
                    auto clipboarddata = new wxDataObjectComposite();
                    auto s = selected.grid->ConvertToText(selected, 0, A_EXPTEXT, this, false,
                                                          currentdrawroot);
                    clipboarddata->Add(new wxTextDataObject(s));
                    if (!selected.TextEdit()) {
                        auto htmlobj = CopyEntireCells(s, action);
                        clipboarddata->Add(htmlobj);
                    }
                    if (wxTheClipboard->Open()) {
                        wxTheClipboard->SetData(clipboarddata);
                        wxTheClipboard->Close();
                    }
                }
                break;
            }
        }
        return;
    }

    bool ZoomSetDrawPath(int dir, bool fromroot = true) {
        int oldlen = drawpath.size();
        int targetlen = max(0, (fromroot ? 0 : oldlen) + dir);
        if (!targetlen && drawpath.empty()) return false;
        if (dir > 0) {
            if (!selected.grid) return false;
            auto c = selected.GetCell();
            CreatePath(c && c->grid ? c : selected.grid->cell, drawpath);
        } else if (dir < 0) {
            auto drawroot = WalkPath(drawpath);
            if (drawroot->grid && drawroot->grid->folded)
                SetSelect(drawroot->parent->grid->FindCell(drawroot));
        }
        int tail = static_cast<int>(drawpath.size()) - targetlen;
        if (tail > 0) drawpath.erase(drawpath.begin(), drawpath.begin() + tail);
        return drawpath.size() != oldlen;
    }

    void Zoom(int dir, bool fromroot = false) {
        if (!ZoomSetDrawPath(dir, fromroot)) return;
        auto drawroot = WalkPath(drawpath);
        if (selected.GetCell() == drawroot && drawroot->grid) {
            // We can't have the drawroot selected, so we must move the selection to the children.
            SetSelect(Selection(drawroot->grid, 0, 0, drawroot->grid->xs, drawroot->grid->ys));
        }
        drawroot->ResetLayout();
        drawroot->ResetChildren();
        paintscrolltoselection = true;
        canvas->Refresh();
    }

    wxString NoSel() { return _("This operation requires a selection."); }
    wxString OneCell() { return _("This operation works on a single selected cell only."); }
    wxString NoThin() { return _("This operation doesn't work on thin selections."); }
    wxString NoGrid() { return _("This operation requires a cell that contains a grid."); }

    wxString Wheel(int dir, bool alt, bool ctrl, bool shift, bool hierarchical = true) {
        if (!dir) return wxEmptyString;
        if (alt) {
            if (!selected.grid) return NoSel();
            if (selected.xs > 0) {
                if (!LastUndoSameCellAny(selected.grid->cell)) selected.grid->cell->AddUndo(this);
                selected.grid->ResizeColWidths(dir, selected, hierarchical);
                selected.grid->cell->ResetLayout();
                selected.grid->cell->ResetChildren();
                paintscrolltoselection = true;
                canvas->Refresh();
                sys->frame->UpdateStatus(selected, false);
                return dir > 0 ? _("Column width increased.") : _("Column width decreased.");
            }
            return _("nothing to resize");
        } else if (shift) {
            if (!selected.grid) return NoSel();
            selected.grid->cell->AddUndo(this);
            selected.grid->ResetChildren();
            selected.grid->RelSize(-dir, selected, pathscalebias);
            paintscrolltoselection = true;
            canvas->Refresh();
            return dir > 0 ? _("Text size increased.") : _("Text size decreased.");
        } else if (ctrl) {
            int steps = abs(dir);
            dir = sign(dir);
            loop(i, steps) Zoom(dir);
            return dir > 0 ? _("Zoomed in.") : _("Zoomed out.");
        } else {
            ASSERT(0);
            return wxEmptyString;
        }
    }

    void Layout(wxReadOnlyDC &dc) {
        ResetFont();
        dc.SetUserScale(1, 1);
        currentdrawroot = WalkPath(drawpath);
        int psb = currentdrawroot == root.get() ? 0 : currentdrawroot->MinRelsize();
        if (psb < 0 || psb == INT_MAX) psb = 0;
        if (psb != pathscalebias) currentdrawroot->ResetChildren();
        pathscalebias = psb;
        currentdrawroot->LazyLayout(this, dc, 0, currentdrawroot->ColWidth(), false);
        ResetFont();
        PickFont(dc, 0, 0, 0);
        hierarchysize = 0;
        for (Cell *p = currentdrawroot->parent; p; p = p->parent)
            if (p->text.t.Len()) hierarchysize += dc.GetCharHeight();
        hierarchysize += fgutter;
        layoutxs = currentdrawroot->sx + hierarchysize + fgutter;
        layoutys = currentdrawroot->sy + hierarchysize + fgutter;
    }

    void ShiftToCenter(wxReadOnlyDC &dc) {
        int dlx = dc.DeviceToLogicalX(0);
        int dly = dc.DeviceToLogicalY(0);
        dc.SetDeviceOrigin(dlx > 0 ? -dlx : centerx, dly > 0 ? -dly : centery);
        dc.SetUserScale(currentviewscale, currentviewscale);
    }

    void Render(wxDC &dc) {
        ResetFont();
        PickFont(dc, 0, 0, 0);
        dc.SetTextForeground(*wxLIGHT_GREY);
        int i = 0;
        for (auto p = currentdrawroot->parent; p; p = p->parent)
            if (p->text.t.Len()) {
                int off = hierarchysize - dc.GetCharHeight() * ++i;
                auto s = p->text.t;
                if (static_cast<int>(s.Len()) > sys->defaultmaxcolwidth) {
                    // should take the width of these into account for layoutys, but really, the
                    // worst that can happen on a thin window is that its rendering gets cut off
                    s = s.Left(sys->defaultmaxcolwidth) + "...";
                }
                dc.DrawText(s, off, off);
            }
        dc.SetTextForeground(LightColor(0x000000));
        currentdrawroot->Render(this, hierarchysize, hierarchysize, dc, 0, 0, 0, 0, 0,
                                currentdrawroot->ColWidth(), 0);
    }

    void SelectClick(bool right = false) {
        begindrag = Selection();
        if (!(right && hover.IsInside(selected))) {
            if (selected.GetCell() == hover.GetCell() && hover.GetCell())
                hover.EnterEditOnly(this);
            else
                hover.ExitEdit(this);
            SetSelect(hover);
        }
    }

    void Draw(wxDC &dc) {
        if (!root) return;
        canvas->GetClientSize(&maxx, &maxy);
        Layout(dc);
        dc.SetBackground(wxBrush(LightColor(Background())));
        dc.Clear();
        double xscale = maxx / static_cast<double>(layoutxs);
        double yscale = maxy / static_cast<double>(layoutys);
        currentviewscale = min(xscale, yscale);
        if (currentviewscale > 5)
            currentviewscale = 5;
        else if (currentviewscale < 1)
            currentviewscale = 1;
        if (scaledviewingmode && currentviewscale > 1) {
            dc.SetUserScale(currentviewscale, currentviewscale);
            canvas->SetVirtualSize(maxx, maxy);
            maxx /= currentviewscale;
            maxy /= currentviewscale;
            scrollx = scrolly = 0;
        } else {
            currentviewscale = 1;
            dc.SetUserScale(1, 1);
            canvas->SetVirtualSize(layoutxs, layoutys);
            canvas->GetViewStart(&scrollx, &scrolly);
            maxx += scrollx;
            maxy += scrolly;
        }
        centerx = sys->centered && !scrollx && maxx > layoutxs
                      ? (maxx - layoutxs) / 2 * currentviewscale
                      : 0;
        centery = sys->centered && !scrolly && maxy > layoutys
                      ? (maxy - layoutys) / 2 * currentviewscale
                      : 0;
        ShiftToCenter(dc);
        Render(dc);
        DrawSelect(dc, selected);
        if (paintscrolltoselection) {
            paintscrolltoselection = false;
                canvas->CallAfter([this, sel = selected, sx = scrollx, sy = scrolly, mx = maxx, my = maxy](){
                    ScrollIfSelectionOutOfView(sel, sx, sy, mx, my);
                    #ifdef __WXMAC__
                        canvas->Refresh();
                    #endif
                });
        }
        if (scaledviewingmode) { dc.SetUserScale(1, 1); }
    }

    void Print(wxDC &dc, wxPrintout &po) {
        Layout(dc);
        maxx = layoutxs;
        maxy = layoutys;
        scrollx = scrolly = 0;
        po.FitThisSizeToPage(printscale ? wxSize(printscale, 1) : wxSize(maxx, maxy));
        wxRect fitRect = po.GetLogicalPageRect();
        wxCoord xoff = (fitRect.width - maxx) / 2;
        wxCoord yoff = (fitRect.height - maxy) / 2;
        po.OffsetLogicalOrigin(xoff, yoff);
        while_printing = true;
        Render(dc);
        while_printing = false;
    }

    int TextSize(int depth, int relsize) {
        return max(g_mintextsize(), g_deftextsize - depth - relsize + pathscalebias);
    }

    bool FontIsMini(int textsize) { return textsize == g_mintextsize(); }

    bool PickFont(wxReadOnlyDC &dc, int depth, int relsize, int stylebits) {
        int textsize = TextSize(depth, relsize);
        if (textsize != lasttextsize || stylebits != laststylebits) {
            wxFont font(textsize - (while_printing || scaledviewingmode),
                        stylebits & STYLE_FIXED ? wxFONTFAMILY_TELETYPE : wxFONTFAMILY_DEFAULT,
                        stylebits & STYLE_ITALIC ? wxFONTSTYLE_ITALIC : wxFONTSTYLE_NORMAL,
                        stylebits & STYLE_BOLD ? wxFONTWEIGHT_BOLD : wxFONTWEIGHT_NORMAL,
                        (stylebits & STYLE_UNDERLINE) != 0,
                        stylebits & STYLE_FIXED ? sys->defaultfixedfont : sys->defaultfont);
            if (stylebits & STYLE_STRIKETHRU) font.SetStrikethrough(true);
            dc.SetFont(font);
            lasttextsize = textsize;
            laststylebits = stylebits;
        }
        return FontIsMini(textsize);
    }

    void ResetFont() {
        lasttextsize = INT_MAX;
        laststylebits = -1;
    }

    bool CheckForChanges() {
        if (modified) {
            ThreeChoiceDialog tcd(sys->frame, filename,
                                  _("Changes have been made, are you sure you wish to continue?"),
                                  _("Save and Close"), _("Discard Changes"), _("Cancel"));
            switch (tcd.Run()) {
                case 0: {
                    bool success = false;
                    Save(false, &success);
                    return !success;
                }
                case 1: return false;
                default:
                case 2: return true;
            }
        }
        return false;
    }

    void RemoveTmpFile() {
        if (!filename.empty() && ::wxFileExists(sys->TmpName(filename)))
            ::wxRemoveFile(sys->TmpName(filename));
    }

    bool CloseDocument() {
        bool keep = CheckForChanges();
        if (!keep) RemoveTmpFile();
        return keep;
    }

    wxString Export(const wxString &fmt, const wxString &pat, const wxString &message, int action) {
        wxFileName tsfn(filename);
        auto exportfilename = ::wxFileSelector(message, tsfn.GetPath(), tsfn.GetName(), fmt, pat,
                                               wxFD_SAVE | wxFD_OVERWRITE_PROMPT | wxFD_CHANGE_DIR);
        if (exportfilename.empty()) return _("Export cancelled.");
        wxFileName expfn(exportfilename);
        if (!expfn.HasExt()) {
            expfn.SetExt(fmt);
            exportfilename = expfn.GetFullPath();
        }
        return ExportFile(exportfilename, action, true);
    }

    wxBitmap GetBitmap() {
        maxx = layoutxs;
        maxy = layoutys;
        scrollx = scrolly = 0;
        wxBitmap bm(maxx, maxy, 24);
        wxMemoryDC mdc(bm);
        DrawView(mdc);
        return bm;
    }

    bool DrawSVG(const wxString &filename) {
        maxx = layoutxs;
        maxy = layoutys;
        scrollx = scrolly = 0;
        wxSVGFileDC sdc(filename, maxx, maxy);
        sdc.SetBitmapHandler(new wxSVGBitmapEmbedHandler());
        DrawView(sdc);
        return sdc.IsOk();
    }

    void DrawView(wxDC &dc) {
        DrawRectangle(dc, Background(), 0, 0, maxx, maxy);
        Layout(dc);
        Render(dc);
    }

    wxBitmap GetSubBitmap(const Selection &sel) {
        wxRect r = sel.grid->GetRect(this, sel, true);
        return GetBitmap().GetSubBitmap(r);
    }

    void RefreshImageRefCount(bool includefolded) {
        loopv(i, sys->imagelist) sys->imagelist[i]->trefc = 0;
        root->ImageRefCount(includefolded);
    }

    wxString ExportFile(const wxString &filename, int action, bool currentview) {
        Cell *exportroot = currentview ? currentdrawroot : root.get();
        if (action == A_EXPIMAGE) {
            auto bitmap = GetBitmap();
            canvas->Refresh();
            if (!bitmap.SaveFile(filename, wxBITMAP_TYPE_PNG)) return _("Error writing PNG file!");
        } else if (action == A_EXPSVG) {
            if (!DrawSVG(filename)) return _("Error exporting to SVG file!");
            canvas->Refresh();
        } else {
            wxFFileOutputStream fos(filename, "w+b");
            if (!fos.IsOk()) {
                wxMessageBox(_("Error exporting file!"), filename.wx_str(), wxOK, sys->frame);
                return _("Error writing to file!");
            }
            wxTextOutputStream dos(fos);
            wxString content = exportroot->ToText(0, Selection(), action, this, true, exportroot);
            switch (action) {
                case A_EXPXML:
                    dos.WriteString(
                        "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n"
                        "<!DOCTYPE cell [\n"
                        "<!ELEMENT cell (grid)>\n"
                        "<!ELEMENT grid (row*)>\n"
                        "<!ELEMENT row (cell*)>\n"
                        "]>\n");
                    dos.WriteString(content);
                    break;
                case A_EXPHTMLT:
                case A_EXPHTMLTI:
                case A_EXPHTMLTE:
                case A_EXPHTMLB:
                case A_EXPHTMLO: {
                    wxString output;
                    output
                        << "<!DOCTYPE html>\n"
                        << "<html>\n<head>\n<style>\n"
                        << "body { font-family: '" << sys->defaultfont << "', sans-serif; }\n"
                        << "table, th, td { border: 1px solid #A0A0A0; border-collapse: collapse;"
                        << " padding: 3px; vertical-align: top; }\n"
                        << "@media (prefers-color-scheme: dark) {\n"
                        << "  html { filter: invert(1); }\n"
                        << "  img { filter: invert(1); }\n"
                        << "}\n"
                        << "li { }\n</style>\n"
                        << "<title>export of TreeSheets file " << this->filename
                        << "</title>\n<meta charset=\"UTF-8\" />\n"
                        << "</head>\n<body style=\""
                        << wxString::Format("background-color: #%06X;", SwapColor(root->cellcolor))
                        << "\">" << content << "</body>\n</html>\n";
                    dos.WriteString(output);
                    break;
                }
                case A_EXPCSV:
                case A_EXPTEXT: dos.WriteString(content); break;
            }
            if (action == A_EXPHTMLTE) ExportAllImages(filename, exportroot);
        }
        return _("File exported successfully.");
    }

    wxString Save(bool saveas, bool *success = nullptr) {
        if (!saveas && !filename.empty()) { return SaveDB(success); }
        auto filename = ::wxFileSelector(_("Choose TreeSheets file to save:"), "", "", "cts",
                                         _("TreeSheets Files (*.cts)|*.cts|All Files (*.*)|*.*"),
                                         wxFD_SAVE | wxFD_OVERWRITE_PROMPT | wxFD_CHANGE_DIR);
        if (filename.empty()) return _("Save cancelled.");  // avoid name being set to ""
        ChangeFileName(filename, true);
        return SaveDB(success);
    }

    void AutoSave(bool minimized, int page) {
        if (sys->autosave && tmpsavesuccess && !filename.empty() && lastmodsinceautosave &&
            (lastmodsinceautosave + 60 < wxGetLocalTime() || lastsave + 300 < wxGetLocalTime() ||
             minimized)) {
            tmpsavesuccess = false;
            SaveDB(&tmpsavesuccess, true, page);
        }
    }

    wxString Key(int uk, int k, bool alt, bool ctrl, bool shift, bool &unprocessed) {
        if (uk == WXK_NONE || k < ' ' && k || k == WXK_DELETE) {
            switch (k) {
                case WXK_BACK:  // no menu shortcut available in wxwidgets
                    if (!ctrl) return Action(A_BACKSPACE);
                    break;  // Prevent Ctrl+H from being treated as Backspace
                case WXK_RETURN:
                    return Action(shift  ? A_ENTERGRID
                                  : ctrl ? A_ENTERCELL_JUMPTOSTART
                                         : A_ENTERCELL);
                case WXK_ESCAPE:  // docs say it can be used as a menu accelerator, but it does not
                                  // trigger from there?
                    return Action(A_CANCELEDIT);
                #ifdef WIN32  // works fine on Linux, not sure OS X
                case WXK_PAGEDOWN: canvas->CursorScroll(0, g_scrollratecursor); return wxEmptyString;
                case WXK_PAGEUP: canvas->CursorScroll(0, -g_scrollratecursor); return wxEmptyString;
                #endif
                #ifdef __WXGTK__
                // Due to limitations within GTK, wxGTK does not support specific keycodes 
                // as accelerator keys for menu items. See wxWidgets documentation for the 
                // wxMenuItem class in order to obtain more details. This is why we implement 
                // the missing handling of these accelerator keys in the following section.
                // Please be aware that the custom implementation has the downside of these
                // "accelerator keys" being suppressed in the menu items on wxGTK.
                    case WXK_DELETE: return Action(A_DELETE);
                    case WXK_LEFT:
                        return Action(shift ? (ctrl ? A_SCLEFT : A_SLEFT)
                                            : (ctrl ? A_MLEFT : A_LEFT));
                    case WXK_RIGHT:
                        return Action(shift ? (ctrl ? A_SCRIGHT : A_SRIGHT)
                                            : (ctrl ? A_MRIGHT : A_RIGHT));
                    case WXK_UP:
                        return Action(shift ? (ctrl ? A_SCUP : A_SUP) : (ctrl ? A_MUP : A_UP));
                    case WXK_DOWN:
                        return Action(shift ? (ctrl ? A_SCDOWN : A_SDOWN)
                                            : (ctrl ? A_MDOWN : A_DOWN));
                    case WXK_HOME:
                        return Action(shift ? (ctrl ? A_SHOME : A_SHOME)
                                            : (ctrl ? A_CHOME : A_HOME));
                    case WXK_END:
                        return Action(shift ? (ctrl ? A_SEND : A_SEND) : (ctrl ? A_CEND : A_END));
                    case WXK_TAB:
                        if (ctrl && !shift) {
                            // WXK_CONTROL_I (italics) arrives as the same keycode as WXK_TAB + ctrl
                            // on Linux?? They're both keycode 9 in defs.h We ignore it here, such
                            // that CTRL+I works, but it means only CTRL+SHIFT+TAB works on Linux as
                            // a way to switch tabs.
                            // Also, even though we ignore CTRL+TAB, and it is not assigned in the
                            // menus, it still has the
                            // effect of de-selecting
                            // the current tab (requires a click to re-activate). FIXME??
                            break;
                        }
                        return Action(shift ? (ctrl ? A_PREVFILE : A_PREV)
                                            : (ctrl ? A_NEXTFILE : A_NEXT));
                    case WXK_PAGEUP:
                        if (ctrl) return Action(alt ? A_INCWIDTHNH : A_ZOOMIN);
                        if (shift) return Action(A_INCSIZE);
                        if (!alt) canvas->CursorScroll(0, -g_scrollratecursor);
                        return wxEmptyString;
                    case WXK_PAGEDOWN:
                        if (ctrl) return Action(alt ? A_DECWIDTHNH : A_ZOOMOUT);
                        if (shift) return Action(A_DECSIZE);
                        if (!alt) canvas->CursorScroll(0, g_scrollratecursor);
                        return wxEmptyString;
                #endif
            }
        } else if (uk >= ' ') {
            if (!selected.grid) return NoSel();
            auto c = selected.ThinExpand(this);
            if (!c) {
                selected.Wrap(this);
                c = selected.GetCell();
            }
            c->AddUndo(this);  // FIXME: not needed for all keystrokes, or at least, merge all
                               // keystroke undos within same cell
            c->text.Key(this, uk, selected);
            paintscrolltoselection = true;
            canvas->Refresh();
            canvas->Update();
            return wxEmptyString;
        }
        unprocessed = true;
        return wxEmptyString;
    }

    wxString Action(int action) {
        switch (action) {
            case wxID_EXECUTE:
                root->AddUndo(this);
                sys->evaluator.Eval(root.get());
                root->ResetChildren();
                selected = Selection();
                begindrag = Selection();
                canvas->Refresh();
                return _("Evaluation finished.");

            case wxID_UNDO:
                if (undolist.size()) {
                    Undo(undolist, redolist);
                    return wxEmptyString;
                } else {
                    return _("Nothing more to undo.");
                }

            case wxID_REDO:
                if (redolist.size()) {
                    Undo(redolist, undolist, true);
                    return wxEmptyString;
                } else {
                    return _("Nothing more to redo.");
                }

            case wxID_SAVE: return Save(false);
            case wxID_SAVEAS: return Save(true);
            case A_SAVEALL: sys->SaveAll(); return wxEmptyString;

            case A_EXPXML: return Export("xml", "*.xml", _("Choose XML file to write"), action);
            case A_EXPHTMLT:
            case A_EXPHTMLTE:
            case A_EXPHTMLB:
            case A_EXPHTMLO:
                return Export("html", "*.html", _("Choose HTML file to write"), action);
            case A_EXPTEXT: return Export("txt", "*.txt", _("Choose Text file to write"), action);
            case A_EXPIMAGE: return Export("png", "*.png", _("Choose PNG file to write"), action);
            case A_EXPSVG: return Export("svg", "*.svg", _("Choose SVG file to write"), action);
            case A_EXPCSV: {
                int maxdepth = 0, leaves = 0;
                currentdrawroot->MaxDepthLeaves(0, maxdepth, leaves);
                if (maxdepth > 1)
                    return _(
                        "Cannot export grid that is not flat (zoom the view to the desired grid, and/or use Flatten).");
                return Export("csv", "*.csv", _("Choose CSV file to write"), action);
            }

            case A_IMPXML:
            case A_IMPXMLA:
            case A_IMPTXTI:
            case A_IMPTXTC:
            case A_IMPTXTS:
            case A_IMPTXTT: {
                wxArrayString filenames;
                GetFilesFromUser(filenames, sys->frame, _("Please select file(s) to import:"),
                                 _("*.*"));
                wxString message = "";
                for (auto &filename : filenames) message = sys->Import(filename, action);
                return message;
            }

            case wxID_OPEN: {
                wxArrayString filenames;
                GetFilesFromUser(filenames, sys->frame,
                                 _("Please select TreeSheets file(s) to load:"),
                                 _("TreeSheets Files (*.cts)|*.cts|All Files (*.*)|*.*"));
                wxString message = "";
                for (auto &filename : filenames) message = sys->Open(filename);
                return message;
            }

            case wxID_CLOSE: {
                if (sys->frame->notebook->GetPageCount() <= 1) {
                    sys->frame->fromclosebox = false;
                    sys->frame->Close();
                    return wxEmptyString;
                }

                if (!CloseDocument()) {
                    int pagenumber = sys->frame->notebook->GetSelection();
                    // sys->frame->notebook->AdvanceSelection();
                    sys->frame->notebook->DeletePage(pagenumber);
                }
                return wxEmptyString;
            }

            case wxID_NEW: {
                int size = static_cast<int>(
                    ::wxGetNumberFromUser(_("What size grid would you like to start with?"),
                                          _("size:"), _("New Sheet"), 10, 1, 25, sys->frame));
                if (size < 0) return _("New file cancelled.");
                sys->InitDB(size);
                sys->frame->GetCurrentTab()->Refresh();
                return wxEmptyString;
            }

            case wxID_ABOUT: {
                wxAboutDialogInfo info;
                info.SetName("TreeSheets");
                info.SetVersion(wxT(PACKAGE_VERSION));
                info.SetCopyright("(C) 2026");
                auto desc = wxString::Format("%s\n\n%s " wxVERSION_STRING,
                                             _("TreeSheets"),
                                             _("Uses"));
                info.SetDescription(desc);
                wxAboutBox(info);
                return wxEmptyString;
            }

            case wxID_HELP: sys->LoadTutorial(); return wxEmptyString;

            case A_HELP_OP_REF: sys->LoadOpRef(); return wxEmptyString;

            case A_TUTORIALWEBPAGE: {
                wxTranslations *trans = wxTranslations::Get();
                wxString lang = trans ? trans->GetBestTranslation("ts") : wxString("");
                wxString tutorialpath;

                for (const wxString &suffix : {"-" + lang, "-" + lang.Left(2), wxString("")}) {
                    if (suffix.Length() == 1) continue;

                    wxString candidate =
                        sys->frame->app->GetDocPath("docs/tutorial" + suffix + ".html");
                    if (::wxFileExists(candidate)) {
                        tutorialpath = candidate;
                        break;
                    }
                }

                if (!tutorialpath.IsEmpty()) {
                    #ifdef __WXMAC__
                        wxLaunchDefaultBrowser("file://" + tutorialpath);
                    #else
                        wxLaunchDefaultBrowser(tutorialpath);
                    #endif
                    return wxEmptyString;
                } else {
                    return _("Tutorial web page could not be found.");
                }
            }

            #ifdef ENABLE_LOBSTER
                case A_SCRIPTREFERENCE:
                    #ifdef __WXMAC__
                    wxLaunchDefaultBrowser(
                        "file://" +
                        sys->frame->app->GetDocPath("docs/script_reference.html"));  // RbrtPntn
                    #else
                    wxLaunchDefaultBrowser(
                        sys->frame->app->GetDocPath("docs/script_reference.html"));
                    #endif
                        return wxEmptyString;
            #endif

            case A_ZOOMIN:
                return Wheel(1, false, true,
                             false);  // Zoom( 1, dc); return "zoomed in (menu)";
            case A_ZOOMOUT:
                return Wheel(-1, false, true,
                             false);  // Zoom(-1, dc); return "zoomed out (menu)";
            case A_INCSIZE: return Wheel(1, false, false, true);
            case A_DECSIZE: return Wheel(-1, false, false, true);
            case A_INCWIDTH: return Wheel(1, true, false, false);
            case A_DECWIDTH: return Wheel(-1, true, false, false);
            case A_INCWIDTHNH: return Wheel(1, true, false, false, false);
            case A_DECWIDTHNH: return Wheel(-1, true, false, false, false);

            case wxID_SELECT_FONT:
            case A_SET_FIXED_FONT: {
                wxFontData fdat;
                fdat.SetInitialFont(wxFont(
                    g_deftextsize,
                    action == wxID_SELECT_FONT ? wxFONTFAMILY_DEFAULT : wxFONTFAMILY_TELETYPE,
                    wxFONTSTYLE_NORMAL, wxFONTWEIGHT_NORMAL, false,
                    action == wxID_SELECT_FONT ? sys->defaultfont : sys->defaultfixedfont));
                if (wxFontDialog fd(sys->frame, fdat); fd.ShowModal() == wxID_OK) {
                    wxFont font = fd.GetFontData().GetChosenFont();
                    g_deftextsize = min(20, max(10, font.GetPointSize()));
                    sys->cfg->Write("defaultfontsize", g_deftextsize);
                    switch (action) {
                        case wxID_SELECT_FONT:
                            sys->defaultfont = font.GetFaceName();
                            sys->cfg->Write("defaultfont", sys->defaultfont);
                            break;
                        case A_SET_FIXED_FONT:
                            sys->defaultfixedfont = font.GetFaceName();
                            sys->cfg->Write("defaultfixedfont", sys->defaultfixedfont);
                            break;
                    }
                    // root->ResetChildren();
                    sys->frame->TabsReset();  // ResetChildren on all
                    canvas->Refresh();
                }
                return wxEmptyString;
            }

            case wxID_PRINT: {
                wxPrintDialogData printDialogData(printData);
                wxPrinter printer(&printDialogData);
                Printout printout(this);
                if (printer.Print(sys->frame, &printout, true)) {
                    printData = printer.GetPrintDialogData().GetPrintData();
                }
                return wxEmptyString;
            }

            case A_PRINTSCALE: {
                printscale = (uint)::wxGetNumberFromUser(
                    _("How many pixels wide should a page be? (0 for auto fit)"), _("scale:"),
                    _("Set Print Scale"), 0, 0, 5000, sys->frame);
                return wxEmptyString;
            }

            case wxID_PREVIEW: {
                wxPrintDialogData printDialogData(printData);
                auto preview =
                    new wxPrintPreview(new Printout(this), new Printout(this), &printDialogData);
                auto pframe = new wxPreviewFrame(preview, sys->frame, _("Print Preview"),
                                                 wxPoint(100, 100), wxSize(600, 650));
                pframe->Centre(wxBOTH);
                pframe->Initialize();
                pframe->Show(true);
                return wxEmptyString;
            }

            case A_PAGESETUP: {
                pageSetupData = printData;
                wxPageSetupDialog pageSetupDialog(sys->frame, &pageSetupData);
                pageSetupDialog.ShowModal();
                printData = pageSetupDialog.GetPageSetupDialogData().GetPrintData();
                pageSetupData = pageSetupDialog.GetPageSetupDialogData();
                return wxEmptyString;
            }

            case A_NEXTFILE: sys->frame->CycleTabs(1); return wxEmptyString;
            case A_PREVFILE: sys->frame->CycleTabs(-1); return wxEmptyString;

            case A_DEFBGCOL: {
                auto oldbg = Background();
                if (auto color = PickColor(sys->frame, oldbg); color != (uint)-1) {
                    root->AddUndo(this);
                    loopallcells(c) {
                        if (c->cellcolor == oldbg && (!c->parent || c->parent->cellcolor == color))
                            c->cellcolor = color;
                    }
                    canvas->Refresh();
                }
                return wxEmptyString;
            }

            case A_DEFCURCOL: {
                if (auto color = PickColor(sys->frame, sys->cursorcolor); color != (uint)-1) {
                    sys->cfg->Write("cursorcolor", sys->cursorcolor = color);
                    canvas->Refresh();
                }
                return wxEmptyString;
            }

            case A_SEARCHNEXT:
            case A_SEARCHPREV: {
                if (sys->searchstring.Len()) return SearchNext(false, true, action == A_SEARCHPREV);
                if (auto c = selected.GetCell()) {
                    auto s = c->text.ToText(0, selected, A_EXPTEXT);
                    if (!s.Len()) return _("No text to search for.");
                    sys->frame->filter->SetFocus();
                    sys->frame->filter->SetValue(s);
                    return wxEmptyString;
                } else {
                    return _("You need to select one cell if you want to search for its text.");
                }
            }

            case A_CASESENSITIVESEARCH: {
                sys->casesensitivesearch = !(sys->casesensitivesearch);
                sys->cfg->Write("casesensitivesearch", sys->casesensitivesearch);
                sys->searchstring = (sys->casesensitivesearch)
                                        ? sys->frame->filter->GetValue()
                                        : sys->frame->filter->GetValue().Lower();
                auto message = SearchNext(false, false, false);
                canvas->Refresh();
                return message;
            }

            case A_ROUND0:
            case A_ROUND1:
            case A_ROUND2:
            case A_ROUND3:
            case A_ROUND4:
            case A_ROUND5:
            case A_ROUND6:
                sys->cfg->Write("roundness", long(sys->roundness = action - A_ROUND0));
                canvas->Refresh();
                return wxEmptyString;

            case A_OPENCELLCOLOR:
                if (sys->frame->cellcolordropdown) sys->frame->cellcolordropdown->ShowPopup();
                break;
            case A_OPENTEXTCOLOR:
                if (sys->frame->textcolordropdown) sys->frame->textcolordropdown->ShowPopup();
                break;
            case A_OPENBORDCOLOR:
                if (sys->frame->bordercolordropdown) sys->frame->bordercolordropdown->ShowPopup();
                break;
            case A_OPENIMGDROPDOWN:
                if (sys->frame->imagedropdown) sys->frame->imagedropdown->ShowPopup();
                break;

            case A_REPLACEONCE:
            case A_REPLACEONCEJ:
            case A_REPLACEALL: {
                if (!sys->searchstring.Len()) return _("No search.");
                auto replaces = sys->frame->replaces->GetValue();
                auto lreplaces =
                    sys->casesensitivesearch ? (wxString)wxEmptyString : replaces.Lower();
                if (action == A_REPLACEALL) {
                    root->AddUndo(this);  // expensive?
                    root->FindReplaceAll(replaces, lreplaces);
                    root->ResetChildren();
                    canvas->Refresh();
                } else {
                    loopallcellssel(c, true) if (c->text.IsInSearch()) c->AddUndo(this);
                    selected.grid->ReplaceStr(this, replaces, lreplaces, selected);
                    if (action == A_REPLACEONCEJ) return SearchNext(false, true, false);
                }
                return _("Text has been replaced.");
            }

            case A_CLEARREPLACE: {
                sys->frame->replaces->Clear();
                canvas->SetFocus();
                return wxEmptyString;
            }

            case A_CLEARSEARCH: {
                sys->frame->filter->Clear();
                canvas->SetFocus();
                return wxEmptyString;
            }

            case A_SCALED:
                scaledviewingmode = !scaledviewingmode;
                root->ResetChildren();
                canvas->Refresh();
                return scaledviewingmode ? _("Now viewing TreeSheet to fit to the screen exactly, press F12 to return to normal.")
                                         : _("1:1 scale restored.");

            case A_FILTERRANGE: {
                DateTimeRangeDialog rd(sys->frame);
                if (rd.Run() == wxID_OK) ApplyEditRangeFilter(rd.begin, rd.end);
                return wxEmptyString;
            }

            case A_FILTER5:
                editfilter = 5;
                ApplyEditFilter();
                return wxEmptyString;
            case A_FILTER10:
                editfilter = 10;
                ApplyEditFilter();
                return wxEmptyString;
            case A_FILTER20:
                editfilter = 20;
                ApplyEditFilter();
                return wxEmptyString;
            case A_FILTER50:
                editfilter = 50;
                ApplyEditFilter();
                return wxEmptyString;
            case A_FILTERM:
                editfilter++;
                ApplyEditFilter();
                return wxEmptyString;
            case A_FILTERL:
                editfilter--;
                ApplyEditFilter();
                return wxEmptyString;
            case A_FILTERS: SetSearchFilter(true); return wxEmptyString;
            case A_FILTEROFF: SetSearchFilter(false); return wxEmptyString;

            case A_CUSTKEY: {
                wxArrayString strs, keys;
                for (auto &[s, k] : sys->frame->menustrings) {
                    strs.push_back(s);
                    keys.push_back(k);
                }
                wxSingleChoiceDialog choice(
                    sys->frame, _("Please pick a menu item to change the key binding for"),
                    _("Key binding"), strs);
                choice.SetSize(wxSize(500, 700));
                choice.Centre();
                if (choice.ShowModal() == wxID_OK) {
                    int sel = choice.GetSelection();
                    wxTextEntryDialog textentry(sys->frame,
                                                _("Please enter the new key binding string"),
                                                _("Key binding"), keys[sel]);
                    if (textentry.ShowModal() == wxID_OK) {
                        auto key = textentry.GetValue();
                        sys->frame->menustrings[strs[sel]] = key;
                        sys->cfg->Write(strs[sel], key);
                        return _("NOTE: key binding will take effect next run of TreeSheets.");
                    }
                }
                return _("Keybinding cancelled.");
            }

            case A_SETLANG: {
                auto trans = wxTranslations::Get();
                if (!trans) return _("Failed to get translation.");
                wxArrayString langs = trans->GetAvailableTranslations("ts");
                langs.Insert(wxEmptyString, 0);
                wxSingleChoiceDialog choice(
                    sys->frame, _("Please select the language for the interface (requires restart). Please select the empty row if you want to use the default language."),
                    _("Available languages"), langs);
                if (choice.ShowModal() == wxID_OK) {
                    sys->cfg->Write("defaultlang", choice.GetStringSelection());
                }
                return wxEmptyString;
            }
        }

        if (!selected.grid) return NoSel();

        auto cell = selected.GetCell();

        switch (action) {
            case A_BACKSPACE:
                if (selected.Thin()) {
                    if (selected.xs)
                        DelRowCol(selected.y, 0, selected.grid->ys, 1, -1, selected.y - 1, 0, -1);
                    else
                        DelRowCol(selected.x, 0, selected.grid->xs, 1, selected.x - 1, -1, -1, 0);
                } else if (cell && selected.TextEdit()) {
                    if (selected.cursorend == 0) return wxEmptyString;
                    cell->AddUndo(this);
                    cell->text.Backspace(selected);
                    canvas->Refresh();
                } else {
                    selected.grid->MultiCellDelete(this, selected);
                    SetSelect(selected);
                }
                ZoomOutIfNoGrid();
                return wxEmptyString;

            case A_DELETE:
                if (selected.Thin()) {
                    if (selected.xs)
                        DelRowCol(selected.y, selected.grid->ys, selected.grid->ys, 0, -1,
                                  selected.y, 0, -1);
                    else
                        DelRowCol(selected.x, selected.grid->xs, selected.grid->xs, 0, selected.x,
                                  -1, -1, 0);
                } else if (cell && selected.TextEdit()) {
                    if (selected.cursor == cell->text.t.Len()) return wxEmptyString;
                    cell->AddUndo(this);
                    cell->text.Delete(selected);
                    canvas->Refresh();
                } else {
                    selected.grid->MultiCellDelete(this, selected);
                    SetSelect(selected);
                }
                ZoomOutIfNoGrid();
                return wxEmptyString;

            case A_DELETE_WORD:
                if (cell && selected.TextEdit()) {
                    if (selected.cursor == cell->text.t.Len()) return wxEmptyString;
                    cell->AddUndo(this);
                    cell->text.DeleteWord(selected);
                    canvas->Refresh();
                }
                ZoomOutIfNoGrid();
                return wxEmptyString;

            case wxID_CUT:
            case wxID_COPY:
            case A_COPYWI:
            case A_COPYCT:
                if (selected.Thin()) return NoThin();
                if (selected.TextEdit()) {
                    if (selected.cursor == selected.cursorend) return _("No text selected.");
                }
                Copy(action);
                if (action == wxID_CUT) {
                    if (!selected.TextEdit()) {
                        selected.grid->cell->AddUndo(this);
                        selected.grid->MultiCellDelete(this, selected);
                        SetSelect(selected);
                    } else if (cell) {
                        cell->AddUndo(this);
                        cell->text.Backspace(selected);
                    }
                    canvas->Refresh();
                }
                ZoomOutIfNoGrid();
                return wxEmptyString;

            case A_COPYBM:
                if (wxTheClipboard->Open()) {
                    wxTheClipboard->SetData(new wxBitmapDataObject(GetSubBitmap(selected)));
                    wxTheClipboard->Close();
                    return _("Bitmap copied to clipboard");
                }
                return wxEmptyString;

            case A_COLLAPSE: {
                if (selected.xs * selected.ys == 1)
                    return _("More than one cell must be selected.");
                auto fc = selected.GetFirst();
                wxString ct = "";
                loopallcellssel(ci, true) if (ci != fc && ci->text.t.Len()) ct += " " + ci->text.t;
                if (!fc->HasContent() && !ct.Len()) return _("There is no content to collapse.");
                fc->parent->AddUndo(this);
                fc->text.t += ct;
                loopallcellssel(ci, false) if (ci != fc) ci->Clear();
                Selection deletesel(selected.grid,
                                    selected.x + int(selected.xs > 1),  // sidestep is possible?
                                    selected.y + int(selected.ys > 1),
                                    selected.xs - int(selected.xs > 1),
                                    selected.ys - int(selected.ys > 1));
                selected.grid->MultiCellDeleteSub(this, deletesel);
                SetSelect(Selection(selected.grid, selected.x, selected.y, 1, 1));
                fc->ResetLayout();
                canvas->Refresh();
                return wxEmptyString;
            }

            case wxID_SELECTALL:
                selected.SelAll();
                canvas->Refresh();
                sys->frame->UpdateStatus(selected, true);
                return wxEmptyString;

            case A_UP:
            case A_DOWN:
            case A_LEFT:
            case A_RIGHT: selected.Cursor(this, action, false, false); return wxEmptyString;

            case A_MUP:
            case A_MDOWN:
            case A_MLEFT:
            case A_MRIGHT:
                selected.Cursor(this, action - A_MUP + A_UP, true, false);
                return wxEmptyString;

            case A_SUP:
            case A_SDOWN:
            case A_SLEFT:
            case A_SRIGHT:
                selected.Cursor(this, action - A_SUP + A_UP, false, true);
                return wxEmptyString;

            case A_SCLEFT:
            case A_SCRIGHT:
            case A_SCUP:
            case A_SCDOWN:
                if (!selected.TextEdit()) {
                    bool horiz = (action == A_SCLEFT || action == A_SCRIGHT);
                    bool ismin = (action == A_SCLEFT || action == A_SCUP);

                    int &pos = horiz ? selected.x : selected.y;
                    int &ext = horiz ? selected.xs : selected.ys;
                    int gridmax = horiz ? selected.grid->xs : selected.grid->ys;

                    if (ismin) {
                        ext = pos + (selected.Thin() ? 0 : 1);
                        pos = 0;
                    } else {
                        ext = gridmax - pos;
                    }

                    sys->frame->UpdateStatus(selected, true);
                    canvas->Refresh();
                } else if (action == A_SCLEFT || action == A_SCRIGHT) {
                    selected.Cursor(this, action - A_SCUP + A_UP, true, true);
                }
                return wxEmptyString;

            case wxID_BOLD:
                selected.grid->SetStyle(this, selected, STYLE_BOLD);
                return wxEmptyString;
            case wxID_ITALIC:
                selected.grid->SetStyle(this, selected, STYLE_ITALIC);
                return wxEmptyString;
            case A_TT: selected.grid->SetStyle(this, selected, STYLE_FIXED); return wxEmptyString;
            case wxID_UNDERLINE:
                selected.grid->SetStyle(this, selected, STYLE_UNDERLINE);
                return wxEmptyString;
            case wxID_STRIKETHROUGH:
                selected.grid->SetStyle(this, selected, STYLE_STRIKETHRU);
                return wxEmptyString;

            case A_MARKDATA:
            case A_MARKVARD:
            case A_MARKVARU:
            case A_MARKVIEWH:
            case A_MARKVIEWV:
            case A_MARKCODE: {
                int newcelltype;
                switch (action) {
                    case A_MARKDATA: newcelltype = CT_DATA; break;
                    case A_MARKVARD: newcelltype = CT_VARD; break;
                    case A_MARKVARU: newcelltype = CT_VARU; break;
                    case A_MARKVIEWH: newcelltype = CT_VIEWH; break;
                    case A_MARKVIEWV: newcelltype = CT_VIEWV; break;
                    case A_MARKCODE: newcelltype = CT_CODE; break;
                }
                selected.grid->cell->AddUndo(this);
                loopallcellssel(c, false) {
                    c->celltype = (newcelltype == CT_CODE) ? sys->evaluator.InferCellType(c->text)
                                                           : newcelltype;
                    canvas->Refresh();
                }
                return wxEmptyString;
            }

            case A_CANCELEDIT:
                if (selected.TextEdit()) break;
                if (selected.grid->cell->parent) {
                    SetSelect(selected.grid->cell->parent->grid->FindCell(selected.grid->cell));
                } else {
                    selected.SelAll();
                }
                ScrollOrZoom();
                return wxEmptyString;

            case A_NEWGRID:
                if (!(cell = selected.ThinExpand(this))) return OneCell();
                if (cell->grid) {
                    SetSelect(Selection(cell->grid, 0, cell->grid->ys, 1, 0));
                    ScrollOrZoom(true);
                } else {
                    cell->AddUndo(this);
                    cell->AddGrid();
                    SetSelect(Selection(cell->grid, 0, 0, 1, 1));
                    paintscrolltoselection = true;
                    canvas->Refresh();
                }
                return wxEmptyString;

            case wxID_PASTE:
                if (!(cell = selected.ThinExpand(this))) return OneCell();
                if (wxTheClipboard->Open()) {
                    if (wxTheClipboard->IsSupported(wxDF_BITMAP)) {
                        wxBitmapDataObject bdo;
                        wxTheClipboard->GetData(bdo);
                        PasteOrDrop(bdo);
                    } else if (wxTheClipboard->IsSupported(wxDF_FILENAME)) {
                        wxFileDataObject fdo;
                        wxTheClipboard->GetData(fdo);
                        PasteOrDrop(fdo);
                    } else if (wxTheClipboard->IsSupported(wxDF_TEXT) ||
                               wxTheClipboard->IsSupported(wxDF_UNICODETEXT)) {
                        wxTextDataObject tdo;
                        wxTheClipboard->GetData(tdo);
                        PasteOrDrop(tdo);
                    }
                    wxTheClipboard->Close();
                    canvas->Refresh();
                } else if (sys->cellclipboard) {
                    cell->Paste(this, sys->cellclipboard.get(), selected);
                    canvas->Refresh();
                }
                return wxEmptyString;

            case A_EDITNOTE: {
                if (!(cell = selected.ThinExpand(this))) return OneCell();

                wxDialog dlg(sys->frame, wxID_ANY, _("Note"), wxDefaultPosition,
                             wxSize(sys->notesizex, sys->notesizey),
                             wxDEFAULT_DIALOG_STYLE | wxRESIZE_BORDER);
                auto *sizer = new wxBoxSizer(wxVERTICAL);
                wxTextCtrl text(&dlg, wxID_ANY, cell->note, wxDefaultPosition, wxDefaultSize,
                                wxTE_MULTILINE);
                sizer->Add(&text, 1, wxEXPAND | wxALL, 10);
                auto *btns = dlg.CreateButtonSizer(wxOK | wxCANCEL);
                sizer->Add(btns, 0, wxALIGN_CENTER | wxBOTTOM, 10);
                dlg.SetSizer(sizer);

                text.SetFocus();

                if (dlg.ShowModal() == wxID_OK) {
                    if (cell->note != text.GetValue()) {
                        cell->AddUndo(this);
                        cell->note = text.GetValue();
                        canvas->Refresh();
                    }
                    sys->notesizex = dlg.GetSize().x;
                    sys->notesizey = dlg.GetSize().y;
                }
                return wxEmptyString;
            }

            case A_PASTESTYLE:
                if (!sys->cellclipboard) return _("No style to paste.");
                selected.grid->cell->AddUndo(this);
                selected.grid->SetStyles(selected, sys->cellclipboard.get());
                selected.grid->cell->ResetChildren();
                canvas->Refresh();
                return wxEmptyString;

            case A_ENTERCELL:
            case A_ENTERCELL_JUMPTOEND:
            case A_ENTERCELL_JUMPTOSTART:
            case A_PROGRESSCELL: {
                if (!(cell = selected.ThinExpand(this, action == A_ENTERCELL_JUMPTOSTART)))
                    return OneCell();
                if (selected.TextEdit()) {
                    selected.Cursor(this, action == A_PROGRESSCELL ? A_RIGHT : A_DOWN, false, false,
                                    true);
                } else {
                    selected.EnterEdit(
                        this,
                        action == A_ENTERCELL_JUMPTOEND ? static_cast<int>(cell->text.t.Len()) : 0,
                        static_cast<int>(cell->text.t.Len()));
                    paintscrolltoselection = true;
                    canvas->Refresh();
                }
                return wxEmptyString;
            }

            case A_IMAGE: {
                if (!(cell = selected.ThinExpand(this))) return OneCell();
                auto filename =
                    ::wxFileSelector(_("Please select an image file:"), "", "", "", "*.*",
                                     wxFD_OPEN | wxFD_FILE_MUST_EXIST | wxFD_CHANGE_DIR);
                cell->AddUndo(this);
                LoadImageIntoCell(filename, cell, sys->frame->FromDIP(1.0));
                canvas->Refresh();
                return wxEmptyString;
            }

            case A_IMAGER: {
                selected.grid->cell->AddUndo(this);
                selected.grid->ClearImages(selected);
                selected.grid->cell->ResetChildren();
                canvas->Refresh();
                return wxEmptyString;
            }

            case A_SORTD: return Sort(true);
            case A_SORT: return Sort(false);

            case A_SCOLS:
                selected.y = 0;
                selected.ys = selected.grid->ys;
                canvas->Refresh();
                return wxEmptyString;

            case A_SROWS:
                selected.x = 0;
                selected.xs = selected.grid->xs;
                canvas->Refresh();
                return wxEmptyString;

            case A_BORD0:
            case A_BORD1:
            case A_BORD2:
            case A_BORD3:
            case A_BORD4:
            case A_BORD5:
                selected.grid->cell->AddUndo(this);
                selected.grid->SetBorder(action - A_BORD0 + 1);
                selected.grid->cell->ResetChildren();
                canvas->Refresh();
                return wxEmptyString;

            case A_TEXTGRID: return layrender(-1, true, true);

            case A_V_GS: return layrender(DS_GRID, true);
            case A_V_BS: return layrender(DS_BLOBSHIER, true);
            case A_V_LS: return layrender(DS_BLOBLINE, true);
            case A_H_GS: return layrender(DS_GRID, false);
            case A_H_BS: return layrender(DS_BLOBSHIER, false);
            case A_H_LS: return layrender(DS_BLOBLINE, false);
            case A_GS: return layrender(DS_GRID, true, false, true);
            case A_BS: return layrender(DS_BLOBSHIER, true, false, true);
            case A_LS: return layrender(DS_BLOBLINE, true, false, true);

            case A_WRAP: return selected.Wrap(this);

            case A_RESETSIZE:
            case A_RESETWIDTH:
            case A_RESETSTYLE:
            case A_RESETCOLOR:
            case A_LASTCELLCOLOR:
            case A_LASTTEXTCOLOR:
            case A_LASTBORDCOLOR:
            case A_LASTIMAGE:
                selected.grid->cell->AddUndo(this);
                loopallcellssel(c, true) switch (action) {
                    case A_RESETSIZE: c->text.relsize = 0; break;
                    case A_RESETWIDTH:
                        for (int x = selected.x; x < selected.x + selected.xs; x++)
                            selected.grid->colwidths[x] = sys->defaultmaxcolwidth;
                        selected.grid->cell->ResetLayout();
                        break;
                    case A_RESETSTYLE: c->text.stylebits = 0; break;
                    case A_RESETCOLOR:
                        if (c->IsTag(this)) {
                            tags[c->text.t] = g_tagcolor_default;
                        } else {
                            c->textcolor = g_textcolor_default;
                        }
                        c->cellcolor = g_cellcolor_default;
                        if (c->grid) c->grid->bordercolor = g_bordercolor_default;
                        break;
                    case A_LASTCELLCOLOR: c->cellcolor = sys->lastcellcolor; break;
                    case A_LASTTEXTCOLOR: c->textcolor = sys->lasttextcolor; break;
                    case A_LASTBORDCOLOR:
                        if (c->parent && c->parent->grid)
                            c->parent->grid->bordercolor = sys->lastbordcolor;
                        break;
                    case A_LASTIMAGE:
                        if (sys->lastimage) c->text.image = sys->lastimage;
                        break;
                }
                selected.grid->cell->ResetChildren();
                canvas->Refresh();
                sys->frame->UpdateStatus(selected, false);
                return wxEmptyString;

            case A_MINISIZE: {
                selected.grid->cell->AddUndo(this);
                CollectCellsSel(false);
                vector<Cell *> outer;
                outer.insert(outer.end(), itercells.begin(), itercells.end());
                for (auto o : outer) {
                    if (o->grid) {
                        loopcellsin(o, c) if (_i) {
                            c->text.relsize = g_deftextsize - g_mintextsize() - c->Depth();
                        }
                    }
                }
                outer.clear();
                selected.grid->cell->ResetChildren();
                canvas->Refresh();
                return wxEmptyString;
            }

            case A_FOLD:
            case A_FOLDALL:
            case A_UNFOLDALL:
                loopallcellssel(c, action != A_FOLD) if (c->grid) {
                    c->AddUndo(this);
                    c->grid->folded = action == A_FOLD ? !c->grid->folded : action == A_FOLDALL;
                    c->ResetChildren();
                }
                canvas->Refresh();
                return wxEmptyString;

            case A_HOME:
            case A_END:
            case A_CHOME:
            case A_CEND:
                if (selected.TextEdit()) break;
                selected.HomeEnd(this, action == A_HOME || action == A_CHOME);
                return wxEmptyString;

            case A_IMAGESCP:
            case A_IMAGESCW:
            case A_IMAGESCF: {
                std::set<Image *> imagestomanipulate;
                long v = 0.0;
                loopallcellssel(c, true) {
                    if (c->text.image) { imagestomanipulate.insert(c->text.image); }
                }
                if (imagestomanipulate.empty()) return wxEmptyString;
                if (action == A_IMAGESCW) {
                    v = wxGetNumberFromUser(_("Please enter the new image width:"), _("Width"),
                                            _("Image Resize"), 500, 10, 4000, sys->frame);
                } else {
                    v = wxGetNumberFromUser(
                        _("Please enter the percentage you want the image scaled by:"), "%",
                        _("Image Resize"), 50, 5, 400, sys->frame);
                }
                if (v < 0) return wxEmptyString;
                for (auto image : imagestomanipulate) {
                    if (action == A_IMAGESCW) {
                        int pw = image->pixel_width;
                        if (pw)
                            image->ImageRescale(static_cast<double>(v) / static_cast<double>(pw));
                    } else if (action == A_IMAGESCP) {
                        image->ImageRescale(v / 100.0);
                    } else {
                        image->DisplayScale(v / 100.0);
                    }
                }
                currentdrawroot->ResetChildren();
                currentdrawroot->ResetLayout();
                canvas->Refresh();
                return wxEmptyString;
            }

            case A_IMAGESCN: {
                loopallcellssel(c, true) if (c->text.image) {
                    c->text.image->ResetScale(sys->frame->FromDIP(1.0));
                }
                currentdrawroot->ResetChildren();
                currentdrawroot->ResetLayout();
                canvas->Refresh();
                return wxEmptyString;
            }

            case A_IMAGESVA: {
                set<Image *> imagestosave;
                loopallcellssel(c, true) if (auto image = c->text.image)
                    imagestosave.insert(image);
                if (imagestosave.empty()) return _("There are no images in the selection.");
                wxString filename = ::wxFileSelector(
                    _("Choose image file to save:"), "", "", "",
                    _("PNG file (*.png)|*.png|JPEG file (*.jpg)|*.jpg|All Files (*.*)|*.*"),
                    wxFD_SAVE | wxFD_OVERWRITE_PROMPT | wxFD_CHANGE_DIR);
                if (filename.empty()) return _("Save cancelled.");
                auto i = 0;
                for (auto image : imagestosave) {
                    wxFileName fn(filename);
                    wxString finalfilename = fn.GetPathWithSep() + fn.GetName() +
                                             (i == 0 ? wxString() : wxString::Format("%d", i)) +
                                             image->GetFileExtension();
                    wxFFileOutputStream os(finalfilename, "w+b");
                    if (!os.IsOk()) {
                        wxMessageBox(
                            _("Error writing image file! (try saving under new filename)."),
                            finalfilename.wx_str(), wxOK, sys->frame);
                        return _("Error writing to file.");
                    }
                    os.Write(image->data.data(), image->data.size());
                    i++;
                }
                return _("Image(s) have been saved to disk.");
            }

            case A_SAVE_AS_JPEG:
            case A_SAVE_AS_PNG: {
                wxString returnmessage = _("No image found to convert");
                loopallcellssel(c, true) {
                    auto image = c->text.image;
                    if (action == A_SAVE_AS_JPEG && image && image->type == 'I') {
                        auto transferimage = ConvertBufferToWxImage(image->data, wxBITMAP_TYPE_PNG);
                        image->data = ConvertWxImageToBuffer(transferimage, wxBITMAP_TYPE_JPEG);
                        image->type = 'J';
                        returnmessage =
                            _("Images in selected cells have been converted to JPEG format.");
                    }
                    if (action == A_SAVE_AS_PNG && image && image->type == 'J') {
                        auto transferimage =
                            ConvertBufferToWxImage(image->data, wxBITMAP_TYPE_JPEG);
                        image->data = ConvertWxImageToBuffer(transferimage, wxBITMAP_TYPE_PNG);
                        image->type = 'I';
                        returnmessage =
                            _("Images in selected cells have been converted to PNG format.");
                    }
                }
                return returnmessage;
            }

            case A_BROWSE: {
                wxString returnmessage = "";
                int counter = 0;
                loopallcellssel(c, false) {
                    if (counter >= g_max_launches) {
                        returnmessage = _("Maximum number of launches reached.");
                        break;
                    }
                    if (!wxLaunchDefaultBrowser(c->text.ToText(0, selected, A_EXPTEXT))) {
                        returnmessage = _("The browser could not open at least one link.");
                    } else {
                        counter++;
                    }
                }
                return returnmessage;
            }

            case A_BROWSEF: {
                wxString returnmessage = "";
                int counter = 0;
                loopallcellssel(c, false) {
                    if (counter >= g_max_launches) {
                        returnmessage = _("Maximum number of launches reached.");
                        break;
                    }
                    auto f = c->text.ToText(0, selected, A_EXPTEXT);
                    wxFileName fn(f);
                    if (fn.IsRelative()) fn.MakeAbsolute(wxFileName(filename).GetPath());
                    if (!wxLaunchDefaultApplication(fn.GetFullPath())) {
                        returnmessage = _("At least one file could not be opened.");
                    } else {
                        counter++;
                    }
                }
                return returnmessage;
            }

            case A_TAGADD: {
                loopallcellssel(c, false) {
                    if (!c->text.t.Len()) continue;
                    tags[c->text.t] = g_tagcolor_default;
                }
                canvas->Refresh();
                return wxEmptyString;
            }

            case A_TAGREMOVE: {
                loopallcellssel(c, false) tags.erase(c->text.t);
                canvas->Refresh();
                return wxEmptyString;
            }

            case A_TRANSPOSE: {
                if (selected.Thin()) return NoThin();
                auto ac = selected.grid->cell;
                ac->AddUndo(this);
                if (selected.IsAll()) {
                    ac->grid->Transpose();
                    SetSelect(ac->parent ? ac->parent->grid->FindCell(ac) : Selection());
                } else {
                    loopallcellssel(c, false) if (c->grid) c->grid->Transpose();
                }
                ac->ResetChildren();
                canvas->Refresh();
                return wxEmptyString;
            }
        }

        if (cell || (!cell && selected.IsAll())) {
            auto ac = cell ? cell : selected.grid->cell;
            switch (action) {
                case A_HIFY:
                    if (!ac->grid) return NoGrid();
                    if (!ac->grid->IsTable())
                        return _(
                            "Selected grid is not a table: cells must not already have sub-grids.");
                    ac->AddUndo(this);
                    ac->grid->Hierarchify(this);
                    ac->ResetChildren();
                    selected = Selection();
                    begindrag = Selection();
                    canvas->Refresh();
                    return wxEmptyString;

                case A_FLATTEN: {
                    if (!ac->grid) return NoGrid();
                    ac->AddUndo(this);
                    int maxdepth = 0, leaves = 0;
                    ac->MaxDepthLeaves(0, maxdepth, leaves);
                    auto g = new Grid(maxdepth, leaves);
                    g->InitCells();
                    ac->grid->Flatten(0, 0, g);
                    DELETEP(ac->grid);
                    ac->grid = g;
                    g->ReParent(ac);
                    ac->ResetChildren();
                    selected = Selection();
                    begindrag = Selection();
                    canvas->Refresh();
                    return wxEmptyString;
                }
            }
        }

        if (!cell) return OneCell();

        switch (action) {
            case A_NEXT: selected.Next(this, false); return wxEmptyString;
            case A_PREV: selected.Next(this, true); return wxEmptyString;

            case A_ENTERGRID:
                if (!cell->grid) Action(A_NEWGRID);
                SetSelect(Selection(cell->grid, 0, 0, 1, 1));
                ScrollOrZoom(true);
                return wxEmptyString;

            case A_LINK:
            case A_LINKIMG:
            case A_LINKREV:
            case A_LINKIMGREV: {
                if ((action == A_LINK || action == A_LINKREV) && !cell->text.t.Len())
                    return _("No text in this cell.");
                if ((action == A_LINKIMG || action == A_LINKIMGREV) && !cell->text.image)
                    return _("No image in this cell.");
                bool t1 = false, t2 = false;
                auto link = root->FindLink(selected, cell, nullptr, t1, t2,
                                           action == A_LINK || action == A_LINKIMG,
                                           action == A_LINKIMG || action == A_LINKIMGREV);
                if (!link || !link->parent) return _("No matching cell found!");
                SetSelect(link->parent->grid->FindCell(link));
                ScrollOrZoom(true);
                return wxEmptyString;
            }

            case A_COLCELL: sys->customcolor = cell->cellcolor; return wxEmptyString;

            case A_HSWAP: {
                auto pp = cell->parent->parent;
                if (!pp) return _("Cannot move this cell up in the hierarchy.");
                if (pp->grid->xs != 1 && pp->grid->ys != 1)
                    return _("Can only move this cell into a Nx1 or 1xN grid.");
                if (cell->parent->grid->xs != 1 && cell->parent->grid->ys != 1)
                    return _("Can only move this cell from a Nx1 or 1xN grid.");
                pp->AddUndo(this);
                SetSelect(pp->grid->HierarchySwap(cell->text.t));
                pp->ResetChildren();
                pp->ResetLayout();
                canvas->Refresh();
                return wxEmptyString;
            }

            case A_FILTERBYCELLBG:
                loopallcells(ci) ci->text.filtered = ci->cellcolor != cell->cellcolor;
                root->ResetChildren();
                canvas->Refresh();
                return wxEmptyString;

            case A_FILTERNOTE:
                loopallcells(ci) ci->text.filtered = ci->note.IsEmpty();
                root->ResetChildren();
                canvas->Refresh();
                return wxEmptyString;

            case A_FILTERMATCHNEXT:
                bool lastsel = true;
                Cell *next = root->FindNextFilterMatch(nullptr, selected.GetCell(), lastsel);
                if (!next) return _("No matches for filter.");
                if (next->parent) SetSelect(next->parent->grid->FindCell(next));
                canvas->SetFocus();
                ScrollOrZoom(true);
                return wxEmptyString;
        }

        if (!selected.TextEdit()) return _("only works in cell text mode");

        switch (action) {
            case A_CANCELEDIT:
                if (LastUndoSameCellTextEdit(cell))
                    Undo(undolist, redolist);
                else
                    canvas->Refresh();
                selected.ExitEdit(this);
                return wxEmptyString;

            case A_BACKSPACE_WORD:
                if (selected.cursorend == 0) return wxEmptyString;
                cell->AddUndo(this);
                cell->text.BackspaceWord(selected);
                canvas->Refresh();
                ZoomOutIfNoGrid();
                return wxEmptyString;

            case A_SHOME:
            case A_SEND:
            case A_CHOME:
            case A_CEND:
            case A_HOME:
            case A_END: {
                switch (action) {
                    case A_SHOME:  // FIXME: this functionality is really SCHOME, SHOME should be
                                   // within line
                        selected.cursor = 0;
                        break;
                    case A_SEND: selected.cursorend = static_cast<int>(cell->text.t.Len()); break;
                    case A_CHOME: selected.cursor = selected.cursorend = 0; break;
                    case A_CEND: selected.cursor = selected.cursorend = selected.MaxCursor(); break;
                    case A_HOME: cell->text.HomeEnd(selected, true); break;
                    case A_END: cell->text.HomeEnd(selected, false); break;
                }
                paintscrolltoselection = true;
                canvas->Refresh();
                return wxEmptyString;
            }
            default: return _("Internal error: unimplemented operation!");
        }
    }

    wxString SearchNext(bool focusmatch, bool jump, bool reverse) {
        if (!root) return wxEmptyString;  // fix crash when opening new doc
        if (!sys->searchstring.Len()) return _("No search string.");
        bool lastsel = true;
        Cell *next = root->FindNextSearchMatch(sys->searchstring, nullptr, selected.GetCell(),
                                               lastsel, reverse);
        if (!next) return _("No matches for search.");
        if (!jump) return wxEmptyString;
        SetSelect(next->parent->grid->FindCell(next));
        if (focusmatch) canvas->SetFocus();
        ScrollOrZoom(true);
        return wxEmptyString;
    }

    wxString layrender(int ds, bool vert, bool toggle = false, bool noset = false) {
        if (selected.Thin()) return NoThin();
        selected.grid->cell->AddUndo(this);
        bool v = toggle ? !selected.GetFirst()->verticaltextandgrid : vert;
        if (ds >= 0 && selected.IsAll()) selected.grid->cell->drawstyle = ds;
        selected.grid->SetGridTextLayout(ds, v, noset, selected);
        selected.grid->cell->ResetChildren();
        canvas->Refresh();
        return wxEmptyString;
    }

    void ZoomOutIfNoGrid() {
        if (!WalkPath(drawpath)->grid) Zoom(-1);
    }

    void PasteSingleText(Cell *c, const wxString &s) { c->text.Insert(this, s, selected, false); }

    // Polymorphism with wxDataObjectSimple does not work on Windows; bitmap format seems to not be
    // recognized.

    void PasteOrDrop(const wxFileDataObject &filedataobject) {
        const wxArrayString &filenames = filedataobject.GetFilenames();
        if (filenames.size() != 1) {
            sys->frame->SetStatus(_("Can paste or drop only exactly one file."));
            return;
        }
        Cell *cell = selected.ThinExpand(this);
        wxString filename = filenames[0];
        wxFFileInputStream fileinputstream(filename);
        if (fileinputstream.IsOk()) {
            char buffer[4];
            fileinputstream.Read(buffer, 4);
            if (!strncmp(buffer, "TSFF", 4)) {
                ThreeChoiceDialog askuser(
                    sys->frame, filename,
                    _("It seems that you are about to paste or drop a TreeSheets file. "
                      "What would you like to do?"),
                    _("Open TreeSheets file"), _("Paste file path"), _("Cancel"));
                switch (askuser.Run()) {
                    case 0: sys->frame->SetStatus(sys->LoadDB(filename));
                    case 2: return;
                    default:
                    case 1:;
                }
            }
        }
        if (!cell) return;
        cell->AddUndo(this);
        if (!LoadImageIntoCell(filename, cell, sys->frame->FromDIP(1.0)))
            PasteSingleText(cell, filename);
    }

    void PasteOrDrop(const wxTextDataObject &textdataobject) {
        if (textdataobject.GetText() != wxEmptyString) {
            Cell *cell = selected.ThinExpand(this);
            auto text = textdataobject.GetText();
            if ((sys->clipboardcopy == text) && sys->cellclipboard) {
                cell->Paste(this, sys->cellclipboard.get(), selected);
            } else {
                const wxArrayString &lines = wxStringTokenize(text, LINE_SEPARATOR);
                if (lines.size() == 1) {
                    cell->AddUndo(this);
                    cell->ResetLayout();
                    PasteSingleText(cell, lines[0]);
                } else if (lines.size() > 1) {
                    cell->parent->AddUndo(this);
                    cell->ResetLayout();
                    DELETEP(cell->grid);
                    sys->FillRows(cell->AddGrid(), lines, sys->CountCol(lines[0]), 0, 0);
                    if (!cell->HasText())
                        cell->grid->MergeWithParent(cell->parent->grid, selected, this);
                }
            }
        }
    }

    void PasteOrDrop(const wxBitmapDataObject &bitmapdataobject) {
        if (bitmapdataobject.GetBitmap().GetRefData() != wxNullBitmap.GetRefData()) {
            Cell *cell = selected.ThinExpand(this);
            cell->AddUndo(this);
            auto image = bitmapdataobject.GetBitmap().ConvertToImage();
            vector<uint8_t> buffer = ConvertWxImageToBuffer(image, wxBITMAP_TYPE_PNG);
            SetImageBM(cell, std::move(buffer), 'I', sys->frame->FromDIP(1.0));
            cell->Reset();
        }
    }

    wxString Sort(bool descending) {
        if (selected.xs != 1 && selected.ys <= 1)
            return _(
                "Can't sort: make a 1xN selection to indicate what column to sort on, and what rows to affect");
        selected.grid->cell->AddUndo(this);
        selected.grid->Sort(selected, descending);
        canvas->Refresh();
        return wxEmptyString;
    }

    void DelRowCol(int &v, int e, int gvs, int dec, int dx, int dy, int nxs, int nys) {
        if (v != e) {
            selected.grid->cell->AddUndo(this);
            if (gvs == 1) {
                selected.grid->DelSelf(this, selected);
            } else {
                selected.grid->DeleteCells(dx, dy, nxs, nys);
                v -= dec;
            }
            canvas->Refresh();
        }
    }

    void CreatePath(Cell *c, auto &path) {
        path.clear();
        while (c->parent) {
            const Selection &s = c->parent->grid->FindCell(c);
            ASSERT(s.grid);
            path.push_back(s);
            c = c->parent;
        }
    }

    Cell *WalkPath(auto &path) {
        Cell *c = root.get();
        loopvrev(i, path) {
            Selection &s = path[i];
            Grid *g = c->grid;
            if (!g) return c;
            ASSERT(g && s.x < g->xs && s.y < g->ys);
            c = g->C(s.x, s.y).get();
        }
        return c;
    }

    bool LastUndoSameCellAny(Cell *c) {
        return undolist.size() && undolist.size() != undolistsizeatfullsave &&
               undolist.back()->cloned_from == (uintptr_t)c;
    }

    bool LastUndoSameCellTextEdit(Cell *c) {
        // hacky way to detect word boundaries to stop coalescing, but works, and
        // not a big deal if selected is not actually related to this cell
        return undolist.size() && !c->grid && undolist.size() != undolistsizeatfullsave &&
               undolist.back()->sel.EqLoc(c->parent->grid->FindCell(c)) &&
               (!c->text.t.EndsWith(" ") || c->text.t.Len() != selected.cursor);
    }

    void AddUndo(Cell *c, bool newgeneration = true) {
        redolist.clear();
        lastmodsinceautosave = wxGetLocalTime();
        if (!modified) {
            modified = true;
            UpdateFileName();
        }
        if (LastUndoSameCellTextEdit(c)) return;
        auto ui = make_unique<UndoItem>();
        ui->clone = c->Clone(nullptr);
        ui->estimated_size = c->EstimatedMemoryUse();
        ui->sel = selected;
        ui->cloned_from = (uintptr_t)c;
        if (undolist.size()) ui->generation = undolist.back()->generation + (newgeneration ? 1 : 0);
        CreatePath(c, ui->path);
        if (selected.grid) CreatePath(selected.grid->cell, ui->selpath);
        undolist.push_back(std::move(ui));
        size_t total_usage = 0;
        size_t old_list_size = undolist.size();
        // Cull undolist. Always at least keeps last item.
        for (auto i = static_cast<int>(undolist.size()) - 1; i >= 0; i--) {
            // Cull old items if using more than 100MB or 1000 items, whichever comes first.
            // TODO: make configurable?
            if (total_usage < 100 * 1024 * 1024 && undolist.size() - i < 1000) {
                total_usage += undolist[i]->estimated_size;
            } else {
                undolist.erase(undolist.begin(), undolist.begin() + i + 1);
                break;
            }
        }
        size_t items_culled = old_list_size - undolist.size();
        undolistsizeatfullsave -= items_culled;  // Allowed to go < 0
    }

    void Undo(vector<unique_ptr<UndoItem>> &fromlist, vector<unique_ptr<UndoItem>> &tolist,
              bool redo = false) {
        for (bool next = true; next; ) {
            UndoEach(fromlist, tolist, redo);
            next = (fromlist.size() && tolist.size() &&
                    fromlist.back()->generation == tolist.back()->generation)
                       ? true
                       : false;
        }
        if (selected.grid)
            ScrollOrZoom();
        else
            canvas->Refresh();
        UpdateFileName();
    }

    void UndoEach(vector<unique_ptr<UndoItem>> &fromlist, vector<unique_ptr<UndoItem>> &tolist,
                  bool redo = false) {
        auto beforesel = selected;
        vector<Selection> beforepath;
        if (beforesel.grid) CreatePath(beforesel.grid->cell, beforepath);

        auto ui = std::move(fromlist.back());
        fromlist.pop_back();

        Cell *c = WalkPath(ui->path);

        if (c->parent && c->parent->grid) {
            Grid *g = c->parent->grid;
            Selection s = g->FindCell(c);
            std::swap(ui->clone, g->C(s.x, s.y));
            c = g->C(s.x, s.y).get();
            c->parent = ui->clone->parent;
        } else {
            std::swap(ui->clone, root);
            c = root.get();
            c->parent = nullptr;
        }
        c->ResetLayout();

        SetSelect(ui->sel);
        if (selected.grid) { selected.grid = WalkPath(ui->selpath)->grid; }

        begindrag = selected;
        ui->sel = beforesel;
        ui->selpath = std::move(beforepath);
        tolist.push_back(std::move(ui));

        if (undolistsizeatfullsave > (int)undolist.size()) undolistsizeatfullsave = -1;
        modified = undolistsizeatfullsave != (int)undolist.size();
    }

    void ColorChange(int which, int idx) {
        if (!selected.grid) return;
        auto col = idx == CUSTOMCOLORIDX ? sys->customcolor : celltextcolors[idx];
        switch (which) {
            case A_CELLCOLOR: sys->lastcellcolor = col; break;
            case A_TEXTCOLOR: sys->lasttextcolor = col; break;
            case A_BORDCOLOR: sys->lastbordcolor = col; break;
        }
        selected.grid->ColorChange(this, which, col, selected);
    }

    void SetImageBM(Cell *c, auto &&data, char type, double scale) {
        c->text.image = sys->lastimage =
            sys->imagelist[sys->AddImageToList(scale, std::move(data), type)].get();
    }

    bool LoadImageIntoCell(const wxString &filename, Cell *c, double scale) {
        if (filename.empty()) return false;
        auto pnghandler = wxImage::FindHandler(wxBITMAP_TYPE_PNG);
        auto jpghandler = wxImage::FindHandler(wxBITMAP_TYPE_JPEG);
        wxImageHandler *activeHandler = nullptr;
        if (pnghandler && pnghandler->CanRead(filename)) {
            activeHandler = pnghandler;
        } else if (jpghandler && jpghandler->CanRead(filename)) {
            activeHandler = jpghandler;
        }
        if (activeHandler) {
            wxFile fn(filename);
            if (!fn.IsOpened()) return false;
            vector<uint8_t> buffer(fn.Length());
            fn.Read(buffer.data(), buffer.size());
            SetImageBM(c, std::move(buffer), (activeHandler == pnghandler) ? 'I' : 'J', scale);
        } else {
            wxImage image;
            if (!image.LoadFile(filename)) return false;
            auto buffer = ConvertWxImageToBuffer(image, wxBITMAP_TYPE_PNG);
            SetImageBM(c, std::move(buffer), 'I', scale);
        }
        c->Reset();
        return true;
    }

    void ImageChange(const wxString &filename, double scale) {
        if (!selected.grid) return;
        selected.grid->cell->AddUndo(this);
        loopallcellssel(c, false) LoadImageIntoCell(filename, c, scale);
        canvas->Refresh();
    }

    void RecreateTagMenu(wxMenu &menu) {
        int i = A_TAGSET;
        for (auto &[tag, color] : tags) { menu.Append(i++, tag); }
        if (tags.size()) menu.AppendSeparator();
        menu.Append(A_TAGADD, _("&Add Cell Text as Tag"));
        menu.Append(A_TAGREMOVE, _("&Remove Cell Text from Tags"));
    }

    wxString TagSet(int tagno) {
        int i = 0;
        for (auto &[tag, color] : tags)
            if (i++ == tagno) {
                selected.grid->cell->AddUndo(this);
                loopallcellssel(c, false) {
                    c->text.Clear(this, selected);
                    c->text.Insert(this, tag, selected, true);
                }
                selected.grid->cell->ResetChildren();
                selected.grid->cell->ResetLayout();
                canvas->Refresh();
                return wxEmptyString;
            }
        ASSERT(0);
        return wxEmptyString;
    }

    void CollectCells(Cell *c) {
        itercells.clear();
        c->CollectCells(itercells);
    }

    void CollectCellsSel(bool recurse) {
        itercells.clear();
        if (selected.grid) selected.grid->CollectCellsSel(itercells, selected, recurse);
    }

    void ApplyEditFilter() {
        searchfilter = false;
        paintscrolltoselection = true;
        editfilter = min(max(editfilter, 1), 99);
        CollectCells(root.get());
        ranges::sort(itercells, [](auto a, auto b) {
            // sort in descending order
            return a->text.lastedit > b->text.lastedit;
        });
        loopv(i, itercells) itercells[i]->text.filtered = i > itercells.size() * editfilter / 100;
        root->ResetChildren();
        canvas->Refresh();
    }

    void ApplyEditRangeFilter(wxDateTime &rangebegin, wxDateTime &rangeend) {
        searchfilter = false;
        paintscrolltoselection = true;
        CollectCells(root.get());
        for (auto c : itercells) {
            c->text.filtered = !c->text.lastedit.IsBetween(rangebegin, rangeend);
        }
        root->ResetChildren();
        canvas->Refresh();
    }

    wxDateTime ParseDateTimeString(const wxString &s) {
        wxDateTime dt;
        wxString::const_iterator end;
        if (!dt.ParseDateTime(s, &end)) dt = wxInvalidDateTime;
        return dt;
    }

    void SetSearchFilter(bool on) {
        searchfilter = on;
        paintscrolltoselection = true;
        loopallcells(c) c->text.filtered = on && !c->text.IsInSearch();
        root->ResetChildren();
        canvas->Refresh();
    }

    void ExportAllImages(const wxString &filename, Cell *exportroot) {
        std::set<Image *> exportimages;
        CollectCells(exportroot);
        for (auto c : itercells)
            if (c->text.image) exportimages.insert(c->text.image);
        wxFileName fn(filename);
        auto directory = fn.GetPathWithSep();
        for (auto image : exportimages)
            if (!image->ExportToDirectory(directory)) break;
    }
};
```

## File: src/evaluator.h
```c
/*
    A structure describing an operation.
*/
struct Operation {
    virtual ~Operation() {};
    const char *args;

    virtual unique_ptr<Cell> run() const { return nullptr; }
    virtual double runn(double a) const { return 0; }
    virtual unique_ptr<Cell> runl(Grid *l) const { return nullptr; }
    virtual void rung(Grid *g) const {}
    virtual unique_ptr<Cell> runc(unique_ptr<Cell> c) const { return nullptr; }
    virtual double runnn(double a, double b) const { return 0; }
};

typedef std::unordered_map<wxString, unique_ptr<Operation>> OperationMap;
typedef std::unordered_map<wxString, unique_ptr<Cell>> VariableMap;

/*
    Provides running evaluation of a grid.
*/
struct Evaluator {
    OperationMap ops;
    VariableMap vars;
    bool vert;

    ~Evaluator() {}

    void ClearVars() {
        vars.clear();
    }

    #define OP(_n, _c, _args, _f)           \
        {                                   \
            struct _op : Operation {        \
                _op() { args = _args; };    \
                _f const override { return _c; }; \
            };                              \
            ops[L## #_n] = make_unique<_op>();       \
        }

    #define OPNN(_n, _c) OP(_n, _c, "nn", double runnn(double a, double b))
    #define OPN(_n, _c) OP(_n, _c, "n", double runn(double a))
    #define OPT(_n, _c) OP(_n, _c, "t", unique_ptr<Cell> runc(unique_ptr<Cell> c))
    #define OPC(_n, _c) OP(_n, _c, "c", unique_ptr<Cell> runc(unique_ptr<Cell> c))
    #define OPL(_n, _c) OP(_n, _c, "l", unique_ptr<Cell> runl(Grid *a))
    #define OPG(_n, _c) OP(_n, _c, "g", void rung(Grid *a))

    void Init() {
        OPNN(+, a + b);
        OPNN(-, a - b);
        OPNN(*, a * b);
        OPNN(/, b != 0 ? a / b : 0);
        OPNN(<, double(a < b));
        OPNN(>, double(a > b));
        OPNN(<=, double(a <= b));
        OPNN(>=, double(a >= b));
        OPNN(=, double(a == b));
        OPNN(==, double(a == b));
        OPNN(!=, double(a != b));
        OPNN(<>, double(a != b));
        OPN(inc, a + 1);
        OPN(dec, a - 1);
        OPN(neg, -a);
        OPT(graph, (c->Graph(), std::move(c)));
        OPL(sum, a->Sum())
        OPG(transpose, a->Transpose())
        struct _if : Operation {
            _if() { args = "nLL"; };
        };
        ops[L"if"] = make_unique<_if>();
    }

    int InferCellType(Text &t) {
        if (ops[t.t])
            return CT_CODE;
        else
            return CT_DATA;
    }

    unique_ptr<Cell> Lookup(const wxString &name) {
        auto it = vars.find(name);
        return (it != vars.end()) ? it->second->Clone(nullptr) : nullptr;
    }

    bool IsValidSymbol(wxString const &symbol) const { return !symbol.IsEmpty(); }
    void SetSymbol(wxString const &symbol, unique_ptr<Cell> val) {
        if (!this->IsValidSymbol(symbol)) { return; }
        vars[symbol] = std::move(val);
    }

    void Assign(const Cell *sym, const Cell *val) {
        this->SetSymbol(sym->text.t, val->Clone(nullptr));
        if (sym->grid && val->grid) this->DestructuringAssign(sym->grid, val->Clone(nullptr));
    }

    void DestructuringAssign(Grid const *names, unique_ptr<Cell> val) {
        Grid const *ng = names;
        Grid const *vg = val->grid;
        if (ng->xs == vg->xs && ng->ys == vg->ys) {
            loop(x, ng->xs) loop(y, ng->ys) {
                this->SetSymbol(ng->C(x, y)->text.t, vg->C(x, y)->Clone(nullptr));
            }
        }
    }

    Operation *FindOp(wxString &name) { return ops[name].get(); }

    unique_ptr<Cell> Execute(const Operation *op) { return op->run(); }

    unique_ptr<Cell> Execute(const Operation *op, unique_ptr<Cell> left) {
        Text &t = left->text;
        Grid *g = left->grid;
        switch (op->args[0]) {
            case 'n':
                if (t.t.Len()) {
                    t.SetNum(op->runn(t.GetNum()));
                    return left;
                } else if (g) {
                    foreachcellingrid(c, g) {
                        unique_ptr<Cell> temp = std::move(c);
                        c = Execute(op, std::move(temp));
                        c->SetParent(left.get());
                    }
                }
                break;
            case 't':
                if (t.t.Len()) {
                    return op->runc(std::move(left));
                } else if (g) {
                    foreachcellingrid(c, g) {
                        unique_ptr<Cell> temp = std::move(c);
                        c = Execute(op, std::move(temp));
                        c->SetParent(left.get());
                    }
                }
                break;
            case 'l':
                if (g) {
                    if (g->xs == 1 || g->ys == 1) {
                        return op->runl(g);
                    } else {
                        vector<unique_ptr<Grid>> gs;
                        g->Split(gs, vert);
                        g = new Grid(vert ? gs.size() : 1, vert ? 1 : gs.size());
                        auto c = make_unique<Cell>(nullptr, left.get(), CT_DATA, g);
                        loopv(i, gs) {
                            auto res = op->runl(gs[i].get());
                            res->SetParent(c.get());
                            g->C(vert ? i : 0, vert ? 0 : i) = std::move(res);
                        }
                        return c;
                    }
                }
                break;
            case 'g':
                if (g) op->rung(g);
                break;
            case 'c': return op->runc(std::move(left));
        }
        return left;
    }

    unique_ptr<Cell> Execute(const Operation *op, unique_ptr<Cell> left, const Cell *_right) {
        auto right = _right->Eval(*this);
        if (!right) return left;
        Text &t1 = left->text;
        Text &t2 = right->text;
        Grid *g1 = left->grid;
        Grid *g2 = right->grid;
        switch (op->args[0]) {
            case 'n':
                if (t1.t.Len() && t2.t.Len()) {
                    t1.SetNum(op->runnn(t1.GetNum(), t2.GetNum()));
                } else if (g1 && g2 && g1->xs == g2->xs && g1->ys == g2->ys) {
                    Grid *g = new Grid(g1->xs, g1->ys);
                    auto c = make_unique<Cell>(nullptr, left.get(), CT_DATA, g);
                    loop(x, g->xs) loop(y, g->ys) {
                        unique_ptr<Cell> c1 = std::move(g1->C(x, y));
                        Cell *c2 = g2->C(x, y).get();
                        auto res = Execute(op, std::move(c1), c2);
                        res->SetParent(c.get());
                        g->C(x, y) = std::move(res);
                    }
                    return c;
                } else if (g1 && t2.t.Len()) {
                    foreachcellingrid(c, g1) {
                        unique_ptr<Cell> res = Execute(op, std::move(c), right.get());
                        res->SetParent(left.get());
                        c = std::move(res);
                    }
                }
                break;
        }
        return left;
    }

    // IF is sofar the only ternary
    unique_ptr<Cell> Execute(const Operation *op, unique_ptr<Cell> left, const Cell *a,
                             const Cell *b) {
        Text &l = left->text;
        if (!l.t.Len()) return left;
        bool cond = l.GetNum() != 0;
        return (cond ? a : b)->Eval(*this);
    }

    void Eval(const Cell *root) {
        root->Eval(*this);
        ClearVars();
    }
};
```

## File: src/events.h
```c
BEGIN_EVENT_TABLE(treesheets::TSFrame, wxFrame)
  EVT_DPI_CHANGED(treesheets::TSFrame::OnDPIChanged)
  EVT_SIZING(treesheets::TSFrame::OnSizing)
  EVT_MENU(wxID_ANY, treesheets::TSFrame::OnMenu)
  EVT_TEXT(A_SEARCH, treesheets::TSFrame::OnSearch)
  EVT_TEXT_ENTER(A_SEARCH, treesheets::TSFrame::OnSearchReplaceEnter)
  EVT_TEXT_ENTER(A_REPLACE, treesheets::TSFrame::OnSearchReplaceEnter)
  EVT_CLOSE(treesheets::TSFrame::OnClosing)
  EVT_MAXIMIZE(treesheets::TSFrame::OnMaximize)
  EVT_ACTIVATE_APP(treesheets::TSFrame::OnActivate)
  EVT_COMBOBOX(A_CELLCOLOR, treesheets::TSFrame::OnChangeColor)
  EVT_COMBOBOX(A_TEXTCOLOR, treesheets::TSFrame::OnChangeColor)
  EVT_COMBOBOX(A_BORDCOLOR, treesheets::TSFrame::OnChangeColor)
  EVT_COMBOBOX(A_DDIMAGE,   treesheets::TSFrame::OnDDImage)
  EVT_ICONIZE(treesheets::TSFrame::OnIconize)
  EVT_AUINOTEBOOK_PAGE_CHANGED(wxID_ANY, treesheets::TSFrame::OnTabChange)
  EVT_AUINOTEBOOK_PAGE_CLOSE(wxID_ANY, treesheets::TSFrame::OnTabClose)
  EVT_SYS_COLOUR_CHANGED(treesheets::TSFrame::OnSysColourChanged)
END_EVENT_TABLE()

BEGIN_EVENT_TABLE(treesheets::TSApp, wxApp)
END_EVENT_TABLE()

BEGIN_EVENT_TABLE(treesheets::TSCanvas, wxScrolledWindow)
  EVT_MOUSEWHEEL(treesheets::TSCanvas::OnMouseWheel)
  EVT_PAINT(treesheets::TSCanvas::OnPaint)
  EVT_MOTION(treesheets::TSCanvas::OnMotion)
  EVT_LEFT_DOWN(treesheets::TSCanvas::OnLeftDown)
  EVT_LEFT_UP(treesheets::TSCanvas::OnLeftUp)
  EVT_RIGHT_DOWN(treesheets::TSCanvas::OnRightDown)
  EVT_LEFT_DCLICK(treesheets::TSCanvas::OnLeftDoubleClick)
  EVT_CHAR(treesheets::TSCanvas::OnChar)
  EVT_KEY_DOWN(treesheets::TSCanvas::OnKeyDown)
  EVT_CONTEXT_MENU(treesheets::TSCanvas::OnContextMenuClick)
  EVT_SIZE(treesheets::TSCanvas::OnSize)
  EVT_SCROLLWIN(treesheets::TSCanvas::OnScrollWin)
END_EVENT_TABLE()

BEGIN_EVENT_TABLE(treesheets::ThreeChoiceDialog, wxDialog)
    EVT_BUTTON(wxID_ANY, treesheets::ThreeChoiceDialog::OnButton)
END_EVENT_TABLE()

BEGIN_EVENT_TABLE(treesheets::DateTimeRangeDialog, wxDialog)
    EVT_BUTTON(wxID_ANY, treesheets::DateTimeRangeDialog::OnButton)
END_EVENT_TABLE()
```

## File: src/genpot.bat
```batch
xgettext --keyword=_ --sort-output --no-location -o ../TS/translations/ts.pot tsframe.h document.h system.h wxtools.h
```

## File: src/grid.h
```c
struct Grid {
    // owning cell.
    Cell *cell;
    // subcells
    vector<unique_ptr<Cell>> cells;
    // widths for each column
    vector<int> colwidths;
    // xsize, ysize
    int xs;
    int ys;
    int view_margin;
    int view_grid_outer_spacing;
    int user_grid_outer_spacing {g_usergridouterspacing_default};
    int cell_margin;
    int bordercolor {g_bordercolor_default};
    bool horiz {false};
    bool tinyborder;
    bool folded {false};

    unique_ptr<Cell> &C(int x, int y) {
        ASSERT(x >= 0 && y >= 0 && x < xs && y < ys);
        return cells[x + y * xs];
    }

    Cell *C(int x, int y) const {
        ASSERT(x >= 0 && y >= 0 && x < xs && y < ys);
        return cells[x + y * xs].get();
    }

    #define foreachcell(c)                \
        for (int y = 0; y < ys; y++)      \
            for (int x = 0; x < xs; x++)  \
                for (bool _f = true; _f;) \
                    for (unique_ptr<Cell> &c = C(x, y); _f; _f = false)

    #define foreachcellconst(c)           \
        for (int y = 0; y < ys; y++)      \
            for (int x = 0; x < xs; x++)  \
                for (bool _f = true; _f;) \
                    for (Cell *c = C(x, y); _f; _f = false)

    #define foreachcellrev(c)                 \
        for (int y = ys - 1; y >= 0; y--)     \
            for (int x = xs - 1; x >= 0; x--) \
                for (bool _f = true; _f;)     \
                    for (unique_ptr<Cell> &c = C(x, y); _f; _f = false)
    #define foreachcelly(c)           \
        for (int y = 0; y < ys; y++)  \
            for (bool _f = true; _f;) \
                for (unique_ptr<Cell> &c = C(0, y); _f; _f = false)
    #define foreachcellcolumn(c)          \
        for (int x = 0; x < xs; x++)      \
            for (int y = 0; y < ys; y++)  \
                for (bool _f = true; _f;) \
                    for (unique_ptr<Cell> &c = C(x, y); _f; _f = false)
    #define foreachcellinsel(c, s)                 \
        for (int y = s.y; y < s.y + s.ys; y++)     \
            for (int x = s.x; x < s.x + s.xs; x++) \
                for (bool _f = true; _f;)          \
                    for (unique_ptr<Cell> &c = C(x, y); _f; _f = false)
    #define foreachcellinselrev(c, s)                   \
        for (int y = s.y + s.ys - 1; y >= s.y; y--)     \
            for (int x = s.x + s.xs - 1; x >= s.x; x--) \
                for (bool _f = true; _f;)               \
                    for (unique_ptr<Cell> &c = C(x, y); _f; _f = false)
    #define foreachcellingrid(c, g)         \
        for (int y = 0; y < g->ys; y++)     \
            for (int x = 0; x < g->xs; x++) \
                for (bool _f = true; _f;)   \
                    for (unique_ptr<Cell> &c = g->C(x, y); _f; _f = false)

    Grid(int _xs, int _ys, Cell *_c = nullptr)
        : xs(_xs), ys(_ys), cell(_c), cells(_xs * _ys) {
        InitColWidths();
        SetOrient();
    }

    void InitCells(Cell *clonestylefrom = nullptr) {
        foreachcell(c) c = make_unique<Cell>(cell, clonestylefrom);
    }
    void CloneStyleFrom(auto o) {
        bordercolor = o->bordercolor;
        // TODO: what others?
    }

    void InitColWidths() {
        colwidths.clear();
        colwidths.resize(xs, cell ? cell->ColWidth() : sys->defaultmaxcolwidth);
    }

    /* Clones g into this grid. This mutates the grid this function is called on. */
    void Clone(Grid *g) {
        g->bordercolor = bordercolor;
        g->user_grid_outer_spacing = user_grid_outer_spacing;
        g->folded = folded;
        foreachcell(c) g->C(x, y) = c->Clone(g->cell);
        loop(x, xs) g->colwidths[x] = colwidths[x];
    }

    unique_ptr<Cell> CloneSel(const Selection &sel) {
        auto cl = make_unique<Cell>(nullptr, sel.grid->cell, CT_DATA, new Grid(sel.xs, sel.ys));
        foreachcellinsel(c, sel) cl->grid->C(x - sel.x, y - sel.y) = c->Clone(cl.get());
        loop(i, sel.xs) cl->grid->colwidths[i] = sel.grid->colwidths[i];
        return cl;
    }

    size_t EstimatedMemoryUse() {
        size_t sum = 0;
        foreachcell(c) sum += c->EstimatedMemoryUse();
        return sizeof(Grid) + xs * ys * sizeof(Cell *) + sum;
    }

    void SetOrient() {
        if (xs > ys) horiz = true;
        if (ys > xs) horiz = false;
    }

    bool Layout(Document *doc, wxReadOnlyDC &dc, int depth, int &sx, int &sy, int startx, int starty,
                bool forcetiny) {
        auto xa = new int[xs];
        auto ya = new int[ys];
        loop(i, xs) xa[i] = 0;
        loop(i, ys) ya[i] = 0;
        tinyborder = true;
        foreachcell(c) {
            c->LazyLayout(doc, dc, depth + 1, colwidths[x], forcetiny);
            tinyborder = c->tiny && tinyborder;
            xa[x] = max(xa[x], c->sx);
            ya[y] = max(ya[y], c->sy);
        }
        view_grid_outer_spacing =
            tinyborder || cell->drawstyle != DS_GRID ? 0 : user_grid_outer_spacing;
        view_margin = tinyborder || cell->drawstyle != DS_GRID ? 0 : g_grid_margin;
        cell_margin = tinyborder ? 0 : (cell->drawstyle == DS_GRID ? 0 : g_cell_margin);
        sx = (xs + 1) * g_line_width + xs * cell_margin * 2 +
             2 * (view_grid_outer_spacing + view_margin) + startx;
        sy = (ys + 1) * g_line_width + ys * cell_margin * 2 +
             2 * (view_grid_outer_spacing + view_margin) + starty;
        loop(i, xs) sx += xa[i];
        loop(i, ys) sy += ya[i];
        int cx = view_grid_outer_spacing + view_margin + g_line_width + cell_margin + startx;
        int cy = view_grid_outer_spacing + view_margin + g_line_width + cell_margin + starty;
        if (!cell->tiny) {
            cx += g_margin_extra;
            cy += g_margin_extra;
        }
        foreachcell(c) {
            c->ox = cx;
            c->oy = cy;
            if (c->drawstyle == DS_BLOBLINE && !c->grid) {
                assert(c->sy <= ya[y]);
                c->ycenteroff = (ya[y] - c->sy) / 2;
            }
            c->sx = xa[x];
            c->sy = ya[y];
            cx += xa[x] + g_line_width + cell_margin * 2;
            if (x == xs - 1) {
                cy += ya[y] + g_line_width + cell_margin * 2;
                cx = view_grid_outer_spacing + view_margin + g_line_width + cell_margin + startx;
                if (!cell->tiny) cx += g_margin_extra;
            }
        }
        delete[] xa;
        delete[] ya;
        return tinyborder;
    }

    void Render(Document *doc, int bx, int by, wxDC &dc, int depth, int sx, int sy, int xoff,
                int yoff) {
        xoff = C(0, 0)->ox - view_margin - view_grid_outer_spacing - 1;
        yoff = C(0, 0)->oy - view_margin - view_grid_outer_spacing - 1;
        int maxx = C(xs - 1, 0)->ox + C(xs - 1, 0)->sx;
        int maxy = C(0, ys - 1)->oy + C(0, ys - 1)->sy;
        if (tinyborder || cell->drawstyle == DS_GRID) {
            int ldelta = view_grid_outer_spacing != 0;
            auto drawlines = [&]() {
                for (int x = ldelta; x <= xs - ldelta; x++) {
                    int xl = (x == xs ? maxx : C(x, 0)->ox - g_line_width) + bx;
                    if (xl >= doc->scrollx && xl <= doc->maxx) loop(line, g_line_width) {
                            dc.DrawLine(
                                xl + line, max(doc->scrolly, by + yoff + view_grid_outer_spacing),
                                xl + line, min(doc->maxy, by + maxy + g_line_width) + view_margin);
                        }
                }
                for (int y = ldelta; y <= ys - ldelta; y++) {
                    int yl = (y == ys ? maxy : C(0, y)->oy - g_line_width) + by;
                    if (yl >= doc->scrolly && yl <= doc->maxy) loop(line, g_line_width) {
                            dc.DrawLine(max(doc->scrollx,
                                            bx + xoff + view_grid_outer_spacing + g_line_width),
                                        yl + line, min(doc->maxx, bx + maxx) + view_margin,
                                        yl + line);
                        }
                }
            };
            if (!sys->fastrender && view_grid_outer_spacing && cell->cellcolor != 0xFFFFFF) {
                dc.SetPen(wxPen(LightColor(0xFFFFFF)));
                drawlines();
            }
            // dotted lines result in very expensive drawline calls
            dc.SetPen(view_grid_outer_spacing && !sys->fastrender ? sys->pen_gridlines
                                                                  : sys->pen_tinygridlines);
            drawlines();
        }

        foreachcell(c) {
            int cx = bx + c->ox;
            int cy = by + c->oy;
            if (cx < doc->maxx && cx + c->sx > doc->scrollx && cy < doc->maxy &&
                cy + c->sy > doc->scrolly) {
                c->Render(doc, cx, cy, dc, depth + 1, (x == 0) * view_margin,
                          (x == xs - 1) * view_margin, (y == 0) * view_margin,
                          (y == ys - 1) * view_margin, colwidths[x], cell_margin);
            }
        }

        if (cell->drawstyle == DS_BLOBLINE && !tinyborder && cell->HasHeader() && !cell->tiny) {
            const int arcsize = 8;
            int srcy = by + cell->ycenteroff +
                       (cell->verticaltextandgrid ? cell->tys + 2 : cell->tys / 2) + g_margin_extra;
            // fixme: the 8 is chosen to fit the smallest text size, not very portable
            int srcx = bx + (cell->verticaltextandgrid ? 8 : cell->txs + 4) + g_margin_extra;
            int destyfirst = -1, destylast = -1;
            dc.SetPen(*wxGREY_PEN);
            foreachcelly(c) if (c->HasContent() && !c->tiny) {
                int desty = c->ycenteroff + by + c->oy + c->tys / 2 + g_margin_extra;
                int destx = bx + c->ox - 2 + g_margin_extra;
                bool visible = srcx < doc->maxx && destx > doc->scrollx &&
                               desty - arcsize < doc->maxy && desty + arcsize > doc->scrolly;
                if (abs(srcy - desty) < arcsize && !cell->verticaltextandgrid) {
                    if (destyfirst < 0) destyfirst = desty;
                    destylast = desty;
                    if (visible) dc.DrawLine(srcx, desty, destx, desty);
                } else {
                    if (desty < srcy) {
                        if (destyfirst < 0) destyfirst = desty + arcsize;
                        destylast = desty + arcsize;
                        if (visible) dc.DrawBitmap(sys->frame->line_nw, srcx, desty, true);
                    } else {
                        destylast = desty - arcsize;
                        if (visible)
                            dc.DrawBitmap(sys->frame->line_sw, srcx, desty - arcsize, true);
                        desty--;
                    }
                    if (visible) dc.DrawLine(srcx + arcsize, desty, destx, desty);
                }
            }
            if (cell->verticaltextandgrid) {
                if (destylast > 0) dc.DrawLine(srcx, srcy, srcx, destylast);
            } else {
                if (destyfirst >= 0 && destylast >= 0 && destyfirst < destylast) {
                    destyfirst = min(destyfirst, srcy);
                    destylast = max(destylast, srcy);
                    dc.DrawLine(srcx, destyfirst, srcx, destylast);
                }
            }
        }
        if (view_grid_outer_spacing && cell->drawstyle == DS_GRID) {
            dc.SetBrush(*wxTRANSPARENT_BRUSH);
            dc.SetPen(wxPen(LightColor(bordercolor)));
            loop(i, view_grid_outer_spacing - 1) {
                dc.DrawRoundedRectangle(
                    bx + xoff + view_grid_outer_spacing - i,
                    by + yoff + view_grid_outer_spacing - i,
                    maxx - xoff - view_grid_outer_spacing + 1 + i * 2 + view_margin,
                    maxy - yoff - view_grid_outer_spacing + 1 + i * 2 + view_margin,
                    sys->roundness + i);
            }
        }
    }

    void FindXY(Document *doc, int px, int py, wxReadOnlyDC &dc) {
        foreachcell(c) {
            int bx = px - c->ox;
            int by = py - c->oy;
            if (bx >= 0 && by >= -g_line_width - g_selmargin && bx < c->sx && by < g_selmargin) {
                doc->hover = Selection(this, x, y, 1, 0);
                return;
            }
            if (bx >= 0 && by >= c->sy - g_selmargin && bx < c->sx &&
                by < c->sy + g_line_width + g_selmargin) {
                doc->hover = Selection(this, x, y + 1, 1, 0);
                return;
            }
            if (bx >= -g_line_width - g_selmargin && by >= 0 && bx < g_selmargin && by < c->sy) {
                doc->hover = Selection(this, x, y, 0, 1);
                return;
            }
            if (bx >= c->sx - g_selmargin && by >= 0 && bx < c->sx + g_line_width + g_selmargin &&
                by < c->sy) {
                doc->hover = Selection(this, x + 1, y, 0, 1);
                return;
            }
            if (c->IsInside(bx, by)) {
                if (c->GridShown(doc)) c->grid->FindXY(doc, bx, by, dc);
                if (doc->hover.grid) return;
                doc->hover = Selection(this, x, y, 1, 1);
                if (c->HasText()) {
                    c->text.FindCursor(doc, bx, by - c->ycenteroff, dc, doc->hover, colwidths[x]);
                }
                return;
            }
        }
    }

    Cell *FindLink(const Selection &sel, Cell *link, Cell *best, bool &lastthis, bool &stylematch,
                   bool forward, bool image) {
        if (forward) {
            foreachcell(c) best =
                c->FindLink(sel, link, best, lastthis, stylematch, forward, image);
        } else {
            foreachcellrev(c) best =
                c->FindLink(sel, link, best, lastthis, stylematch, forward, image);
        }
        return best;
    }

    Cell *FindNextSearchMatch(const wxString &search, Cell *best, Cell *selected,
                              bool &lastwasselected, bool reverse) {
        if (reverse) {
            foreachcellrev(c) best =
                c->FindNextSearchMatch(search, best, selected, lastwasselected, reverse);
        } else {
            foreachcell(c) best =
                c->FindNextSearchMatch(search, best, selected, lastwasselected, reverse);
        }
        return best;
    }

    Cell *FindNextFilterMatch(Cell *best, Cell *selected, bool &lastwasselected) {
        foreachcell(c) best = c->FindNextFilterMatch(best, selected, lastwasselected);
        return best;
    }

    void FindReplaceAll(const wxString &s, const wxString &ls) {
        foreachcell(c) c->FindReplaceAll(s, ls);
    }

    void ReplaceCell(Cell *o, Cell *n) { foreachcell(c) if (c.get() == o) c.reset(n); }
    Selection FindCell(Cell *o) {
        foreachcell(c) if (c.get() == o) return Selection(this, x, y, 1, 1);
        return Selection();
    }

    Selection SelectAll() { return Selection(this, 0, 0, xs, ys); }
    void ImageRefCount(bool includefolded) {
        if (includefolded || !folded) foreachcell(c) c->ImageRefCount(includefolded);
    }

    void DrawCursor(Document *doc, wxDC &dc, Selection &sel, bool full, uint color) {
        if (auto c = sel.GetCell(); c && !c->tiny && (c->HasText() || !c->grid))
            c->text.DrawCursor(doc, dc, sel, full, color, colwidths[sel.x]);
    }

    void DrawInsert(Document *doc, wxDC &dc, Selection &sel, uint colour) {
        dc.SetPen(sys->pen_thinselect);
        if (!sel.xs) {
            auto c = C(sel.x - (sel.x == xs), sel.y).get();
            int x = c->GetX(doc) + (c->sx + g_line_width + cell_margin) * (sel.x == xs) -
                    g_line_width - cell_margin;
            loop(line, g_line_width)
                dc.DrawLine(x + line, max(cell->GetY(doc), doc->scrolly), x + line,
                            min(cell->GetY(doc) + cell->sy, doc->maxy));
            DrawRectangle(dc, colour, x - 1, c->GetY(doc), g_line_width + 2, c->sy);
        } else {
            auto c = C(sel.x, sel.y - (sel.y == ys)).get();
            int y = c->GetY(doc) + (c->sy + g_line_width + cell_margin) * (sel.y == ys) -
                    g_line_width - cell_margin;
            loop(line, g_line_width)
                dc.DrawLine(max(cell->GetX(doc), doc->scrollx), y + line,
                            min(cell->GetX(doc) + cell->sx, doc->maxx), y + line);
            DrawRectangle(dc, colour, c->GetX(doc), y - 1, c->sx, g_line_width + 2);
        }
    }

    wxRect GetRect(Document *doc, const Selection &sel, bool minimal = false) {
        if (sel.Thin()) {
            if (sel.xs) {
                if (sel.y < ys) {
                    auto tl = C(sel.x, sel.y).get();
                    return wxRect(tl->GetX(doc), tl->GetY(doc), tl->sx, 0);
                } else {
                    auto br = C(sel.x, ys - 1).get();
                    return wxRect(br->GetX(doc), br->GetY(doc) + br->sy, br->sx, 0);
                }
            } else {
                if (sel.x < xs) {
                    auto tl = C(sel.x, sel.y).get();
                    return wxRect(tl->GetX(doc), tl->GetY(doc), 0, tl->sy);
                } else {
                    auto br = C(xs - 1, sel.y).get();
                    return wxRect(br->GetX(doc) + br->sx, br->GetY(doc), 0, br->sy);
                }
            }
        } else {
            auto tl = C(sel.x, sel.y).get();
            auto br = C(sel.x + sel.xs - 1, sel.y + sel.ys - 1).get();
            wxRect r(tl->GetX(doc) - cell_margin, tl->GetY(doc) - cell_margin,
                     br->GetX(doc) + br->sx - tl->GetX(doc) + cell_margin * 2,
                     br->GetY(doc) + br->sy - tl->GetY(doc) + cell_margin * 2);
            if (minimal && tl == br) r.width -= tl->sx - tl->minx;
            return r;
        }
    }

    void DrawSelect(Document *doc, wxDC &dc, Selection &sel) {
        if (sel.Thin()) {
            DrawInsert(doc, dc, sel, 0);
        } else {
            dc.SetBrush(wxBrush(LightColor(0x000000)));
            dc.SetPen(wxPen(LightColor(0x000000)));
            wxRect g = GetRect(doc, sel);
            int lw = g_line_width;
            int te = sel.TextEdit();
            dc.DrawRectangle(g.x - 1 - lw, g.y - 1 - lw, g.width + 2 + 2 * lw, 2 + lw - te);
            dc.DrawRectangle(g.x - 1 - lw, g.y - 1 + g.height + te, g.width + 2 + 2 * lw - 5,
                             2 + lw - te);

            dc.DrawRectangle(g.x - 1 - lw, g.y + 1 - te, 2 + lw - te, g.height - 2 + 2 * te);
            dc.DrawRectangle(g.x - 1 + g.width + te, g.y + 1 - te, 2 + lw - te,
                             g.height - 2 + 2 * te - 2 - te);

            dc.DrawRectangle(g.x + g.width, g.y + g.height - 2, lw + 2, lw + 4);
            dc.DrawRectangle(g.x + g.width - lw - 1, g.y + g.height - 2 + 2 * te, lw + 1,
                             lw + 4 - 2 * te);
            if (sel.TextEdit()) {
                DrawCursor(doc, dc, sel, true, sys->cursorcolor);
            } else {
                HintIMELocation(doc, g.x, g.y, g_deftextsize * 1.333, 0);
            }
        }
    }

    void DeleteCells(int dx, int dy, int nxs, int nys) {
        vector<unique_ptr<Cell>> ncells;
        ncells.reserve((xs + nxs) * (ys + nys));
        foreachcell(c) if (!(x == dx || y == dy)) ncells.push_back(std::move(c));
        cells = std::move(ncells);
        xs += nxs;
        ys += nys;
        if (dx >= 0) colwidths.erase(colwidths.begin() + dx);
        SetOrient();
    }

    void MultiCellDelete(Document *doc, Selection &sel) {
        cell->AddUndo(doc);
        MultiCellDeleteSub(doc, sel);
        doc->canvas->Refresh();
    }

    void MultiCellDeleteSub(Document *doc, Selection &sel) {
        foreachcellinsel(c, sel) c->Clear();
        bool delhoriz = true, delvert = true;
        foreachcell(c) {
            if (c->HasContent()) {
                if (y >= sel.y && y < sel.y + sel.ys) delhoriz = false;
                if (x >= sel.x && x < sel.x + sel.xs) delvert = false;
            }
        }
        if (delhoriz && (!delvert || sel.xs >= sel.ys)) {
            if (sel.ys == ys) {
                DelSelf(doc, sel);
            } else {
                loop(i, sel.ys) DeleteCells(-1, sel.y, 0, -1);
                sel.ys = 0;
                sel.xs = 1;
            }
        } else if (delvert) {
            if (sel.xs == xs) {
                DelSelf(doc, sel);
            } else {
                loop(i, sel.xs) DeleteCells(sel.x, -1, -1, 0);
                sel.xs = 0;
                sel.ys = 1;
            }
        } else {
            auto c = sel.GetCell();
            if (c) sel.EnterEdit(doc);
        }
    }

    void DelSelf(Document *doc, Selection &s) {
        if (!doc->drawpath.empty() && doc->drawpath.back().grid == this) {
            doc->drawpath.pop_back();
            doc->currentdrawroot = doc->WalkPath(doc->drawpath);
        }
        if (!cell->parent) return;  // FIXME: deletion of root cell, what would be better?
        s = cell->parent->grid->FindCell(cell);
        auto &pthis = cell->grid;
        DELETEP(pthis);
    }

    void InsertCells(int dx, int dy, int nxs, int nys, unique_ptr<Cell> nc = nullptr) {
        vector<unique_ptr<Cell>> ocells = std::move(cells);
        cells.clear();
        cells.resize((xs + nxs) * (ys + nys));

        int oxs = xs;
        int oys = ys;
        xs += nxs;
        ys += nys;
        SetOrient();
        int opos = 0;
        foreachcell(c) {
            if (x != dx && y != dy) { c = std::move(ocells[opos++]); }
        }
        foreachcell(c) {
            if (x == dx || y == dy) {
                if (nc) {
                    c = std::move(nc);
                } else {
                    int sx = nxs ? max(0, min(dx - 1, xs - 1)) : x;
                    int sy = nxs ? y : max(0, min(dy - 1, ys - 1));
                    Cell *colcell = C(sx, sy).get();
                    c = make_unique<Cell>(cell, colcell);
                    if (colcell) c->text.relsize = colcell->text.relsize;
                }
            }
        }

        if (dx >= 0) colwidths.insert(colwidths.begin() + dx, cell->ColWidth());
    }

    void Save(wxDataOutputStream &dos, Cell *ocs) const {
        dos.Write32(xs);
        dos.Write32(ys);
        dos.Write32(bordercolor);
        dos.Write32(user_grid_outer_spacing);
        dos.Write8(cell->verticaltextandgrid);
        dos.Write8(folded);
        loop(x, xs) dos.Write32(colwidths[x]);
        foreachcellconst(c) c->Save(dos, ocs);
    }

    bool LoadContents(wxDataInputStream &dis, int &numcells, int &textbytes, Cell *&ics) {
        if (sys->versionlastloaded >= 10) {
            bordercolor = dis.Read32() & 0xFFFFFF;
            user_grid_outer_spacing = dis.Read32();
            if (sys->versionlastloaded >= 11) {
                cell->verticaltextandgrid = dis.Read8() != 0;
                if (sys->versionlastloaded >= 13) {
                    if (sys->versionlastloaded >= 16) {
                        folded = dis.Read8() != 0;
                        if (folded && sys->versionlastloaded <= 17) {
                            // Before v18, folding would use the image slot. So if this cell
                            // contains an image, clear it.
                            cell->text.image = nullptr;
                        }
                    }
                    loop(x, xs) colwidths[x] = dis.Read32();
                }
            }
        }
        foreachcell(c) {
            Cell *rc = Cell::LoadWhich(dis, cell, numcells, textbytes, ics);
            if (!rc) return false;
            c.reset(rc);
        }
        return true;
    }

    void Formatter(wxString &r, int format, int indent, const wxString &xml, const wxString &html,
                   const wxString &htmlb) {
        if (format == A_EXPXML) {
            r.Append(' ', indent);
            r.Append(xml);
        } else if (format == A_EXPHTMLT || format == A_EXPHTMLTI || format == A_EXPHTMLTE) {
            r.Append(' ', indent);
            r.Append(html);
        } else if (format == A_EXPHTMLB && !htmlb.IsEmpty()) {
            r.Append(' ', indent);
            r.Append(htmlb);
        }
    }

    wxString ToText(int indent, const Selection &sel, int format, Document *doc, bool inheritstyle,
                    Cell *root) {
        return ConvertToText(SelectAll(), indent + 2, format, doc, inheritstyle, root);
    };

    wxString ConvertToText(const Selection &sel, int indent, int format, Document *doc,
                           bool inheritstyle, Cell *root) {
        wxString r;
        const int root_grid_spacing = 2;  // Can't be adjusted in editor, so use a default.
        const int font_size = 14 - indent / 2;
        const int grid_border_width =
            cell == doc->root.get() ? root_grid_spacing : user_grid_outer_spacing - 1;

        wxString xmlstr("<grid");
        if (folded) xmlstr.Append(wxString::Format(" folded=\"%d\"", folded));
        if (bordercolor != g_bordercolor_default) {
            xmlstr.Append(wxString::Format(" bordercolor=\"0x%06X\"", bordercolor));
        }
        if (user_grid_outer_spacing != g_usergridouterspacing_default) {
            xmlstr.Append(wxString::Format(" outerspacing=\"%d\"", user_grid_outer_spacing));
        }
        xmlstr.Append(">\n");

        Formatter(r, format, indent, xmlstr,
                  wxString::Format("<table style=\"border-width: %dpt; font-size: %dpt;\">\n",
                                   grid_border_width, font_size),
                  wxString::Format("<ul style=\"font-size: %dpt;\">\n", font_size));
        foreachcellinsel(c, sel) {
            if (x == sel.x) Formatter(r, format, indent, "<row>\n", "<tr>\n", "");
            r.Append(c->ToText(indent, sel, format, doc, inheritstyle, root));
            if (format == A_EXPCSV) r.Append(x == sel.x + sel.xs - 1 ? '\n' : ',');
            if (x == sel.x + sel.xs - 1) Formatter(r, format, indent, "</row>\n", "</tr>\n", "");
        }
        Formatter(r, format, indent, "</grid>\n", "</table>\n", "</ul>\n");
        return r;
    }

    void RelSize(int dir, int zoomdepth) { foreachcell(c) c->RelSize(dir, zoomdepth); }
    void RelSize(int dir, const Selection &sel, int zoomdepth) {
        foreachcellinsel(c, sel) c->RelSize(dir, zoomdepth);
    }
    void SetBorder(int width) {
        user_grid_outer_spacing = width;
    }

    int MinRelsize(int rs) {
        foreachcell(c) {
            int crs = c->MinRelsize();
            rs = min(rs, crs);
        }
        return rs;
    }

    void ResetChildren() {
        cell->Reset();
        foreachcell(c) c->ResetChildren();
    }

    void Move(int dx, int dy, const Selection &sel) {
        if (dx < 0 || dy < 0)
            foreachcellinsel(c, sel) std::swap(c, C((x + dx + xs) % xs, (y + dy + ys) % ys));
        else
            foreachcellinselrev(c, sel) std::swap(c, C((x + dx + xs) % xs, (y + dy + ys) % ys));
    }

    void Add(unique_ptr<Cell> c) {
        c->parent = cell;
        if (horiz)
            InsertCells(xs, -1, 1, 0, std::move(c));
        else
            InsertCells(-1, ys, 0, 1, std::move(c));
    }

    void MergeWithParent(Grid *p, Selection &sel, Document *doc) {
        cell->grid = nullptr;
        foreachcell(c) {
            if (x + sel.x >= p->xs) p->InsertCells(p->xs, -1, 1, 0);
            if (y + sel.y >= p->ys) p->InsertCells(-1, p->ys, 0, 1);
            Cell *pc = p->C(x + sel.x, y + sel.y).get();
            if (pc->HasContent()) {
                if (x) p->InsertCells(sel.x + x, -1, 1, 0);
                pc = p->C(x + sel.x, y + sel.y).get();
                if (pc->HasContent()) {
                    if (y) p->InsertCells(-1, sel.y + y, 0, 1);
                    pc = p->C(x + sel.x, y + sel.y).get();
                }
            }
            p->C(x + sel.x, y + sel.y) = std::move(c);
            p->C(x + sel.x, y + sel.y)->parent = p->cell;
        }
        sel.grid = p;
        sel.xs += xs - 1;
        sel.ys += ys - 1;
        sel.ExitEdit(doc);
        delete this;
    }

    void SetStyle(Document *doc, const Selection &sel, int sb) {
        cell->AddUndo(doc);
        cell->ResetChildren();
        foreachcellinsel(c, sel) {
            c->text.stylebits ^= sb;
            c->text.WasEdited();
        }
        doc->canvas->Refresh();
    }

    void ColorChange(Document *doc, int which, uint color, const Selection &sel) {
        cell->AddUndo(doc);
        cell->ResetChildren();
        foreachcellinsel(c, sel) c->ColorChange(doc, which, color);
        doc->canvas->Refresh();
    }

    void ReplaceStr(Document *doc, const wxString &s, const wxString &ls, const Selection &sel) {
        cell->AddUndo(doc);
        cell->ResetChildren();
        foreachcellinsel(c, sel) c->text.ReplaceStr(s, ls);
        doc->canvas->Refresh();
    }

    void CSVImport(const wxArrayString &as, const wxString &sep) {
        int cy = 0;
        loop(y, as.size()) {
            auto s = as[y];
            wxString word;
            for (int x = 0; s[0]; x++) {
                if (s[0] == '\"') {
                    word = "";
                    for (int i = 1;; i++) {
                        if (!s[i]) {
                            if (y < static_cast<int>(as.size()) - 1) {
                                s = as[++y];
                                i = 0;
                            } else {
                                s = L"";
                                break;
                            }
                        } else if (s[i] == '\"') {
                            if (s[i + 1] == '\"')
                                word += s[++i];
                            else {
                                s = s.size() == i + 1 ? wxString(L"") : s.Mid(i + 2);
                                break;
                            }
                        } else
                            word += s[i];
                    }
                } else {
                    int pos = s.Find(sep);
                    if (pos < 0) {
                        word = s;
                        s = "";
                    } else {
                        word = s.Left(pos);
                        s = s.Mid(pos + 1);
                    }
                }
                if (x >= xs) InsertCells(x, -1, 1, 0);
                Cell *c = C(x, cy).get();
                c->text.t = word;
            }
            cy++;
        }

        ys = cy;  // throws memory away, but doesn't matter
    }

    unique_ptr<Cell> EvalGridCell(auto &ev, unique_ptr<Cell> &c, unique_ptr<Cell> acc, int &x,
                                  int &y, bool &alldata, bool vert) {
        int ct = c->celltype;  // Type of subcell being evaluated
        // Update alldata condition (variable reads act like data)
        alldata = alldata && (ct == CT_DATA || ct == CT_VARU);
        ev.vert = vert;  // Inform evaluatour of vert status. (?)
        switch (ct) {
            // Var assign
            case CT_VARD: {
                if (vert) return acc;  // (Reject vertical assignments)
                // If we have no data, lets see if we can generate something useful from the
                // subgrid.
                if (!acc->grid && acc->text.t.IsEmpty()) {
                    acc = c->Eval(ev);
                    if (!acc) { return nullptr; }
                }
                // Assign the current data temporary to the text
                ev.Assign(c.get(), acc.get());
                // Pass the original data onwards
                return acc;
            }
            // View
            case CT_VIEWV:
            case CT_VIEWH:
                if (vert ? ct == CT_VIEWH : ct == CT_VIEWV) { return c->Clone(nullptr); }
                c = acc ? acc->Clone(cell) : make_unique<Cell>(cell);
                c->celltype = ct;
                return acc;
            // Operation
            case CT_CODE: {
                auto op = ev.FindOp(c->text.t);
                switch (op ? strlen(op->args) : -1) {
                    default: return nullptr;
                    case 0: return ev.Execute(op);
                    case 1: return acc ? ev.Execute(op, std::move(acc)) : nullptr;
                    case 2:
                        if (vert) {
                            if (acc && y + 1 < ys) {
                                return ev.Execute(op, std::move(acc), C(x, ++y).get());
                            } else {
                                return nullptr;
                            }
                        } else {
                            if (acc && x + 1 < xs) {
                                return ev.Execute(op, std::move(acc), C(++x, y).get());
                            } else {
                                return nullptr;
                            }
                        }
                    case 3:
                        if (vert) {
                            if (acc && y + 2 < ys) {
                                y += 2;
                                return ev.Execute(op, std::move(acc), C(x, y - 1).get(),
                                                  C(x, y).get());
                            } else {
                                return nullptr;
                            }
                        } else {
                            if (acc && x + 2 < xs) {
                                x += 2;
                                return ev.Execute(op, std::move(acc), C(x - 1, y).get(),
                                                  C(x, y).get());
                            } else {
                                return nullptr;
                            }
                        }
                }
            }
            // Var read, Data
            default: return c->Eval(ev);
        }
    }

    unique_ptr<Cell> Eval(auto &ev) {
        unique_ptr<Cell> acc;  // Actual/Accumulating data temporary
        bool alldata = true;   // Is the grid all data?
        // Do left to right processing
        if (xs > 1 || ys == 1) foreachcell(c) {
                if (x == 0) acc.reset();
                acc = EvalGridCell(ev, c, std::move(acc), x, y, alldata, false);
            }
        // Do top to bottom processing
        if (ys > 1) foreachcellcolumn(c) {
                if (y == 0) acc.reset();
                acc = EvalGridCell(ev, c, std::move(acc), x, y, alldata, true);
            }
        // If all data is true then we can exit now.
        if (alldata) {
            auto result = cell->Clone(nullptr);  // Potential result if all data.
            foreachcellingrid(rc, result->grid) { rc = rc->Eval(ev); }
            return result;
        }
        return acc;
    }

    void Split(vector<unique_ptr<Grid>> &gs, bool vert) {
        loop(i, vert ? xs : ys) gs.push_back(make_unique<Grid>(vert ? 1 : xs, vert ? ys : 1));
        foreachcell(c) {
            auto g = gs[vert ? x : y].get();
            c->SetParent(g->cell);
            g->cells[vert ? y : x] = std::move(c);
        }
    }

    unique_ptr<Cell> Sum() {
        double total = 0;
        foreachcell(c) {
            if (c->HasText()) total += c->text.GetNum();
        }
        auto c = make_unique<Cell>();
        c->text.SetNum(total);
        return c;
    }

    void Transpose() {
        vector<unique_ptr<Cell>> tr(xs * ys);
        foreachcell(c) tr[y + x * ys] = std::move(c);
        cells = std::move(tr);
        swap_(xs, ys);
        SetOrient();
        InitColWidths();
    }

    void Sort(Selection &sel, bool descending) {
        vector<int> row_indices(sel.ys);
        loop(i, sel.ys) row_indices[i] = i;

        std::stable_sort(row_indices.begin(), row_indices.end(), [&](int i, int j) {
            loop(k, xs) {
                int col = (k + sel.x) % xs;
                int cmp = C(col, sel.y + i)->text.t.CmpNoCase(C(col, sel.y + j)->text.t);
                if (cmp) return descending ? cmp > 0 : cmp < 0;
            }
            return false;
        });

        vector<unique_ptr<Cell>> new_cells;
        new_cells.reserve(sel.ys * xs);
        for (int i : row_indices) {
            loop(c, xs) {
                new_cells.push_back(std::move(C(c, sel.y + i)));
            }
        }

        loop(i, sel.ys * xs) {
            cells[sel.y * xs + i] = std::move(new_cells[i]);
        }
    }

    Cell *FindExact(const wxString &s) {
        foreachcell(c) {
            auto f = c->FindExact(s);
            if (f) return f;
        }
        return nullptr;
    }

    Selection HierarchySwap(wxString tag) {
        Cell *selcell = nullptr;
        bool done = false;
    lookformore:
        foreachcell(c) if (c->grid && !done) {
            auto f = c->grid->FindExact(tag);
            if (f) {
                // add all parent tags as extra hierarchy inside the cell
                for (auto p = f->parent; p != cell; p = p->parent) {
                    // Special case check: if parents have same name, this would cause infinite
                    // swapping.
                    if (p->text.t == tag) done = true;
                    auto t = make_unique<Cell>(f, p);
                    t->text = p->text;
                    t->text.cell = t.get();
                    t->note = p->note;
                    t->grid = f->grid;
                    if (t->grid) t->grid->ReParent(t.get());
                    f->grid = new Grid(1, 1);
                    f->grid->cell = f;
                    f->grid->cells[0] = std::move(t);
                }
                // remove cell from parent, recursively if parent becomes empty
                for (auto r = f; r && r != cell; r = r->parent->grid->DeleteTagParent(r, cell, f));
                // merge newly constructed hierarchy at this level
                if (!cells[0]) {
                    cells[0].reset(f);
                    f->parent = cell;
                    selcell = f;
                } else {
                    MergeTagCell(unique_ptr<Cell>(f), selcell);
                }
                goto lookformore;
            }
        }
        ASSERT(selcell);
        return FindCell(selcell);
    }

    void ReParent(auto p) {
        cell = p;
        foreachcell(c) c->parent = p;
    }

    Cell *DeleteTagParent(Cell *tag, Cell *basecell, Cell *found) {
        Cell *next = tag->parent;
        unique_ptr<Cell> detached_tag;
        int found_x = -1, found_y = -1;
        foreachcell(c) if (c.get() == tag) {
            detached_tag = std::move(c);
            found_x = x;
            found_y = y;
            break;
        }
        ASSERT(detached_tag);
        if (xs * ys == 1) {
            if (cell != basecell) {
                cell->grid = nullptr;
                delete this;
            }
            if (tag == found) detached_tag.release();
            return next;
        } else {
            if (ys > 1)
                DeleteCells(-1, found_y, 0, -1);
            else
                DeleteCells(found_x, -1, -1, 0);
            if (tag == found) detached_tag.release();
            return nullptr;
        }
    }

    void MergeTagCell(unique_ptr<Cell> f, Cell *&selcell) {
        foreachcell(c) if (c->text.t == f->text.t) {
            if (!selcell) selcell = c.get();

            if (f->grid) {
                if (c->grid) {
                    f->grid->MergeTagAll(c.get());
                } else {
                    c->grid = f->grid;
                    c->grid->ReParent(c.get());
                    f->grid = nullptr;
                }
            }
            return;
        }
        if (!selcell) selcell = f.get();
        Add(std::move(f));
    }

    void MergeTagAll(Cell *into) {
        foreachcell(c) {
            into->grid->MergeTagCell(std::move(c), into /*dummy*/);
        }
    }

    void SetGridTextLayout(int ds, bool vert, bool noset, const Selection &sel) {
        foreachcellinsel(c, sel) c->SetGridTextLayout(ds, vert, noset);
    }

    bool IsTable() {
        foreachcell(c) if (c->grid) return false;
        return true;
    }

    void Hierarchify(Document *doc) {
        loop(y, ys) {
            unique_ptr<Cell> rest;
            if (xs > 1) {
                Selection s(this, 1, y, xs - 1, 1);
                rest = CloneSel(s);
            }
            Cell *c = C(0, y).get();
            loop(prevy, y) {
                if (Cell *prev = C(0, prevy).get(); prev->text.t == c->text.t) {
                    if (rest) {
                        ASSERT(prev->grid);
                        prev->grid->MergeRow(rest->grid);
                        rest = nullptr;
                    }

                    Selection s(this, 0, y, xs, 1);
                    MultiCellDeleteSub(doc, s);
                    y--;

                    goto done;
                }
            }
            if (rest) {
                swap_(c->grid, rest->grid);
                c->grid->ReParent(c);
            }
        done:;
        }
        Selection s(this, 1, 0, xs - 1, ys);
        MultiCellDeleteSub(doc, s);
        foreachcell(c) if (c->grid && c->grid->xs > 1) c->grid->Hierarchify(doc);
    }

    void MergeRow(Grid *tm) {
        ASSERT(xs == tm->xs && tm->ys == 1);
        InsertCells(-1, ys, 0, 1);
        loop(x, xs) {
            std::swap(C(x, ys - 1), tm->C(x, 0));
            C(x, ys - 1)->parent = cell;
        }
    }

    void MaxDepthLeaves(int curdepth, int &maxdepth, int &leaves) {
        foreachcell(c) c->MaxDepthLeaves(curdepth, maxdepth, leaves);
    }

    int Flatten(int curdepth, int cury, Grid *g) {
        foreachcell(c) if (c->grid) { cury = c->grid->Flatten(curdepth + 1, cury, g); }
        else {
            Cell *ic = c.get();
            for (int i = curdepth; i >= 0; i--) {
                Cell *dest = g->C(i, cury).get();
                dest->text = ic->text;
                dest->text.cell = dest;
                ic = ic->parent;
            }
            cury++;
        }
        return cury;
    }

    void ResizeColWidths(int dir, const Selection &sel, bool hierarchical) {
        for (int x = sel.x; x < sel.x + sel.xs; x++) {
            colwidths[x] += dir * 5;
            if (colwidths[x] < 5) colwidths[x] = 5;
            loop(y, ys) {
                if (C(x, y)->grid && hierarchical)
                    C(x, y)->grid->ResizeColWidths(dir, C(x, y)->grid->SelectAll(), hierarchical);
            }
        }
    }

    int GetColWidth(Cell *ct) {
        foreachcell(c) {
            if (c.get() == ct) return colwidths[x];
        }
        return 0;
    }

    void SetColWidth(Cell *ct, int w) {
        foreachcell(c) {
            if (c.get() == ct) {
                colwidths[x] = w;
                return;
            }
        }
    }

    void CollectCells(auto &itercells) { foreachcell(c) c->CollectCells(itercells); }
    void CollectCellsSel(auto &itercells, const Selection &sel, bool recurse) {
        foreachcellinsel(c, sel) c->CollectCells(itercells, recurse);
    }

    void SetStyles(const Selection &sel, Cell *o) {
        foreachcellinsel(c, sel) {
            c->cellcolor = o->cellcolor;
            c->textcolor = o->textcolor;
            c->text.stylebits = o->text.stylebits;
            c->text.image = o->text.image;
            c->note = o->note;
        }
    }

    void ClearImages(const Selection &sel) { foreachcellinsel(c, sel) c->text.image = nullptr; }
};
```

## File: src/image.h
```c
struct Image {
    vector<uint8_t> data;
    char type;
    wxBitmap bm_display;
    int trefc {0};
    int savedindex {-1};
    uint64_t hash {0};

    // This indicates a relative scale, where 1.0 means bitmap pixels match display pixels on
    // a low res 96 dpi display. On a high dpi screen it will look scaled up. Higher values
    // look better on most screens.
    // This is all relative to GetContentScalingFactor.
    double display_scale;
    int pixel_width {0};

    Image(auto _hash, auto _sc, auto &&_data, auto _type)
        : hash(_hash), display_scale(_sc), data(std::move(_data)), type(_type) {}

    void ImageRescale(double scale) {
        auto &[it, mime] = imagetypes.at(type);
        auto im = ConvertBufferToWxImage(data, it);
        im.Rescale(im.GetWidth() * scale, im.GetHeight() * scale);
        data = ConvertWxImageToBuffer(im, it);
        hash = CalculateHash(data);
        bm_display = wxNullBitmap;
    }

    void DisplayScale(double scale) {
        display_scale /= scale;
        bm_display = wxNullBitmap;
    }

    void ResetScale(double scale) {
        display_scale = scale;
        bm_display = wxNullBitmap;
    }

    wxBitmap &Display() {
        // This might run in multiple threads in parallel
        // so this function must not touch any global resources
        // and callees must be thread-safe.
        if (!bm_display.IsOk()) {
            auto &[it, mime] = imagetypes.at(type);
            auto bm = ConvertBufferToWxBitmap(data, it);
            pixel_width = bm.GetWidth();
            ScaleBitmap(bm, sys->frame->FromDIP(1.0) / display_scale, bm_display);
        }
        return bm_display;
    }

    bool ExportToDirectory(const wxString &directory) {
        wxString targetname = directory + wxString::Format("%llu", hash) + GetFileExtension();
        wxFFileOutputStream os(targetname, "w+b");
        if (!os.IsOk()) {
            wxMessageBox(_("Error writing image file!"), targetname.wx_str(), wxOK, sys->frame);
            return false;
        }
        os.Write(data.data(), data.size());
        return true;
    }

    wxString GetFileExtension() {
        switch (type) {
            case 'J': return ".jpg";
            case 'I':
            default: return ".png";
        }
    }
};
```

## File: src/lobster_impl.cpp
```cpp
#include "lobster/stdafx.h"

#include "script_interface.h"

#include "lobster/compiler.h"

using namespace lobster;

namespace script {

ScriptInterface *si = nullptr;

void AddTreeSheets(NativeRegistry &nfr) {
nfr("goto_root", "", "", "",
    "makes the root of the document the current cell. this is the default at the start "
    "of any script, so this function is only needed to return there.",
    [](StackPtr &, VM &) {
        si->GoToRoot();
        return NilVal();
    });

nfr("goto_view", "", "", "", "makes what the user has zoomed into the current cell",
    [](StackPtr &, VM &) {
        si->GoToView();
        return NilVal();
    });

nfr("has_selection", "", "", "I", "whether there is a selection",
    [](StackPtr &, VM &) { return Value(si->HasSelection()); });

nfr("goto_selection", "", "", "",
    "makes the current cell the one selected, or the first of a selection",
    [](StackPtr &, VM &) {
        si->GoToSelection();
        return NilVal();
    });

nfr("has_parent", "", "", "I", "whether the current cell has a parent (is the root cell)",
    [](StackPtr &, VM &) { return Value(si->HasParent()); });

nfr("goto_parent", "", "", "", "makes the current cell the parent of the current cell, if any",
    [](StackPtr &, VM &) {
        si->GoToParent();
        return NilVal();
    });

nfr("num_children", "", "", "I",
    "returns the total number of children of the current cell (rows * columns). "
    "returns 0 if this cell doesn't have a sub-grid at all.",
    [](StackPtr &, VM &) { return Value(si->NumChildren()); });

nfr("num_columns_rows", "", "", "I}:2",
    "returns the number of columns and rows in the current cell",
    [](StackPtr &sp, VM &) { PushVec(sp, int2(si->NumColumnsRows())); });

nfr("selection", "", "", "I}:2I}:2",
    "returns the (xs,ys) and (x,y) of the current selection, or zeroes if none",
    [](StackPtr &sp, VM &) {
        auto b = si->SelectionBox();
        PushVec(sp, int2(b.second));
        PushVec(sp, int2(b.first));
    });

nfr("goto_child", "n", "I", "", "makes the current cell the nth child of the current cell",
    [](StackPtr &, VM &, Value n) {
        si->GoToChild(n.intval());
        return NilVal();
    });

nfr("goto_column_row", "col,row", "II", "", "makes the current cell the child at col / row",
    [](StackPtr &, VM &, Value x, Value y) {
        si->GoToColumnRow(x.intval(), y.intval());
        return NilVal();
    });

nfr("get_text", "", "", "S", "gets the text of the current cell.",
    [](StackPtr &, VM &vm) { return Value(vm.NewString(si->GetText())); });

nfr("get_note", "", "", "S", "gets the note of the current cell.",
    [](StackPtr &, VM &vm) { return Value(vm.NewString(si->GetNote())); });

nfr("set_text", "text", "S", "", "sets the text of the current cell",
    [](StackPtr &, VM &, Value s) {
        si->SetText(s.sval()->strv());
        return NilVal();
    });

nfr("set_note", "text", "S", "", "sets the note of the current cell",
    [](StackPtr &, VM &, Value s) {
        si->SetNote(s.sval()->strv());
        return NilVal();
    });

nfr("create_grid", "cols,rows", "II", "",
    "creates a grid in the current cell if there is not one yet",
    [](StackPtr &, VM &, Value x, Value y) {
        si->CreateGrid(x.intval(), y.intval());
        return NilVal();
    });

nfr("insert_column", "c", "I", "", "inserts a column before column c in an existing grid",
    [](StackPtr &, VM &, Value x) {
        si->InsertColumn(x.intval());
        return NilVal();
    });

nfr("insert_row", "r", "I", "", "inserts a row before row r in an existing grid",
    [](StackPtr &, VM &, Value x) {
        si->InsertRow(x.intval());
        return NilVal();
    });

nfr("delete", "position,size", "I}:2I}:2", "",
    "clears the cells denoted by position/size. also removes columns/rows if they become "
    "completely empty, or the entire grid.",
    [](StackPtr &sp, VM &) {
        auto s = PopVec<int2>(sp);
        auto p = PopVec<int2>(sp);
        si->Delete(p.x, p.y, s.x, s.y);
    });

nfr("set_background_color", "color", "F}:4", "", "sets the background color of the current cell",
    [](StackPtr &sp, VM &) {
        auto col = PopVec<float3>(sp);
        si->SetBackgroundColor(*(uint32_t *)quantizec(col, 0.0f).data());
    });

nfr("set_text_color", "color", "F}:4", "", "sets the text color of the current cell",
    [](StackPtr &sp, VM &) {
        auto col = PopVec<float3>(sp);
        si->SetTextColor(*(uint32_t *)quantizec(col, 0.0f).data());
    });

nfr("set_text_filtered", "filtered", "B", "", "sets the text filtered of the current cell",
    [](StackPtr &, VM &, Value filtered) {
        si->SetTextFiltered(filtered.True());
        return NilVal();
    });

nfr("is_text_filtered", "", "", "B", "whether the text of the current cell is filtered",
    [](StackPtr &, VM &) { return Value(si->IsTextFiltered()); });

nfr("set_border_color", "color", "F}:4", "", "sets the border color of the current grid",
    [](StackPtr &sp, VM &) {
        auto col = PopVec<float3>(sp);
        si->SetBorderColor(*(uint32_t *)quantizec(col, 0.0f).data());
    });

nfr("get_relative_size", "", "", "I", "returns the relative text size of the current cell",
    [](StackPtr &, VM &) { return Value(si->GetRelativeSize()); });

nfr("set_relative_size", "size", "I", "",
    "sets the relative size (0 is normal, -1 is smaller etc.) of the current cell",
    [](StackPtr &, VM &, Value s) {
        si->SetRelativeSize(geom::clamp(s.intval(), -10, 10));
        return NilVal();
    });

nfr("set_style_bits", "stylebits", "I", "",
    "sets one or more styles (bold = 1, italic = 2, fixed = 4, underline = 8,"
    " strikethru = 16) on the current cell",
    [](StackPtr &, VM &, Value s) {
        si->SetStyle(s.intval());
        return NilVal();
    });

nfr("get_style_bits", "", "", "I", "returns the stylebits of the current cell",
    [](StackPtr &, VM &) { return Value(si->GetStyle()); });

nfr("set_status_message", "message", "S", "", "sets the status message in TreeSheets",
    [](StackPtr &, VM &, Value s) {
        si->SetStatusMessage(s.sval()->strv());
        return NilVal();
    });

nfr("get_filename_from_user", "is_save", "I", "S",
    "gets a filename using a file dialog. empty string if cancelled.",
    [](StackPtr &, VM &vm, Value is_save) {
        return Value(vm.NewString(si->GetFileNameFromUser(is_save.True())));
    });

nfr("get_filename", "", "", "S", "gets the current documents file name",
    [](StackPtr &, VM &vm) { return Value(vm.NewString(si->GetFileName())); });

nfr("load_document", "filename", "S", "B",
    "loads a document, and makes it the active one. returns false if failed.",
    [](StackPtr &, VM &, Value filename) {
        return Value(si->LoadDocument(filename.sval()->data()));
    });

nfr("set_window_size", "width,height", "II", "", "resizes the window",
    [](StackPtr &, VM &, Value w, Value h) {
        si->SetWindowSize(w.intval(), h.intval());
        return NilVal();
    });

nfr("get_last_edit", "", "", "I",
    "gets the timestamp of the last edit in milliseconds since the Unix/C epoch",
    [](StackPtr &, VM &) { return Value(si->GetLastEdit()); });

nfr("get_current_time", "", "", "I",
    "gets the current timestamp in milliseconds since the Unix/C epoch", [](StackPtr &, VM &) {
        auto now = std::chrono::system_clock::now();
        auto duration = now.time_since_epoch();
        auto milliseconds = std::chrono::duration_cast<std::chrono::milliseconds>(duration).count();
        return Value(milliseconds);
    });

nfr("is_tag", "", "", "B", "whether the current cell text is a tag",
    [](StackPtr &, VM &) {
        return Value(si->IsTag());
    });

nfr("has_image", "", "", "B", "whether the current cell has an image",
    [](StackPtr &, VM &) { return Value(si->HasImage()); });

nfr("get_column_width", "", "", "I", "get the column width of the current cell",
    [](StackPtr &, VM &) {
        return Value(si->GetColWidth());
    });

nfr("set_column_width", "width", "I", "", "set the column width of the current cell",
    [](StackPtr &, VM &, Value w) {
        si->SetColWidth(w.intval());
        return NilVal();
    });

nfr("remove_image", "", "", "", "remove image in the current cell",
    [](StackPtr &, VM &) {
        si->RemoveImage();
        return NilVal();
    });

nfr("set_image", "filename", "S", "B", "set image for the current cell",
    [](StackPtr &, VM &, Value filename) { return Value(si->SetImage(filename.sval()->data())); });
}

NativeRegistry natreg;  // FIXME: global.

string InitLobster(ScriptInterface *_si, const char *exefilepath, const char *auxfilepath,
                   bool from_bundle, FileLoader sl) {
    si = _si;
    min_output_level = OUTPUT_PROGRAM;
    string err;
    try {
        InitPlatform(exefilepath, auxfilepath, from_bundle, sl);
        RegisterBuiltin(natreg, "ts", "treesheets", AddTreeSheets);
        RegisterCoreLanguageBuiltins(natreg);
    } catch (string &s) { err = s; }
    return err;
}

string RunLobster(std::string_view filename, std::string_view code, bool dump_builtins) {
    string err;
    try {
        string bytecode;
        string codegen;
        Compile(natreg, filename, code, bytecode, nullptr, nullptr, false, RUNTIME_ASSERT, nullptr,
                1, false, true, codegen, false, filename);
        auto ret = RunTCC(natreg, bytecode, filename, nullptr, {}, false, err, RUNTIME_ASSERT, true,
                          false, codegen);
    } catch (string &s) {
        err = s;
    }
    return err;
}

void TSDumpBuiltinDoc() { DumpBuiltinDoc(natreg, true); }

}  // namespace script

namespace lobster {

FileLoader EnginePreInit(NativeRegistry &nfr) {
    // nfr.DoneRegistering();
    return DefaultLoadFile;
}

}  // namespace lobster

extern "C" void GLFrame(StackPtr, VM &) {}
string BreakPoint(lobster::VM &vm, string_view reason) { return {}; }
```

## File: src/main.cpp
```cpp
#include "stdafx.h"

static const auto TS_VERSION = 25;
static const auto g_grid_margin = 1;
static const auto g_cell_margin = 2;
static const auto g_margin_extra = 2;  // TODO, could make this configurable: 0/2/4/6
static const auto g_usergridouterspacing_default = 3;
static const auto g_line_width = 1;
static const auto g_selmargin = 2;
static const auto g_scrollratecursor = 240;  // FIXME: must be configurable
static const auto g_scrollratewheel = 2;  // relative to 1 step on a fixed wheel usually being 120
static const auto g_max_launches = 20;
static const auto g_deftextsize_default = 12;
static const auto g_mintextsize_delta = 8;
static const auto g_maxtextsize_delta = 32;
static const auto BLINK_TIME = 400;
static const auto CUSTOMCOLORIDX = 0;
static const auto TS_SELECTION_MASK = 0x80;
static const auto g_bordercolor_default = 0xA0A0A0;
static const auto g_cellcolor_default = 0xFFFFFFU;
static const auto g_textcolor_default = 0x000000U;
static const auto g_tagcolor_default = 0xFF0000;
static const std::array<uint, 42> celltextcolors = {
    0xFFFFFF,  // CUSTOM COLOR!
    0xFFFFFF, 0x000000, 0x202020, 0x404040, 0x606060, 0x808080, 0xA0A0A0, 0xC0C0C0, 0xD0D0D0,
    0xE0E0E0, 0xE8E8E8, 0x000080, 0x0000FF, 0x8080FF, 0xC0C0FF, 0xC0C0E0, 0x008000, 0x00FF00,
    0x80FF80, 0xC0FFC0, 0xC0E0C0, 0x800000, 0xFF0000, 0xFF8080, 0xFFC0C0, 0xE0C0C0, 0x800080,
    0xFF00FF, 0xFF80FF, 0xFFC0FF, 0xE0C0E0, 0x008080, 0x00FFFF, 0x80FFFF, 0xC0FFFF, 0xC0E0E0,
    0x808000, 0xFFFF00, 0xFFFF80, 0xFFFFC0, 0xE0E0C0,
};

static const std::map<char, pair<wxBitmapType, wxString>> imagetypes = {
    {'I', {wxBITMAP_TYPE_PNG, "image/png"}}, {'J', {wxBITMAP_TYPE_JPEG, "image/jpeg"}}};

static auto g_deftextsize = g_deftextsize_default;
static int g_mintextsize() { return g_deftextsize - g_mintextsize_delta; }
static int g_maxtextsize() { return g_deftextsize + g_maxtextsize_delta; }

enum { TS_TEXT = 0, TS_GRID = 1, TS_BOTH = 2, TS_NEITHER = 3 };

enum {
    A_SAVEALL = 500,
    A_COLLAPSE,
    A_NEWGRID,
    A_CLRVIEW,
    A_MARKDATA,
    A_MARKVIEWH,
    A_MARKVIEWV,
    A_MARKCODE,
    A_IMAGE,
    A_EXPIMAGE,
    A_EXPSVG,
    A_EXPXML,
    A_EXPHTMLT,
    A_EXPHTMLTI,
    A_EXPHTMLTE,
    A_EXPHTMLO,
    A_EXPHTMLB,
    A_EXPTEXT,
    A_ZOOMIN,
    A_ZOOMOUT,
    A_TRANSPOSE,
    A_DELETE,
    A_BACKSPACE,
    A_DELETE_WORD,
    A_BACKSPACE_WORD,
    A_LEFT,
    A_RIGHT,
    A_UP,
    A_DOWN,
    A_MLEFT,
    A_MRIGHT,
    A_MUP,
    A_MDOWN,
    A_SLEFT,
    A_SRIGHT,
    A_SUP,
    A_SDOWN,
    A_ALEFT,
    A_ARIGHT,
    A_AUP,
    A_ADOWN,
    A_SCLEFT,
    A_SCRIGHT,
    A_SCUP,
    A_SCDOWN,
    A_IMPXML,
    A_IMPXMLA,
    A_IMPTXTI,
    A_IMPTXTC,
    A_IMPTXTS,
    A_IMPTXTT,
    A_TUTORIALWEBPAGE,
    #ifdef ENABLE_LOBSTER
        A_SCRIPTREFERENCE,
    #endif
    A_MARKVARD,
    A_MARKVARU,
    A_SHOWSBAR,
    A_SHOWTBAR,
    A_LEFTTABS,
    A_TRADSCROLL,
    A_HOME,
    A_END,
    A_CHOME,
    A_CEND,
    A_PAGESETUP,
    A_PRINTSCALE,
    A_NEXT,
    A_PREV,
    A_TT,
    A_SEARCH,
    A_CASESENSITIVESEARCH,
    A_CLEARSEARCH,
    A_CLEARREPLACE,
    A_REPLACE,
    A_REPLACEONCE,
    A_REPLACEONCEJ,
    A_REPLACEALL,
    A_CANCELEDIT,
    A_BROWSE,
    A_ENTERCELL,
    A_ENTERCELL_JUMPTOSTART,
    A_ENTERCELL_JUMPTOEND,
    A_PROGRESSCELL,
    A_CELLCOLOR,
    A_TEXTCOLOR,
    A_BORDCOLOR,
    A_INCSIZE,
    A_DECSIZE,
    A_INCWIDTH,
    A_DECWIDTH,
    A_ENTERGRID,
    A_LINK,
    A_LINKREV,
    A_LINKIMG,
    A_LINKIMGREV,
    A_SEARCHNEXT,
    A_SEARCHPREV,
    A_CUSTCOL,
    A_COLCELL,
    A_SORT,
    A_MAKEBAKS,
    A_TOTRAY,
    A_AUTOSAVE,
    A_FULLSCREEN,
    A_SCALED,
    A_SCOLS,
    A_SROWS,
    A_SHOME,
    A_SEND,
    A_BORD0,
    A_BORD1,
    A_BORD2,
    A_BORD3,
    A_BORD4,
    A_BORD5,
    A_HSWAP,
    A_TEXTGRID,
    A_TAGADD,
    A_TAGREMOVE,
    A_WRAP,
    A_HIFY,
    A_FLATTEN,
    A_BROWSEF,
    A_ROUND0,
    A_ROUND1,
    A_ROUND2,
    A_ROUND3,
    A_ROUND4,
    A_ROUND5,
    A_ROUND6,
    A_FILTER5,
    A_FILTER10,
    A_FILTER20,
    A_FILTER50,
    A_FILTERM,
    A_FILTERL,
    A_FILTERS,
    A_FILTEROFF,
    A_FILTERNOTE,
    A_FILTERBYCELLBG,
    A_FILTERMATCHNEXT,
    A_FILTERRANGE,
    A_FILTERDIALOG,
    A_FASTRENDER,
    A_INVERTRENDER,
    A_EXPCSV,
    A_PASTESTYLE,
    A_PREVFILE,
    A_NEXTFILE,
    A_IMAGER,
    A_INCWIDTHNH,
    A_DECWIDTHNH,
    A_ZOOMSCR,
    A_V_GS,
    A_V_BS,
    A_V_LS,
    A_H_GS,
    A_H_BS,
    A_H_LS,
    A_GS,
    A_BS,
    A_LS,
    A_RESETSIZE,
    A_RESETWIDTH,
    A_RESETSTYLE,
    A_RESETCOLOR,
    A_LASTCELLCOLOR,
    A_LASTTEXTCOLOR,
    A_LASTBORDCOLOR,
    A_LASTIMAGE,
    A_OPENCELLCOLOR,
    A_OPENTEXTCOLOR,
    A_OPENBORDCOLOR,
    A_OPENIMGDROPDOWN,
    A_DDIMAGE,
    A_MINCLOSE,
    A_SINGLETRAY,
    A_STARTMINIMIZED,
    A_CENTERED,
    A_SORTD,
    A_FOLD,
    A_FOLDALL,
    A_UNFOLDALL,
    A_IMAGESCP,
    A_IMAGESCW,
    A_IMAGESCF,
    A_IMAGESCN,
    A_IMAGESVA,
    A_SAVE_AS_JPEG,
    A_SAVE_AS_PNG,
    A_HELP_OP_REF,
    A_FSWATCH,
    A_DEFBGCOL,
    A_DEFCURCOL,
    A_RESETPERSPECTIVE,
    A_THINSELC,
    A_COPYCT,
    A_COPYBM,
    A_COPYWI,
    A_MINISIZE,
    A_CUSTKEY,
    A_SETLANG,
    A_AUTOEXPORT_HTML_NONE,
    A_AUTOEXPORT_HTML_WITH_IMAGES,
    A_AUTOEXPORT_HTML_WITHOUT_IMAGES,
    A_DRAGANDDROP,
    A_DEFAULTMAXCOLWIDTH,
    #ifdef ENABLE_LOBSTER
        A_ADDSCRIPT,
        A_DETSCRIPT,
    #endif
    A_SET_FIXED_FONT,
    A_EDITNOTE,
    A_NOP,
    A_TAGSET = 1000,  // and all values from here on
    #ifdef ENABLE_LOBSTER
        A_SCRIPT = 2000,  // and all values from here on
    #endif
    A_MAXACTION = 3000
};

enum {
    STYLE_BOLD = 1,
    STYLE_ITALIC = 2,
    STYLE_FIXED = 4,
    STYLE_UNDERLINE = 8,
    STYLE_STRIKETHRU = 16
};

enum { TEXT_SPACE = 3, TEXT_SEP = 2, TEXT_CHAR = 1 };

#ifdef ENABLE_LOBSTER

    // script_interface.h is both used by TreeSheets and lobster-impl
    // and uses data types that are already defined by lobster.

    // Define these data types separately on the TreeSheets side here
    // to avoid redefinitions.

    struct string_view_nt {
        string_view sv;
        string_view_nt(const string &s) : sv(s) {}
        explicit string_view_nt(const char *s) : sv(s) {}
        explicit string_view_nt(string_view osv) : sv(osv) { check_null_terminated(); }
        void check_null_terminated() const { assert(!sv.data()[sv.size()]); }
        size_t size() const { return sv.size(); }
        const char *data() const { return sv.data(); }
        const char *c_str() const {
            check_null_terminated();  // Catch appends to parent buffer since construction.
            return sv.data();
        }
    };

    using FileLoader = int64_t (*)(string_view_nt absfilename, std::string *dest, int64_t start,
                                   int64_t len);

    #include "script_interface.h"

    using namespace script;

#endif

struct treesheets {
    #ifdef ENABLE_LOBSTER
        struct TreeSheetsScriptImpl;
    #endif

    struct Image;
    struct Text;
    struct Cell;
    struct Grid;
    struct Selection;
    struct Document;
    struct Evaluator;

    struct System;
    struct TSCanvas;
    struct TSFrame;
    struct TSApp;

    static System *sys;

    #ifdef ENABLE_LOBSTER
        #include "treesheets_impl.h"
    #endif

    #include "image.h"
    #include "text.h"
    #include "cell.h"
    #include "grid.h"
    #include "selection.h"
    #include "document.h"
    #include "evaluator.h"

    #include "system.h"

    #include "wxtools.h"
    #include "tscanvas.h"
    #include "tsframe.h"
    #include "tsapp.h"
};

treesheets::System *treesheets::sys = nullptr;
#ifdef ENABLE_LOBSTER
    treesheets::TreeSheetsScriptImpl treesheets::tssi;
#endif

IMPLEMENT_APP(treesheets::TSApp)

#include "events.h"
```

## File: src/pot_update.sh
```bash
#!/bin/sh
xgettext -j --keyword=_ --sort-output --no-location -o ../TS/translations/ts.pot tsframe.h document.h system.h wxtools.h
```

## File: src/script_interface.h
```c
namespace script {

typedef std::pair<int, int> icoord;
typedef std::pair<icoord, icoord> ibox;

struct ScriptInterface {
    virtual bool LoadDocument(const char *filename) = 0;
    virtual void GoToRoot() = 0;
    virtual void GoToView() = 0;
    virtual bool HasSelection() = 0;
    virtual void GoToSelection() = 0;
    virtual bool HasParent() = 0;
    virtual void GoToParent() = 0;
    virtual int NumChildren() = 0;
    virtual icoord NumColumnsRows() = 0;
    virtual ibox SelectionBox() = 0;
    virtual void GoToChild(int n) = 0;
    virtual void GoToColumnRow(int x, int y) = 0;
    virtual std::string GetText() = 0;
    virtual std::string GetNote() = 0;
    virtual void SetText(std::string_view t) = 0;
    virtual void SetNote(std::string_view t) = 0;
    virtual void CreateGrid(int x, int n) = 0;
    virtual void InsertColumn(int x) = 0;
    virtual void InsertRow(int y) = 0;
    virtual void Delete(int x, int y, int xs, int ys) = 0;
    virtual void SetBackgroundColor(uint32_t col) = 0;
    virtual void SetTextColor(uint32_t col) = 0;
    virtual void SetTextFiltered(bool filtered) = 0;
    virtual bool IsTextFiltered() = 0;
    virtual void SetBorderColor(uint32_t col) = 0;
    virtual int GetRelativeSize() = 0;
    virtual void SetRelativeSize(int s) = 0;
    virtual void SetStyle(int s) = 0;
    virtual int GetStyle() = 0;
    virtual int GetColWidth() = 0;
    virtual void SetColWidth(int w) = 0;
    virtual void SetStatusMessage(std::string_view message) = 0;
    virtual void SetWindowSize(int width, int height) = 0;
    virtual std::string GetFileNameFromUser(bool is_save) = 0;
    virtual std::string GetFileName() = 0;
    virtual int64_t GetLastEdit() = 0;
    virtual bool IsTag() = 0;
    virtual bool HasImage() = 0;
    virtual void RemoveImage() = 0;
    virtual bool SetImage(std::string_view filename) = 0;
    virtual ~ScriptInterface() {};
};

extern std::string InitLobster(ScriptInterface *_si, const char *exefilepath,
                               const char *auxfilepath, bool from_bundle, FileLoader sl);
extern std::string RunLobster(std::string_view filename, std::string_view code, bool dump_builtins);
extern void TSDumpBuiltinDoc();

}  // namespace script
```

## File: src/selection.h
```c
class Selection {
    bool textedit {false};

    public:
    Grid *grid;
    int x;
    int y;
    int xs;
    int ys;
    int cursor {0};
    int cursorend {0};
    int firstdx {0};
    int firstdy {0};

    Selection(Grid *_grid = nullptr, int _x = 0, int _y = 0, int _xs = 0, int _ys = 0)
        : grid(_grid), x(_x), y(_y), xs(_xs), ys(_ys) {}

    void SelAll() {
        if (textedit) {
            cursor = 0;
            cursorend = MaxCursor();
        } else {
            x = y = 0;
            xs = grid->xs;
            ys = grid->ys;
        }
    }

    Cell *GetCell() const { return grid && xs == 1 && ys == 1 ? grid->C(x, y).get() : nullptr; }
    Cell *GetFirst() const { return grid && xs >= 1 && ys >= 1 ? grid->C(x, y).get() : nullptr; }
    bool EqLoc(const Selection &s) {
        return grid == s.grid && x == s.x && y == s.y && xs == s.xs && ys == s.ys;
    }
    bool operator==(Selection &s) {
        return EqLoc(s) && cursor == s.cursor && cursorend == s.cursorend;
    }
    bool Thin() const { return !(xs * ys); }
    bool IsAll() const { return xs == grid->xs && ys == grid->ys; }
    void SetCursorEdit(Document *doc, bool edit) {
        wxCursor c(edit ? wxCURSOR_IBEAM : wxCURSOR_ARROW);
        #ifdef WIN32
        // this changes the cursor instantly, but gets overridden by the local window cursor
        ::SetCursor((HCURSOR)c.GetHCURSOR());
        #endif
        // this doesn't change the cursor immediately, only on mousemove:
        doc->canvas->SetCursor(c);

        firstdx = firstdy = 0;
    }

    bool TextEdit() { return textedit; }
    void EnterEditOnly(Document *doc) {
        textedit = true;
        SetCursorEdit(doc, true);
    }
    void EnterEdit(Document *doc, int cursor = 0, int cursorend = 0) {
        EnterEditOnly(doc);
        this->cursor = cursor;
        this->cursorend = cursorend;
    }
    void ExitEdit(Document *doc) {
        textedit = false;
        cursor = cursorend = 0;
        SetCursorEdit(doc, false);
    }

    bool IsInside(Selection &o) {
        if (!o.grid || !grid) return false;
        if (grid != o.grid)
            return grid->cell->parent && grid->cell->parent->grid->FindCell(grid->cell).IsInside(o);
        return x >= o.x && y >= o.y && x + xs <= o.x + o.xs && y + ys <= o.y + o.ys;
    }

    void Merge(const Selection &a, const Selection &b) {
        textedit = false;
        if (a.grid == b.grid) {
            if (a.GetCell() == b.GetCell() && a.GetCell() && (a.textedit || b.textedit)) {
                if (a.cursor != a.cursorend) {
                    Selection c = b;
                    a.GetCell()->text.SelectWord(c);
                    cursor = min(a.cursor, c.cursor);
                    cursorend = max(a.cursorend, c.cursorend);
                } else {
                    cursor = min(a.cursor, b.cursor);
                    cursorend = max(a.cursor, b.cursor);
                }
                textedit = true;
            } else {
                cursor = cursorend = 0;
            }
        } else {
            auto at = a.GetCell();
            auto bt = b.GetCell();
            int ad = at->Depth();
            int bd = bt->Depth();
            int i = 0;
            while (i < ad && i < bd && at->Parent(ad - i) == bt->Parent(bd - i)) i++;
            auto g = at->Parent(ad - i + 1)->grid;
            Merge(g->FindCell(at->Parent(ad - i)), g->FindCell(bt->Parent(bd - i)));
            return;
        }
        grid = a.grid;
        x = min(a.x, b.x);
        y = min(a.y, b.y);
        xs = abs(a.x - b.x) + 1;
        ys = abs(a.y - b.y) + 1;
    }

    int MaxCursor() { return int(GetCell()->text.t.Len()); }

    inline bool IsWordSep(wxChar ch) {
        // represents: !"#$%&'()*+,-./    :;<=>?@    [\]^    {|}~    `
        return 32 < ch && ch < 48 || 57 < ch && ch < 65 || 90 < ch && ch < 95 ||
               122 < ch && ch < 127 || ch == 96;
    }

    inline int CharType(wxChar ch) {
        if (wxIsspace(ch)) return TEXT_SPACE;
        if (IsWordSep(ch)) return TEXT_SEP;
        return TEXT_CHAR;
    }

    void Dir(Document *doc, bool ctrl, bool shift, int dx, int dy, int &v, int &vs, int &ovs,
             bool notboundaryperp, bool notboundarypar, bool exitedit) {
        if (ctrl && !textedit) {
            grid->cell->AddUndo(doc);

            grid->Move(dx, dy, *this);
            x = (x + dx + grid->xs) % grid->xs;
            y = (y + dy + grid->ys) % grid->ys;
            if (x + xs > grid->xs || y + ys > grid->ys) grid = nullptr;

            // FIXME: this is null in the case of a whole column selection, and doesn't do the right
            // thing.
            if (grid) grid->cell->ResetChildren();
            doc->paintscrolltoselection = true;
            doc->canvas->Refresh();
        } else {
            if (ctrl && dx)  // implies textedit
            {
                if (cursor == cursorend) firstdx = dx;
                int &curs = firstdx < 0 ? cursor : cursorend;
                int c = curs + dx;
                wxChar ch;
                if (c >= 0 && c <= MaxCursor()) {
                    ch = GetCell()->text.t[min(c, curs)];
                    // TEXT_SPACE > TEXT_SEP > TEXT_CHAR > 0.
                    // Accepts smaller or equal type when positive, only equal when negative.
                    // in regex terms (space/sep/char = s/p/c): match (s+p*|s+c*|p+c*|c+)
                    int allowed = CharType(ch);
                    curs = c;
                    for (;;) {
                        c += dx;
                        if (c < 0 || c > MaxCursor()) break;
                        ch = GetCell()->text.t[min(c, curs)];
                        int chtype = CharType(ch);
                        // type increase when positive or type change when negative => break
                        if (chtype > allowed && chtype != -allowed) break;
                        curs = c;
                        // type decrease when positive => negate
                        if (chtype < allowed) allowed = -chtype;
                    }
                }
                if (shift) {
                    if (cursorend < cursor) swap_(cursorend, cursor);
                } else
                    cursorend = cursor = curs;
            } else if (shift) {
                if (textedit) {
                    if (cursor == cursorend) firstdx = dx;
                    (firstdx < 0 ? cursor : cursorend) += dx;
                    if (cursor < 0) cursor = 0;
                    if (cursorend > MaxCursor()) cursorend = MaxCursor();
                } else {
                    if (!xs) firstdx = 0;  // redundant: just in case someone else changed it
                    if (!ys) firstdy = 0;
                    if (!firstdx) firstdx = dx;
                    if (!firstdy) firstdy = dy;
                    if (firstdx < 0) {
                        x += dx;
                        xs += -dx;
                    } else
                        xs += dx;
                    if (firstdy < 0) {
                        y += dy;
                        ys += -dy;
                    } else
                        ys += dy;
                    if (x < 0) {
                        x = 0;
                        xs--;
                    }
                    if (y < 0) {
                        y = 0;
                        ys--;
                    }
                    if (x + xs > grid->xs) xs--;
                    if (y + ys > grid->ys) ys--;
                    if (!xs) firstdx = 0;
                    if (!ys) firstdy = 0;
                    if (!xs && !ys) grid = nullptr;
                }
            } else {
                if (vs) {
                    if (ovs)  // (multi) cell selection
                    {
                        bool intracell = true;
                        if (textedit && !exitedit && GetCell()) {
                            if (dy) {
                                cursorend = cursor;
                                auto &text = GetCell()->text;
                                int maxcolwidth = GetCell()->parent->grid->colwidths[x];

                                int i = 0;
                                int laststart, lastlen;
                                int nextoffset = -1;
                                for (int l = 0;; l++) {
                                    int start = i;
                                    auto ls = text.GetLine(i, maxcolwidth);
                                    auto len = static_cast<int>(ls.Len());
                                    int end = start + len;

                                    if (len && nextoffset >= 0) {
                                        cursor = cursorend =
                                            start + (nextoffset > len ? len : nextoffset);
                                        intracell = false;
                                        break;
                                    }

                                    if (cursor >= start && cursor <= end) {
                                        if (dy < 0) {
                                            if (l != 0) {
                                                cursor = cursorend =
                                                    laststart + (cursor - start > lastlen
                                                                     ? lastlen
                                                                     : cursor - start);
                                                intracell = false;
                                            }
                                            break;
                                        } else {
                                            nextoffset = cursor - start;
                                        }
                                    }

                                    laststart = start;
                                    lastlen = len;

                                    if (!len) break;
                                }
                            } else {
                                intracell = false;
                                if (cursor != cursorend) {
                                    if (dx < 0)
                                        cursorend = cursor;
                                    else
                                        cursor = cursorend;
                                } else {
                                    if ((dx < 0 && cursor) || (dx > 0 && MaxCursor() > cursor))
                                        cursorend = cursor += dx;
                                }
                            }
                        }

                        if (intracell) {
                            if (sys->thinselc) {
                                if (dx + dy > 0) v += vs;
                                vs = 0;  // make it a thin selection, in direction
                                ovs = 1;
                            } else {
                                if (x + dx >= 0 && x + dx + xs <= grid->xs && y + dy >= 0 &&
                                    y + dy + ys <= grid->ys) {
                                    x += dx;
                                    y += dy;
                                }
                            }
                            ExitEdit(doc);
                        }
                    } else if (notboundarypar)  // thin selection, moving in parallel direction
                    {
                        v += dx + dy;
                    }
                } else if (notboundaryperp)  // thin selection, moving in perpendicular direction
                {
                    if (dx + dy < 0) v--;
                    vs = 1;  // make it a cell selection
                } else {     // selection cycle, jump to the opposite side of the grid
                    if (y + dy > grid->ys) {
                        y = 0;
                        vs = 1;
                    } else if (y + dy < 0) {
                        y = grid->ys - 1;
                        vs = 1;
                    } else if (x + dx > grid->xs) {
                        x = 0;
                        vs = 1;
                    } else if (x + dx < 0) {
                        x = grid->xs - 1;
                        vs = 1;
                    }
                };
            }
            doc->paintscrolltoselection = true;
            doc->canvas->Refresh();
        };
    }

    void Cursor(Document *doc, int action, bool ctrl, bool shift, bool exitedit = false) {
        switch (action) {
            case A_UP: Dir(doc, ctrl, shift, 0, -1, y, ys, xs, y != 0, y != 0, exitedit); break;
            case A_DOWN:
                Dir(doc, ctrl, shift, 0, 1, y, ys, xs, y < grid->ys, y < grid->ys - 1, exitedit);
                break;
            case A_LEFT: Dir(doc, ctrl, shift, -1, 0, x, xs, ys, x != 0, x != 0, exitedit); break;
            case A_RIGHT:
                Dir(doc, ctrl, shift, 1, 0, x, xs, ys, x < grid->xs, x < grid->xs - 1, exitedit);
                break;
        }
        sys->frame->UpdateStatus(doc->selected, true);
    }

    void Next(Document *doc, bool backwards) {
        ExitEdit(doc);
        if (backwards) {
            if (x > 0)
                x--;
            else if (y > 0) {
                y--;
                x = grid->xs - 1;
            } else {
                x = grid->xs - 1;
                y = grid->ys - 1;
            }
        } else {
            if (x < grid->xs - 1)
                x++;
            else if (y < grid->ys - 1) {
                y++;
                x = 0;
            } else
                x = y = 0;
        }
        EnterEdit(doc, 0, MaxCursor());
        doc->paintscrolltoselection = true;
        doc->canvas->Refresh();
    }

    wxString Wrap(Document *doc) {
        if (Thin()) return doc->NoThin();
        grid->cell->AddUndo(doc);
        Cell *np = grid->CloneSel(*this).release();
        grid->C(x, y)->text.t = ".";  // avoid this cell getting deleted
        if (xs > 1) {
            Selection s(grid, x + 1, y, xs - 1, ys);
            grid->MultiCellDeleteSub(doc, s);
        }
        if (ys > 1) {
            Selection s(grid, x, y + 1, 1, ys - 1);
            grid->MultiCellDeleteSub(doc, s);
        }
        Cell *old = grid->C(x, y).get();
        np->text.relsize = old->text.relsize;
        np->CloneStyleFrom(old);
        grid->C(x, y).reset(np);
        np->parent = grid->cell;
        xs = ys = 1;
        EnterEdit(doc, MaxCursor(), MaxCursor());
        doc->canvas->Refresh();
        return wxEmptyString;
    }

    Cell *ThinExpand(Document *doc, bool jumptofirst = false) {
        if (Thin()) {
            if (xs) {
                grid->cell->AddUndo(doc);
                grid->InsertCells(-1, y, 0, 1);
                ys = 1;
                if (jumptofirst) x = 0;
            } else {
                grid->cell->AddUndo(doc);
                grid->InsertCells(x, -1, 1, 0);
                xs = 1;
                if (jumptofirst) y = 0;
            }
        }
        return GetCell();
    }

    void HomeEnd(Document *doc, bool ishome) {
        xs = ys = 1;
        if (ishome)
            x = y = 0;
        else {
            x = grid->xs - 1;
            y = grid->ys - 1;
        }
        doc->paintscrolltoselection = true;
        doc->canvas->Refresh();
    }
};
```

## File: src/stdafx.cpp
```cpp
#include "stdafx.h"
```

## File: src/stdafx.h
```c
#pragma once

#ifdef _WIN32
    #define _CRT_SECURE_NO_WARNINGS
    #define _SCL_SECURE_NO_WARNINGS
#endif

#include <ctype.h>
#include <wx/aboutdlg.h>
#include <wx/clipbrd.h>
#include <wx/confbase.h>
#include <wx/config.h>
#include <wx/datstrm.h>
#include <wx/dcbuffer.h>
#include <wx/dir.h>
#include <wx/dnd.h>
#include <wx/fileconf.h>
#include <wx/numdlg.h>
#include <wx/tokenzr.h>
#include <wx/txtstrm.h>
#include <wx/wfstream.h>
#include <wx/wx.h>
#include <wx/zstream.h>

#ifdef _WIN32
    #include <WinUser.h>
    #include <imm.h>
    #ifdef _MSC_VER
        #pragma comment(lib, "imm32")
    #endif
    #include <wx/msw/dc.h>
    #include <wx/msw/regconf.h>
#endif

#include <wx/aui/aui.h>
#include <wx/base64.h>
#include <wx/bmpbndl.h>
#include <wx/colordlg.h>
#include <wx/datectrl.h>
#include <wx/dcsvg.h>
#include <wx/display.h>
#include <wx/docview.h>
#include <wx/filename.h>
#include <wx/fontdlg.h>
#include <wx/fswatcher.h>
#include <wx/ipc.h>
#include <wx/mstream.h>
#include <wx/notebook.h>
#include <wx/odcombo.h>
#include <wx/print.h>
#include <wx/printdlg.h>
#include <wx/sizer.h>
#include <wx/snglinst.h>
#include <wx/srchctrl.h>
#include <wx/stdpaths.h>
#include <wx/sysopt.h>
#include <wx/taskbar.h>
#include <wx/timectrl.h>
#include <wx/translation.h>
#include <wx/uilocale.h>
#include <wx/xml/xml.h>

#include <algorithm>
#include <array>
#include <clocale>
#include <condition_variable>
#include <filesystem>
#include <functional>
#include <future>
#include <locale>
#include <map>
#include <memory>
#include <mutex>
#include <new>
#include <queue>
#include <set>
#include <sstream>
#include <stdexcept>
#include <string>
#include <string_view>
#include <thread>
#include <utility>
#include <vector>

#include "threadpool.h"
#include "tools.h"

#ifdef __WXMAC__
    #include <mach-o/dyld.h>
#endif

using namespace std;
```

## File: src/system.h
```c
struct System {
    TSFrame *frame;
    wxString defaultfont {
        #ifdef WIN32
            "Lucida Sans Unicode"
        #else
            "Verdana"
        #endif
    };
    wxString defaultfixedfont {"Courier New"};
    wxString defaultlang {wxEmptyString};
    wxString searchstring;
    unique_ptr<wxConfigBase> cfg;
    Evaluator evaluator;
    wxString clipboardcopy;
    unique_ptr<Cell> cellclipboard;
    vector<unique_ptr<Image>> imagelist;
    vector<int> loadimageids;
    uchar versionlastloaded {0};
    wxLongLong fakelasteditonload;
    wxPen pen_tinytext {wxColour(0x808080ul)};
    wxPen pen_gridborder {wxColour(0xb5a6a4)};
    wxPen pen_tinygridlines {wxColour(0xf2dcd8)};
    wxPen pen_gridlines {wxColour(0xe5b7b0)};
    wxPen pen_thinselect {*wxLIGHT_GREY};
    int roundness {3};
    int defaultmaxcolwidth {80};
    bool makebaks {true};
    bool totray {false};
    bool autosave {true};
    bool zoomscroll {false};
    bool thinselc {true};
    bool minclose {false};
    bool singletray {false};
    bool startminimized {false};
    bool centered {true};
    bool fswatch {true};
    int autohtmlexport {0};
    bool casesensitivesearch {true};
    bool darkennonmatchingcells {false};
    bool fastrender {true};
    bool showtoolbar {true};
    bool showstatusbar {true};
    bool followdarkmode {false};
    uint colormask {0};
    int notesizex {300};
    int notesizey {255};
    std::set<wxString> watchedpaths;
    bool insidefiledialog {false};
    struct TimerStruct : wxTimer {
        void Notify() {
            sys->SaveCheck();
            sys->cfg->Flush();
        }
    } every_second_timer;
    int lastcellcolor {0xFFFFFF};
    int lasttextcolor {0};
    int lastbordcolor {0xA0A0A0};
    Image *lastimage {nullptr};
    int customcolor {0xFFFFFF};
    int cursorcolor {0x00FF00};

    System(bool portable)
        : cfg(portable
                  ? (wxConfigBase *)new wxFileConfig("", "", wxGetCwd() + "/TreeSheets.ini", "", 0)
                  : (wxConfigBase *)new wxConfig("TreeSheets")) {
        static const wxDash glpattern[] = {1, 3};
        pen_gridlines.SetDashes(2, glpattern);
        pen_gridlines.SetStyle(wxPENSTYLE_USER_DASH);
        static const wxDash tspattern[] = {2, 4};
        pen_thinselect.SetDashes(2, tspattern);
        pen_thinselect.SetStyle(wxPENSTYLE_USER_DASH);

        roundness = static_cast<int>(cfg->Read("roundness", roundness));
        autohtmlexport = static_cast<int>(cfg->Read("autohtmlexport", autohtmlexport));
        defaultfont = cfg->Read("defaultfont", defaultfont);
        defaultfixedfont = cfg->Read("defaultfixedfont", defaultfixedfont);
        defaultlang = cfg->Read("defaultlang", defaultlang);
        cfg->Read("defaultmaxcolwidth", &defaultmaxcolwidth, defaultmaxcolwidth);
        cfg->Read("makebaks", &makebaks, makebaks);
        cfg->Read("totray", &totray, totray);
        cfg->Read("zoomscroll", &zoomscroll, zoomscroll);
        cfg->Read("thinselc", &thinselc, thinselc);
        cfg->Read("autosave", &autosave, autosave);
        cfg->Read("fastrender", &fastrender, fastrender);
        cfg->Read("followdarkmode", &followdarkmode, followdarkmode);
        cfg->Read("minclose", &minclose, minclose);
        cfg->Read("singletray", &singletray, singletray);
        cfg->Read("startminimized", &startminimized, startminimized);
        cfg->Read("centered", &centered, centered);
        cfg->Read("fswatch", &fswatch, fswatch);
        cfg->Read("casesensitivesearch", &casesensitivesearch, casesensitivesearch);
        cfg->Read("defaultfontsize", &g_deftextsize, g_deftextsize);
        cfg->Read("customcolor", &customcolor, customcolor);
        cfg->Read("cursorcolor", &cursorcolor, cursorcolor);
        cfg->Read("showtoolbar", &showtoolbar, showtoolbar);
        cfg->Read("showstatusbar", &showstatusbar, showstatusbar);
        cfg->Read("notesizex", &notesizex, notesizex);
        cfg->Read("notesizey", &notesizey, notesizey);
        cfg->Read("lastcellcolor", &lastcellcolor, lastcellcolor);
        cfg->Read("lasttextcolor", &lasttextcolor, lasttextcolor);
        cfg->Read("lastbordcolor", &lastbordcolor, lastbordcolor);
        // fsw.Connect(wxID_ANY, wxID_ANY, wxEVT_FSWATCHER,
        // wxFileSystemWatcherEventHandler(System::OnFileChanged));
        colormask = (followdarkmode && wxSystemSettings::GetAppearance().IsDark()) ? 0x00FFFFFF : 0;
    }

    Document *NewTabDoc(bool append = false, int insert_at = -1) {
        auto doc = make_unique<Document>();
        auto canvas = frame->NewTab(std::move(doc), append, insert_at);
        return canvas->doc.get();
    }

    void TabChange(Document *newdoc) {
        // SetSelect(hover = Selection());
        newdoc->canvas->SetFocus();
        newdoc->UpdateFileName();
    }

    void Init(const wxString &filename) {
        evaluator.Init();

        auto numfiles = static_cast<int>(cfg->Read("numopenfiles", static_cast<long>(0)));
        wxString focusfile = cfg->Read("lastopenfile", "");
        int selection = -1;
        loop(i, numfiles) {
            wxString filename;
            cfg->Read(wxString::Format("lastopenfile_%d", i), &filename);
            if (!LoadDB(filename) && filename == focusfile) selection = i;
        }

        if (!filename.IsEmpty()) {
            LoadDB(filename);
        } else if (selection >= 0) {
            frame->notebook->SetSelection(selection);
        }

        if (!frame->notebook->GetPageCount()) LoadTutorial();

        if (!frame->notebook->GetPageCount()) InitDB(10);

        // Refresh();
        every_second_timer.Start(1000);
    }

    void LoadTutorial() {
        auto trans = wxTranslations::Get();
        auto language = trans ? trans->GetBestTranslation("ts") : wxString("");

        if (language.Len() == 5 &&
            !LoadDB(frame->app->GetDocPath("examples/tutorial-" + language + ".cts"))[0]) {
            return;
        }

        language.Truncate(2);
        if (language.Len() == 2 &&
            !LoadDB(frame->app->GetDocPath("examples/tutorial-" + language + ".cts"))[0]) {
            return;
        }

        LoadDB(frame->app->GetDocPath("examples/tutorial.cts"));
    }

    void LoadOpRef() { LoadDB(frame->app->GetDocPath("examples/operation-reference.cts")); }

    unique_ptr<Cell> &InitDB(int sizex, int sizey = 0) {
        unique_ptr<Cell> c = make_unique<Cell>(nullptr, nullptr, CT_DATA, new Grid(sizex, sizey ? sizey : sizex));
        c->cellcolor = 0xCCDCE2;
        c->grid->InitCells();
        auto doc = NewTabDoc();
        doc->InitWith(std::move(c), "", nullptr, 1, 1);
        return doc->root;
    }

    wxString BakName(const wxString &filename) { return ExtName(filename, ".bak"); }
    wxString TmpName(const wxString &filename) { return ExtName(filename, ".tmp"); }
    wxString NewName(const wxString &filename) { return filename + ".new"; }
    wxString ExtName(const wxString &filename, auto ext) {
        wxFileName fn(filename);
        return fn.GetPathWithSep() + fn.GetName() + ext;
    }

    wxString LoadDB(const wxString &filename, bool fromreload = false, int insert_at = -1) {
        auto fn = filename;
        auto loadedfromtmp = false;

        if (!fromreload) {
            if (frame->GetTabByFileName(filename))
                return wxEmptyString;  //"this file is already loaded";

            if (::wxFileExists(TmpName(filename))) {
                if (::wxMessageBox(
                        _("A temporary autosave file exists, would you like to load it instead?"),
                        _("Autosave load"), wxYES_NO, frame) == wxYES) {
                    fn = TmpName(filename);
                    loadedfromtmp = true;
                }
            }
        }

        Document *doc = nullptr;
        auto anyimagesfailed = false;
        auto start_loading_time = wxGetLocalTimeMillis();
        int zoomlevel = 0;

        {  // limit destructors
            wxBusyCursor wait;
            Cell *ics = nullptr;
            wxFFileInputStream fis(fn);
            wxDataInputStream dis(fis);
            if (!fis.IsOk()) {
                for (int i = 0, n = frame->filehistory.GetCount(); i < n; i++) {
                    if (frame->filehistory.GetHistoryFile(i) == filename)
                        frame->filehistory.RemoveFileFromHistory(i);
                }
                return _("Cannot open file.");
            }

            char buf[4];
            fis.Read(buf, 4);
            if (strncmp(buf, "TSFF", 4)) return _("Not a TreeSheets file.");
            fis.Read(&versionlastloaded, 1);
            if (versionlastloaded > TS_VERSION) return _("File of newer version.");
            auto xs = versionlastloaded >= 21 ? dis.Read8() : 1;
            auto ys = versionlastloaded >= 21 ? dis.Read8() : 1;
            zoomlevel = versionlastloaded >= 23 ? dis.Read8() : 0;
            fakelasteditonload = wxDateTime::Now().GetValue();

            loadimageids.clear();

            for (;;) {
                fis.Read(buf, 1);
                switch (*buf) {
                    case 'I':
                    case 'J': {
                        char iti = *buf;
                        if (!imagetypes.contains(iti))
                            return _("Found an image type that is not defined in this program.");
                        if (versionlastloaded < 9) dis.ReadString();
                        auto sc = versionlastloaded >= 19 ? dis.ReadDouble() : 1.0;
                        vector<uint8_t> image_data;
                        if (versionlastloaded >= 22) {
                            auto imagelen = (size_t)dis.Read64();
                            image_data.resize(imagelen);
                            fis.Read(image_data.data(), imagelen);
                        } else {
                            off_t beforeimage = fis.TellI();

                            if (iti == 'I') {
                                uchar header[8];
                                fis.Read(header, 8);
                                uchar expected[] = {0x89, 'P', 'N', 'G', '\r', '\n', 0x1A, '\n'};
                                if (memcmp(header, expected, 8)) return _("Corrupt PNG header.");
                                dis.BigEndianOrdered(true);
                                for (;;) {  // Skip all chunks.
                                    wxInt32 len = dis.Read32();
                                    char fourcc[4];
                                    fis.Read(fourcc, 4);
                                    fis.SeekI(len, wxFromCurrent);  // skip data
                                    dis.Read32();                   // skip CRC
                                    if (memcmp(fourcc, "IEND", 4) == 0) break;
                                }
                            } else if (iti == 'J') {
                                wxImage im;
                                im.LoadFile(fis);
                                if (!im.IsOk()) { return _("JPEG file is corrupted!"); }
                            }

                            off_t afterimage = fis.TellI();
                            fis.SeekI(beforeimage);
                            auto sz = afterimage - beforeimage;
                            image_data.resize(sz);
                            fis.Read(image_data.data(), sz);
                            fis.SeekI(afterimage);
                        }
                        if (!fis.IsOk()) image_data.clear();

                        loadimageids.push_back(AddImageToList(sc, std::move(image_data), iti));
                        break;
                    }

                    case 'D': {
                        wxZlibInputStream zis(fis);
                        if (!zis.IsOk()) return _("Cannot decompress file.");
                        wxDataInputStream dis(zis);
                        auto numcells = 0, textbytes = 0;
                        unique_ptr<Cell> root(Cell::LoadWhich(dis, nullptr, numcells, textbytes, ics));
                        if (!root) return _("File corrupted!");

                        doc = NewTabDoc(true, insert_at);
                        if (loadedfromtmp) {
                            doc->undolistsizeatfullsave =
                                -1;  // if not, user will lose tmp without warning when he closes
                            doc->modified = true;
                        }
                        doc->InitWith(std::move(root), filename, ics, xs, ys);

                        if (versionlastloaded >= 11) {
                            for (;;) {
                                auto tag = dis.ReadString();
                                if (!tag.Len()) break;
                                doc->tags[tag] =
                                    versionlastloaded >= 24 ? dis.Read32() : g_tagcolor_default;
                            }
                        }

                        auto end_loading_time = wxGetLocalTimeMillis();

                        frame->SetStatus(wxString::Format(
                            _("Loaded %s (%d cells, %d characters) in %lld milliseconds."),
                            filename, numcells, textbytes, end_loading_time - start_loading_time));

                        goto done;
                    }

                    default: return _("Corrupt block header.");
                }
            }
        }

    done:

        doc->RefreshImageRefCount(false);
        {
            ThreadPool pool(std::thread::hardware_concurrency());
            for (const auto &image : sys->imagelist) {
                pool.enqueue(
                    [](auto img) {
                        if (img->trefc) img->Display();
                    },
                    image.get());
            }
        }  // wait until all tasks are finished

        FileUsed(filename, doc);
        doc->Zoom(zoomlevel, true);
        if (anyimagesfailed)
            wxMessageBox(_("PNG decode failed on some images in this document\nThey have been replaced by red squares."),
                         _("PNG decoder failure"), wxOK, frame);

        return wxEmptyString;
    }

    void FileUsed(const wxString &filename, Document *doc) {
        frame->filehistory.AddFileToHistory(filename);
        if (fswatch) {
            doc->lastmodificationtime = wxFileName(filename).GetModificationTime();
            const auto &directorypath = wxFileName(filename).GetPath(wxPATH_GET_VOLUME | wxPATH_GET_SEPARATOR);
            if (watchedpaths.insert(directorypath).second) {
                frame->watcher->Add(wxFileName(directorypath), wxFSW_EVENT_ALL);
            }
        }
    }

    wxString Open(const wxString &filename) {
        if (!filename.empty()) {
            auto msg = LoadDB(filename);
            if (!msg.IsEmpty()) wxMessageBox(msg, filename, wxOK, frame);
            return msg;
        }
        return _("Open file cancelled.");
    }

    void RememberOpenFiles() {
        cfg->Write("lastopenfile", frame->GetCurrentTab()->doc->filename);
        auto namedfiles = 0;
        for(auto i: frame->notebook->GetPagesInDisplayOrder(frame->notebook->GetActiveTabCtrl())) {
            auto canvas = static_cast<TSCanvas *>(frame->notebook->GetPage(i));
            if (canvas->doc->filename.Len()) {
                cfg->Write(wxString::Format("lastopenfile_%d", namedfiles), canvas->doc->filename);
                namedfiles++;
            }
        }

        cfg->Write("numopenfiles", namedfiles);
        cfg->Flush();
    }

    void SaveCheck() {
        loop(i, frame->notebook->GetPageCount()) {
            static_cast<TSCanvas *>(frame->notebook->GetPage(i))
                ->doc->AutoSave(!frame->IsActive(), i);
        }
    }

    void SaveAll() {
        loop(i, frame->notebook->GetPageCount()) {
            frame->GetCurrentTab()->doc->Save(false);
            frame->CycleTabs(1);
        }
    }

    wxString Import(const wxString &filename, int action) {
        if (!filename.empty()) {
            wxBusyCursor wait;
            switch (action) {
                case A_IMPXML:
                case A_IMPXMLA: {
                    wxXmlDocument doc;
                    if (!doc.Load(filename)) goto problem;
                    unique_ptr<Cell> &root = InitDB(1);
                    Cell *c = root->grid->cells[0].get();
                    FillXML(c, doc.GetRoot(), action == A_IMPXMLA);
                    if (!c->HasText() && c->grid) {
                        unique_ptr<Cell> child = std::move(root->grid->cells[0]);
                        root = std::move(child);
                        root->parent = nullptr;
                    }
                    break;
                }
                case A_IMPTXTI:
                case A_IMPTXTC:
                case A_IMPTXTS:
                case A_IMPTXTT: {
                    wxFFile file(filename);
                    if (!file.IsOpened()) goto problem;
                    wxString content;
                    if (!file.ReadAll(&content)) goto problem;
                    const auto &lines = wxStringTokenize(content, LINE_SEPARATOR);

                    if (lines.size()) switch (action) {
                            case A_IMPTXTI: {
                                FillRows(InitDB(1)->grid, lines, CountCol(lines[0]), 0, 0);
                            }; break;
                            case A_IMPTXTC:
                                InitDB(1, static_cast<int>(lines.size()))
                                    ->grid->CSVImport(lines, L',');
                                break;
                            case A_IMPTXTS:
                                InitDB(1, static_cast<int>(lines.size()))
                                    ->grid->CSVImport(lines, L';');
                                break;
                            case A_IMPTXTT:
                                InitDB(1, static_cast<int>(lines.size()))
                                    ->grid->CSVImport(lines, L'\t');
                                break;
                        }
                    break;
                }
            }
            Document *doc = frame->GetCurrentTab()->doc.get();
            doc->modified = true;
            doc->UpdateFileName();
            doc->selected = Selection();
            doc->begindrag = Selection();
            doc->canvas->Refresh();
        }
        return wxEmptyString;
    problem:
        wxMessageBox(_("couldn't import file!"), filename, wxOK, frame);
        return _("File load error.");
    }

    int GetXMLNodes(wxXmlNode *node, auto &nodes, vector<wxXmlAttribute *> *attributes = nullptr,
                    bool attributestoo = false) {
        for (auto child = node->GetChildren(); child; child = child->GetNext()) {
            if (child->GetType() == wxXML_ELEMENT_NODE) nodes.push_back(child);
        }
        if (attributestoo && attributes)
            for (auto attribute = node->GetAttributes(); attribute;
                 attribute = attribute->GetNext()) {
                attributes->push_back(attribute);
            }
        return nodes.size() + (attributes ? attributes->size() : 0);
    }

    void FillXML(Cell *c, wxXmlNode *node, bool attributestoo) {
        const auto &words = wxStringTokenize(
            node->GetType() == wxXML_ELEMENT_NODE ? node->GetNodeContent() : node->GetContent());
        loop(i, words.GetCount()) {
            if (c->text.t.Len()) c->text.t.Append(L' ');
            c->text.t.Append(words[i]);
        }

        if (node->GetName() == "cell") {
            c->text.relsize = -wxAtoi(node->GetAttribute("relsize", "0"));
            c->text.stylebits = wxAtoi(node->GetAttribute("stylebits", "0"));
            c->cellcolor =
                std::stoi(node->GetAttribute("colorbg", "0xFFFFFF").ToStdString(), nullptr, 0);
            c->textcolor =
                std::stoi(node->GetAttribute("colorfg", "0x000000").ToStdString(), nullptr, 0);
            c->celltype = wxAtoi(node->GetAttribute("type", "0"));
        }

        vector<wxXmlNode *> nodes;
        vector<wxXmlAttribute *> attributes;
        auto numrows = GetXMLNodes(node, nodes, &attributes, attributestoo);
        if (!numrows) return;

        if (nodes.size() == 1 && (!c->text.t.Len() || nodes[0]->IsWhitespaceOnly()) &&
            nodes[0]->GetName() != "row") {
            FillXML(c, nodes[0], attributestoo);
        } else {
            auto allrow = node->GetName() == "grid";
            for (auto node : nodes)
                if (node->GetName() != "row") {
                    allrow = false;
                    break;
                }
            if (allrow) {
                int desiredxs;
                loopv(i, nodes) {
                    vector<wxXmlNode *> ins;
                    auto xs = GetXMLNodes(nodes[i], ins);
                    if (!i) {
                        desiredxs = xs ? xs : 1;
                        c->AddGrid(desiredxs, nodes.size());
                        SetGridSettingsFromXML(c, node);
                    }
                    loop(j, desiredxs) if (ins.size() > j)
                        FillXML(c->grid->C(j, i).get(), ins[j], attributestoo);
                }
            } else {
                c->AddGrid(1, numrows);
                SetGridSettingsFromXML(c, node);
                loopv(i, attributes) c->grid->C(0, i)->text.t = attributes[i]->GetValue();
                loopv(i, nodes)
                    FillXML(c->grid->C(0, i + attributes.size()).get(), nodes[i], attributestoo);
            }
        }
    }

    void SetGridSettingsFromXML(Cell *c, wxXmlNode *node) {
        c->grid->folded = wxAtoi(node->GetAttribute("folded", "0"));
        c->grid->bordercolor = std::stoi(
            node->GetAttribute("bordercolor", wxString() << g_bordercolor_default).ToStdString(),
            nullptr, 0);
        c->grid->user_grid_outer_spacing = wxAtoi(
            node->GetAttribute("outerspacing", wxString() << g_usergridouterspacing_default));
    }

    int CountCol(const auto &s) {
        auto col = 0;
        while (s[col] == ' ' || s[col] == '\t') col++;
        return col;
    }

    int FillRows(Grid *g, const wxArrayString &as, int column, int startrow, int starty) {
        auto y = starty;
        for (int i = startrow, n = as.size(); i < n; i++) {
            auto s = as[i];
            auto col = CountCol(s);
            if (col < column && startrow != 0) return i;
            if (col > column) {
                auto c = g->C(0, y - 1).get();
                auto sg = c->grid;
                i = FillRows(sg ? sg : c->AddGrid(), as, col, i, sg ? sg->ys : 0) - 1;
            } else {
                if (g->ys <= y) g->InsertCells(-1, y, 0, 1);
                auto &t = g->C(0, y)->text;
                t.t = s.Trim(false);
                y++;
            }
        }
        return static_cast<int>(as.size());
    }

    int AddImageToList(double scale, auto &&data, char type) {
        auto hash = CalculateHash(data);
        loopv(i, imagelist) {
            if (imagelist[i]->hash == hash && imagelist[i]->type == type) return i;
        }
        imagelist.push_back(make_unique<Image>(hash, scale, std::move(data), type));
        return imagelist.size() - 1;
    }

    void ImageSize(wxBitmap *bm, int &xs, int &ys) {
        if (!bm) return;
        xs = bm->GetWidth();
        ys = bm->GetHeight();
    }

    void ImageDraw(wxBitmap *bm, wxDC &dc, int x, int y) { dc.DrawBitmap(*bm, x, y); }
};
```

## File: src/text.h
```c
struct Text {
    Cell *cell {nullptr};
    Image *image {nullptr};
    wxString t {wxEmptyString};
    int relsize {0};
    int stylebits {0};
    int extent {0};
    wxDateTime lastedit;
    bool filtered {false};

    void WasEdited() { lastedit = wxDateTime::Now(); }

    Text() { WasEdited(); }

    wxBitmap *DisplayImage() {
        return cell->grid && cell->grid->folded ? &sys->frame->foldicon
                                                : (image ? &image->Display() : nullptr);
    }

    size_t EstimatedMemoryUse() {
        ASSERT(wxUSE_UNICODE);
        return sizeof(Text) + t.Length() * sizeof(wchar_t);
    }

    double GetNum() {
        std::wstringstream ss(t.ToStdWstring());
        double r;
        ss >> r;
        return r;
    }

    void SetNum(double d) {
        std::wstringstream ss;
        ss << std::fixed;

        // We're going to use at most 19 digits after '.'. Add small value round remainder.
        size_t max_significant = 10;
        d += 0.00000000005;

        ss << d;

        auto s = ss.str();
        // First trim whatever lies beyond the precision to avoid garbage digits.
        max_significant += 2;  // "0."
        if (s[0] == '-') max_significant++;
        if (s.length() > max_significant) s.erase(max_significant);
        // Now strip unnecessary trailing zeroes.
        while (s.back() == '0') s.pop_back();
        // If there were only zeroes, remove '.'.
        if (s.back() == '.') s.pop_back();

        t = s;
    }

    wxString htmlify(wxString &str) {
        wxString r;
        for (auto cref : str) {
            switch (wxChar c = cref.GetValue()) {
                case '&': r += "&amp;"; break;
                case '<': r += "&lt;"; break;
                case '>': r += "&gt;"; break;
                default: r += c;
            }
        }
        return r;
    }

    wxString ToText(int indent, const Selection &s, int format) {
        wxString str = s.cursor != s.cursorend ? t.Mid(s.cursor, s.cursorend - s.cursor) : t;
        if (format == A_EXPXML || format == A_EXPHTMLT || format == A_EXPHTMLTI ||
            format == A_EXPHTMLTE || format == A_EXPHTMLO || format == A_EXPHTMLB)
            str = htmlify(str);
        if (format == A_EXPHTMLTI && image)
            str.Prepend("<img src=\"data:" + imagetypes.at(image->type).second + ";base64," +
                        wxBase64Encode(image->data.data(), image->data.size()) + "\" />");
        else if (format == A_EXPHTMLTE && image) {
            wxString relsize = wxString::Format(
                "%d%%", static_cast<int>(100.0 * sys->frame->FromDIP(1.0) / image->display_scale));
            str.Prepend("<img src=\"" + wxString::Format("%llu", image->hash) +
                        image->GetFileExtension() + "\" width=\"" + relsize + "\" height=\"" +
                        relsize + "\" />");
        }
        return str;
    };

    auto MinRelsize(int rs) { return min(relsize, rs); }
    auto RelSize(int dir, int zoomdepth) {
        relsize = max(min(relsize + dir, g_deftextsize - g_mintextsize() + zoomdepth),
                      g_deftextsize - g_maxtextsize() - zoomdepth);
    }

    auto IsWord(wxChar c) { return wxIsalnum(c) || wxStrchr(L"_\"\'()", c) || wxIspunct(c); }
    auto GetLinePart(int &currentpos, int breakpos, int limitpos) {
        auto startpos = currentpos;
        currentpos = breakpos;

        for (auto j = t.begin() + startpos; (j != t.end()) && !wxIsspace(*j) && !IsWord(*j); j++) {
            currentpos++;
            breakpos++;
        }
        // gobble up any trailing punctuation
        if (currentpos != startpos && currentpos < limitpos &&
            (t[currentpos] == '\"' || t[currentpos] == '\'')) {
            currentpos++;
            breakpos++;
        }  // special case: if punctuation followed by quote, quote is meant to be part of word

        for (auto k = t.begin() + currentpos; (k != t.end()) && wxIsspace(*k); k++) {
            // gobble spaces, but do not copy them
            currentpos++;
            if (currentpos == limitpos)
                breakpos = currentpos;  // happens with a space at the last line, user is most
                                        // likely about to type another word, so
            // need to show space. Alternatively could check if the cursor is actually on this spot.
            // Simply
            // showing a blank new line would not be a good idea, unless the cursor is here for
            // sure, and
            // even then, placing the cursor there again after deselect may be hard.
        }

        ASSERT(startpos != currentpos);

        return t.Mid(startpos, breakpos - startpos);
    }

    wxString GetLine(auto &i, auto maxcolwidth) {
        auto l = static_cast<int>(t.Len());

        if (i >= l) return wxEmptyString;

        if (!i && l <= maxcolwidth) {
            i = l;
            return t;
        }  // subsumed by the case below, but this case happens 90% of the time, so more optimal
        if (l - i <= maxcolwidth) return GetLinePart(i, l, l);

        for (auto p = i + maxcolwidth; p >= i; p--)
            if (!IsWord(t[p])) return GetLinePart(i, p, l);

        // A single word is > maxcolwidth. We split it up anyway.
        // This happens with long urls and e.g. Japanese text without spaces.
        // Should really do proper unicode linebreaking instead (see libunibreak),
        // but for now this is better than the old code below which allowed for arbitrary long
        // words.
        return GetLinePart(i, min(i + maxcolwidth, l), l);

        // for(int p = i+maxcolwidth; p<l;  p++) if (!IsWord(t[p])) return GetLinePart(i, p, l);  //
        // we arrive here only
        // if a single word is too big for maxcolwidth, so simply return that word
        // return GetLinePart(i, l, l);     // big word was the last one
    }

    void TextSize(wxReadOnlyDC &dc, int &sx, int &sy, int tiny, int &leftoffset, int maxcolwidth) {
        sx = sy = 0;
        auto i = 0;
        for (;;) {
            auto curl = GetLine(i, maxcolwidth);
            if (!curl.Len()) break;
            int x, y;
            if (tiny) {
                x = static_cast<int>(curl.Len());
                y = 1;
            } else
                dc.GetTextExtent(curl, &x, &y);
            sx = max(x, sx);
            sy += y;
            leftoffset = y;
        }
        if (!tiny) sx += 4;
    }

    bool IsInSearch() {
        return sys->searchstring.Len() &&
               (sys->casesensitivesearch ? t.Find(sys->searchstring)
                                         : t.Lower().Find(sys->searchstring)) >= 0;
    }

    int Render(Document *doc, int bx, int by, int depth, wxDC &dc, int &leftoffset,
               int maxcolwidth) {
        auto ixs = 0, iys = 0;
        if (!cell->tiny) sys->ImageSize(DisplayImage(), ixs, iys);

        if (ixs && iys) {
            sys->ImageDraw(DisplayImage(), dc, bx + 1 + g_margin_extra,
                           by + (cell->tys - iys) / 2 + g_margin_extra);
            ixs += 2;
            iys += 2;
        }

        if (t.empty()) return iys;

        doc->PickFont(dc, depth, relsize, stylebits);

        auto h = cell->tiny ? 1 : dc.GetCharHeight();
        leftoffset = h;
        auto i = 0;
        auto lines = 0;
        auto searchfound = IsInSearch();
        auto istag = cell->IsTag(doc);
        if (cell->tiny) {
            if (searchfound)
                dc.SetPen(*wxRED_PEN);
            else if (filtered)
                dc.SetPen(*wxLIGHT_GREY_PEN);
            else if (istag)
                dc.SetPen(wxPen(LightColor(doc->tags[t])));
            else
                dc.SetPen(sys->pen_tinytext);
        }
        for (;;) {
            auto curl = GetLine(i, maxcolwidth);
            if (!curl.Len()) break;
            if (cell->tiny) {
                if (sys->fastrender) {
                    dc.DrawLine(bx + ixs, by + lines * h, bx + ixs + static_cast<int>(curl.Len()),
                                by + lines * h);
                    /*
                    wxPoint points[] = { wxPoint(bx + ixs, by + lines * h), wxPoint(bx + ixs +
                    curl.Len(), by + lines * h) }; dc.DrawLines(1, points, 0, 0);
                     */
                } else {
                    auto word = 0;
                    loop(p, static_cast<int>(curl.Len()) + 1) {
                        if (static_cast<int>(curl.Len()) <= p || curl[p] == ' ') {
                            if (word)
                                dc.DrawLine(bx + p - word + ixs, by + lines * h, bx + p,
                                            by + lines * h);
                            word = 0;
                        } else
                            word++;
                    }
                }
            } else {
                if (searchfound)
                    dc.SetTextForeground(*wxRED);
                else if (filtered)
                    dc.SetTextForeground(*wxLIGHT_GREY);
                else if (istag)
                    dc.SetTextForeground(LightColor(doc->tags[t]));
                else if (cell->textcolor)
                    dc.SetTextForeground(LightColor(cell->textcolor));  // FIXME: clean up
                auto tx = bx + 2 + ixs;
                auto ty = by + lines * h;
                dc.DrawText(curl, tx + g_margin_extra, ty + g_margin_extra);
                if (searchfound || filtered || istag || cell->textcolor)
                    dc.SetTextForeground(LightColor(0x000000));
            }
            lines++;
        }

        return max(lines * h, iys);
    }

    void FindCursor(Document *doc, int bx, int by, wxReadOnlyDC &dc, Selection &s, int maxcolwidth) {
        bx -= g_margin_extra;
        by -= g_margin_extra;

        auto ixs = 0, iys = 0;
        if (!cell->tiny) sys->ImageSize(DisplayImage(), ixs, iys);
        if (ixs) ixs += 2;

        doc->PickFont(dc, cell->Depth() - doc->drawpath.size(), relsize, stylebits);

        auto i = 0, linestart = 0;
        auto line = by / dc.GetCharHeight();
        wxString ls;

        loop(l, line + 1) {
            linestart = i;
            ls = GetLine(i, maxcolwidth);
        }

        for (;;) {
            auto x = 0, y = 0;
            dc.GetTextExtent(ls, &x, &y);  // FIXME: can we do this more intelligently?
            if (x <= bx - ixs + 2 || !x) break;
            ls.Truncate(ls.Len() - 1);
        }

        s.cursor = s.cursorend = linestart + static_cast<int>(ls.Len());
        ASSERT(s.cursor >= 0 && s.cursor <= static_cast<int>(t.Len()));
    }

    void DrawCursor(Document *doc, wxDC &dc, Selection &s, bool full, uint color, int maxcolwidth) {
        auto ixs = 0, iys = 0;
        if (!cell->tiny) sys->ImageSize(DisplayImage(), ixs, iys);
        if (ixs) ixs += 2;
        doc->PickFont(dc, cell->Depth() - doc->drawpath.size(), relsize, stylebits);
        auto h = dc.GetCharHeight();
        {
            auto i = 0;
            for (auto l = 0;; l++) {
                auto start = i;
                auto ls = GetLine(i, maxcolwidth);
                auto len = static_cast<int>(ls.Len());
                auto end = start + len;

                if (s.cursor != s.cursorend) {
                    if (s.cursor <= end && s.cursorend >= start) {
                        ls.Truncate(min(s.cursorend, end) - start);
                        auto x1 = 0, x2 = 0;
                        dc.GetTextExtent(ls, &x2, nullptr);
                        ls.Truncate(max(s.cursor, start) - start);
                        dc.GetTextExtent(ls, &x1, nullptr);
                        if (x1 != x2) {
                            int startx = cell->GetX(doc) + x1 + 2 + ixs + g_margin_extra;
                            int starty =
                                cell->GetY(doc) + l * h + 1 + cell->ycenteroff + g_margin_extra;
                            DrawRectangle(dc, color, startx, starty, x2 - x1, h - 1, true);
                            HintIMELocation(doc, startx, starty, h - 1, stylebits);
                        }
                    }
                } else if (s.cursor >= start && s.cursor <= end) {
                    ls.Truncate(s.cursor - start);
                    auto x = 0;
                    dc.GetTextExtent(ls, &x, nullptr);
                    int startx = cell->GetX(doc) + x + 1 + ixs + g_margin_extra;
                    int starty = cell->GetY(doc) + l * h + 1 + cell->ycenteroff + g_margin_extra;
                    DrawRectangle(dc, color, startx, starty, 2, h - 2);
                    HintIMELocation(doc, startx, starty, h - 2, stylebits);
                    break;
                }

                if (!len) break;
            }
        }
    }

    void ExpandToWord(Selection &s) {
        if (!wxIsalnum(t[s.cursor])) return;
        while (s.cursor > 0 && wxIsalnum(t[s.cursor - 1])) s.cursor--;
        while (s.cursorend < static_cast<int>(t.Len()) && wxIsalnum(t[s.cursorend])) s.cursorend++;
    }

    void SelectWord(Selection &s) {
        if (s.cursor >= static_cast<int>(t.Len())) return;
        s.cursorend = s.cursor + 1;
        ExpandToWord(s);
    }

    void SelectWordBefore(Selection &s) {
        if (s.cursor <= 1) return;
        s.cursorend = s.cursor--;
        ExpandToWord(s);
    }

    bool RangeSelRemove(Selection &s) {
        WasEdited();
        if (s.cursor != s.cursorend) {
            t.Remove(s.cursor, s.cursorend - s.cursor);
            s.cursorend = s.cursor;
            return true;
        }
        return false;
    }

    void SetRelSize(Selection &s) {
        if (t.Len() || !cell->parent) return;
        int dd[] = {0, 1, 1, 0, 0, -1, -1, 0};
        for (auto i = 0; i < 4; i++) {
            auto x = max(0, min(s.x + dd[i * 2], s.grid->xs - 1));
            auto y = max(0, min(s.y + dd[i * 2 + 1], s.grid->ys - 1));
            auto c = s.grid->C(x, y).get();
            if (c->text.t.Len()) {
                relsize = c->text.relsize;
                break;
            }
        }
    }

    auto Insert(Document *doc, const auto &ins, Selection &s, bool keeprelsize) {
        auto prevl = t.Len();
        if (!s.TextEdit()) Clear(doc, s);
        RangeSelRemove(s);
        if (!prevl && !keeprelsize) SetRelSize(s);
        t.insert(s.cursor, ins);
        s.cursor = s.cursorend = s.cursor + static_cast<int>(ins.Len());
    }
    void Key(Document *doc, int k, Selection &s) {
        wxString ins;
        ins += k;
        Insert(doc, ins, s, false);
    }

    void Delete(Selection &s) {
        if (!RangeSelRemove(s))
            if (s.cursor < static_cast<int>(t.Len())) { t.Remove(s.cursor, 1); };
    }
    void Backspace(Selection &s) {
        if (!RangeSelRemove(s))
            if (s.cursor > 0) {
                t.Remove(--s.cursor, 1);
                --s.cursorend;
            };
    }
    void DeleteWord(Selection &s) {
        SelectWord(s);
        Delete(s);
    }
    void BackspaceWord(Selection &s) {
        SelectWordBefore(s);
        Backspace(s);
    }

    void ReplaceStr(const wxString &str, const wxString &lstr) {
        if (sys->casesensitivesearch) {
            for (auto i = 0, j = 0; (j = t.Mid(i).Find(sys->searchstring)) >= 0;) {
                // does this need WasEdited()?
                i += j;
                t.Remove(i, sys->searchstring.Len());
                t.insert(i, str);
                i += str.Len();
            }
        } else {
            auto lowert = t.Lower();
            for (auto i = 0, j = 0; (j = lowert.Mid(i).Find(sys->searchstring)) >= 0;) {
                // does this need WasEdited()?
                i += j;
                lowert.Remove(i, sys->searchstring.Len());
                t.Remove(i, sys->searchstring.Len());
                lowert.insert(i, lstr);
                t.insert(i, str);
                i += str.Len();
            }
        }
    }

    void Clear(Document *doc, Selection &s) {
        t.Clear();
        s.EnterEdit(doc);
    }

    void HomeEnd(Selection &s, bool home) {
        auto i = 0;
        auto cw = cell->ColWidth();
        auto findwhere = home ? s.cursor : s.cursorend;
        for (;;) {
            auto start = i;
            auto curl = GetLine(i, cw);
            if (!curl.Len()) break;
            auto end = i == t.Len() ? i : i - 1;
            if (findwhere >= start && findwhere <= end) {
                s.cursor = s.cursorend = home ? start : end;
                break;
            }
        }
    }

    void Save(wxDataOutputStream &dos) const {
        dos.WriteString(t.wx_str());
        dos.Write32(relsize);
        dos.Write32(image ? image->savedindex : -1);
        dos.Write32(stylebits);
        wxLongLong le = lastedit.GetValue();
        dos.Write64(&le, 1);
    }

    void Load(wxDataInputStream &dis) {
        t = dis.ReadString();

        // if (t.length() > 10000)
        //    printf("");

        if (sys->versionlastloaded <= 11) dis.Read32();  // numlines

        relsize = dis.Read32();

        int i = dis.Read32();
        image = i >= 0 ? sys->imagelist[sys->loadimageids[i]].get() : nullptr;

        if (sys->versionlastloaded >= 7) stylebits = dis.Read32();

        wxLongLong time;
        if (sys->versionlastloaded >= 14) {
            dis.Read64(&time, 1);
        } else {
            time = sys->fakelasteditonload--;
        }
        lastedit = wxDateTime(time);
    }

    auto Eval(auto &ev) const {
        switch (cell->celltype) {
            // Load variable's data.
            case CT_VARU: {
                auto v = ev.Lookup(t);
                if (!v) {
                    v = cell->Clone(nullptr);
                    v->celltype = CT_DATA;
                    v->text.t = "**Variable Load Error**";
                }
                return v;
            }

            // Return our current data.
            case CT_DATA: return cell->Clone(nullptr);

            default: return unique_ptr<Cell>();
        }
    }
};
```

## File: src/threadpool.h
```c
class ThreadPool {
    public:
    ThreadPool(size_t);
    template<class F, class... Args>
    auto enqueue(F&& f, Args&&... args)
        -> std::future<typename std::invoke_result<F, Args...>::type>;
    ~ThreadPool();

    private:
    // need to keep track of threads so we can join them
    std::vector<std::thread> workers;
    // the task queue
    std::queue<std::function<void()>> tasks;

    // synchronization
    std::mutex queue_mutex;
    std::condition_variable condition;
    bool stop;
};

// the constructor just launches some amount of workers
inline ThreadPool::ThreadPool(size_t threads) : stop(false) {
    for (size_t i = 0; i < threads; ++i)
        workers.emplace_back([this] {
            for (;;) {
                std::function<void()> task;

                {
                    std::unique_lock<std::mutex> lock(this->queue_mutex);
                    this->condition.wait(lock,
                                         [this] { return this->stop || !this->tasks.empty(); });
                    if (this->stop && this->tasks.empty()) return;
                    task = std::move(this->tasks.front());
                    this->tasks.pop();
                }

                task();
            }
        });
}

// add new work item to the pool
template<class F, class... Args>
auto ThreadPool::enqueue(F&& f, Args&&... args)
    -> std::future<typename std::invoke_result<F, Args...>::type> {
    using return_type = typename std::invoke_result<F, Args...>::type;

    auto task = std::make_shared<std::packaged_task<return_type()>>(
        std::bind(std::forward<F>(f), std::forward<Args>(args)...));

    std::future<return_type> res = task->get_future();
    {
        std::unique_lock<std::mutex> lock(queue_mutex);

        // don't allow enqueueing after stopping the pool
        assert(!stop);

        tasks.emplace([task]() { (*task)(); });
    }
    condition.notify_one();
    return res;
}

// the destructor joins all threads
inline ThreadPool::~ThreadPool() {
    {
        std::unique_lock<std::mutex> lock(queue_mutex);
        stop = true;
    }
    condition.notify_all();
    for (std::thread& worker : workers) worker.join();
}
```

## File: src/tools.h
```c
typedef unsigned char uchar;
typedef unsigned short ushort;
typedef unsigned int uint;

#ifdef _DEBUG
#define ASSERT(c) assert(c)
#else
#define ASSERT(c) \
    if (c) {}
#endif

#define loop(i, m) for (int i = 0; i < int(m); i++)
#define loopv(i, v) for (int i = 0; i < (v).size(); i++)
#define loopvrev(i, v) for (int i = (v).size() - 1; i >= 0; i--)

#define max(a, b) ((a) < (b) ? (b) : (a))
#define min(a, b) ((a) > (b) ? (b) : (a))
#define sign(x) ((x) < 0 ? -1 : 1)

#define varargs(v, fmt, body) \
    {                         \
        va_list v;            \
        va_start(v, fmt);     \
        body;                 \
        va_end(v);            \
    }

#define DELETEP(p)       \
    {                    \
        if (p) {         \
            delete p;    \
            p = nullptr; \
        }                \
    }
#define DELETEA(a)       \
    {                    \
        if (a) {         \
            delete[] a;  \
            a = nullptr; \
        }                \
    }

#define bound(v, a, s, e)     \
    {                         \
        v += a;               \
        if (v > (e)) v = (e); \
        if (v < (s)) v = (s); \
    }

// Use the same on all platforms, because:
// Win32: usually contains both.
// Macos: older versions use \r and newer \n in clipboard?
// Linux: should only ever be \n but if we encounter \r we want to strip it.
#ifdef __WXGTK__
    #define LINE_SEPARATOR "\r\n"
#else
    #define LINE_SEPARATOR "\n"
#endif

#ifdef WIN32
#define PATH_SEPARATOR "\\"
#else
#define PATH_SEPARATOR "/"
#define __cdecl
#define _vsnprintf vsnprintf
#endif

template<class T> inline void swap_(T &a, T &b) {
    T c = a;
    a = b;
    b = c;
};

#ifdef WIN32
#pragma warning(3 : 4189)        // local variable is initialized but not referenced
#pragma warning(disable : 4244)  // conversion from 'int' to 'float', possible loss of data
#pragma warning(disable : 4355)  // 'this' : used in base member initializer list
#pragma warning(disable : 4996)  // 'strncpy' was declared deprecated
#endif

inline uchar *loadfile(const char *fn, size_t *lenret = nullptr) {
    FILE *f = fopen(fn, "rb");
    if (!f) return nullptr;
    fseek(f, 0, SEEK_END);
    size_t len = ftell(f);
    fseek(f, 0, SEEK_SET);
    uchar *buf = (uchar *)malloc(len + 1);
    if (!buf) {
        fclose(f);
        return nullptr;
    }
    buf[len] = 0;
    size_t rlen = fread(buf, 1, len, f);
    fclose(f);
    if (len != rlen || len <= 0) {
        free(buf);
        return nullptr;
    }
    if (lenret) *lenret = len;
    return buf;
}

// for use with vc++ crtdbg

#if defined(_DEBUG) && defined(_WIN32)
inline void *__cdecl operator new(size_t n, const char *fn, int l) {
    return ::operator new(n, 1, fn, l);
}
inline void *__cdecl operator new[](size_t n, const char *fn, int l) {
    return ::operator new[](n, 1, fn, l);
}
inline void __cdecl operator delete(void *p, const char *fn, int l) {
    ::operator delete(p, 1, fn, l);
}
inline void __cdecl operator delete[](void *p, const char *fn, int l) {
    ::operator delete[](p, 1, fn, l);
}
#define new new (__FILE__, __LINE__)
#endif

inline uint64_t FNV1A64(uint8_t *data, size_t size) {
    uint64_t hash = 0xCBF29CE484222325;
    for (size_t i = 0; i < size; ++i) {
        hash ^= data[i];
        hash *= 0x100000001B3;
    }
    return hash;
}
```

## File: src/treesheets_impl.h
```c
struct TreeSheetsScriptImpl : public ScriptInterface {
    Document *document = nullptr;
    Cell *current = nullptr;
    Cell *lowestcommonancestor = nullptr;

    enum { max_new_grid_cells = 256 * 256 };  // Don't allow crazy sizes.

    void SwitchToCurrentDocument() {
        document = sys->frame->GetCurrentTab()->doc.get();
        current = document->root.get();
        lowestcommonancestor = nullptr;
    }

    void AddUndoIfNecessary() {
        if (!lowestcommonancestor) {
            UpdateLowestCommonAncestor(true);
        } else {
            for (auto p = current; p; p = p->parent) {
                if (p == lowestcommonancestor) {
                    // There is no need to add current to the undo stack as
                    // lowestcommonancestor including subordinated current
                    // is already in there.
                    return;
                }
            }
            UpdateLowestCommonAncestor(false);
        }
    }

    void UpdateLowestCommonAncestor(bool newgeneration) {
        // Use parent as lowestcommonancestor so changes to siblings are already covered
        lowestcommonancestor = current->parent;
        document->AddUndo(lowestcommonancestor, newgeneration);
    }

    std::string ScriptRun(const char *filename) {
        SwitchToCurrentDocument();

        bool dump_builtins = false;
        #ifdef _DEBUG
            //dump_builtins = true;
        #endif

        auto errormessage = RunLobster(filename, {}, dump_builtins);

        document->root->ResetChildren();
        document->canvas->Refresh();

        document = nullptr;
        current = nullptr;

        return errormessage;
    }

    bool LoadDocument(const char *filename) {
        auto message = sys->LoadDB(filename);
        if (message.IsEmpty()) return false;

        SwitchToCurrentDocument();
        return true;
    }

    void GoToRoot() { current = document->root.get(); }
    void GoToView() { current = document->currentdrawroot; }
    bool HasSelection() { return document->selected.grid; }
    void GoToSelection() {
        auto cell = document->selected.GetFirst();
        if (cell) current = cell;
    }
    bool HasParent() { return current->parent; }
    void GoToParent() {
        if (current->parent) current = current->parent;
    }
    int NumChildren() { return current->grid ? current->grid->xs * current->grid->ys : 0; }

    icoord NumColumnsRows() {
        return current->grid ? icoord(current->grid->xs, current->grid->ys) : icoord(0, 0);
    }

    int GetColWidth() { return current->parent ? current->parent->grid->GetColWidth(current) : 0; }

    void SetColWidth(int w) {
        if (current->parent) { current->parent->grid->SetColWidth(current, w); }
    }

    ibox SelectionBox() {
        auto &selection = document->selected;
        return selection.grid ? ibox(icoord(selection.x, selection.y), icoord(selection.xs, selection.ys))
                      : ibox(icoord(0, 0), icoord(0, 0));
    }

    void GoToChild(int n) {
        if (current->grid && n < current->grid->xs * current->grid->ys)
            current = current->grid->cells[n].get();
    }

    void GoToColumnRow(int x, int y) {
        if (current->grid && x < current->grid->xs && y < current->grid->ys)
            current = current->grid->C(x, y).get();
    }

    std::string GetText() { return current->text.t.utf8_string(); }

    std::string GetNote() { return current->note.utf8_string(); }

    void SetText(std::string_view t) {
        if (current->parent) {
            AddUndoIfNecessary();
            current->text.t = wxString::FromUTF8(t.data(), t.size());
        }
    }

    void SetNote(std::string_view t) {
        if (current->parent) {
            AddUndoIfNecessary();
            current->note = wxString::FromUTF8(t.data(), t.size());
        }
    }

    void CreateGrid(int x, int y) {
        if (x > 0 && y > 0 && x * y < max_new_grid_cells) {
            AddUndoIfNecessary();
            current->AddGrid(x, y);
        }
    }

    void InsertColumn(int x) {
        if (current->grid && x >= 0 && x <= current->grid->xs) {
            AddUndoIfNecessary();
            current->grid->InsertCells(x, -1, 1, 0);
        }
    }

    void InsertRow(int y) {
        if (current->grid && y >= 0 && y <= current->grid->ys) {
            AddUndoIfNecessary();
            current->grid->InsertCells(-1, y, 0, 1);
        }
    }

    void Delete(int x, int y, int xs, int ys) {
        if (current->grid && x >= 0 && x + xs <= current->grid->xs && y >= 0 &&
            y + ys <= current->grid->ys) {
            AddUndoIfNecessary();
            Selection s(current->grid, x, y, xs, ys);
            current->grid->MultiCellDeleteSub(document, s);
            document->SetSelect(Selection());
            document->Zoom(-100);
        }
    }

    void SetBackgroundColor(uint color) {
        AddUndoIfNecessary();
        current->cellcolor = color;
    }

    void SetTextColor(uint color) {
        AddUndoIfNecessary();
        current->textcolor = color;
    }

    void SetTextFiltered(bool filtered) {
        if (current->parent) {
            AddUndoIfNecessary();
            current->text.filtered = filtered;
        }
    }

    bool IsTextFiltered() { return current->text.filtered; }

    void SetBorderColor(uint color) {
        if (current->grid) {
            AddUndoIfNecessary();
            current->grid->bordercolor = color;
        }
    }

    int GetRelativeSize() { return -current->text.relsize; }

    void SetRelativeSize(int relsize) {
        AddUndoIfNecessary();
        current->text.relsize = -relsize;
    }

    void SetStyle(int stylebits) {
        AddUndoIfNecessary();
        current->text.stylebits = stylebits;
    }

    int GetStyle() { return current->text.stylebits; }

    void RemoveImage() {
        AddUndoIfNecessary();
        current->text.image = nullptr;
    }

    void SetStatusMessage(std::string_view message) {
        auto ws = wxString(message.data(), message.size());
        sys->frame->SetStatus(ws);
    }

    void SetWindowSize(int width, int height) { sys->frame->SetSize(width, height); }

    std::string GetFileNameFromUser(bool is_save) {
        int flags = wxFD_CHANGE_DIR;
        if (is_save)
            flags |= wxFD_OVERWRITE_PROMPT | wxFD_SAVE;
        else
            flags |= wxFD_OPEN | wxFD_FILE_MUST_EXIST;
        wxString fn = ::wxFileSelector(_("Choose file:"), "", "", "", "*.*", flags);
        return fn.utf8_string();
    }

    std::string GetFileName() { return document->filename.utf8_string(); }

    int64_t GetLastEdit() { return current->text.lastedit.GetValue().GetValue(); }

    bool IsTag() { return current->IsTag(document); }

    bool HasImage() { return current->text.image; }
    bool SetImage(std::string_view fn) {
        AddUndoIfNecessary();
        return document->LoadImageIntoCell(wxString::FromUTF8(fn.data(), fn.size()), current,
                                           sys->frame->FromDIP(1.0));
    }
};

static int64_t TreeSheetsLoader(string_view_nt absfilename, std::string *dest, int64_t start,
                                int64_t len) {
    size_t l = 0;
    auto buf = (char *)loadfile(absfilename.c_str(), &l);
    if (!buf) return -1;
    dest->assign(buf, l);
    free(buf);
    return l;
}

static TreeSheetsScriptImpl tssi;

static string ScriptInit(const wxString &datapath) {
    return InitLobster(&tssi, datapath, "", false, TreeSheetsLoader);
}
```

## File: src/tsapp.h
```c
struct IPCServer : wxServer {
    wxConnectionBase *OnAcceptConnection(const wxString &topic) {
        sys->frame->DeIconize();
        if (topic.Len() && topic != "*") sys->Open(topic);
        return new wxConnection();
    }
};

struct TSApp : wxApp {
    TSFrame *frame {nullptr};
    unique_ptr<IPCServer> serv {make_unique<IPCServer>()};
    wxString service {
        #ifdef __WXMSW__
                "4242"
        #else
                "/tmp/TreeSheets-socket"
        #endif
    };
    wxString filename;
    bool initiateventloop {false};
    wxString exename;
    wxString exepath;
    unique_ptr<wxSingleInstanceChecker> instance_checker {nullptr};

    bool OnInit() override {
        #if wxUSE_UNICODE == 0
            #error "must use unicode version of wx libs to ensure data integrity of .cts files"
        #endif
        ASSERT(wxUSE_UNICODE);

        exename = GetExecutablePath();
        exepath = wxFileName(exename).GetPath();

        #ifdef __WXMAC__
            int cut = exepath.Find("/MacOS");
            if (cut > 0) { exepath = exepath.SubString(0, cut) + "/Resources"; }
            wxDisableAsserts();
        #endif

        bool portable = false;
        bool single_instance = true;
        bool dump_builtins = false;
        bool start_minimized = false;
        for (int i = 1; i < argc; i++) {
            if (argv[i][0] == '-') {
                switch (static_cast<int>(argv[i][1])) {
                    case 'p': portable = true; break;
                    case 'i': single_instance = false; break;
                    case 'm': start_minimized = true; break;
                    case 'd':
                        dump_builtins = true;
                        single_instance = false;
                        break;
                }
            } else {
                filename = argv[i];
            }
        }

        if (single_instance) {
            instance_checker.reset(new wxSingleInstanceChecker(
                wxTheApp->GetAppName() + '-' + wxGetUserId(), wxStandardPaths::Get().GetTempDir()));
            if (instance_checker->IsAnotherRunning()) {
                wxClient client;
                client.MakeConnection("localhost", service,
                                      filename.Len() ? filename : wxString("*"));  // fire and forget
                return false;
            }
        }

        wxStandardPaths::Get().SetFileLayout(wxStandardPathsBase::FileLayout_XDG);
        sys = new System(portable);
        if (start_minimized) sys->startminimized = true;
        SetupInternationalization();
        #ifdef __WXMSW__
            DeclareHiDpiAwareOnWindows();
            MSWEnableDarkMode();
        #endif
        frame = new TSFrame(this);

        #ifdef ENABLE_LOBSTER
            auto serr = ScriptInit(GetDataPath("scripts/"));
            if (!serr.empty()) {
                wxLogFatalError("Script system could not initialize: %s", serr);
                return false;
            }
        #endif

        if (dump_builtins) {
            #ifdef ENABLE_LOBSTER
                TSDumpBuiltinDoc();
            #endif
            return false;
        }

        SetTopWindow(frame);

        serv->Create(service);
        return true;
    }

    void OnEventLoopEnter(wxEventLoopBase *WXUNUSED(loop)) override {
        if (!initiateventloop) {
            initiateventloop = true;
            frame->AppOnEventLoopEnter();
            sys->Init(filename);
        }
    }

    #ifdef __WXMAC__
        void MacOpenFiles(const wxArrayString &filenames) override {
            if (!sys) return;
            // MacOpenFiles does not trigger OnEventLoopEnter so we need
            // to do this manually
            if (!initiateventloop) {
                initiateventloop = true;
                frame->AppOnEventLoopEnter();
            }
            for (auto &fn : filenames) { sys->Init(fn); }
        }
    #endif

    int OnExit() override {
        DELETEP(sys);
        return 0;
    }

    wxString GetExecutablePath() {
        wxString executablepath = argv[0];
        #if defined(__WXMAC__)
            char path[PATH_MAX];
            uint32_t size = sizeof(path);
            if(_NSGetExecutablePath(path, &size) == 0) executablepath = path;
        #elif defined(__WXGTK__)
            // argv[0] could be relative, this is apparently a more robust way to get the
            // full path.
            char path[PATH_MAX];
            auto len = readlink("/proc/self/exe", path, PATH_MAX - 1);
            if (len >= 0) {
                path[len] = 0;
                executablepath = path;
            }
        #endif
        return executablepath;
    }

    void SetupInternationalization() {
        wxUILocale::UseDefault();

        #ifdef __WXGTK__
            wxFileTranslationsLoader::AddCatalogLookupPathPrefix("/usr");
            wxFileTranslationsLoader::AddCatalogLookupPathPrefix("/usr/local");
            #ifdef LOCALEDIR
                wxFileTranslationsLoader::AddCatalogLookupPathPrefix(LOCALEDIR);
            #endif
            wxString prefix = wxStandardPaths::Get().GetInstallPrefix();
            wxFileTranslationsLoader::AddCatalogLookupPathPrefix(prefix);
        #endif
        wxFileTranslationsLoader::AddCatalogLookupPathPrefix(GetDataPath("translations"));

        auto trans = new wxTranslations();
        trans->SetLanguage(sys->defaultlang);
        trans->AddCatalog("ts");

        wxTranslations::Set(trans);
    }

    wxString GetDataPath(const wxString &relpath) {
        std::filesystem::path candidatePaths[] = {
            std::filesystem::path(exepath.Length()
                                      ? exepath.ToStdString() + "/" + relpath.ToStdString()
                                      : relpath.ToStdString()),
            #ifdef TREESHEETS_DATADIR
                std::filesystem::path(TREESHEETS_DATADIR "/" + relpath.ToStdString()),
            #endif
        };
        std::filesystem::path relativePath;
        for (auto path : candidatePaths) {
            relativePath = path;
            if (std::filesystem::exists(relativePath)) { break; }
        }

        return wxString(relativePath);
    }

    wxString GetDocPath(const wxString &relpath) {
        std::filesystem::path candidatePaths[] = {
            std::filesystem::path(exepath.Length()
                                      ? exepath.ToStdString() + "/" + relpath.ToStdString()
                                      : relpath.ToStdString()),
            #ifdef TREESHEETS_DOCDIR
                std::filesystem::path(TREESHEETS_DOCDIR "/" + relpath.ToStdString()),
            #endif
        };
        std::filesystem::path relativePath;
        for (auto path : candidatePaths) {
            relativePath = path;
            if (std::filesystem::exists(relativePath)) { break; }
        }

        return wxString(relativePath);
    }

    #ifdef __WXMSW__
        void DeclareHiDpiAwareOnWindows() {
            // wxWidgets should really be doing this itself, but it does not (or expects you to
            // want to use a manifest), so we try to use the most recent Windows API to declare
            // ourselves as HiDPI compatible.

            #ifndef DPI_ENUMS_DECLARED
                typedef enum PROCESS_DPI_AWARENESS {
                    PROCESS_DPI_UNAWARE = 0,
                    PROCESS_SYSTEM_DPI_AWARE = 1,
                    PROCESS_PER_MONITOR_DPI_AWARE = 2
                } PROCESS_DPI_AWARENESS;
            #endif

            using SetProcessDPIAware_T = BOOL(WINAPI *)(void);
            using SetProcessDpiAwareness_T = HRESULT(WINAPI *)(PROCESS_DPI_AWARENESS);
            using SetProcessDpiAwarenessContext_T = BOOL(WINAPI *)(DPI_AWARENESS_CONTEXT);

            SetProcessDPIAware_T SetProcessDPIAware = nullptr;
            SetProcessDpiAwareness_T SetProcessDpiAwareness = nullptr;
            SetProcessDpiAwarenessContext_T SetProcessDpiAwarenessContext = nullptr;

            HMODULE user32 = LoadLibraryA("User32.dll");
            HMODULE shcore = LoadLibraryA("Shcore.dll");

            if (user32) {
                SetProcessDPIAware = (SetProcessDPIAware_T)GetProcAddress(user32, "SetProcessDPIAware");
                SetProcessDpiAwarenessContext = (SetProcessDpiAwarenessContext_T)GetProcAddress(
                    user32, "SetProcessDpiAwarenessContext");
            }
            if (shcore) {
                SetProcessDpiAwareness =
                    (SetProcessDpiAwareness_T)GetProcAddress(shcore, "SetProcessDpiAwareness");
            }

            if (SetProcessDpiAwarenessContext) {
                SetProcessDpiAwarenessContext(DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2);
            } else if (SetProcessDpiAwareness) {
                SetProcessDpiAwareness(PROCESS_PER_MONITOR_DPI_AWARE);
            } else if (SetProcessDPIAware) {
                SetProcessDPIAware();
            }

            if (user32) FreeLibrary(user32);
            if (shcore) FreeLibrary(shcore);
        }
    #endif

    DECLARE_EVENT_TABLE()
};
```

## File: src/tscanvas.h
```c
struct TSCanvas : public wxScrolledCanvas {
    TSFrame *frame;
    unique_ptr<Document> doc {nullptr};
    int mousewheelaccum {0};
    bool lastrmbwaswithctrl {false};
    wxPoint lastmousepos;

    TSCanvas(TSFrame *fr, wxWindow *parent, const wxSize &size = wxDefaultSize)
        : wxScrolledCanvas(parent, wxID_ANY, wxDefaultPosition, size,
                           wxScrolledWindowStyle | wxWANTS_CHARS | wxFULL_REPAINT_ON_RESIZE),
          frame(fr) {
        SetBackgroundStyle(wxBG_STYLE_PAINT);
        SetBackgroundColour(*wxWHITE);
        DisableKeyboardScrolling();
        // Without this, canvas does its own scrolling upon mousewheel events, which
        // interferes with our own.
        EnableScrolling(false, false);
    }

    ~TSCanvas() { frame = nullptr; }

    void OnPaint(wxPaintEvent &event) {
        #ifdef __WXMSW__
            auto sz = GetClientSize();
            if (sz.GetX() <= 0 || sz.GetY() <= 0) return;
            wxBitmap bmp;
            auto sf = GetDPIScaleFactor();
            bmp.CreateWithDIPSize(sz, sf, 24);
            wxBufferedPaintDC dc(this, bmp);
        #else
            wxPaintDC dc(this);
        #endif
        DoPrepareDC(dc);
        doc->Draw(dc);
    };

    void OnMotion(wxMouseEvent &me) {
        wxInfoDC dc(this);
        doc->UpdateHover(dc, me.GetX(), me.GetY());
        if (me.LeftIsDown() || me.RightIsDown()) {
            if (me.AltDown() && me.ShiftDown()) {
                doc->Copy(A_DRAGANDDROP);
                Refresh();
            } else {
                if (doc->isctrlshiftdrag) {
                    doc->begindrag = doc->hover;
                } else if (!doc->hover.Thin()) {
                    if (doc->begindrag.Thin() || doc->selected.Thin()) {
                        doc->SetSelect(doc->hover);
                        doc->ResetCursor();
                        Refresh();
                    } else {
                        Selection old = doc->selected;
                        doc->selected.Merge(doc->begindrag, doc->hover);
                        if (!(old == doc->selected)) {
                            doc->ResetCursor();
                            Refresh();
                        }
                    }
                }
            }
            sys->frame->UpdateStatus(doc->selected, true);
        } else if (me.MiddleIsDown()) {
            wxPoint p = me.GetPosition() - lastmousepos;
            CursorScroll(-p.x, -p.y);
        } else {
            if (doc->hover != doc->prev && !doc->hover.Thin()) sys->frame->UpdateStatus(doc->hover, false);
        }
        lastmousepos = me.GetPosition();
    }

    void SelectClick(int mx, int my, bool right, int isctrlshift) {
        wxInfoDC dc(this);
        if (mx < 0 || my < 0)
            return;  // for some reason, using just the "menu" key sends a right-click at (-1, -1)
        doc->isctrlshiftdrag = isctrlshift;
        doc->UpdateHover(dc, mx, my);
        doc->SelectClick(right);
        sys->frame->UpdateStatus(doc->selected, true);
        Refresh();
    }

    void OnLeftDown(wxMouseEvent &me) {
        #ifndef __WXMSW__
        // seems to not want to give the canvas focus otherwise (thinks its already in focus
        // when its not?)
        if (frame->filter) frame->filter->SetFocus();
        #endif
        SetFocus();
        if (me.ShiftDown())
            OnMotion(me);
        else
            SelectClick(me.GetX(), me.GetY(), false, me.CmdDown() + me.AltDown() * 2);
    }

    void OnLeftUp(wxMouseEvent &me) {
        if (me.CmdDown() || me.AltDown()) {
            wxInfoDC dc(this);
            doc->UpdateHover(dc, me.GetX(), me.GetY());
            doc->SelectUp();
            sys->frame->UpdateStatus(doc->selected, true);
            Refresh();
        }
    }

    void OnRightDown(wxMouseEvent &me) {
        SetFocus();
        SelectClick(me.GetX(), me.GetY(), true, 0);
        lastrmbwaswithctrl = me.CmdDown();
        #ifndef __WXMSW__
        me.Skip();  // otherwise EVT_CONTEXT_MENU won't be triggered?
        #endif
    }

    void OnLeftDoubleClick(wxMouseEvent &me) {
        wxInfoDC dc(this);
        doc->UpdateHover(dc, me.GetX(), me.GetY());
        doc->DoubleClick();
        sys->frame->UpdateStatus(doc->selected, true);
        Refresh();
    }

    void OnKeyDown(wxKeyEvent &ce) { ce.Skip(); }
    void OnChar(wxKeyEvent &ce) {
        /*
        if (sys->insidefiledialog)
        {
            ce.Skip();
            return;
        }
        */
        #ifndef __WXMAC__
            // Without this check, Alt+[Alphanumericals], Alt+Shift+[Alphanumericals] and
            // Alt+[Shift]+cursor (scrolling) don't work. The 128 makes sure unicode entry on e.g.
            // Polish keyboards still works. (on Linux in particular).
            if ((ce.GetModifiers() == wxMOD_ALT || ce.GetModifiers() == (wxMOD_ALT | wxMOD_SHIFT)) &&
                (ce.GetUnicodeKey() < 128)) {
                ce.Skip();
                return;
            }
        #endif

        bool unprocessed = false;
        sys->frame->SetStatus(doc->Key(ce.GetUnicodeKey(), ce.GetKeyCode(), ce.AltDown(),
                                       ce.CmdDown(), ce.ShiftDown(), unprocessed));
        if (unprocessed) ce.Skip();
    }

    void OnMouseWheel(wxMouseEvent &me) {
        bool ctrl = me.CmdDown();
        if (sys->zoomscroll) ctrl = !ctrl;
        if (me.AltDown() || ctrl || me.ShiftDown()) {
            mousewheelaccum += me.GetWheelRotation();
            int steps = mousewheelaccum / me.GetWheelDelta();
            if (!steps) return;
            mousewheelaccum -= steps * me.GetWheelDelta();
            sys->frame->SetStatus(doc->Wheel(steps, me.AltDown(), ctrl, me.ShiftDown()));
        } else if (me.GetWheelAxis()) {
            CursorScroll(me.GetWheelRotation() * g_scrollratewheel, 0);
        } else {
            CursorScroll(0, -me.GetWheelRotation() * g_scrollratewheel);
        }
    }

    void OnSize(wxSizeEvent &se) {}
    void OnContextMenuClick(wxContextMenuEvent &cme) {
        if (lastrmbwaswithctrl) {
            auto tagmenu = make_unique<wxMenu>();
            doc->RecreateTagMenu(*tagmenu);
            PopupMenu(tagmenu.get());
        } else {
            PopupMenu(frame->editmenupopup);
        }
    }

    void OnScrollWin(wxScrollWinEvent &swe) {
        // This only gets called when scrolling using the scroll bar, not with mousewheel.
        swe.Skip();  // Use default scrolling behavior.
    }

    void CursorScroll(int dx, int dy) {
        int x, y;
        GetViewStart(&x, &y);
        x += dx;
        y += dy;
        // EnableScrolling(true, true);
        Scroll(x, y);
        // EnableScrolling(false, false);
    }

    DECLARE_EVENT_TABLE()
};
```

## File: src/tsframe.h
```c
struct TSFrame : wxFrame {
    TSApp *app;
    wxIcon icon;
    wxTaskBarIcon taskbaricon;
    wxMenu *editmenupopup;
    wxFileHistory filehistory;
    #ifdef ENABLE_LOBSTER
        wxFileHistory scripts {A_MAXACTION - A_SCRIPT, A_SCRIPT};
    #endif
    wxFileSystemWatcher *watcher;
    wxAuiNotebook *notebook {nullptr};
    wxAuiManager aui {this};
    wxBitmap line_nw;
    wxBitmap line_sw;
    wxBitmap foldicon;
    bool fromclosebox {true};
    bool watcherwaitingforuser {false};
    wxColour toolbarbackgroundcolor {0xD8C7BC};
    wxTextCtrl *filter {nullptr};
    wxTextCtrl *replaces {nullptr};
    ColorDropdown *cellcolordropdown {nullptr};
    ColorDropdown *textcolordropdown {nullptr};
    ColorDropdown *bordercolordropdown {nullptr};
    ImageDropdown *imagedropdown {nullptr};
    wxString imagepath;
    int refreshhack {0};
    int refreshhackinstances {0};
    std::map<wxString, wxString> menustrings;

    TSFrame(TSApp *_app)
        : wxFrame((wxFrame *)nullptr, wxID_ANY, "TreeSheets", wxDefaultPosition, wxDefaultSize,
                  wxDEFAULT_FRAME_STYLE),
          app(_app) {
        sys->frame = this;

        class MyLog : public wxLog {
            void DoLogString(const wxChar *message, time_t timestamp) { DoLogText(*message); }
            void DoLogText(const wxString &message) {
                #ifdef WIN32
                OutputDebugString(message.c_str());
                OutputDebugString(L"\n");
                #else
                fputws(message.c_str(), stderr);
                fputws(L"\n", stderr);
                #endif
            }
        };

        wxLog::SetActiveTarget(new MyLog());

        wxLogMessage("%s", wxVERSION_STRING);

        aui.SetManagedWindow(this);

        wxInitAllImageHandlers();

        wxIconBundle icons;
        wxIcon iconbig;
        #ifdef WIN32
            int iconsmall = ::GetSystemMetrics(SM_CXSMICON);
            int iconlarge = ::GetSystemMetrics(SM_CXICON);
        #endif
        icon.LoadFile(app->GetDataPath("images/icon16.png"), wxBITMAP_TYPE_PNG
            #ifdef WIN32
                , iconsmall, iconsmall
            #endif
        );
        iconbig.LoadFile(app->GetDataPath("images/icon32.png"), wxBITMAP_TYPE_PNG
            #ifdef WIN32
                , iconlarge, iconlarge
            #endif
        );
        if (!icon.IsOk() || !iconbig.IsOk()) {
            wxMessageBox(_("Error loading core data file (TreeSheets not installed correctly?)"),
                         _("Initialization Error"), wxOK, this);
            exit(1);
        }
        icons.AddIcon(icon);
        icons.AddIcon(iconbig);
        SetIcons(icons);

        RenderFolderIcon();
        line_nw.LoadFile(app->GetDataPath("images/render/line_nw.png"), wxBITMAP_TYPE_PNG);
        line_sw.LoadFile(app->GetDataPath("images/render/line_sw.png"), wxBITMAP_TYPE_PNG);

        imagepath = app->GetDataPath("images/nuvola/dropdown/");

        if (sys->singletray)
            taskbaricon.Connect(wxID_ANY, wxEVT_TASKBAR_LEFT_UP,
                        wxTaskBarIconEventHandler(TSFrame::OnTBIDBLClick), nullptr, this);
        else
            taskbaricon.Connect(wxID_ANY, wxEVT_TASKBAR_LEFT_DCLICK,
                        wxTaskBarIconEventHandler(TSFrame::OnTBIDBLClick), nullptr, this);

        bool showtbar, showsbar, lefttabs;

        sys->cfg->Read("showtbar", &showtbar, true);
        sys->cfg->Read("showsbar", &showsbar, true);
        sys->cfg->Read("lefttabs", &lefttabs, true);

        filehistory.Load(*sys->cfg);
        #ifdef ENABLE_LOBSTER
            auto oldpath = sys->cfg->GetPath();
            sys->cfg->SetPath("/scripts");
            scripts.Load(*sys->cfg);
            sys->cfg->SetPath(oldpath);
        #endif

        #ifdef __WXMAC__
            #define CTRLORALT "CTRL"
        #else
            #define CTRLORALT "ALT"
        #endif

        #ifdef __WXMAC__
            #define ALTORCTRL "ALT"
        #else
            #define ALTORCTRL "CTRL"
        #endif

        auto expmenu = new wxMenu();
        MyAppend(expmenu, A_EXPXML, _("&XML..."),
                 _("Export the current view as XML (which can also be reimported without losing structure)"));
        expmenu->AppendSeparator();
        MyAppend(expmenu, A_EXPHTMLT, _("&HTML (Tables+Styling)..."),
                 _("Export the current view as HTML using nested tables, that will look somewhat like the TreeSheet"));
        MyAppend(expmenu, A_EXPHTMLTE, _("&HTML (Tables+Styling+Images)..."),
                 _("Export the curent view as HTML using nested tables and exported images"));
        MyAppend(expmenu, A_EXPHTMLB, _("HTML (&Bullet points)..."),
                 _("Export the current view as HTML as nested bullet points."));
        MyAppend(expmenu, A_EXPHTMLO, _("HTML (&Outline)..."),
                 _("Export the current view as HTML as nested headers, suitable for importing into Word's outline mode"));
        expmenu->AppendSeparator();
        MyAppend(
            expmenu, A_EXPTEXT, _("Indented &Text..."),
            _("Export the current view as tree structured text, using spaces for each indentation level. Suitable for importing into mindmanagers and general text programs"));
        MyAppend(
            expmenu, A_EXPCSV, _("&Comma delimited text (CSV)..."),
            _("Export the current view as CSV. Good for spreadsheets and databases. Only works on grids with no sub-grids (use the Flatten operation first if need be)"));
        expmenu->AppendSeparator();
        MyAppend(expmenu, A_EXPIMAGE, _("&Image..."),
                 _("Export the current view as an image. Useful for faithful renderings of the TreeSheet, and programs that don't accept any of the above options"));
        MyAppend(expmenu, A_EXPSVG, _("&Vector graphics..."),
                _("Export the current view to a SVG vector file."));

        auto impmenu = new wxMenu();
        MyAppend(impmenu, A_IMPXML, _("XML..."));
        MyAppend(impmenu, A_IMPXMLA, _("XML (attributes too, for OPML etc)..."));
        MyAppend(impmenu, A_IMPTXTI, _("Indented text..."));
        MyAppend(impmenu, A_IMPTXTC, _("Comma delimited text (CSV)..."));
        MyAppend(impmenu, A_IMPTXTS, _("Semi-Colon delimited text (CSV)..."));
        MyAppend(impmenu, A_IMPTXTT, _("Tab delimited text..."));

        auto recentmenu = new wxMenu();
        filehistory.UseMenu(recentmenu);
        filehistory.AddFilesToMenu();

        auto filemenu = new wxMenu();
        MyAppend(filemenu, wxID_NEW, _("&New") + "\tCTRL+N", _("Create a new document"));
        MyAppend(filemenu, wxID_OPEN, _("&Open...") + "\tCTRL+O", _("Open an existing document"));
        MyAppend(filemenu, wxID_CLOSE, _("&Close") + "\tCTRL+W", _("Close current document"));
        filemenu->AppendSubMenu(recentmenu, _("&Recent files"));
        MyAppend(filemenu, wxID_SAVE, _("&Save") + "\tCTRL+S", _("Save current document"));
        MyAppend(filemenu, wxID_SAVEAS, _("Save &As..."),
                 _("Save current document with a different filename"));
        MyAppend(filemenu, A_SAVEALL, _("Save All"));
        filemenu->AppendSeparator();
        MyAppend(filemenu, A_PAGESETUP, _("Page Setup..."));
        MyAppend(filemenu, A_PRINTSCALE, _("Set Print Scale..."));
        MyAppend(filemenu, wxID_PREVIEW, _("Print preview..."));
        MyAppend(filemenu, wxID_PRINT, _("&Print...") + "\tCTRL+P");
        filemenu->AppendSeparator();
        filemenu->AppendSubMenu(expmenu, _("Export &view as"));
        filemenu->AppendSubMenu(impmenu, _("Import from"));
        filemenu->AppendSeparator();
        MyAppend(filemenu, wxID_EXIT, _("&Exit") + "\tCTRL+Q", _("Quit this program"));

        wxMenu *editmenu;
        loop(twoeditmenus, 2) {
            auto sizemenu = new wxMenu();
            MyAppend(sizemenu, A_INCSIZE,
                     _("&Increase text size (SHIFT+mousewheel)") + "\tSHIFT+PGUP");
            MyAppend(sizemenu, A_DECSIZE,
                     _("&Decrease text size (SHIFT+mousewheel)") + "\tSHIFT+PGDN");
            MyAppend(sizemenu, A_RESETSIZE, _("&Reset text sizes") + "\tCTRL+SHIFT+S");
            MyAppend(sizemenu, A_MINISIZE, _("&Shrink text of all sub-grids") + "\tCTRL+SHIFT+M");
            sizemenu->AppendSeparator();
            MyAppend(sizemenu, A_INCWIDTH,
                     _("Increase column width (ALT+mousewheel)") + "\tALT+PGUP");
            MyAppend(sizemenu, A_DECWIDTH,
                     _("Decrease column width (ALT+mousewheel)") + "\tALT+PGDN");
            MyAppend(sizemenu, A_INCWIDTHNH,
                     _("Increase column width (no sub grids)") + "\tCTRL+ALT+PGUP");
            MyAppend(sizemenu, A_DECWIDTHNH,
                     _("Decrease column width (no sub grids)") + "\tCTRL+ALT+PGDN");
            MyAppend(sizemenu, A_RESETWIDTH, _("Reset column widths") + "\tCTRL+R",
                     _("Reset the column widths in the selection to the default column width"));

            auto bordmenu = new wxMenu();
            MyAppend(bordmenu, A_BORD0, _("Border &0") + "\tCTRL+SHIFT+9");
            MyAppend(bordmenu, A_BORD1, _("Border &1") + "\tCTRL+SHIFT+1");
            MyAppend(bordmenu, A_BORD2, _("Border &2") + "\tCTRL+SHIFT+2");
            MyAppend(bordmenu, A_BORD3, _("Border &3") + "\tCTRL+SHIFT+3");
            MyAppend(bordmenu, A_BORD4, _("Border &4") + "\tCTRL+SHIFT+4");
            MyAppend(bordmenu, A_BORD5, _("Border &5") + "\tCTRL+SHIFT+5");

            auto selmenu = new wxMenu();
            MyAppend(selmenu, A_NEXT,
                #ifdef __WXGTK__
                    _("Move to next cell (TAB)")
                #else
                     _("Move to next cell") + "\tTAB"
                #endif
            );
            MyAppend(selmenu, A_PREV,
                #ifdef __WXGTK__
                    _("Move to previous cell (SHIFT+TAB)")
                #else
                     _("Move to previous cell") + "\tSHIFT+TAB"
                #endif
            );
            selmenu->AppendSeparator();
            MyAppend(selmenu, wxID_SELECTALL, _("Select &all in current grid/cell") + "\tCTRL+A");
            selmenu->AppendSeparator();
            MyAppend(selmenu, A_LEFT,
                #ifdef __WXGTK__
                    _("Move Selection Left (LEFT)")
                #else
                     _("Move Selection Left") + "\tLEFT"
                #endif
            );
            MyAppend(selmenu, A_RIGHT,
                #ifdef __WXGTK__
                    _("Move Selection Right (RIGHT)")
                #else
                     _("Move Selection Right") + "\tRIGHT"
                #endif
            );
            MyAppend(selmenu, A_UP,
                #ifdef __WXGTK__
                    _("Move Selection Up (UP)")
                #else
                     _("Move Selection Up") + "\tUP"
                #endif
            );
            MyAppend(selmenu, A_DOWN,
                #ifdef __WXGTK__
                    _("Move Selection Down (DOWN)")
                #else
                     _("Move Selection Down") + "\tDOWN"
                #endif
            );
            selmenu->AppendSeparator();
            MyAppend(selmenu, A_MLEFT, _("Move Cells Left") + "\tCTRL+LEFT");
            MyAppend(selmenu, A_MRIGHT, _("Move Cells Right") + "\tCTRL+RIGHT");
            MyAppend(selmenu, A_MUP, _("Move Cells Up") + "\tCTRL+UP");
            MyAppend(selmenu, A_MDOWN, _("Move Cells Down") + "\tCTRL+DOWN");
            selmenu->AppendSeparator();
            MyAppend(selmenu, A_SLEFT, _("Extend Selection Left") + "\tSHIFT+LEFT");
            MyAppend(selmenu, A_SRIGHT, _("Extend Selection Right") + "\tSHIFT+RIGHT");
            MyAppend(selmenu, A_SUP, _("Extend Selection Up") + "\tSHIFT+UP");
            MyAppend(selmenu, A_SDOWN, _("Extend Selection Down") + "\tSHIFT+DOWN");
            selmenu->AppendSeparator();
            MyAppend(selmenu, A_SROWS, _("Extend Selection Full Rows") + "\tCTRL+SHIFT+B");
            MyAppend(selmenu, A_SCLEFT, _("Extend Selection Rows Left") + "\tCTRL+SHIFT+LEFT");
            MyAppend(selmenu, A_SCRIGHT, _("Extend Selection Rows Right") + "\tCTRL+SHIFT+RIGHT");
            selmenu->AppendSeparator();
            MyAppend(selmenu, A_SCOLS, _("Extend Selection Full Columns") + "\tCTRL+SHIFT+A");
            MyAppend(selmenu, A_SCUP, _("Extend Selection Columns Up") + "\tCTRL+SHIFT+UP");
            MyAppend(selmenu, A_SCDOWN, _("Extend Selection Columns Down") + "\tCTRL+SHIFT+DOWN");
            selmenu->AppendSeparator();
            MyAppend(selmenu, A_CANCELEDIT, _("Select &Parent") + "\tESC");
            MyAppend(selmenu, A_ENTERGRID, _("Select First &Child") + "\tSHIFT+ENTER");
            selmenu->AppendSeparator();
            MyAppend(selmenu, A_LINK, _("Go To &Matching Cell (Text)") + "\tF6");
            MyAppend(selmenu, A_LINKREV, _("Go To Matching Cell (Text, Reverse)") + "\tSHIFT+F6");
            MyAppend(selmenu, A_LINKIMG, _("Go To Matching Cell (Image)") + "\tF7");
            MyAppend(selmenu, A_LINKIMGREV,
                     _("Go To Matching Cell (Image, Reverse)") + "\tSHIFT+F7");

            auto temenu = new wxMenu();
            MyAppend(temenu, A_LEFT, _("Cursor Left") + "\tLEFT");
            MyAppend(temenu, A_RIGHT, _("Cursor Right") + "\tRIGHT");
            MyAppend(temenu, A_MLEFT, _("Word Left") + "\tCTRL+LEFT");
            MyAppend(temenu, A_MRIGHT, _("Word Right") + "\tCTRL+RIGHT");
            temenu->AppendSeparator();
            MyAppend(temenu, A_SLEFT, _("Extend Selection Left") + "\tSHIFT+LEFT");
            MyAppend(temenu, A_SRIGHT, _("Extend Selection Right") + "\tSHIFT+RIGHT");
            MyAppend(temenu, A_SCLEFT, _("Extend Selection Word Left") + "\tCTRL+SHIFT+LEFT");
            MyAppend(temenu, A_SCRIGHT, _("Extend Selection Word Right") + "\tCTRL+SHIFT+RIGHT");
            MyAppend(temenu, A_SHOME, _("Extend Selection to Start") + "\tSHIFT+HOME");
            MyAppend(temenu, A_SEND, _("Extend Selection to End") + "\tSHIFT+END");
            temenu->AppendSeparator();
            MyAppend(temenu, A_HOME, _("Start of line of text") + "\tHOME");
            MyAppend(temenu, A_END, _("End of line of text") + "\tEND");
            MyAppend(temenu, A_CHOME, _("Start of text") + "\tCTRL+HOME");
            MyAppend(temenu, A_CEND, _("End of text") + "\tCTRL+END");
            temenu->AppendSeparator();
            MyAppend(temenu, A_ENTERCELL, _("Enter/exit text edit mode") + "\tENTER");
            MyAppend(temenu, A_ENTERCELL_JUMPTOEND,
                     _("...and jump to the end of the text") + "\tF2");
            MyAppend(
                temenu, A_ENTERCELL_JUMPTOSTART,
                _("...and progress to the first cell in the new row") + "\t" ALTORCTRL "+ENTER");
            MyAppend(temenu, A_PROGRESSCELL,
                     _("...and progress to the next cell on the right") + "\t" CTRLORALT "+ENTER");
            MyAppend(temenu, A_CANCELEDIT, _("Cancel text edits") + "\tESC");

            auto stmenu = new wxMenu();
            MyAppend(stmenu, wxID_BOLD, _("Toggle cell &BOLD") + "\tCTRL+B");
            MyAppend(stmenu, wxID_ITALIC, _("Toggle cell &ITALIC") + "\tCTRL+I");
            MyAppend(stmenu, A_TT, _("Toggle cell &typewriter") + "\tCTRL+ALT+T");
            MyAppend(stmenu, wxID_UNDERLINE, _("Toggle cell &underlined") + "\tCTRL+U");
            MyAppend(stmenu, wxID_STRIKETHROUGH, _("Toggle cell &strikethrough") + "\tCTRL+T");
            stmenu->AppendSeparator();
            MyAppend(stmenu, A_RESETSTYLE, _("&Reset text styles") + "\tCTRL+SHIFT+R");
            MyAppend(stmenu, A_RESETCOLOR, _("Reset &colors") + "\tCTRL+SHIFT+C");
            stmenu->AppendSeparator();
            MyAppend(stmenu, A_LASTCELLCOLOR, _("Apply last cell color") + "\tSHIFT+ALT+C");
            MyAppend(stmenu, A_LASTTEXTCOLOR, _("Apply last text color") + "\tSHIFT+ALT+T");
            MyAppend(stmenu, A_LASTBORDCOLOR, _("Apply last border color") + "\tSHIFT+ALT+B");
            MyAppend(stmenu, A_OPENCELLCOLOR, _("Open cell colors") + "\tSHIFT+ALT+F9");
            MyAppend(stmenu, A_OPENTEXTCOLOR, _("Open text colors") + "\tSHIFT+ALT+F10");
            MyAppend(stmenu, A_OPENBORDCOLOR, _("Open border colors") + "\tSHIFT+ALT+F11");
            MyAppend(stmenu, A_OPENIMGDROPDOWN, _("Open image dropdown") + "\tSHIFT+ALT+F12");

            auto tagmenu = new wxMenu();
            MyAppend(tagmenu, A_TAGADD, _("&Add Cell Text as Tag"));
            MyAppend(tagmenu, A_TAGREMOVE, _("&Remove Cell Text from Tags"));
            MyAppend(tagmenu, A_NOP, _("&Set Cell Text to tag (use CTRL+RMB)"),
                     _("Hold CTRL while pressing right mouse button to quickly set a tag for the current cell using a popup menu"));

            auto orgmenu = new wxMenu();
            MyAppend(orgmenu, A_TRANSPOSE, _("&Transpose") + "\tCTRL+SHIFT+T",
                     _("changes the orientation of a grid"));
            MyAppend(orgmenu, A_SORT, _("Sort &Ascending"),
                     _("Make a 1xN selection to indicate which column to sort on, and which rows to affect"));
            MyAppend(orgmenu, A_SORTD, _("Sort &Descending"),
                     _("Make a 1xN selection to indicate which column to sort on, and which rows to affect"));
            MyAppend(orgmenu, A_HSWAP, _("Hierarchy &Swap") + "\tF8",
                     _("Swap all cells with this text at this level (or above) with the parent"));
            MyAppend(orgmenu, A_HIFY, _("&Hierarchify"),
                     _("Convert an NxN grid with repeating elements per column into an 1xN grid with hierarchy, useful to convert data from spreadsheets"));
            MyAppend(orgmenu, A_FLATTEN, _("&Flatten"),
                     _("Takes a hierarchy (nested 1xN or Nx1 grids) and converts it into a flat NxN grid, useful for export to spreadsheets"));

            auto imgmenu = new wxMenu();
            MyAppend(imgmenu, A_IMAGE, _("&Add..."), _("Add an image to the selected cell"));
            MyAppend(imgmenu, A_IMAGESVA, _("&Save as..."),
                     _("Save image(s) from selected cell(s) to disk. Multiple images will be saved with a counter appended to each file name."));
            imgmenu->AppendSeparator();
            MyAppend(
                imgmenu, A_IMAGESCP, _("Scale (re-sa&mple pixels, by %)"),
                _("Change the image(s) size if it is too big, by reducing the amount of pixels"));
            MyAppend(
                imgmenu, A_IMAGESCW, _("Scale (re-sample pixels, by &width)"),
                _("Change the image(s) size if it is too big, by reducing the amount of pixels"));
            MyAppend(imgmenu, A_IMAGESCF, _("Scale (&display only)"),
                     _("Change the image(s) size if it is too big or too small, by changing the size shown on screen. Applies to all uses of this image."));
            MyAppend(imgmenu, A_IMAGESCN, _("&Reset Scale (display only)"),
                     _("Change the image(s) scale to match DPI of the current display. Applies to all uses of this image."));
            imgmenu->AppendSeparator();
            MyAppend(
                imgmenu, A_SAVE_AS_JPEG, _("Embed as &JPEG"),
                _("Embed the image(s) in the selected cells in JPEG format (reduces data size)"));
            MyAppend(imgmenu, A_SAVE_AS_PNG, _("Embed as &PNG"),
                     _("Embed the image(s) in the selected cells in PNG format (default)"));
            imgmenu->AppendSeparator();
            MyAppend(imgmenu, A_LASTIMAGE, _("Insert last image") + "\tSHIFT+ALT+i",
                     _("Insert the last image that has been inserted before in TreeSheets."));
            MyAppend(imgmenu, A_IMAGER, _("Remo&ve"),
                     _("Remove image(s) from the selected cells"));

            auto navmenu = new wxMenu();
            MyAppend(navmenu, A_BROWSE, _("Open link in &browser") + "\tF5",
                     _("Opens up the text from the selected cell in browser (should start be a valid URL)"));
            MyAppend(navmenu, A_BROWSEF, _("Open &file") + "\tF4",
                     _("Opens up the text from the selected cell in default application for the file type"));

            auto laymenu = new wxMenu();
            MyAppend(laymenu, A_V_GS,
                     _("Vertical Layout with Grid Style Rendering") + "\t" CTRLORALT "+1");
            MyAppend(laymenu, A_V_BS,
                     _("Vertical Layout with Bubble Style Rendering") + "\t" CTRLORALT "+2");
            MyAppend(laymenu, A_V_LS,
                     _("Vertical Layout with Line Style Rendering") + "\t" CTRLORALT "+3");
            laymenu->AppendSeparator();
            MyAppend(laymenu, A_H_GS,
                     _("Horizontal Layout with Grid Style Rendering") + "\t" CTRLORALT "+4");
            MyAppend(laymenu, A_H_BS,
                     _("Horizontal Layout with Bubble Style Rendering") + "\t" CTRLORALT "+5");
            MyAppend(laymenu, A_H_LS,
                     _("Horizontal Layout with Line Style Rendering") + "\t" CTRLORALT "+6");
            laymenu->AppendSeparator();
            MyAppend(laymenu, A_GS, _("Grid Style Rendering") + "\t" CTRLORALT "+7");
            MyAppend(laymenu, A_BS, _("Bubble Style Rendering") + "\t" CTRLORALT "+8");
            MyAppend(laymenu, A_LS, _("Line Style Rendering") + "\t" CTRLORALT "+9");
            laymenu->AppendSeparator();
            MyAppend(laymenu, A_TEXTGRID, _("Toggle Vertical Layout") + "\t" CTRLORALT "+0",
                     _("Make a hierarchy layout more vertical (default) or more horizontal"));

            editmenu = new wxMenu();
            MyAppend(editmenu, wxID_CUT, _("Cu&t") + "\tCTRL+X", _("Cut selection"));
            MyAppend(editmenu, wxID_COPY, _("&Copy") + "\tCTRL+C", _("Copy selection"));
            editmenu->AppendSeparator();
            MyAppend(editmenu, A_COPYWI, _("Copy with &Images") + "\tCTRL+ALT+C");
            MyAppend(editmenu, A_COPYBM, _("&Copy as Bitmap"));
            MyAppend(editmenu, A_COPYCT, _("Copy As Continuous Text"));
            editmenu->AppendSeparator();
            MyAppend(editmenu, wxID_PASTE, _("&Paste") + "\tCTRL+V", _("Paste clipboard contents"));
            MyAppend(editmenu, A_PASTESTYLE, _("Paste Style Only") + "\tCTRL+SHIFT+V",
                     _("only sets the colors and style of the copied cell, and keeps the text"));
            MyAppend(editmenu, A_COLLAPSE, _("Collapse Ce&lls") + "\tCTRL+L");
            editmenu->AppendSeparator();

            MyAppend(editmenu, A_EDITNOTE, _("Edit &Note") + "\tCTRL+E",
                     _("Edit the note of the selected cell"));
            MyAppend(editmenu, wxID_UNDO, _("&Undo") + "\tCTRL+Z",
                     _("revert the changes, one step at a time"));
            MyAppend(editmenu, wxID_REDO, _("&Redo") + "\tCTRL+Y",
                     _("redo any undo steps, if you haven't made changes since"));
            editmenu->AppendSeparator();
            MyAppend(
                editmenu, A_DELETE, _("&Delete After") + "\tDEL",
                _("Deletes the column of cells after the selected grid line, or the row below"));
            MyAppend(
                editmenu, A_BACKSPACE, _("Delete Before") + "\tBACK",
                _("Deletes the column of cells before the selected grid line, or the row above"));
            MyAppend(editmenu, A_DELETE_WORD, _("Delete Word After") + "\tCTRL+DEL",
                     _("Deletes the entire word after the cursor"));
            MyAppend(editmenu, A_BACKSPACE_WORD, _("Delete Word Before") + "\tCTRL+BACK",
                     _("Deletes the entire word before the cursor"));
            editmenu->AppendSeparator();
            MyAppend(editmenu, A_NEWGRID,
                     #ifdef __WXMAC__
                     _("&Insert New Grid") + "\tCTRL+G",
                     #else
                     _("&Insert New Grid") + "\tINS",
                     #endif
                     _("Adds a grid to the selected cell"));
            MyAppend(editmenu, A_WRAP, _("&Wrap in new parent") + "\tF9",
                     _("Creates a new level of hierarchy around the current selection"));
            editmenu->AppendSeparator();
            // F10 is tied to the OS on both Ubuntu and OS X, and SHIFT+F10 is now right
            // click on all platforms?
            MyAppend(editmenu, A_FOLD,
                     #ifndef WIN32
                     _("Toggle Fold") + "\tCTRL+F10",
                     #else
                     _("Toggle Fold") + "\tF10",
                     #endif
                     _("Toggles showing the grid of the selected cell(s)"));
            MyAppend(editmenu, A_FOLDALL, _("Fold All") + "\tCTRL+SHIFT+F10",
                     _("Folds the grid of the selected cell(s) recursively"));
            MyAppend(editmenu, A_UNFOLDALL, _("Unfold All") + "\tCTRL+ALT+F10",
                     _("Unfolds the grid of the selected cell(s) recursively"));
            editmenu->AppendSeparator();
            editmenu->AppendSubMenu(selmenu, _("&Selection"));
            editmenu->AppendSubMenu(orgmenu, _("&Grid Reorganization"));
            editmenu->AppendSubMenu(laymenu, _("&Layout && Render Style"));
            editmenu->AppendSubMenu(imgmenu, _("&Images"));
            editmenu->AppendSubMenu(navmenu, _("&Browsing"));
            editmenu->AppendSubMenu(temenu, _("Text &Editing"));
            editmenu->AppendSubMenu(sizemenu, _("Text Sizing"));
            editmenu->AppendSubMenu(stmenu, _("Text Style"));
            editmenu->AppendSubMenu(bordmenu, _("Set Grid Border Width"));
            editmenu->AppendSubMenu(tagmenu, _("Tag"));

            if (!twoeditmenus) editmenupopup = editmenu;
        }

        auto semenu = new wxMenu();
        MyAppend(semenu, wxID_FIND, _("&Search") + "\tCTRL+F", _("Find in document"));
        semenu->AppendCheckItem(A_CASESENSITIVESEARCH, _("Case-sensitive search"));
        semenu->Check(A_CASESENSITIVESEARCH, sys->casesensitivesearch);
        semenu->AppendSeparator();
        MyAppend(semenu, A_SEARCHNEXT, _("&Next Match") + "\tF3", _("Go to next search match"));
        MyAppend(semenu, A_SEARCHPREV, _("&Previous Match") + "\tSHIFT+F3",
                 _("Go to previous search match"));
        semenu->AppendSeparator();
        MyAppend(semenu, wxID_REPLACE, _("&Replace") + "\tCTRL+H",
                 _("Find and replace in document"));
        MyAppend(semenu, A_REPLACEONCE, _("Replace in Current &Selection") + "\tCTRL+K");
        MyAppend(semenu, A_REPLACEONCEJ,
                 _("Replace in Current Selection && &Jump Next") + "\tCTRL+J");
        MyAppend(semenu, A_REPLACEALL, _("Replace &All"));

        auto scrollmenu = new wxMenu();
        MyAppend(scrollmenu, A_AUP, _("Scroll Up (mousewheel)") + "\tPGUP");
        MyAppend(scrollmenu, A_AUP, _("Scroll Up (mousewheel)") + "\tALT+UP");
        MyAppend(scrollmenu, A_ADOWN, _("Scroll Down (mousewheel)") + "\tPGDN");
        MyAppend(scrollmenu, A_ADOWN, _("Scroll Down (mousewheel)") + "\tALT+DOWN");
        MyAppend(scrollmenu, A_ALEFT, _("Scroll Left") + "\tALT+LEFT");
        MyAppend(scrollmenu, A_ARIGHT, _("Scroll Right") + "\tALT+RIGHT");

        auto filtermenu = new wxMenu();
        MyAppend(filtermenu, A_FILTEROFF, _("Turn filter &off") + "\tCTRL+SHIFT+F");
        MyAppend(filtermenu, A_FILTERS, _("Show only cells in current search"));
        MyAppend(filtermenu, A_FILTERRANGE, _("Show last edits in specific date range"));
        // xgettext:no-c-format
        MyAppend(filtermenu, A_FILTER5, _("Show 5% of last edits"));
        // xgettext:no-c-format
        MyAppend(filtermenu, A_FILTER10, _("Show 10% of last edits"));
        // xgettext:no-c-format
        MyAppend(filtermenu, A_FILTER20, _("Show 20% of last edits"));
        // xgettext:no-c-format
        MyAppend(filtermenu, A_FILTER50, _("Show 50% of last edits"));
        // xgettext:no-c-format
        MyAppend(filtermenu, A_FILTERM, _("Show 1% more than the last filter"));
        // xgettext:no-c-format
        MyAppend(filtermenu, A_FILTERL, _("Show 1% less than the last filter"));
        MyAppend(filtermenu, A_FILTERNOTE, _("Show cells with notes"));
        MyAppend(filtermenu, A_FILTERBYCELLBG, _("Filter by the same cell color"));
        MyAppend(filtermenu, A_FILTERMATCHNEXT, _("Go to next filter match") + "\tCTRL+F3");

        auto viewmenu = new wxMenu();
        MyAppend(viewmenu, A_ZOOMIN, _("Zoom &In (CTRL+mousewheel)") + "\tCTRL+PGUP");
        MyAppend(viewmenu, A_ZOOMOUT, _("Zoom &Out (CTRL+mousewheel)") + "\tCTRL+PGDN");
        viewmenu->AppendSeparator();
        MyAppend(
            viewmenu, A_NEXTFILE,
            _("&Next tab")
                 #ifndef __WXGTK__
                    // On Linux, this conflicts with CTRL+I, see Document::Key()
                    // CTRL+SHIFT+TAB below still works, so that will have to be used to switch tabs.
                     + "\tCTRL+TAB"
                 #endif
            ,
            _("Go to the document in the next tab"));
        MyAppend(viewmenu, A_PREVFILE, _("Previous tab") + "\tCTRL+SHIFT+TAB",
                 _("Go to the document in the previous tab"));
        viewmenu->AppendSeparator();
        MyAppend(viewmenu, A_FULLSCREEN,
                 #ifdef __WXMAC__
                 _("Toggle &Fullscreen View") + "\tCTRL+F11");
                 #else
                 _("Toggle &Fullscreen View") + "\tF11");
                 #endif
        MyAppend(viewmenu, A_SCALED,
                 #ifdef __WXMAC__
                 _("Toggle &Scaled Presentation View") + "\tCTRL+F12");
                 #else
                 _("Toggle &Scaled Presentation View") + "\tF12");
                 #endif
        viewmenu->AppendSeparator();
        viewmenu->AppendSubMenu(scrollmenu, _("Scroll Sheet"));
        viewmenu->AppendSubMenu(filtermenu, _("Filter"));

        auto roundmenu = new wxMenu();
        roundmenu->AppendRadioItem(A_ROUND0, _("Radius &0"));
        roundmenu->AppendRadioItem(A_ROUND1, _("Radius &1"));
        roundmenu->AppendRadioItem(A_ROUND2, _("Radius &2"));
        roundmenu->AppendRadioItem(A_ROUND3, _("Radius &3"));
        roundmenu->AppendRadioItem(A_ROUND4, _("Radius &4"));
        roundmenu->AppendRadioItem(A_ROUND5, _("Radius &5"));
        roundmenu->AppendRadioItem(A_ROUND6, _("Radius &6"));
        roundmenu->Check(sys->roundness + A_ROUND0, true);

        auto autoexportmenu = new wxMenu();
        autoexportmenu->AppendRadioItem(A_AUTOEXPORT_HTML_NONE, _("No autoexport"));
        autoexportmenu->AppendRadioItem(A_AUTOEXPORT_HTML_WITH_IMAGES, _("Export with images"),
                                        _("Export to a HTML file with exported images alongside "
                                          "the original TreeSheets file when document is saved"));
        autoexportmenu->AppendRadioItem(A_AUTOEXPORT_HTML_WITHOUT_IMAGES,
                                        _("Export without images"),
                                        _("Export to a HTML file alongside the original "
                                          "TreeSheets file when document is saved"));
        autoexportmenu->Check(sys->autohtmlexport + A_AUTOEXPORT_HTML_NONE, true);

        auto optmenu = new wxMenu();
        MyAppend(optmenu, wxID_SELECT_FONT, _("Font..."),
                 _("Set the font the document text is displayed with"));
        MyAppend(optmenu, A_SET_FIXED_FONT, _("Typewriter font..."),
                 _("Set the font the typewriter text is displayed with."));
        MyAppend(optmenu, A_CUSTKEY, _("Key bindings..."),
                 _("Change the key binding of a menu item"));
        MyAppend(optmenu, A_SETLANG, _("Change language..."), _("Change interface language"));
        MyAppend(optmenu, A_DEFAULTMAXCOLWIDTH, _("Default column width..."),
                 _("Set the default column width for a new grid"));
        optmenu->AppendSeparator();
        MyAppend(optmenu, A_CUSTCOL, _("Custom &color..."),
                 _("Set a custom color for the color dropdowns"));
        MyAppend(
            optmenu, A_COLCELL, _("&Set custom color from cell background"),
            _("Set a custom color for the color dropdowns from the selected cell background"));
        MyAppend(optmenu, A_DEFBGCOL, _("Background color..."),
                 _("Set the color for the document background"));
        MyAppend(optmenu, A_DEFCURCOL, _("Cu&rsor color..."),
                 _("Set the color for the text cursor"));
        optmenu->AppendSeparator();
        MyAppend(optmenu, A_RESETPERSPECTIVE, _("Reset toolbar"),
                 _("Reset the toolbar appearance"));
        optmenu->AppendCheckItem(
            A_SHOWTBAR, _("Toolbar"),
            _("Toggle whether toolbar is shown between menu bar and documents"));
        optmenu->Check(A_SHOWTBAR, sys->showtoolbar);
        optmenu->AppendCheckItem(A_SHOWSBAR, _("Statusbar"),
                                 _("Toggle whether statusbar is shown below the documents"));
        optmenu->Check(A_SHOWSBAR, sys->showstatusbar);
        optmenu->AppendCheckItem(
            A_LEFTTABS, _("File Tabs on the bottom"),
            _("Toggle whether file tabs are shown on top or on bottom of the documents"));
        optmenu->Check(A_LEFTTABS, lefttabs);
        optmenu->AppendCheckItem(A_TOTRAY, _("Minimize to tray"),
                                 _("Toogle whether window is minimized to system tray"));
        optmenu->Check(A_TOTRAY, sys->totray);
        optmenu->AppendCheckItem(A_MINCLOSE, _("Minimize on close"),
                                 _("Toggle whether the window is minimized instead of closed"));
        optmenu->Check(A_MINCLOSE, sys->minclose);
        optmenu->AppendCheckItem(
            A_SINGLETRAY, _("Single click maximize from tray"),
            _("Toggle whether only one click is required to maximize from system tray"));
        optmenu->Check(A_SINGLETRAY, sys->singletray);
        optmenu->AppendCheckItem(A_STARTMINIMIZED, _("Start minimized"),
                                 _("Start the application minimized"));
        optmenu->Check(A_STARTMINIMIZED, sys->startminimized);
        optmenu->AppendSeparator();
        optmenu->AppendCheckItem(A_ZOOMSCR, _("Swap mousewheel scrolling and zooming"));
        optmenu->Check(A_ZOOMSCR, sys->zoomscroll);
        optmenu->AppendCheckItem(A_THINSELC, _("Navigate in between cells with cursor keys"),
                                 _("Toggle whether the cursor keys are used for navigation in addition to text editing"));
        optmenu->Check(A_THINSELC, sys->thinselc);
        optmenu->AppendSeparator();
        optmenu->AppendCheckItem(A_MAKEBAKS, _("Backup files"),
                                 _("Create backup file before document is saved to file"));
        optmenu->Check(A_MAKEBAKS, sys->makebaks);
        optmenu->AppendCheckItem(A_AUTOSAVE, _("Autosave"),
                                 _("Save open documents periodically to temporary files"));
        optmenu->Check(A_AUTOSAVE, sys->autosave);
        optmenu->AppendCheckItem(
            A_FSWATCH, _("Autoreload documents"),
            _("Reload when another computer has changed a file (if you have made changes, asks)"));
        optmenu->Check(A_FSWATCH, sys->fswatch);
        optmenu->AppendSubMenu(autoexportmenu, _("Autoexport to HTML"));
        optmenu->AppendSeparator();
        optmenu->AppendCheckItem(
            A_CENTERED, _("Render document centered"),
            _("Toggle whether documents are rendered centered or left aligned"));
        optmenu->Check(A_CENTERED, sys->centered);
        optmenu->AppendCheckItem(
            A_FASTRENDER, _("Faster line rendering"),
            _("Toggle whether lines are drawn solid (faster rendering) or dashed"));
        optmenu->Check(A_FASTRENDER, sys->fastrender);
        optmenu->AppendCheckItem(A_INVERTRENDER, _("Invert in dark mode"),
                                 _("Invert the document in dark mode"));
        optmenu->Check(A_INVERTRENDER, sys->followdarkmode);
        optmenu->AppendSubMenu(roundmenu, _("&Roundness of grid borders"));

        #ifdef ENABLE_LOBSTER
            auto scriptmenu = new wxMenu();
            MyAppend(scriptmenu, A_ADDSCRIPT, _("Add...") + "\tCTRL+ALT+L",
                     _("Add Lobster scripts to the menu"));
            MyAppend(scriptmenu, A_DETSCRIPT, _("Remove...") + "\tCTRL+SHIFT+ALT+L",
                     _("Remove script from list in the menu"));
            scripts.UseMenu(scriptmenu);
            scripts.SetMenuPathStyle(wxFH_PATH_SHOW_NEVER);
            scripts.AddFilesToMenu();

            auto scriptpath = app->GetDataPath("scripts/");
            auto sf = wxFindFirstFile(scriptpath + "*.lobster");
            int sidx = 0;
            while (!sf.empty()) {
                auto fn = wxFileName::FileName(sf).GetFullName();
                scripts.AddFileToHistory(fn);
                sf = wxFindNextFile();
            }
        #endif

        auto markmenu = new wxMenu();
        MyAppend(markmenu, A_MARKDATA, _("&Data") + "\tCTRL+ALT+D");
        MyAppend(markmenu, A_MARKCODE, _("&Operation") + "\tCTRL+ALT+O");
        MyAppend(markmenu, A_MARKVARD, _("Variable &Assign") + "\tCTRL+ALT+A");
        MyAppend(markmenu, A_MARKVARU, _("Variable &Read") + "\tCTRL+ALT+R");
        MyAppend(markmenu, A_MARKVIEWH, _("&Horizontal View") + "\tCTRL+ALT+.");
        MyAppend(markmenu, A_MARKVIEWV, _("&Vertical View") + "\tCTRL+ALT+,");

        auto langmenu = new wxMenu();
        MyAppend(langmenu, wxID_EXECUTE, _("&Run") + "\tCTRL+ALT+F5");
        langmenu->AppendSubMenu(markmenu, _("&Mark as"));
        MyAppend(langmenu, A_CLRVIEW, _("&Clear Views"));

        auto helpmenu = new wxMenu();
        MyAppend(helpmenu, wxID_ABOUT, _("&About..."), _("Show About dialog"));
        helpmenu->AppendSeparator();
        MyAppend(helpmenu, wxID_HELP, _("Interactive &tutorial") + "\tF1",
                 _("Load an interactive tutorial in TreeSheets"));
        MyAppend(helpmenu, A_HELP_OP_REF, _("Operation reference") + "\tCTRL+ALT+F1",
                 _("Load an interactive program operation reference in TreeSheets"));
        helpmenu->AppendSeparator();
        MyAppend(helpmenu, A_TUTORIALWEBPAGE, _("Tutorial &web page"),
                 _("Open the tutorial web page in browser"));
        #ifdef ENABLE_LOBSTER
            MyAppend(helpmenu, A_SCRIPTREFERENCE, _("&Script reference"),
                 _("Open the Lobster script reference in browser"));
        #endif

        wxAcceleratorEntry entries[3];
        entries[0].Set(wxACCEL_SHIFT, WXK_DELETE, wxID_CUT);
        entries[1].Set(wxACCEL_SHIFT, WXK_INSERT, wxID_PASTE);
        entries[2].Set(wxACCEL_CTRL, WXK_INSERT, wxID_COPY);
        wxAcceleratorTable accel(3, entries);
        SetAcceleratorTable(accel);

        auto menubar = new wxMenuBar();
        menubar->Append(filemenu, _("&File"));
        menubar->Append(editmenu, _("&Edit"));
        menubar->Append(semenu, _("&Search"));
        menubar->Append(viewmenu, _("&View"));
        menubar->Append(optmenu, _("&Options"));
        #ifdef ENABLE_LOBSTER
            menubar->Append(scriptmenu, _("S&cript"));
        #endif
        menubar->Append(langmenu, _("&Program"));
        menubar->Append(helpmenu,
                        #ifdef __WXMAC__
                        wxApp::s_macHelpMenuTitleName  // so merges with osx provided help
                        #else
                        _("&Help")
                        #endif
                        );
        #ifdef __WXMAC__
        // these don't seem to work anymore in the newer wxWidgets, handled in the menu event
        // handler below instead
        wxApp::s_macAboutMenuItemId = wxID_ABOUT;
        wxApp::s_macExitMenuItemId = wxID_EXIT;
        wxApp::s_macPreferencesMenuItemId =
            wxID_SELECT_FONT;  // we have no prefs, so for now just select the font
        #endif
        SetMenuBar(menubar);

        RefreshToolBar();

        auto sb = CreateStatusBar(5);
        SetStatusBarPane(0);
        SetDPIAwareStatusWidths();
        sb->Show(sys->showstatusbar);

        notebook =
            new wxAuiNotebook(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                              wxAUI_NB_TAB_MOVE | wxAUI_NB_TAB_SPLIT | wxAUI_NB_SCROLL_BUTTONS |
                                  wxAUI_NB_WINDOWLIST_BUTTON | wxAUI_NB_CLOSE_ON_ALL_TABS |
                                  (lefttabs ? wxAUI_NB_BOTTOM : wxAUI_NB_TOP));

        int display_id = wxDisplay::GetFromWindow(this);
        wxRect disprect = wxDisplay(display_id == wxNOT_FOUND ? 0 : display_id).GetClientArea();
        const int boundary = 64;
        const int defx = disprect.width - 2 * boundary;
        const int defy = disprect.height - 2 * boundary;
        int resx, resy, posx, posy;
        sys->cfg->Read("resx", &resx, defx);
        sys->cfg->Read("resy", &resy, defy);
        sys->cfg->Read("posx", &posx, boundary + disprect.x);
        sys->cfg->Read("posy", &posy, boundary + disprect.y);
        #ifndef __WXGTK__
        // On X11, disprect only refers to the primary screen. Thus, for a multi-head display,
        // the conditions below might be fulfilled (e.g. large window spanning multiple screens
        // or being on the secondary screen), so just ignore them.
        if (resx > disprect.width || resy > disprect.height || posx < disprect.x ||
            posy < disprect.y || posx + resx > disprect.width + disprect.x ||
            posy + resy > disprect.height + disprect.y) {
            // Screen res has been resized since we last ran, set sizes to default to avoid being
            // off-screen.
            resx = defx;
            resy = defy;
            posx = posy = boundary;
            posx += disprect.x;
            posy += disprect.y;
        }
        #endif
        SetSize(resx, resy);
        Move(posx, posy);

        bool ismax;
        sys->cfg->Read("maximized", &ismax, true);

        aui.AddPane(
            notebook,
            wxAuiPaneInfo().Name("notebook").Caption("Notebook").CenterPane().PaneBorder(false));
        aui.LoadPerspective(sys->cfg->Read("perspective", ""));
        aui.Update();

        Show(!IsIconized());

        // needs to be after Show() to avoid scrollbars rendered in the wrong place?
        if (ismax && !IsIconized()) Maximize(true);

        if (sys->startminimized)
            #ifdef __WXGTK__
                CallAfter([this]() { Iconize(true); });
            #else
                Iconize(true);
            #endif

        SetFileAssoc(app->exename);

        wxSafeYield();
    }

    wxArrayString GetToolbarPaneNames() {
        wxArrayString toolbarNames;
        wxAuiPaneInfoArray &all_panes = aui.GetAllPanes();
        for (size_t i = 0; i < all_panes.GetCount(); ++i) {
            wxAuiPaneInfo &pane = all_panes.Item(i);
            if (pane.IsToolbar()) { toolbarNames.Add(pane.name); }
        }
        return toolbarNames;
    }

    void DestroyToolbarPane(const wxString &name) {
        wxAuiPaneInfo &pane = aui.GetPane(name);
        if (pane.IsOk()) {
            wxWindow *wnd = pane.window;
            aui.DetachPane(wnd);
            if (wnd) { wnd->Destroy(); }
        }
    }

    void RefreshToolBar() {
        for (const auto &name : GetToolbarPaneNames()) { DestroyToolbarPane(name); }
        auto iconpath = app->GetDataPath("images/material/toolbar/");
        auto AddToolbarIcon = [&](wxAuiToolBar *tb, const wxChar *name, int action,
                                  wxString iconpath, wxString lighticon, wxString darkicon) {
            tb->AddTool(
                action, name,
                wxBitmapBundle::FromSVGFile(
                    iconpath + (wxSystemSettings::GetAppearance().IsDark() ? darkicon : lighticon),
                    wxSize(24, 24)),
                name, wxITEM_NORMAL);
        };

        auto filetb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                                       wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
        AddToolbarIcon(filetb, _("New (CTRL+n)"), wxID_NEW, iconpath, "filenew.svg",
                       "filenew_dark.svg");
        AddToolbarIcon(filetb, _("Open (CTRL+o)"), wxID_OPEN, iconpath, "fileopen.svg",
                       "fileopen_dark.svg");
        AddToolbarIcon(filetb, _("Save (CTRL+s)"), wxID_SAVE, iconpath, "filesave.svg",
                       "filesave_dark.svg");
        AddToolbarIcon(filetb, _("Save as..."), wxID_SAVEAS, iconpath, "filesaveas.svg",
                       "filesaveas_dark.svg");
        filetb->Realize();

        auto edittb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                                       wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
        AddToolbarIcon(edittb, _("Undo (CTRL+z)"), wxID_UNDO, iconpath, "undo.svg",
                       "undo_dark.svg");
        AddToolbarIcon(edittb, _("Copy (CTRL+c)"), wxID_COPY, iconpath, "editcopy.svg",
                       "editcopy_dark.svg");
        AddToolbarIcon(edittb, _("Paste (CTRL+v)"), wxID_PASTE, iconpath, "editpaste.svg",
                       "editpaste_dark.svg");
        edittb->Realize();

        auto zoomtb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                                       wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
        AddToolbarIcon(zoomtb, _("Zoom In (CTRL+mousewheel)"), A_ZOOMIN, iconpath, "zoomin.svg",
                       "zoomin_dark.svg");
        AddToolbarIcon(zoomtb, _("Zoom Out (CTRL+mousewheel)"), A_ZOOMOUT, iconpath, "zoomout.svg",
                       "zoomout_dark.svg");
        zoomtb->Realize();

        auto celltb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                                       wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
        AddToolbarIcon(celltb, _("New Grid (INS)"), A_NEWGRID, iconpath, "newgrid.svg",
                       "newgrid_dark.svg");
        AddToolbarIcon(celltb, _("Add Image"), A_IMAGE, iconpath, "image.svg", "image_dark.svg");
        AddToolbarIcon(celltb, _("Run"), wxID_EXECUTE, iconpath, "run.svg", "run_dark.svg");
        celltb->Realize();

        auto findtb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                                       wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
        findtb->AddControl(new wxStaticText(findtb, wxID_ANY, _("Search ")));
        findtb->AddControl(filter = new wxTextCtrl(findtb, A_SEARCH, "", wxDefaultPosition,
                                                   FromDIP(wxSize(80, 22)),
                                                   wxWANTS_CHARS | wxTE_PROCESS_ENTER));
        AddToolbarIcon(findtb, _("Clear search"), A_CLEARSEARCH, iconpath, "cancel.svg",
                       "cancel_dark.svg");
        AddToolbarIcon(findtb, _("Go to Next Search Result"), A_SEARCHNEXT, iconpath, "search.svg",
                       "search_dark.svg");
        findtb->Realize();

        auto repltb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                                       wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
        repltb->AddControl(new wxStaticText(repltb, wxID_ANY, _("Replace ")));
        repltb->AddControl(replaces = new wxTextCtrl(repltb, A_REPLACE, "", wxDefaultPosition,
                                                     FromDIP(wxSize(80, 22)),
                                                     wxWANTS_CHARS | wxTE_PROCESS_ENTER));
        AddToolbarIcon(repltb, _("Clear replace"), A_CLEARREPLACE, iconpath, "cancel.svg",
                       "cancel_dark.svg");
        AddToolbarIcon(repltb, _("Replace in selection"), A_REPLACEONCE, iconpath, "replace.svg",
                       "replace_dark.svg");
        AddToolbarIcon(repltb, _("Replace All"), A_REPLACEALL, iconpath, "replaceall.svg",
                       "replaceall_dark.svg");
        repltb->Realize();

        auto GetColorIndex = [&](int targetcolor, int defaultindex) {
            for (auto i = 1; i < celltextcolors.size(); ++i) {
                if (celltextcolors[i] == targetcolor) return i;
            }
            if (sys->customcolor == targetcolor) return 0;
            return defaultindex;
        };

        auto cellcolortb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                                            wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
        cellcolortb->AddControl(new wxStaticText(cellcolortb, wxID_ANY, _("Cell ")));

        cellcolordropdown =
            new ColorDropdown(cellcolortb, A_CELLCOLOR, GetColorIndex(sys->lastcellcolor, 1));
        cellcolortb->AddControl(cellcolordropdown);
        cellcolortb->Realize();

        auto textcolortb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                                            wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
        textcolortb->AddControl(new wxStaticText(textcolortb, wxID_ANY, _("Text ")));
        textcolordropdown =
            new ColorDropdown(textcolortb, A_TEXTCOLOR, GetColorIndex(sys->lasttextcolor, 2));
        textcolortb->AddControl(textcolordropdown);
        textcolortb->Realize();

        auto bordercolortb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                                              wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
        bordercolortb->AddControl(new wxStaticText(bordercolortb, wxID_ANY, _("Border ")));
        bordercolordropdown =
            new ColorDropdown(bordercolortb, A_BORDCOLOR, GetColorIndex(sys->lastbordcolor, 7));
        bordercolortb->AddControl(bordercolordropdown);
        bordercolortb->Realize();

        auto imagetb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
                                        wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
        imagetb->AddControl(new wxStaticText(imagetb, wxID_ANY, _("Image ")));
        imagedropdown = new ImageDropdown(imagetb, imagepath);
        imagetb->AddControl(imagedropdown);
        imagetb->Realize();

        aui.AddPane(filetb, wxAuiPaneInfo()
                                .Name("filetb")
                                .Caption("File operations")
                                .ToolbarPane()
                                .Top()
                                .Row(0)
                                .LeftDockable(false)
                                .RightDockable(false)
                                .Gripper(true));
        aui.AddPane(edittb, wxAuiPaneInfo()
                                .Name("edittb")
                                .Caption("Edit operations")
                                .ToolbarPane()
                                .Top()
                                .Row(0)
                                .LeftDockable(false)
                                .RightDockable(false)
                                .Gripper(true));
        aui.AddPane(zoomtb, wxAuiPaneInfo()
                                .Name("zoomtb")
                                .Caption("Zoom operations")
                                .ToolbarPane()
                                .Top()
                                .Row(0)
                                .LeftDockable(false)
                                .RightDockable(false)
                                .Gripper(true));
        aui.AddPane(celltb, wxAuiPaneInfo()
                                .Name("celltb")
                                .Caption("Cell operations")
                                .ToolbarPane()
                                .Top()
                                .Row(0)
                                .LeftDockable(false)
                                .RightDockable(false)
                                .Gripper(true));
        aui.AddPane(findtb, wxAuiPaneInfo()
                                .Name("findtb")
                                .Caption("Find operations")
                                .ToolbarPane()
                                .Top()
                                .Row(0)
                                .LeftDockable(false)
                                .RightDockable(false)
                                .Gripper(true));
        aui.AddPane(repltb, wxAuiPaneInfo()
                                .Name("repltb")
                                .Caption("Replace operations")
                                .ToolbarPane()
                                .Top()
                                .Row(0)
                                .LeftDockable(false)
                                .RightDockable(false)
                                .Gripper(true));
        aui.AddPane(cellcolortb, wxAuiPaneInfo()
                                     .Name("cellcolortb")
                                     .Caption("Cell color operations")
                                     .ToolbarPane()
                                     .Top()
                                     .Row(0)
                                     .LeftDockable(false)
                                     .RightDockable(false)
                                     .Gripper(true));
        aui.AddPane(textcolortb, wxAuiPaneInfo()
                                     .Name("textcolortb")
                                     .Caption("Text color operations")
                                     .ToolbarPane()
                                     .Top()
                                     .Row(0)
                                     .LeftDockable(false)
                                     .RightDockable(false)
                                     .Gripper(true));
        aui.AddPane(bordercolortb, wxAuiPaneInfo()
                                       .Name("bordercolortb")
                                       .Caption("Border color operations")
                                       .ToolbarPane()
                                       .Top()
                                       .Row(0)
                                       .LeftDockable(false)
                                       .RightDockable(false)
                                       .Gripper(true));
        aui.AddPane(imagetb, wxAuiPaneInfo()
                                 .Name("imagetb")
                                 .Caption("Image operations")
                                 .ToolbarPane()
                                 .Top()
                                 .Row(0)
                                 .LeftDockable(false)
                                 .RightDockable(false)
                                 .Gripper(true));
        auto artprovider = aui.GetArtProvider();
        artprovider->SetColour(wxAUI_DOCKART_BACKGROUND_COLOUR, wxSystemSettings::GetColour(wxSYS_COLOUR_BTNFACE));
        artprovider->SetMetric(wxAUI_DOCKART_PANE_BORDER_SIZE, 0);
    }

    void AppOnEventLoopEnter() {
        watcher = new wxFileSystemWatcher();
        watcher->SetOwner(this);
        Connect(wxEVT_FSWATCHER, wxFileSystemWatcherEventHandler(TSFrame::OnFileSystemEvent));
    }

    // event handling functions

    void OnMenu(wxCommandEvent &ce) {
        wxTextCtrl *tc;
        auto canvas = GetCurrentTab();
        if ((tc = filter) && filter == wxWindow::FindFocus() ||
            (tc = replaces) && replaces == wxWindow::FindFocus()) {
            long from, to;
            tc->GetSelection(&from, &to);
            switch (ce.GetId()) {
                #if defined(__WXMSW__) || defined(__WXMAC__)
                // FIXME: have to emulate this behavior on Windows and Mac because menu always captures these events (??)
                case A_MLEFT:
                case A_LEFT:
                    if (from != to)
                        tc->SetInsertionPoint(from);
                    else if (from)
                        tc->SetInsertionPoint(from - 1);
                    return;
                case A_MRIGHT:
                case A_RIGHT:
                    if (from != to)
                        tc->SetInsertionPoint(to);
                    else if (to < tc->GetLineLength(0))
                        tc->SetInsertionPoint(to + 1);
                    return;

                case A_SHOME: tc->SetSelection(0, to); return;
                case A_SEND: tc->SetSelection(from, 1000); return;

                case A_SCLEFT:
                case A_SLEFT:
                    if (from) tc->SetSelection(from - 1, to);
                    return;
                case A_SCRIGHT:
                case A_SRIGHT:
                    if (to < tc->GetLineLength(0)) tc->SetSelection(from, to + 1);
                    return;

                case A_BACKSPACE: tc->Remove(from - (from == to), to); return;
                case A_DELETE: tc->Remove(from, to + (from == to)); return;
                case A_HOME: tc->SetSelection(0, 0); return;
                case A_END: tc->SetSelection(1000, 1000); return;
                case wxID_SELECTALL: tc->SetSelection(0, 1000); return;
                #endif
                #ifdef __WXMSW__
                case A_ENTERCELL: {
                    if (tc == filter) {
                        // OnSearchEnter equivalent implementation for MSW
                        // as EVT_TEXT_ENTER event is not generated.
                        if (sys->searchstring.IsEmpty()) {
                            canvas->SetFocus();
                        } else {
                            canvas->doc->Action(A_SEARCHNEXT);
                        }
                    } else if (tc == replaces) {
                        // OnReplaceEnter equivalent implementation for MSW
                        // as EVT_TEXT_ENTER event is not generated.
                        canvas->doc->Action(A_REPLACEONCEJ);
                    }
                    return;
                }
                #endif
                case A_CANCELEDIT:
                    tc->Clear();
                    canvas->SetFocus();
                    return;
            }
        }
        auto Check = [&](const wxString &cfg) {
            sys->cfg->Write(cfg, ce.IsChecked());
            SetStatus(_("change will take effect next run of TreeSheets"));
        };
        switch (ce.GetId()) {
            case A_NOP: break;

            case A_ALEFT: canvas->CursorScroll(-g_scrollratecursor, 0); break;
            case A_ARIGHT: canvas->CursorScroll(g_scrollratecursor, 0); break;
            case A_AUP: canvas->CursorScroll(0, -g_scrollratecursor); break;
            case A_ADOWN: canvas->CursorScroll(0, g_scrollratecursor); break;
            case A_RESETPERSPECTIVE:
                RefreshToolBar();
                sys->showtoolbar = true;
                aui.Update();
                break;
            case A_SHOWSBAR:
                if (!IsFullScreen()) {
                    sys->cfg->Write("showstatusbar", sys->showstatusbar = ce.IsChecked());
                    auto wsb = GetStatusBar();
                    wsb->Show(sys->showstatusbar);
                    SendSizeEvent();
                    Refresh();
                    wsb->Refresh();
                }
                break;
            case A_SHOWTBAR:
                if (!IsFullScreen()) {
                    sys->cfg->Write("showtoolbar", sys->showtoolbar = ce.IsChecked());
                    for (const auto &name : GetToolbarPaneNames()) {
                        if (sys->showtoolbar)
                            aui.GetPane(name).Show();
                        else
                            aui.GetPane(name).Hide();
                    }
                    aui.Update();
                }
                break;
            case A_CUSTCOL: {
                if (auto color = PickColor(sys->frame, sys->customcolor); color != (uint)-1)
                    sys->cfg->Write("customcolor", sys->customcolor = color);
                break;
            }

            #ifdef ENABLE_LOBSTER
                case A_ADDSCRIPT: {
                    wxArrayString filenames;
                    GetFilesFromUser(filenames, this, _("Please select Lobster script file(s):"),
                                     _("Lobster Files (*.lobster)|*.lobster|All Files (*.*)|*.*"));
                    for (auto &filename : filenames) scripts.AddFileToHistory(filename);
                    break;
                }

                case A_DETSCRIPT: {
                    wxArrayString filenames;
                    for (int i = 0, n = scripts.GetCount(); i < n; i++) {
                        filenames.Add(scripts.GetHistoryFile(i));
                    }
                    auto dialog = wxSingleChoiceDialog(
                        this, _("Please select the script you want to remove from the list:"),
                        _("Remove script from list..."), filenames);
                    if (dialog.ShowModal() == wxID_OK)
                        scripts.RemoveFileFromHistory(dialog.GetSelection());
                    break;
                }
            #endif

            case A_DEFAULTMAXCOLWIDTH: {
                int w = wxGetNumberFromUser(_("Please enter the default column width:"),
                                            _("Width"), _("Default column width"),
                                            sys->defaultmaxcolwidth, 1, 1000, sys->frame);
                if (w > 0) sys->cfg->Write("defaultmaxcolwidth", sys->defaultmaxcolwidth = w);
                break;
            }

            case A_LEFTTABS: Check("lefttabs"); break;
            case A_SINGLETRAY: Check("singletray"); break;
            case A_MAKEBAKS: sys->cfg->Write("makebaks", sys->makebaks = ce.IsChecked()); break;
            case A_TOTRAY: sys->cfg->Write("totray", sys->totray = ce.IsChecked()); break;
            case A_MINCLOSE: sys->cfg->Write("minclose", sys->minclose = ce.IsChecked()); break;
            case A_STARTMINIMIZED:
                sys->cfg->Write("startminimized", sys->startminimized = ce.IsChecked());
                break;
            case A_ZOOMSCR: sys->cfg->Write("zoomscroll", sys->zoomscroll = ce.IsChecked()); break;
            case A_THINSELC: sys->cfg->Write("thinselc", sys->thinselc = ce.IsChecked()); break;
            case A_AUTOSAVE: sys->cfg->Write("autosave", sys->autosave = ce.IsChecked()); break;
            case A_CENTERED:
                sys->cfg->Write("centered", sys->centered = ce.IsChecked());
                Refresh();
                break;
            case A_FSWATCH:
                Check("fswatch");
                sys->fswatch = ce.IsChecked();
                break;
            case A_AUTOEXPORT_HTML_NONE:
            case A_AUTOEXPORT_HTML_WITH_IMAGES:
            case A_AUTOEXPORT_HTML_WITHOUT_IMAGES:
                sys->cfg->Write(
                    "autohtmlexport",
                    static_cast<long>(sys->autohtmlexport = ce.GetId() - A_AUTOEXPORT_HTML_NONE));
                break;
            case A_FASTRENDER:
                sys->cfg->Write("fastrender", sys->fastrender = ce.IsChecked());
                Refresh();
                break;
            case A_INVERTRENDER:
                sys->cfg->Write("followdarkmode", sys->followdarkmode = ce.IsChecked());
                sys->colormask = (sys->followdarkmode && wxSystemSettings::GetAppearance().IsDark())
                                     ? 0x00FFFFFF
                                     : 0;
                Refresh();
                break;
            case A_FULLSCREEN:
                ShowFullScreen(!IsFullScreen());
                if (IsFullScreen()) SetStatus(_("Press F11 to exit fullscreen mode."));
                break;
            case wxID_FIND:
                if (filter) {
                    filter->SetFocus();
                    filter->SetSelection(0, 1000);
                } else {
                    SetStatus(_("Please enable (Options -> Show Toolbar) to use search."));
                }
                break;
            case wxID_REPLACE:
                if (replaces) {
                    replaces->SetFocus();
                    replaces->SetSelection(0, 1000);
                } else {
                    SetStatus(_("Please enable (Options -> Show Toolbar) to use replace."));
                }
                break;
            #ifdef __WXMAC__
                case wxID_OSX_HIDE: Iconize(true); break;
                case wxID_OSX_HIDEOTHERS: SetStatus("NOT IMPLEMENTED"); break;
                case wxID_OSX_SHOWALL: Iconize(false); break;
                case wxID_ABOUT: canvas->doc->Action(wxID_ABOUT); break;
                case wxID_PREFERENCES: canvas->doc->Action(wxID_SELECT_FONT); break;
            #endif
            case wxID_EXIT:
                fromclosebox = false;
                Close();
                break;
            case wxID_CLOSE:
                canvas->doc->Action(ce.GetId());
                break;  // canvas dangling pointer on return
            default:
                if (ce.GetId() >= wxID_FILE1 && ce.GetId() <= wxID_FILE9) {
                    wxString filename(filehistory.GetHistoryFile(ce.GetId() - wxID_FILE1));
                    SetStatus(sys->Open(filename));
                #ifdef ENABLE_LOBSTER
                    } else if (ce.GetId() >= A_TAGSET && ce.GetId() < A_SCRIPT) {
                        SetStatus(canvas->doc->TagSet(ce.GetId() - A_TAGSET));
                    } else if (ce.GetId() >= A_SCRIPT && ce.GetId() < A_MAXACTION) {
                        auto message =
                            tssi.ScriptRun(scripts.GetHistoryFile(ce.GetId() - A_SCRIPT).c_str());
                        message.erase(std::remove(message.begin(), message.end(), '\n'), message.end());
                        SetStatus(wxString(message));
                #else
                    } else if (ce.GetId() >= A_TAGSET && ce.GetId() < A_MAXACTION) {
                        SetStatus(canvas->doc->TagSet(ce.GetId() - A_TAGSET));
                #endif
                } else {
                    SetStatus(canvas->doc->Action(ce.GetId()));
                    break;
                }
        }
    }

    void OnTabChange(wxAuiNotebookEvent &nbe) {
        auto canvas = static_cast<TSCanvas *>(notebook->GetPage(nbe.GetSelection()));
        ClearStatus();
        sys->TabChange(canvas->doc.get());
        nbe.Skip();
    }

    void OnTabClose(wxAuiNotebookEvent &nbe) {
        auto canvas = static_cast<TSCanvas *>(notebook->GetPage(nbe.GetSelection()));
        if (notebook->GetPageCount() <= 1) {
            nbe.Veto();
            Close();
        } else if (canvas->doc->CloseDocument()) {
            nbe.Veto();
        } else {
            nbe.Skip();
        }
    }

    void OnSearch(wxCommandEvent &ce) {
        auto searchstring = ce.GetString();
        sys->darkennonmatchingcells = searchstring.Len() != 0;
        sys->searchstring = sys->casesensitivesearch ? searchstring : searchstring.Lower();
        TSCanvas *canvas = GetCurrentTab();
        Document *doc = canvas->doc.get();
        if (doc->searchfilter) {
            doc->SetSearchFilter(sys->searchstring.Len() != 0);
            doc->searchfilter = true;
        }
        canvas->Refresh();
    }

    void OnSearchReplaceEnter(wxCommandEvent &ce) {
        auto canvas = GetCurrentTab();
        if (ce.GetId() == A_SEARCH && ce.GetString().IsEmpty())
            canvas->SetFocus();
        else
            canvas->doc->Action(ce.GetId() == A_SEARCH ? A_SEARCHNEXT : A_REPLACEONCEJ);
    }

    void OnChangeColor(wxCommandEvent &ce) {
        GetCurrentTab()->doc->ColorChange(ce.GetId(), ce.GetInt());
        ReFocus();
    }

    void OnDDImage(wxCommandEvent &ce) {
        GetCurrentTab()->doc->ImageChange(imagedropdown->filenames[ce.GetInt()], dd_icon_res_scale);
        ReFocus();
    }

    void OnActivate(wxActivateEvent &ae) {
        // This causes warnings in the debug log, but without it keyboard entry upon window select
        // doesn't work.
        ReFocus();
    }

    void OnSizing(wxSizeEvent &se) { se.Skip(); }

    void OnMaximize(wxMaximizeEvent &me) {
        ReFocus();
        me.Skip();
    }

    void OnIconize(wxIconizeEvent &me) {
        if (me.IsIconized()) {
            #ifndef __WXMAC__
            if (sys->totray) {
                taskbaricon.SetIcon(icon, "TreeSheets");
                Show(false);
                Iconize();
            }
            #endif
        } else {
            #ifdef __WXGTK__
            if (sys->totray) {
                Show(true);
            }
            #endif
            if (TSCanvas *canvas = GetCurrentTab()) canvas->SetFocus();
        }
    }

    void OnTBIDBLClick(wxTaskBarIconEvent &e) { DeIconize(); }

    void OnClosing(wxCloseEvent &ce) {
        bool fcb = fromclosebox;
        fromclosebox = true;
        if (fcb && sys->minclose) {
            ce.Veto();
            Iconize();
            return;
        }
        sys->RememberOpenFiles();
        if (ce.CanVeto()) {
            // ask to save/discard all files before closing any
            loop(i, notebook->GetPageCount()) {
                auto canvas = static_cast<TSCanvas *>(notebook->GetPage(i));
                if (canvas->doc->modified) {
                    notebook->SetSelection(i);
                    if (canvas->doc->CheckForChanges()) {
                        ce.Veto();
                        return;
                    }
                }
            }
            // all files have been saved/discarded
            while (notebook->GetPageCount()) {
                GetCurrentTab()->doc->RemoveTmpFile();
                notebook->DeletePage(notebook->GetSelection());
            }
        }
        sys->every_second_timer.Stop();
        filehistory.Save(*sys->cfg);
        #ifdef ENABLE_LOBSTER
            auto oldpath = sys->cfg->GetPath();
            sys->cfg->SetPath("/scripts");
            scripts.Save(*sys->cfg);
            sys->cfg->SetPath(oldpath);
        #endif
        if (!IsIconized()) {
            sys->cfg->Write("maximized", IsMaximized());
            if (!IsMaximized()) {
                sys->cfg->Write("resx", GetSize().x);
                sys->cfg->Write("resy", GetSize().y);
                sys->cfg->Write("posx", GetPosition().x);
                sys->cfg->Write("posy", GetPosition().y);
            }
        }
        sys->cfg->Write("notesizex", sys->notesizex);
        sys->cfg->Write("notesizey", sys->notesizey);
        sys->cfg->Write("perspective", aui.SavePerspective());
        sys->cfg->Write("lastcellcolor", sys->lastcellcolor);
        sys->cfg->Write("lasttextcolor", sys->lasttextcolor);
        sys->cfg->Write("lastbordcolor", sys->lastbordcolor);
        aui.ClearEventHashTable();
        aui.UnInit();
        DELETEP(editmenupopup);
        DELETEP(watcher);
        Destroy();
    }

    void OnFileSystemEvent(wxFileSystemWatcherEvent &event) {
        // 0xF == create/delete/rename/modify
        if ((event.GetChangeType() & 0xF) == 0 || watcherwaitingforuser || !notebook) return;
        const auto &modfile = event.GetPath().GetFullPath();
        loop(i, notebook->GetPageCount()) {
            Document *doc = static_cast<TSCanvas *>(notebook->GetPage(i))->doc.get();
            if (modfile == doc->filename) {
                auto modtime = wxFileName(modfile).GetModificationTime();
                // Compare with last modified to trigger multiple times.
                if (!modtime.IsValid() || !doc->lastmodificationtime.IsValid() ||
                    modtime == doc->lastmodificationtime) {
                    return;
                }
                if (doc->modified) {
                    // TODO: this dialog is problematic since it may be on an unattended
                    // computer and more of these events may fire. since the occurrence of this
                    // situation is rare, it may be better to just take the most
                    // recently changed version (which is the one that has just been modified
                    // on disk) this potentially throws away local changes, but this can only
                    // happen if the user left changes unsaved, then decided to go edit an older
                    // version on another computer.
                    // for now, we leave this code active, and guard it with
                    // watcherwaitingforuser
                    auto message = wxString::Format(
                        _("%s\nhas been modified on disk by another program / computer:\nWould you like to discard your changes and re-load from disk?"),
                        doc->filename);
                    watcherwaitingforuser = true;
                    int res = wxMessageBox(message, _("File modification conflict!"),
                                           wxYES_NO | wxICON_QUESTION, this);
                    watcherwaitingforuser = false;
                    if (res != wxYES) return;
                }
                auto message = sys->LoadDB(doc->filename, true, i);
                if (!message.IsEmpty()) {
                    SetStatus(message);
                } else {
                    notebook->DeletePage(i + 1);
                    ::wxRemoveFile(sys->TmpName(modfile));
                    SetStatus(
                        _("File has been re-loaded because of modifications of another program / computer"));
                }
                return;
            }
        }
    }

    void OnDPIChanged(wxDPIChangedEvent &dce) {
        // block all other events until we finished preparing
        wxEventBlocker blocker(this);
        wxBusyCursor wait;
        {
            ThreadPool pool(std::thread::hardware_concurrency());
            for (const auto &image : sys->imagelist) {
                pool.enqueue(
                    [](auto img) {
                        img->bm_display = wxNullBitmap;
                        img->Display();
                    },
                    image.get());
            }
        }  // wait until all tasks are finished
        RenderFolderIcon();
        dce.Skip();
    }

    void OnSysColourChanged(wxSysColourChangedEvent &se) {
        sys->colormask =
            (sys->followdarkmode && wxSystemSettings::GetAppearance().IsDark()) ? 0x00FFFFFF : 0;
        auto perspective = aui.SavePerspective();
        RefreshToolBar();
        aui.LoadPerspective(perspective);
        aui.Update();
        se.Skip();
    }

    // helper functions

    void CycleTabs(int offset = 1) {
        auto numtabs = static_cast<int>(notebook->GetPageCount());
        offset = offset >= 0 ? 1 : numtabs - 1;  // normalize to non-negative wrt modulo
        notebook->SetSelection((notebook->GetSelection() + offset) % numtabs);
    }

    void DeIconize() {
        if (!IsIconized()) {
            RequestUserAttention();
            return;
        }
        Show(true);
        Iconize(false);
        taskbaricon.RemoveIcon();
    }

    TSCanvas *GetCurrentTab() {
        return notebook ? static_cast<TSCanvas *>(notebook->GetCurrentPage()) : nullptr;
    }

    TSCanvas *GetTabByFileName(const wxString &filename) {
        if (notebook) loop(i, notebook->GetPageCount()) {
                auto canvas = static_cast<TSCanvas *>(notebook->GetPage(i));
                if (canvas->doc->filename == filename) {
                    notebook->SetSelection(i);
                    return canvas;
                }
            }
        return nullptr;
    }

    void MyAppend(wxMenu *menu, int tag, const wxString &contents, const wxString &help = "") {
        auto item = contents;
        wxString key = "";
        if (int pos = contents.Find("\t"); pos >= 0) {
            item = contents.Mid(0, pos);
            key = contents.Mid(pos + 1);
        }
        key = sys->cfg->Read(item, key);
        auto newcontents = item;
        if (key.Length()) newcontents += "\t" + key;
        menu->Append(tag, newcontents, help);
        menustrings[item] = key;
    }

    TSCanvas *NewTab(unique_ptr<Document> doc, bool append = false, int insert_at = -1) {
        TSCanvas *canvas = new TSCanvas(this, notebook);
        doc->canvas = canvas;
        canvas->doc = std::move(doc);
        canvas->SetScrollRate(1, 1);
        if (insert_at >= 0)
            notebook->InsertPage(insert_at, canvas, _("<unnamed>"), true, wxNullBitmap);
        else if (append)
            notebook->AddPage(canvas, _("<unnamed>"), true, wxNullBitmap);
        else
            notebook->InsertPage(0, canvas, _("<unnamed>"), true, wxNullBitmap);
        canvas->SetDropTarget(new DropTarget(canvas->doc->dndobjc));
        canvas->SetFocus();
        return canvas;
    }

    void ReFocus() {
        if (TSCanvas *canvas = GetCurrentTab()) canvas->SetFocus();
    }

    void RenderFolderIcon() {
        wxImage foldiconi;
        foldiconi.LoadFile(app->GetDataPath("images/nuvola/fold.png"));
        foldicon = wxBitmap(foldiconi);
        ScaleBitmap(foldicon, FromDIP(1.0) / 3.0, foldicon);
    }

    void SetDPIAwareStatusWidths() {
        int statusbarfieldwidths[] = {-1, FromDIP(300), FromDIP(120), FromDIP(100), FromDIP(150)};
        SetStatusWidths(5, statusbarfieldwidths);
    }

    void SetFileAssoc(const wxString &exename) {
        #ifdef WIN32
        SetRegistryKey("HKEY_CURRENT_USER\\Software\\Classes\\.cts", "TreeSheets");
        SetRegistryKey("HKEY_CURRENT_USER\\Software\\Classes\\TreeSheets", "TreeSheets file");
        SetRegistryKey("HKEY_CURRENT_USER\\Software\\Classes\\TreeSheets\\Shell\\Open\\Command",
                       "\"" + exename + "\" \"%1\"");
        SetRegistryKey("HKEY_CURRENT_USER\\Software\\Classes\\TreeSheets\\DefaultIcon",
                       "\"" + exename + "\",0");
        #else
        // TODO: do something similar for mac/kde/gnome?
        #endif
    }

    void SetPageTitle(const wxString &filename, wxString mods, int page = -1) {
        if (page < 0) page = notebook->GetSelection();
        if (page < 0) return;
        if (page == notebook->GetSelection()) SetTitle("TreeSheets - " + filename + mods);
        notebook->SetPageText(
            page,
            (filename.empty() ? wxString(_("<unnamed>")) : wxFileName(filename).GetName()) + mods);
    }

    #ifdef WIN32
    void SetRegistryKey(const wxString &key, wxString value) {
        wxRegKey registrykey(key);
        registrykey.Create();
        registrykey.SetValue("", value);
    }
    #endif

    void SetStatus(const wxString &message) {
        if (GetStatusBar() && !message.IsEmpty()) SetStatusText(message, 0);
    }

    void ClearStatus() {
        if (GetStatusBar()) SetStatusText("", 0);
    }

    void TabsReset() {
        if (notebook) loop(i, notebook->GetPageCount()) {
                auto canvas = static_cast<TSCanvas *>(notebook->GetPage(i));
                canvas->doc->root->ResetChildren();
            }
    }

    void UpdateStatus(const Selection &s, bool updateamount) {
        if (GetStatusBar()) {
            if (Cell *c = s.GetCell(); c && s.xs) {
                SetStatusText(wxString::Format(_("Size %d"), -c->text.relsize), 3);
                SetStatusText(wxString::Format(_("Width %d"), s.grid->colwidths[s.x]), 2);
                SetStatusText(wxString::Format(_("Edited %s %s"), c->text.lastedit.FormatDate(),
                                               c->text.lastedit.FormatTime()),
                              1);
            } else
                for (int field : {1, 2, 3}) SetStatusText("", field);
            if (updateamount) SetStatusText(wxString::Format(_("%d cell(s)"), s.xs * s.ys), 4);
        }
    }

    DECLARE_EVENT_TABLE()
};
```

## File: src/wxtools.h
```c
static void DrawRectangle(wxDC &dc, uint color, int x, int y, int xs, int ys,
                          bool outline = false) {
    if (outline)
        dc.SetBrush(*wxTRANSPARENT_BRUSH);
    else
        dc.SetBrush(wxBrush(LightColor(color)));
    dc.SetPen(wxPen(LightColor(color)));
    dc.DrawRectangle(x, y, xs, ys);
}

static uint SwapColor(uint c) { return ((c & 0xFF) << 16) | (c & 0xFF00) | ((c & 0xFF0000) >> 16); }

struct DropTarget : wxDropTarget {
    DropTarget(wxDataObject *data) : wxDropTarget(data) {};

    wxDragResult OnDragOver(wxCoord x, wxCoord y, wxDragResult def) {
        auto canvas = sys->frame->GetCurrentTab();
        wxInfoDC dc(canvas);
        canvas->doc->UpdateHover(dc, x, y);
        return canvas->doc->hover.grid ? wxDragCopy : wxDragNone;
    }

    bool OnDrop(wxCoord x, wxCoord y) {
        return sys->frame->GetCurrentTab()->doc->hover.grid != nullptr;
    }
    wxDragResult OnData(wxCoord x, wxCoord y, wxDragResult def) {
        GetData();
        auto canvas = sys->frame->GetCurrentTab();
        wxInfoDC dc(canvas);
        canvas->doc->UpdateHover(dc, x, y);
        canvas->doc->SelectClick();
        canvas->doc->Drop();
        canvas->Refresh();
        return wxDragCopy;
    }
};

struct ThreeChoiceDialog : public wxDialog {
    ThreeChoiceDialog(wxWindow *parent, const wxString &title, const wxString &msg,
                      const wxString &ch1, const wxString &ch2, const wxString &ch3)
        : wxDialog(parent, wxID_ANY, title) {
        auto bsv = new wxBoxSizer(wxVERTICAL);
        bsv->Add(new wxStaticText(this, -1, msg), 0, wxALL, 5);
        auto bsb = new wxBoxSizer(wxHORIZONTAL);
        bsb->Prepend(new wxButton(this, 2, ch3), 0, wxALL, 5);
        bsb->PrependStretchSpacer(1);
        bsb->Prepend(new wxButton(this, 1, ch2), 0, wxALL, 5);
        bsb->PrependStretchSpacer(1);
        bsb->Prepend(new wxButton(this, 0, ch1), 0, wxALL, 5);
        bsv->Add(bsb, 1, wxEXPAND);
        SetSizer(bsv);
        bsv->SetSizeHints(this);
    }

    void OnButton(wxCommandEvent &ce) { EndModal(ce.GetId()); }
    int Run() { return ShowModal(); }
    DECLARE_EVENT_TABLE()
};

struct DateTimeRangeDialog : public wxDialog {
    wxStaticText introtext {this, wxID_ANY, _("Please select the datetime range.")};
    wxStaticText starttext {this, wxID_ANY, _("Start date and time")};
    wxDatePickerCtrl startdate {this, wxID_ANY};
    wxTimePickerCtrl starttime {this, wxID_ANY};
    wxStaticText endtext {this, wxID_ANY, _("End date and time")};
    wxDatePickerCtrl enddate {this, wxID_ANY};
    wxTimePickerCtrl endtime {this, wxID_ANY};
    wxButton okbtn {this, wxID_OK, _("Filter")};
    wxButton cancelbtn {this, wxID_CANCEL, _("Cancel")};
    wxDateTime begin;
    wxDateTime end;
    DateTimeRangeDialog(wxWindow *parent) : wxDialog(parent, wxID_ANY, _("Date and time range")) {
        wxSizerFlags sizerflags(1);
        auto startsizer = new wxFlexGridSizer(2, wxSize(5, 5));
        startsizer->Add(&startdate, 0, wxALL, 5);
        startsizer->Add(&starttime, 0, wxALL, 5);
        auto endsizer = new wxFlexGridSizer(2, wxSize(5, 5));
        endsizer->Add(&enddate, 0, wxALL, 5);
        endsizer->Add(&endtime, 0, wxALL, 5);
        auto btnsizer = new wxFlexGridSizer(2, wxSize(5, 5));
        btnsizer->Add(&okbtn, 0, wxALL, 5);
        btnsizer->Add(&cancelbtn, 0, wxALL, 5);
        auto topsizer = new wxFlexGridSizer(1);
        topsizer->Add(&introtext, 0, wxALL, 5);
        topsizer->Add(&starttext, 0, wxALL, 5);
        topsizer->Add(startsizer, sizerflags);
        topsizer->Add(&endtext, 0, wxALL, 5);
        topsizer->Add(endsizer, sizerflags);
        topsizer->Add(btnsizer, sizerflags);
        SetSizerAndFit(topsizer);
        topsizer->SetSizeHints(this);
    }
    void OnButton(wxCommandEvent &ce) {
        if (ce.GetId() == wxID_OK) {
            int starthour, startmin, startsec;
            starttime.GetTime(&starthour, &startmin, &startsec);
            wxTimeSpan starttimespan(starthour, startmin, startsec);
            int endhour, endmin, endsec;
            endtime.GetTime(&endhour, &endmin, &endsec);
            wxTimeSpan endtimespan(endhour, endmin, endsec);
            begin = startdate.GetValue().Add(starttimespan);
            end = enddate.GetValue().Add(endtimespan);
        }
        EndModal(ce.GetId());
    }
    int Run() { return ShowModal(); }
    DECLARE_EVENT_TABLE()
};

struct ColorPopup : wxVListBoxComboPopup {
    ColorPopup(wxWindow *parent) {}

    void OnComboDoubleClick() {
        sys->frame->GetCurrentTab()->doc->ColorChange(m_combo->GetId(), GetSelection());
    }
};

struct ColorDropdown : wxOwnerDrawnComboBox {
    ColorDropdown(wxWindow *parent, wxWindowID id, int sel) {
        wxArrayString as;
        as.Add("", sizeof(celltextcolors) / sizeof(uint));
        Create(parent, id, "", wxDefaultPosition, FromDIP(wxSize(44, 22)), as,
               wxCB_READONLY | wxCC_SPECIAL_DCLICK);
        SetPopupControl(new ColorPopup(this));
        SetSelection(sel);
        SetPopupMaxHeight(wxDisplay().GetGeometry().GetHeight() * 3 / 4);
    }

    wxCoord OnMeasureItem(size_t item) const { return FromDIP(22); }
    wxCoord OnMeasureItemWidth(size_t item) const { return FromDIP(40); }
    void OnDrawBackground(wxDC &dc, const wxRect &rect, int item, int flags) const {
        DrawRectangle(dc, flags & wxODCB_PAINTING_SELECTED ? 0xA9A9A9 : 0xFFFFFF, rect.x, rect.y,
                      rect.width, rect.height);
    }

    void OnDrawItem(wxDC &dc, const wxRect &rect, int item, int flags) const {
        DrawRectangle(dc, item == CUSTOMCOLORIDX ? sys->customcolor : celltextcolors[item],
                      rect.x + 1, rect.y + 1, rect.width - 2, rect.height - 2);
        if (item == CUSTOMCOLORIDX) {
            dc.SetTextForeground(LightColor(0x000000));
            dc.SetFont(wxFont(9, wxFONTFAMILY_DEFAULT, wxFONTSTYLE_NORMAL, wxFONTWEIGHT_NORMAL,
                              false, ""));
            dc.DrawText(_("Custom"), rect.x + 1, rect.y + 1);
        }
    }
};

static uint PickColor(wxWindow *parent, uint defaultcolor) {
    auto color = wxGetColourFromUser(parent, wxColour(defaultcolor));
    if (color.IsOk()) return (color.Blue() << 16) + (color.Green() << 8) + color.Red();
    return -1;
}

inline static uint LightColor(uint color) { return color ^ sys->colormask; }

#define dd_icon_res_scale 3.0

struct ImagePopup : wxVListBoxComboPopup {
    void OnComboDoubleClick() {
        auto filename = GetString(GetSelection());
        sys->frame->GetCurrentTab()->doc->ImageChange(filename, dd_icon_res_scale);
    }
};

struct ImageDropdown : wxOwnerDrawnComboBox {
    vector<unique_ptr<wxBitmap>> bitmaps_display;
    wxArrayString filenames;
    const int image_space = 22;

    ImageDropdown(wxWindow *parent, const wxString &directory) {
        FillBitmapVector(directory);
        Create(parent, A_DDIMAGE, "", wxDefaultPosition,
               FromDIP(wxSize(image_space * 2, image_space)), filenames,
               wxCB_READONLY | wxCC_SPECIAL_DCLICK);
        SetPopupControl(new ImagePopup());
        SetSelection(0);
        SetPopupMaxHeight(wxDisplay().GetGeometry().GetHeight() * 3 / 4);
    }

    wxCoord OnMeasureItem(size_t item) const { return FromDIP(image_space); }
    wxCoord OnMeasureItemWidth(size_t item) const { return FromDIP(image_space); }
    void OnDrawBackground(wxDC &dc, const wxRect &rect, int item, int flags) const {
        DrawRectangle(dc, 0xFFFFFF, rect.x, rect.y, rect.width, rect.height);
    }

    void OnDrawItem(wxDC &dc, const wxRect &rect, int item, int flags) const {
        sys->ImageDraw(bitmaps_display[item].get(), dc, rect.x + FromDIP(3), rect.y + FromDIP(3));
    }

    void FillBitmapVector(const wxString &directory) {
        if (!bitmaps_display.empty()) bitmaps_display.resize(0);
        auto filename = wxFindFirstFile(directory + "*.*");
        while (!filename.empty()) {
            wxBitmap bitmap;
            if (bitmap.LoadFile(filename, wxBITMAP_TYPE_PNG)) {
                auto scaledbitmap = make_unique<wxBitmap>();
                ScaleBitmap(bitmap, FromDIP(1.0) / dd_icon_res_scale, *scaledbitmap);
                bitmaps_display.push_back(std::move(scaledbitmap));
                filenames.Add(filename);
            }
            filename = wxFindNextFile();
        }
    }
};

static void ScaleBitmap(const wxBitmap &source, double scale, wxBitmap &destination) {
    destination = wxBitmap(source.ConvertToImage().Scale(
        source.GetWidth() * scale, source.GetHeight() * scale, wxIMAGE_QUALITY_HIGH));
}

static vector<uint8_t> ConvertWxImageToBuffer(const wxImage &image, wxBitmapType bitmaptype) {
    wxMemoryOutputStream imageoutputstream(NULL, 0);
    image.SaveFile(imageoutputstream, bitmaptype);
    auto size = imageoutputstream.TellO();
    vector<uint8_t> buffer(size);
    imageoutputstream.CopyTo(buffer.data(), size);
    return buffer;
}

static wxImage ConvertBufferToWxImage(const vector<uint8_t> &buffer, wxBitmapType bitmaptype) {
    wxMemoryInputStream imageinputstream(buffer.data(), buffer.size());
    wxImage image(imageinputstream, bitmaptype);
    if (!image.IsOk()) {
        int size = 32;
        image.Create(size, size, false);
        image.SetRGB(wxRect(0, 0, size, size), 0xFF, 0, 0);
        // Set to red to indicate error.
    }
    return image;
}

static wxBitmap ConvertBufferToWxBitmap(const vector<uint8_t> &buffer, wxBitmapType bmt) {
    auto image = ConvertBufferToWxImage(buffer, bmt);
    wxBitmap bitmap(image, 32);
    return bitmap;
}

static uint64_t CalculateHash(vector<uint8_t> &buffer) {
    return FNV1A64(buffer.data(), buffer.size());
}

static void GetFilesFromUser(wxArrayString &filenames, wxWindow *parent, const wxString &title,
                             const wxString &filter) {
    wxFileDialog filedialog(parent, title, "", "", filter,
                            wxFD_OPEN | wxFD_FILE_MUST_EXIST | wxFD_CHANGE_DIR | wxFD_MULTIPLE);
    if (filedialog.ShowModal() == wxID_OK) filedialog.GetPaths(filenames);
}

static void HintIMELocation(Document *doc, int bx, int by, int bh, int stylebits) {
    // TODO: implement on other platforms
    #ifdef __WXMSW__
        HWND hwnd = doc->canvas->GetHandle();
        if (hwnd == 0) return;
        int scrollx, scrolly;
        doc->canvas->GetViewStart(&scrollx, &scrolly);
        int imx = doc->centerx + (bx + doc->hierarchysize) * doc->currentviewscale - scrollx;
        int imy = doc->centery + (by + doc->hierarchysize) * doc->currentviewscale - scrolly;
        if (HIMC himc = ImmGetContext(hwnd)) {
            COMPOSITIONFORM cof = {.dwStyle = CFS_FORCE_POSITION,
                                   .ptCurrentPos = {.x = imx, .y = imy}};
            ImmSetCompositionWindow(himc, &cof);
            LOGFONT lf = {.lfHeight = static_cast<LONG>(-bh * doc->currentviewscale),
                          .lfWeight = stylebits & STYLE_BOLD ? FW_BOLD : FW_REGULAR,
                          .lfItalic = static_cast<BYTE>(stylebits & STYLE_ITALIC),
                          .lfUnderline = static_cast<BYTE>(stylebits & STYLE_UNDERLINE),
                          .lfStrikeOut = static_cast<BYTE>(stylebits & STYLE_STRIKETHRU),
                          .lfPitchAndFamily = static_cast<BYTE>(stylebits & STYLE_FIXED
                                                                    ? FIXED_PITCH | FF_MODERN
                                                                    : VARIABLE_PITCH | FF_SWISS)};
            ImmSetCompositionFont(himc, &lf);
            CANDIDATEFORM caf = {.dwStyle = CFS_CANDIDATEPOS, .ptCurrentPos = {.x = imx, .y = imy}};
            ImmSetCandidateWindow(himc, &caf);
            ImmReleaseContext(hwnd, himc);
        }
    #endif
}
```

## File: .clang-format
```
---
BasedOnStyle: Google
---
Language: Cpp
IndentWidth: 4
ColumnLimit: 100
UseTab: Never
AccessModifierOffset: 0
AlignTrailingComments: true
AllowShortBlocksOnASingleLine: true
AllowShortCaseLabelsOnASingleLine: true
AllowShortFunctionsOnASingleLine : All
AllowShortLoopsOnASingleLine: true
BinPackParameters: true
ConstructorInitializerAllOnOneLineOrOnePerLine: true
IndentCaseLabels: true
NamespaceIndentation: None
PointerAlignment: Right
SpaceBeforeParens: ControlStatements
SpaceAfterTemplateKeyword: false
Standard: Cpp11
Cpp11BracedListStyle: true
SpaceBeforeCpp11BracedList: true
IndentPPDirectives: BeforeHash
AlwaysBreakTemplateDeclarations: false
```

## File: .gitattributes
```
* text=auto
```

## File: CMakeLists.txt
```
cmake_minimum_required(VERSION 3.25)

### Project

set(TREESHEETS_VERSION "" CACHE STRING "Version string (e.g., 1.2.3). Leave empty for auto-timestamp.")

if("${TREESHEETS_VERSION}" STREQUAL "")
    string(TIMESTAMP TREESHEETS_VERSION "%y%m%d.%H%M" UTC)
    message(STATUS "Auto-generated version: ${TREESHEETS_VERSION}")
else()
    message(STATUS "User-defined version: ${TREESHEETS_VERSION}")
endif()

project(TreeSheets
    DESCRIPTION "TreeSheets"
    HOMEPAGE_URL "https://github.com/ib-bsb-br/TreeSheets"
    VERSION "${TREESHEETS_VERSION}")

### Settings

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)
OPTION(ENABLE_LOBSTER "Enable Lobster scripting" ON)
option(ENABLE_IPO "Enable interprocedural/link time optimization" ON)
if(ENABLE_IPO)
    include(CheckIPOSupported)
    check_ipo_supported(RESULT iporesult OUTPUT output)
    if(iporesult)
        set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)
    else()
        message(WARNING "IPO is not supported: ${output}")
    endif()
endif()

## Compiler-specific

# Use statically-linked libraries with MSVC
if(MSVC)
    set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
    set(wxBUILD_USE_STATIC_RUNTIME ON CACHE BOOL "" FORCE)
    set(wxBUILD_SHARED OFF CACHE BOOL "" FORCE)
endif(MSVC)

# Silence warnings in GCC that contain lots of false positives
if(CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_CXX_FLAGS
    "${CMAKE_CXX_FLAGS} -Wno-array-bounds -Wno-stringop-overflow -Wno-maybe-uninitialized")
endif()

### Thirdparty dependencies

# Set wxWidgets options to exclude unused libraries

set(wxUSE_WEBVIEW FALSE)
set(wxUSE_STC FALSE)
set(wxUSE_RIBBON FALSE)
set(wxUSE_PROPGRID FALSE)
set(wxUSE_RICHTEXT FALSE)
set(wxUSE_MEDIACTRL FALSE)
if (NOT wxBUILD_SHARED)
    set(wxUSE_HTML FALSE)
    set(wxUSE_WXHTML_HELP FALSE)
endif()

# Include dependencies

include(FetchContent)

FetchContent_Declare(
    wxwidgets
    URL https://github.com/wxWidgets/wxWidgets/releases/download/v3.3.2/wxWidgets-3.3.2.tar.bz2
    URL_HASH SHA256=50a28cb668de47b0e006cd6ebed8cf4f76c1cac6116fb3c978c44478219103f2
    FIND_PACKAGE_ARGS 3.3.2 NAMES wxWidgets
)
FetchContent_MakeAvailable(wxwidgets)

if(ENABLE_LOBSTER)
    FetchContent_Declare(
        lobster
        URL https://github.com/aardappel/lobster/archive/refs/tags/v2026.1.tar.gz
        URL_HASH SHA256=fecd443c8bf03052f0eca3308bb8428e1f40292d61fb370e060e55094f853d67
    )
    FetchContent_MakeAvailable(lobster)
endif()

### Options

## Run clang-tidy linter

OPTION(WITH_CLANG_TIDY "Run clang-tidy linter" OFF)
if (WITH_CLANG_TIDY)
    set(CMAKE_CXX_CLANG_TIDY clang-tidy -checks=cppcoreguidelines-*,clang-analyzer-*,readability-*,performance-*,portability-*,concurrency-*,modernize-*)
endif()

### Libraries (lobster, lobster-impl)

## lobster (script interpreter)
if(ENABLE_LOBSTER)
    add_library(lobster STATIC
        ${lobster_SOURCE_DIR}/dev/external/flatbuffers/src/idl_gen_text.cpp
        ${lobster_SOURCE_DIR}/dev/external/flatbuffers/src/idl_parser.cpp
        ${lobster_SOURCE_DIR}/dev/external/flatbuffers/src/util.cpp
        ${lobster_SOURCE_DIR}/dev/src/builtins.cpp
        ${lobster_SOURCE_DIR}/dev/src/compiler.cpp
        ${lobster_SOURCE_DIR}/dev/src/file.cpp
        ${lobster_SOURCE_DIR}/dev/src/lobsterreader.cpp
        ${lobster_SOURCE_DIR}/dev/src/platform.cpp
        ${lobster_SOURCE_DIR}/dev/src/vm.cpp
        ${lobster_SOURCE_DIR}/dev/src/vmdata.cpp
        ${lobster_SOURCE_DIR}/dev/src/tccbind.cpp
        ${lobster_SOURCE_DIR}/dev/external/libtcc/libtcc.c)
    if(WIN32)
        target_sources(lobster PRIVATE
            ${lobster_SOURCE_DIR}/dev/include/StackWalker/StackWalker.cpp
            ${lobster_SOURCE_DIR}/dev/include/StackWalker/StackWalkerHelpers.cpp)
    endif(WIN32)
    target_include_directories(lobster PUBLIC
        ${lobster_SOURCE_DIR}/dev/src
        ${lobster_SOURCE_DIR}/dev/include
        ${lobster_SOURCE_DIR}/dev/external
        ${lobster_SOURCE_DIR}/dev/external/libtcc)

    ## lobster-impl (provider of TreeSheets functions in lobster)

    add_library(lobster-impl STATIC src/lobster_impl.cpp)
    target_link_libraries(lobster-impl PRIVATE lobster)
endif(ENABLE_LOBSTER)

### TreeSheets executable

add_executable(TreeSheets
    src/main.cpp
    src/stdafx.cpp)

target_compile_definitions(TreeSheets PRIVATE "PACKAGE_VERSION=\"${TREESHEETS_VERSION}\"")
if(ENABLE_LOBSTER)
    target_compile_definitions(TreeSheets PRIVATE "ENABLE_LOBSTER=1")
endif(ENABLE_LOBSTER)

target_precompile_headers(TreeSheets PUBLIC src/stdafx.h)

## Link wxWidgets and lobster-impl into TreeSheets
if(ENABLE_LOBSTER)
    target_link_libraries(TreeSheets PRIVATE lobster-impl)
endif(ENABLE_LOBSTER)
target_link_libraries(TreeSheets PRIVATE wx::aui wx::adv wx::core wx::xml wx::net)

### Installation

## Platform specific installation paths

if(LINUX OR BSD)
    OPTION(TREESHEETS_RELOCATABLE_INSTALLATION "Install data relative to the TreeSheets binary, instead of respecting the Filesystem Hierarchy Standard" OFF)
endif()

if((LINUX OR BSD) AND NOT TREESHEETS_RELOCATABLE_INSTALLATION)
    include(GNUInstallDirs)

    set(TREESHEETS_BINDIR ${CMAKE_INSTALL_BINDIR})
    set(TREESHEETS_DOCDIR ${CMAKE_INSTALL_DOCDIR})
    set(TREESHEETS_FULL_DOCDIR ${CMAKE_INSTALL_FULL_DOCDIR})
    set(TREESHEETS_PKGDATADIR ${CMAKE_INSTALL_DATADIR}/${CMAKE_PROJECT_NAME})
    set(TREESHEETS_FULL_PKGDATADIR ${CMAKE_INSTALL_FULL_DATADIR}/${CMAKE_PROJECT_NAME})

    # Convert relative to absolute paths because only absolute paths are looked up on Linux (and BSD)
    target_compile_definitions(TreeSheets PRIVATE
        "LOCALEDIR=L\"${CMAKE_INSTALL_FULL_LOCALEDIR}\""
        "TREESHEETS_DOCDIR=\"${TREESHEETS_FULL_DOCDIR}\""
        "TREESHEETS_DATADIR=\"${TREESHEETS_FULL_PKGDATADIR}\""
    )

    # Adapt the AppStream metainfo to release version and date
    string(TIMESTAMP TREESHEETS_RELEASE_DATE "%Y-%m-%d")
    configure_file(
        "${CMAKE_CURRENT_SOURCE_DIR}/platform/linux/com.strlen.TreeSheets.metainfo.xml.in"
        "${CMAKE_CURRENT_BINARY_DIR}/com.strlen.TreeSheets.metainfo.xml"
        @ONLY
    )

    install(FILES platform/linux/com.strlen.TreeSheets.svg DESTINATION ${CMAKE_INSTALL_DATADIR}/icons/hicolor/scalable/apps)
    install(FILES platform/linux/com.strlen.TreeSheets.desktop DESTINATION ${CMAKE_INSTALL_DATADIR}/applications)
    install(FILES platform/linux/com.strlen.TreeSheets.xml DESTINATION ${CMAKE_INSTALL_DATADIR}/mime/packages)
    install(FILES "${CMAKE_CURRENT_BINARY_DIR}/com.strlen.TreeSheets.metainfo.xml" DESTINATION ${CMAKE_INSTALL_DATADIR}/metainfo)
elseif(APPLE)
    # Paths must be relative to use with CPack
    set(TREESHEETS_BINDIR .)
    set(TREESHEETS_DOCDIR TreeSheets.app/Contents/Resources)
    set(TREESHEETS_PKGDATADIR TreeSheets.app/Contents/Resources)
else()
    set(TREESHEETS_BINDIR .)
    set(TREESHEETS_DOCDIR .)
    set(TREESHEETS_PKGDATADIR .)
endif()

## Installation

install(TARGETS TreeSheets DESTINATION ${TREESHEETS_BINDIR})
install(DIRECTORY TS/docs DESTINATION ${TREESHEETS_DOCDIR})
install(DIRECTORY TS/examples DESTINATION ${TREESHEETS_DOCDIR})
install(DIRECTORY TS/images DESTINATION ${TREESHEETS_PKGDATADIR})
install(DIRECTORY TS/scripts DESTINATION ${TREESHEETS_PKGDATADIR})
if(ENABLE_LOBSTER)
    set(lobster_modules
        ${lobster_SOURCE_DIR}/modules/std.lobster
        ${lobster_SOURCE_DIR}/modules/stdtype.lobster
        ${lobster_SOURCE_DIR}/modules/vec.lobster
        ${lobster_SOURCE_DIR}/modules/color.lobster
    )
    install(FILES ${lobster_modules} DESTINATION ${TREESHEETS_PKGDATADIR}/scripts/modules)
endif(ENABLE_LOBSTER)

### Packaging with CPack

set(CPACK_PACKAGE_VENDOR "TreeSheets")
set(CPACK_GENERATOR "DEB")
set(CPACK_DEBIAN_PACKAGE_SECTION "contrib/text")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "TreeSheets")
set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
set(CPACK_DEBIAN_FILE_NAME DEB-DEFAULT)
set(CPACK_DEBIAN_PACKAGE_EPOCH 2)

include(CPack)
```
