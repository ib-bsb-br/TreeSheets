This README is a merged representation of the entire codebase, combined into a single document.
The content has been processed where line numbers have been added.
The content has been formatted for parsing in plain style.

================================================================
File Summary
================================================================

Purpose:
--------
This file contains a packed representation of the entire repository's contents.
It is designed to be easily consumable by AI systems for analysis, code review, or other automated processes.

File Format:
------------
The content is organized as follows:
1. This summary section
2. Repository information
3. Directory structure
4. Repository files
5. Multiple file entries, each consisting of:
  a. A separator line (================)
  b. The file path (File: path/to/file)
  c. Another separator line
  d. The full contents of the file
  e. A blank line

Usage Guidelines:
-----------------
- This file should be treated as read-only. Any changes should be made to the original repository files, not this packed version.
- When processing this file, use the file path to distinguish between different files in the repository.

Notes:
------
- Binary files are not included in this packed representation. Please refer to the Repository Structure section for a complete list of file paths, including binary files
- Line numbers have been added to the beginning of each line
- Content has been formatted for parsing in plain style

================================================================
Directory Structure
================================================================
.github/
  workflows/
    build.yml
platform/
  linux/
    com.strlen.TreeSheets.desktop
    com.strlen.TreeSheets.metainfo.xml.in
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
TS/
  docs/
    script_reference.html
.clang-format
.gitattributes
CMakeLists.txt

================================================================
Files
================================================================

================
File: .github/workflows/build.yml
================
 1: name: CI
 2: 
 3: on:
 4:   push:
 5:     branches:
 6:       - master
 7:   pull_request:
 8:     branches:
 9:       - master
10: 
11: jobs:
12:   build-linux:
13:     name: Build Linux (${{ matrix.arch }})
14:     runs-on: ${{ matrix.os }}
15:     strategy:
16:       matrix:
17:         include:
18:           - arch: x64
19:             os: ubuntu-latest
20:           - arch: arm64
21:             os: ubuntu-24.04-arm
22:     permissions:
23:       contents: write
24:     steps:
25:     - uses: actions/checkout@v6
26:     - name: cache build
27:       uses: actions/cache@v5
28:       with:
29:         path: _build/_deps
30:         key: ${{ runner.os }}-${{ matrix.arch }}-cmake-02-build-${{ hashFiles('CMakeLists.txt') }}
31:         restore-keys: ${{ runner.os }}-${{ matrix.arch }}-cmake-02-build
32:     - name: apt update
33:       run: sudo apt-get -o Acquire::Retries=3 update
34:     - name: install opengl
35:       run: sudo apt-get -o Acquire::Retries=3 install mesa-common-dev libgl1-mesa-dev libgl1 libglx-mesa0 libxext-dev
36:     - name: install gtk
37:       run: sudo apt-get -o Acquire::Retries=3 install libgtk-3-dev
38:     - name: cmake
39:       run: |
40:         cmake -S . -B _build \
41:         -DCMAKE_INSTALL_PREFIX=/usr \
42:         -DCPACK_PACKAGING_INSTALL_PREFIX=/usr \
43:         -DCMAKE_BUILD_TYPE=Release \
44:         -DwxBUILD_SHARED=OFF \
45:         -DwxBUILD_INSTALL=OFF \
46:         -DwxUSE_SYS_LIBS=OFF \
47:         -DTREESHEETS_VERSION=${{ github.run_number }}
48:     - name: build and package TreeSheets
49:       run: cmake --build _build --target package -j4
50:     - name: Remove epoch from .deb filename
51:       run: |
52:         shopt -s extglob
53:         for file in _build/treesheets_*:*.deb; do mv -v "${file}" "${file/_+([[:digit:]]):/_}"; done
54:     - name: Create release
55:       if: github.event_name == 'push'
56:       uses: ncipollo/release-action@v1
57:       with:
58:         tag: ${{ github.run_number }}
59:         allowUpdates: true
60:         omitBody: true
61:         commit: ${{ github.sha }}
62:         artifacts: "_build/treesheets_*.deb"
63:     - name: Upload artifacts
64:       if: github.event_name == 'pull_request'
65:       uses: actions/upload-artifact@v4
66:       with:
67:         name: linux-builds-${{ matrix.arch }}
68:         path: _build/treesheets_*.deb
69:     - name: Remove CPack artifact
70:       run: rm -f _build/treesheets_*.deb

================
File: platform/linux/com.strlen.TreeSheets.desktop
================
 1: [Desktop Entry]
 2: Version=1.0
 3: Type=Application
 4: Name=TreeSheets
 5: GenericName=Hierarchical Spreadsheet
 6: GenericName[it]=Fogli di Calcolo
 7: Comment=A hierarchical spreadsheet / outliner productivity tool.
 8: TryExec=TreeSheets
 9: Exec=TreeSheets %f
10: Terminal=false
11: Icon=com.strlen.TreeSheets
12: StartupWMClass=TreeSheets
13: MimeType=application/x-treesheets;
14: Categories=Office;Utility;Spreadsheet;TextEditor;
15: Keywords=mindmaps;knowledge;organizer;organiser;information;brainstorming;pim;database;todo;

================
File: platform/linux/com.strlen.TreeSheets.metainfo.xml.in
================
 1: <?xml version="1.0" encoding="UTF-8"?>
 2: <!-- Copyright 2026  Wouter van Oortmerssen and Tobias Predel -->
 3: <component type="desktop">
 4:   <id>com.strlen.TreeSheets</id>
 5:   <name>TreeSheets</name>
 6:   <summary>Free Form Data Organizer</summary>
 7:   <url type="homepage">https://strlen.com/treesheets</url>
 8:   <url type="bugtracker">https://github.com/aardappel/treesheets/issues</url>
 9:   <url type="vcs-browser">https://github.com/aardappel/treesheets</url>
10:   <url type="contribute">https://github.com/aardappel/treesheets</url>
11:   <url type="donation">https://github.com/sponsors/aardappel</url>
12:   <metadata_license>CC0-1.0</metadata_license>
13:   <project_license>Zlib</project_license>
14:   <content_rating type="oars-1.0" />
15:   <developer id="com.strlen">
16:     <name>Wouter van Oortmerssen</name>
17:   </developer>
18:   <description>
19:     <p>
20:       A "hierarchical spreadsheet" that is a great replacement for
21:       spreadsheets, mind mappers, outliners, PIMs, text editors and small
22:       databases.
23:     </p>
24:     <p>
25:       Suitable for any kind of data organization, such as todo lists,
26:       calendars, project management, brainstorming, organizing ideas, planning,
27:       requirements gathering, presentation of information, etc.
28:     </p>
29:     <p>
30:       It's like a spreadsheet, immediately familiar, but much more suitable
31:       for complex data because it's hierarchical.
32:     </p>
33:     <ul>
34:         <li>It's like a mind mapper, but more organized and compact.</li>
35:         <li>It's like an outliner, but in more than one dimension.</li>
36:         <li>It's like a text editor, but with structure.</li>
37:     </ul>
38:   </description>
39:   <launchable type="desktop-id">com.strlen.TreeSheets.desktop</launchable>
40:   <screenshots>
41:     <screenshot type="default">
42:       <image>https://strlen.com/treesheets/docs/images/screenshots/screenshot_todo.png</image>
43:       <caption>A to-do list organized using TreeSheets' hierarchical grid</caption>
44:     </screenshot>
45:     <screenshot>
46:       <image>https://strlen.com/treesheets/docs/images/screenshots/screenshot_unicode.png</image>
47:       <caption>Support for complex Unicode characters and varied formatting</caption>
48:     </screenshot>
49:     <screenshot>
50:       <image>https://strlen.com/treesheets/docs/images/screenshots/screenshot_tutorial.png</image>
51:       <caption>An interactive tutorial demonstrating the grid-within-grid structure</caption>
52:     </screenshot>
53:   </screenshots>
54:   <releases>
55:     <release version="@TREESHEETS_VERSION@" date="@TREESHEETS_RELEASE_DATE@">
56:       <description>
57:         <p>Please check the GitHub page for the updates.</p>
58:       </description>
59:     </release>
60:   </releases>
61: </component>

================
File: platform/linux/com.strlen.TreeSheets.svg
================
  1: <?xml version="1.0" encoding="UTF-8" standalone="no"?>
  2: <!-- Created with Inkscape (http://www.inkscape.org/) -->
  3: 
  4: <svg
  5:    xmlns:osb="http://www.openswatchbook.org/uri/2009/osb"
  6:    xmlns:dc="http://purl.org/dc/elements/1.1/"
  7:    xmlns:cc="http://creativecommons.org/ns#"
  8:    xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
  9:    xmlns:svg="http://www.w3.org/2000/svg"
 10:    xmlns="http://www.w3.org/2000/svg"
 11:    xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd"
 12:    xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
 13:    width="680"
 14:    height="680"
 15:    viewBox="0 0 179.91666 179.91667"
 16:    version="1.1"
 17:    id="svg8"
 18:    inkscape:version="0.92.5 (2060ec1f9f, 2020-04-08)"
 19:    sodipodi:docname="treesheets.svg"
 20:    inkscape:export-filename="/home/matthew/Desktop/treesheets.png"
 21:    inkscape:export-xdpi="96"
 22:    inkscape:export-ydpi="96">
 23:   <defs
 24:      id="defs2">
 25:     <linearGradient
 26:        id="linearGradient901"
 27:        osb:paint="solid">
 28:       <stop
 29:          style="stop-color:#000000;stop-opacity:1;"
 30:          offset="0"
 31:          id="stop899" />
 32:     </linearGradient>
 33:   </defs>
 34:   <sodipodi:namedview
 35:      id="base"
 36:      pagecolor="#ffffff"
 37:      bordercolor="#666666"
 38:      borderopacity="1.0"
 39:      inkscape:pageopacity="0.0"
 40:      inkscape:pageshadow="2"
 41:      inkscape:zoom="0.733172"
 42:      inkscape:cx="322.37837"
 43:      inkscape:cy="271.15816"
 44:      inkscape:document-units="px"
 45:      inkscape:current-layer="layer1"
 46:      showgrid="false"
 47:      inkscape:window-width="1920"
 48:      inkscape:window-height="1014"
 49:      inkscape:window-x="0"
 50:      inkscape:window-y="0"
 51:      inkscape:window-maximized="1"
 52:      units="px" />
 53:   <metadata
 54:      id="metadata5">
 55:     <rdf:RDF>
 56:       <cc:Work
 57:          rdf:about="">
 58:         <dc:format>image/svg+xml</dc:format>
 59:         <dc:type
 60:            rdf:resource="http://purl.org/dc/dcmitype/StillImage" />
 61:         <dc:title></dc:title>
 62:       </cc:Work>
 63:     </rdf:RDF>
 64:   </metadata>
 65:   <g
 66:      inkscape:label="Layer 1"
 67:      inkscape:groupmode="layer"
 68:      id="layer1"
 69:      transform="translate(-19.704056,-40.32725)">
 70:     <g
 71:        id="g23"
 72:        transform="matrix(1.0592611,0,0,1.0770814,-1.1013222,-16.908959)">
 73:       <rect
 74:          y="59.368668"
 75:          x="25.867487"
 76:          height="154.58766"
 77:          width="157.39896"
 78:          id="rect75-3"
 79:          style="fill:#ffffff;fill-opacity:1;stroke:#767676;stroke-width:12.45077991;stroke-miterlimit:4;stroke-dasharray:none;stroke-opacity:1" />
 80:       <path
 81:          inkscape:connector-curvature="0"
 82:          id="path15"
 83:          d="M 37.041672,132.95834 H 170.08929"
 84:          style="fill:none;stroke:#6d6d6d;stroke-width:3.06500006;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-dasharray:12.26, 12.26;stroke-dashoffset:0;stroke-opacity:1" />
 85:       <path
 86:          inkscape:connector-curvature="0"
 87:          id="path15-8"
 88:          d="M 36.588093,163.95238 H 169.63572"
 89:          style="fill:none;stroke:#6d6d6d;stroke-width:3.06500006;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-dasharray:12.26000013, 12.26000013;stroke-dashoffset:0;stroke-opacity:1" />
 90:       <path
 91:          inkscape:connector-curvature="0"
 92:          id="path15-5"
 93:          d="M 129.26786,203.41308 V 70.36547"
 94:          style="fill:none;stroke:#6d6d6d;stroke-width:3.06500006;stroke-linecap:butt;stroke-linejoin:miter;stroke-miterlimit:4;stroke-dasharray:12.26000013, 12.26000013;stroke-dashoffset:0;stroke-opacity:1" />
 95:       <g
 96:          id="flowRoot913"
 97:          style="font-style:normal;font-variant:normal;font-weight:bold;font-stretch:normal;font-size:74.66666412px;line-height:1.25;font-family:'Libris ADF Std';-inkscape-font-specification:'Libris ADF Std, Bold';font-variant-ligatures:normal;font-variant-caps:normal;font-variant-numeric:normal;font-feature-settings:normal;text-align:start;letter-spacing:0px;word-spacing:0px;writing-mode:lr-tb;text-anchor:start;fill:#000000;fill-opacity:1;stroke:none;stroke-opacity:1"
 98:          transform="matrix(0.76673,0,0,0.76673,893.21672,-3.6280039)"
 99:          aria-label="TS">
100:         <path
101:            id="path825"
102:            style="font-style:normal;font-variant:normal;font-weight:bold;font-stretch:normal;font-size:74.66666412px;font-family:'Libris ADF Std';-inkscape-font-specification:'Libris ADF Std, Bold';font-variant-ligatures:normal;font-variant-caps:normal;font-variant-numeric:normal;font-feature-settings:normal;text-align:start;writing-mode:lr-tb;text-anchor:start;fill:#000000;fill-opacity:1;stroke:none;stroke-opacity:1"
103:            d="m -1078.6855,155.60379 c 0,-15.38133 -0.075,-31.35999 -0.075,-46.74133 h 15.1574 l 2.3893,-6.79466 h -41.664 l -2.9867,6.79466 h 17.7707 c 0,8.43734 0.1493,15.456 0.1493,21.056 0,21.87733 -0.7466,25.16267 -2.3146,27.104 z"
104:            inkscape:connector-curvature="0" />
105:         <path
106:            id="path827"
107:            style="font-style:normal;font-variant:normal;font-weight:bold;font-stretch:normal;font-size:74.66666412px;font-family:'Libris ADF Std';-inkscape-font-specification:'Libris ADF Std, Bold';font-variant-ligatures:normal;font-variant-caps:normal;font-variant-numeric:normal;font-feature-settings:normal;text-align:start;writing-mode:lr-tb;text-anchor:start;fill:#000000;fill-opacity:1;stroke:none;stroke-opacity:1"
108:            d="m -1064.2003,150.07846 c 2.688,4.40533 11.2747,6.12267 15.9787,7.69067 12.2453,0 18.6667,-10.37867 18.6667,-18.592 0,-3.808 -1.792,-7.24267 -4.928,-9.856 -2.0907,-1.64267 -8.6614,-2.83733 -12.32,-4.85333 -3.8827,-2.09067 -6.1227,-5.67467 -6.1227,-9.25867 0,-2.61333 0.5973,-4.85333 1.6427,-6.34667 0.896,-0.14933 1.7173,-0.224 2.24,-0.224 3.2853,0 8.96,1.86667 11.7226,5.74934 l 7.9894,-5.00267 c -2.7627,-4.40533 -7.5414,-6.72 -11.9467,-6.72 -3.5093,0 -10.0053,1.49333 -12.3947,3.36 -4.928,4.032 -10.4533,8.88533 -10.4533,14.26133 0,3.50934 1.1947,6.64534 3.808,9.10934 h -0.075 c 2.3894,2.464 8.8854,3.65866 11.3494,4.85333 l 1.8666,0.896 c 4.6294,2.24 8.736,4.256 8.736,8.96 0,5.00266 -1.344,7.24266 -2.6133,7.24266 -3.8827,0 -10.1547,-2.016 -12.8427,-6.64533 z"
109:            inkscape:connector-curvature="0" />
110:       </g>
111:     </g>
112:   </g>
113: </svg>

================
File: platform/linux/com.strlen.TreeSheets.xml
================
 1: <?xml version="1.0" encoding="UTF-8"?>
 2: <mime-info xmlns="http://www.freedesktop.org/standards/shared-mime-info">
 3:   <mime-type type="application/x-treesheets">
 4:     <comment xml:lang="en">TreeSheets document</comment>
 5:     <icon name="com.strlen.TreeSheets"/>
 6:     <glob pattern="*.cts"/>
 7:     <magic priority="50">
 8:       <match type="string" value="TSFF" offset="0"/>
 9:     </magic>
10:     <sub-class-of type="application/octet-stream"/>
11:   </mime-type>
12: </mime-info>

================
File: src/cell.h
================
  1: /* The evaluation types for a cell.
  2: CT_DATA: "Data"
  3: CT_CODE: "Operation"
  4: CT_VARD: "Variable Assign"
  5: CT_VARU: "Variable Read"
  6: CT_VIEWH: "Horizontal View"
  7: CT_VIEWV: "Vertical View"
  8: */
  9: enum { CT_DATA = 0, CT_CODE, CT_VARD, CT_VIEWH, CT_VARU, CT_VIEWV };
 10: 
 11: /* The drawstyles for a cell:
 12: 
 13: */
 14: enum { DS_GRID, DS_BLOBSHIER, DS_BLOBLINE };
 15: 
 16: /**
 17:     The Cell structure represents the editable cells in the sheet.
 18: 
 19:     They are mutable structures containing a text and grid object. Along with
 20:     formatting information.
 21: */
 22: struct Cell {
 23:     Cell *parent;
 24:     int sx {0};
 25:     int sy {0};
 26:     int ox {0};
 27:     int oy {0};
 28:     int minx {0};
 29:     int miny {0};
 30:     int ycenteroff {0};
 31:     int txs {0};
 32:     int tys {0};
 33:     int celltype;
 34:     Text text;
 35:     Grid *grid;
 36:     uint cellcolor {g_cellcolor_default};
 37:     uint actualcellcolor {g_cellcolor_default};
 38:     uint textcolor {g_textcolor_default};
 39:     bool tiny {false};
 40:     bool verticaltextandgrid {true};
 41:     wxUint8 drawstyle {DS_GRID};
 42:     wxString note;
 43: 
 44:     Cell(Cell *_p = nullptr, const Cell *_clonefrom = nullptr, int _ct = CT_DATA,
 45:          Grid *_g = nullptr)
 46:         : parent(_p), celltype(_ct), grid(_g) {
 47:         text.cell = this;
 48:         if (_g) _g->cell = this;
 49:         if (_p) {
 50:             text.relsize = _p->text.relsize;
 51:             verticaltextandgrid = _p->verticaltextandgrid;
 52:         }
 53:         if (_clonefrom) CloneStyleFrom(_clonefrom);
 54:     }
 55: 
 56:     ~Cell() { DELETEP(grid); }
 57:     void Clear() {
 58:         DELETEP(grid);
 59:         text.t.Clear();
 60:         text.image = nullptr;
 61:         Reset();
 62:     }
 63: 
 64:     bool HasText() const { return !text.t.empty(); }
 65:     bool HasTextSize() const { return HasText() || text.relsize; }
 66:     bool HasTextState() const { return HasTextSize() || text.image; }
 67:     bool HasHeader() const { return HasText() || text.image; }
 68:     bool HasContent() const { return HasHeader() || grid; }
 69:     bool GridShown(Document *doc) const {
 70:         return grid && (!grid->folded || this == doc->currentdrawroot);
 71:     }
 72:     int MinRelsize()  // the smallest relsize is actually the biggest text
 73:     {
 74:         int rs = INT_MAX;
 75:         if (grid) {
 76:             rs = grid->MinRelsize(rs);
 77:         } else if (HasText()) {
 78:             // the "else" causes oversized titles but a readable grid when you zoom, if only
 79:             // the grid has been shrunk
 80:             rs = text.MinRelsize(rs);
 81:         }
 82:         return rs;
 83:     }
 84: 
 85:     size_t EstimatedMemoryUse() {
 86:         return sizeof(Cell) + text.EstimatedMemoryUse() + (grid ? grid->EstimatedMemoryUse() : 0);
 87:     }
 88: 
 89:     void Layout(Document *doc, wxReadOnlyDC &dc, int depth, int maxcolwidth, bool forcetiny) {
 90:         tiny = text.filtered && !grid || forcetiny ||
 91:                doc->PickFont(dc, depth, text.relsize, text.stylebits);
 92:         int ixs = 0, iys = 0;
 93:         if (!tiny) sys->ImageSize(text.DisplayImage(), ixs, iys);
 94:         int leftoffset = 0;
 95:         if (!HasText()) {
 96:             if (!ixs || !iys) {
 97:                 sx = sy = tiny ? 1 : dc.GetCharHeight();
 98:             } else {
 99:                 leftoffset = dc.GetCharHeight();
100:             }
101:         } else {
102:             text.TextSize(dc, sx, sy, tiny, leftoffset, maxcolwidth);
103:         }
104:         if (ixs && iys) {
105:             sx += ixs + 2;
106:             sy = max(iys + 2, sy);
107:         }
108:         text.extent = sx + depth * dc.GetCharHeight();
109:         txs = sx;
110:         tys = sy;
111:         if (GridShown(doc)) {
112:             if (HasHeader()) {
113:                 if (verticaltextandgrid) {
114:                     int osx = sx;
115:                     if (drawstyle == DS_BLOBLINE && !tiny) sy += 4;
116:                     grid->Layout(doc, dc, depth, sx, sy, leftoffset, sy, tiny || forcetiny);
117:                     sx = max(sx, osx);
118:                 } else {
119:                     int osy = sy;
120:                     if (drawstyle == DS_BLOBLINE && !tiny) sx += 18;
121:                     grid->Layout(doc, dc, depth, sx, sy, sx, 0, tiny || forcetiny);
122:                     sy = max(sy, osy);
123:                 }
124:             } else
125:                 tiny = grid->Layout(doc, dc, depth, sx, sy, 0, 0, forcetiny);
126:         }
127:         ycenteroff = !verticaltextandgrid ? (sy - tys) / 2 : 0;
128:         if (!tiny) {
129:             sx += g_margin_extra * 2;
130:             sy += g_margin_extra * 2;
131:         }
132:     }
133: 
134:     void Render(Document *doc, int bx, int by, wxDC &dc, int depth, int ml, int mr, int mt, int mb,
135:                 int maxcolwidth, int cell_margin) {
136:         // Choose color from celltype (program operations)
137:         switch (celltype) {
138:             case CT_VARD: actualcellcolor = 0xFF8080; break;
139:             case CT_VARU: actualcellcolor = 0xFFA0A0; break;
140:             case CT_VIEWH:
141:             case CT_VIEWV: actualcellcolor = 0x80FF80; break;
142:             case CT_CODE: actualcellcolor = 0x8080FF; break;
143:             default: actualcellcolor = cellcolor; break;
144:         }
145:         uint parentcolor = doc->Background();
146:         if (parent && this != doc->currentdrawroot) {
147:             Cell *p = parent;
148:             while (p && p->drawstyle == DS_BLOBLINE)
149:                 p = p == doc->currentdrawroot ? nullptr : p->parent;
150:             if (p) parentcolor = p->actualcellcolor;
151:         }
152: 
153:         if (sys->darkennonmatchingcells && !text.IsInSearch()) {
154:             auto cp = (uchar *)&actualcellcolor;
155:             loop(i, 4) cp[i] = cp[i] * 800 / 1000;
156:         }
157: 
158:         if (drawstyle == DS_GRID && actualcellcolor != parentcolor) {
159:             DrawRectangle(dc, actualcellcolor, bx - ml, by - mt, sx + ml + mr, sy + mt + mb);
160:         }
161:         if (drawstyle != DS_GRID && HasContent() && !tiny) {
162:             if (actualcellcolor == parentcolor) {
163:                 auto cp = (uchar *)&actualcellcolor;
164:                 loop(i, 4) cp[i] = cp[i] * 850 / 1000;
165:             }
166:             dc.SetBrush(wxBrush(LightColor(actualcellcolor)));
167:             dc.SetPen(wxPen(LightColor(actualcellcolor)));
168: 
169:             if (drawstyle == DS_BLOBSHIER)
170:                 dc.DrawRoundedRectangle(bx - cell_margin, by - cell_margin, minx + cell_margin * 2,
171:                                         miny + cell_margin * 2, sys->roundness);
172:             else if (HasHeader())
173:                 dc.DrawRoundedRectangle(bx - cell_margin + g_margin_extra / 2,
174:                                         by - cell_margin + ycenteroff + g_margin_extra / 2,
175:                                         txs + cell_margin * 2 + g_margin_extra,
176:                                         tys + cell_margin * 2 + g_margin_extra, sys->roundness);
177:             // FIXME: this half a g_margin_extra is a bit of hack
178:         }
179:         dc.SetTextBackground(LightColor(actualcellcolor));
180:         int xoff = verticaltextandgrid ? 0 : text.extent - depth * dc.GetCharHeight();
181:         int yoff = text.Render(doc, bx, by + ycenteroff, depth, dc, xoff, maxcolwidth);
182:         yoff = verticaltextandgrid ? yoff : 0;
183:         if (GridShown(doc)) grid->Render(doc, bx, by, dc, depth, sx - xoff, sy - yoff, xoff, yoff);
184: 
185:         if (!note.IsEmpty() && !tiny && this != doc->currentdrawroot) {
186:             wxPoint points[3];
187:             int size = 6;
188:             int right = bx + sx + mr;
189:             int top = by - mt;
190:             points[0] = wxPoint(right, top);
191:             points[1] = wxPoint(right, top + size);
192:             points[2] = wxPoint(right - size, top);
193:             dc.SetBrush(wxBrush(LightColor(textcolor)));
194:             dc.SetPen(wxPen(LightColor(textcolor)));
195:             dc.DrawPolygon(3, points);
196:         }
197:     }
198: 
199:     void CloneStyleFrom(Cell const *o) {
200:         cellcolor = o->cellcolor;
201:         textcolor = o->textcolor;
202:         verticaltextandgrid = o->verticaltextandgrid;
203:         drawstyle = o->drawstyle;
204:         text.stylebits = o->text.stylebits;
205:     }
206: 
207:     unique_ptr<Cell> Clone(Cell *_parent) const {
208:         unique_ptr<Cell> c = make_unique<Cell>(_parent, this, celltype,
209:                                                grid ? new Grid(grid->xs, grid->ys) : nullptr);
210:         c->text = text;
211:         c->text.cell = c.get();
212:         c->note = note;
213:         if (grid) { grid->Clone(c->grid); }
214:         return c;
215:     }
216: 
217:     bool IsInside(int x, int y) const { return x >= 0 && y >= 0 && x < sx && y < sy; }
218:     int GetX(Document *doc) const { return ox + (parent ? parent->GetX(doc) : doc->hierarchysize); }
219:     int GetY(Document *doc) const { return oy + (parent ? parent->GetY(doc) : doc->hierarchysize); }
220:     int Depth() const { return parent ? parent->Depth() + 1 : 0; }
221:     Cell *Parent(int i) { return i ? parent->Parent(i - 1) : this; }
222:     Cell *SetParent(Cell *g) {
223:         parent = g;
224:         return this;
225:     }
226:     bool IsParentOf(const Cell *c) {
227:         return c->parent == this || (c->parent && IsParentOf(c->parent));
228:     }
229: 
230:     wxString ToText(int indent, const Selection &sel, int format, Document *doc, bool inheritstyle,
231:                     Cell *root) {
232:         wxString str = text.ToText(indent, sel, format);
233:         if ((format == A_EXPHTMLT || format == A_EXPHTMLTI || format == A_EXPHTMLTE) &&
234:             (text.stylebits & (STYLE_UNDERLINE | STYLE_STRIKETHRU)) && this != root &&
235:             !str.IsEmpty()) {
236:             wxString spanstyle = "text-decoration:";
237:             spanstyle += (text.stylebits & STYLE_UNDERLINE) ? " underline" : "";
238:             spanstyle += (text.stylebits & STYLE_STRIKETHRU) ? " line-through" : "";
239:             spanstyle += ";";
240:             str.Prepend("<span style=\"" + spanstyle + "\">");
241:             str.Append("</span>");
242:         }
243:         if (format == A_EXPCSV) {
244:             if (grid) return grid->ToText(indent, sel, format, doc, inheritstyle, root);
245:             str.Replace("\"", "\"\"");
246:             return "\"" + str + "\"";
247:         }
248:         if (sel.cursor != sel.cursorend) return str;
249:         str.Append(LINE_SEPARATOR);
250:         if (grid) str.Append(grid->ToText(indent, sel, format, doc, inheritstyle, root));
251:         if (format == A_EXPXML) {
252:             str.Prepend(">");
253:             if (text.relsize) {
254:                 str.Prepend("\"");
255:                 str.Prepend(wxString() << -text.relsize);
256:                 str.Prepend(" relsize=\"");
257:             }
258:             if (text.stylebits) {
259:                 str.Prepend("\"");
260:                 str.Prepend(wxString() << text.stylebits);
261:                 str.Prepend(" stylebits=\"");
262:             }
263:             if (cellcolor != 0xFFFFFF) {
264:                 str.Prepend("\"");
265:                 str.Prepend(wxString::Format("0x%06X", cellcolor));
266:                 str.Prepend(" colorbg=\"");
267:             }
268:             if (textcolor != 0x000000) {
269:                 str.Prepend("\"");
270:                 str.Prepend(wxString::Format("0x%06X", textcolor));
271:                 str.Prepend(" colorfg=\"");
272:             }
273:             if (celltype != CT_DATA) {
274:                 str.Prepend("\"");
275:                 str.Prepend(wxString() << celltype);
276:                 str.Prepend(" type=\"");
277:             }
278:             str.Prepend("<cell");
279:             str.Append(' ', indent);
280:             str.Append("</cell>\n");
281:         } else if ((format == A_EXPHTMLT || format == A_EXPHTMLTI || format == A_EXPHTMLTE) &&
282:                    this != root) {
283:             wxString style;
284:             if (!inheritstyle || !parent ||
285:                 (text.stylebits & STYLE_BOLD) != (parent->text.stylebits & STYLE_BOLD))
286:                 style +=
287:                     text.stylebits & STYLE_BOLD ? "font-weight: bold;" : "font-weight: normal;";
288:             if (!inheritstyle || !parent ||
289:                 (text.stylebits & STYLE_ITALIC) != (parent->text.stylebits & STYLE_ITALIC))
290:                 style +=
291:                     text.stylebits & STYLE_ITALIC ? "font-style: italic;" : "font-style: normal;";
292:             if (!inheritstyle || !parent ||
293:                 (text.stylebits & STYLE_FIXED) != (parent->text.stylebits & STYLE_FIXED)) {
294:                 style += "font-family: '";
295:                 style += text.stylebits & STYLE_FIXED ? sys->defaultfixedfont + "', monospace;"
296:                                                       : sys->defaultfont + "', sans-serif;";
297:             }
298:             if (!inheritstyle || cellcolor != (parent ? parent->cellcolor : doc->Background()))
299:                 style += wxString::Format("background-color: #%06X;", SwapColor(cellcolor));
300:             auto exporttextcolor = IsTag(doc) ? doc->tags[text.t] : textcolor;
301:             auto parenttextcolor =
302:                 parent ? parent->IsTag(doc) ? doc->tags[parent->text.t] : parent->textcolor
303:                        : 0x000000;
304:             if (!inheritstyle || exporttextcolor != parenttextcolor)
305:                 style += wxString::Format("color: #%06X;", SwapColor(exporttextcolor));
306:             str.Prepend(style.IsEmpty() ? wxString("<td>")
307:                                         : wxString("<td style=\"") + style + wxString("\">"));
308:             str.Append(' ', indent);
309:             str.Append("</td>\n");
310:         } else if (format == A_EXPHTMLB && (text.t.Len() || grid) && this != root) {
311:             str.Prepend("<li>");
312:             str.Append(' ', indent);
313:             str.Append("</li>\n");
314:         } else if (format == A_EXPHTMLO && text.t.Len()) {
315:             wxString h = wxString("h") + wxChar('0' + indent / 2) + ">";
316:             str.Prepend("<" + h);
317:             str.Append(' ', indent);
318:             str.Append("</" + h + "\n");
319:         }
320:         str.Pad(indent, ' ', false);
321:         return str;
322:     }
323: 
324:     void RelSize(int dir, int zoomdepth) {
325:         text.RelSize(dir, zoomdepth);
326:         if (grid) grid->RelSize(dir, zoomdepth);
327:     }
328: 
329:     void Reset() { ox = oy = sx = sy = minx = miny = ycenteroff = 0; }
330:     void ResetChildren() {
331:         Reset();
332:         if (grid) grid->ResetChildren();
333:     }
334: 
335:     void ResetLayout() {
336:         Reset();
337:         if (parent) parent->ResetLayout();
338:     }
339: 
340:     void LazyLayout(Document *doc, wxReadOnlyDC &dc, int depth, int maxcolwidth, bool forcetiny) {
341:         if (sx == 0) {
342:             Layout(doc, dc, depth, maxcolwidth, forcetiny);
343:             minx = sx;
344:             miny = sy;
345:         } else {
346:             sx = minx;
347:             sy = miny;
348:         }
349:     }
350: 
351:     void AddUndo(Document *doc) {
352:         ResetLayout();
353:         doc->AddUndo(this);
354:     }
355: 
356:     void Save(wxDataOutputStream &dos, Cell *ocs) const {
357:         dos.Write8(celltype);
358:         dos.Write32(cellcolor);
359:         dos.Write32(textcolor);
360:         dos.Write8(drawstyle);
361:         dos.WriteString(note);
362:         uint cellflags = this == ocs ? TS_SELECTION_MASK : 0;
363:         if (HasTextState()) {
364:             cellflags |= grid ? TS_BOTH : TS_TEXT;
365:             dos.Write8(cellflags);
366:             text.Save(dos);
367:             if (grid) grid->Save(dos, ocs);
368:         } else if (grid) {
369:             cellflags |= TS_GRID;
370:             dos.Write8(cellflags);
371:             grid->Save(dos, ocs);
372:         } else {
373:             cellflags |= TS_NEITHER;
374:             dos.Write8(cellflags);
375:         }
376:     }
377: 
378:     Grid *AddGrid(int x = 1, int y = 1) {
379:         if (!grid) {
380:             grid = new Grid(x, y, this);
381:             grid->InitCells(this);
382:             if (parent) grid->CloneStyleFrom(parent->grid);
383:         }
384:         return grid;
385:     }
386: 
387:     Cell *LoadGrid(wxDataInputStream &dis, int &numcells, int &textbytes, Cell *&ics) {
388:         int xs = dis.Read32();
389:         auto g = new Grid(xs, dis.Read32());
390:         grid = g;
391:         g->cell = this;
392:         if (!g->LoadContents(dis, numcells, textbytes, ics)) return nullptr;
393:         return this;
394:     }
395: 
396:     static Cell *LoadWhich(wxDataInputStream &dis, Cell *_p, int &numcells, int &textbytes, Cell *&ics) {
397:         auto c = new Cell(_p, nullptr, dis.Read8());
398:         numcells++;
399:         if (sys->versionlastloaded >= 8) {
400:             c->cellcolor = dis.Read32() & 0xFFFFFF;
401:             c->textcolor = dis.Read32() & 0xFFFFFF;
402:         }
403:         if (sys->versionlastloaded >= 15) c->drawstyle = dis.Read8();
404:         if (sys->versionlastloaded >= 25) c->note = dis.ReadString();
405:         int ts = dis.Read8();
406:         if (ts & TS_SELECTION_MASK) {
407:             ics = c;
408:             ts &= ~TS_SELECTION_MASK;
409:         }
410:         switch (ts) {
411:             case TS_BOTH:
412:             case TS_TEXT:
413:                 c->text.Load(dis);
414:                 textbytes += c->text.t.Len();
415:                 if (ts == TS_TEXT) return c;
416:             case TS_GRID: return c->LoadGrid(dis, numcells, textbytes, ics);
417:             case TS_NEITHER: return c;
418:             default: return nullptr;
419:         }
420:     }
421: 
422:     unique_ptr<Cell> Eval(auto &ev) const {
423:         // Evaluates the internal grid if it exists, otherwise, evaluate the text.
424:         return grid ? grid->Eval(ev) : text.Eval(ev);
425:     }
426: 
427:     void Paste(Document *document, const Cell *original, Selection &selection) {
428:         parent->AddUndo(document);
429:         ResetLayout();
430:         if (!HasText() || !selection.TextEdit()) { note = original->note; }
431:         if (original->HasText()) {
432:             if (!HasText() || !selection.TextEdit()) {
433:                 cellcolor = original->cellcolor;
434:                 textcolor = original->textcolor;
435:                 text.stylebits = original->text.stylebits;
436:             }
437:             text.Insert(document, original->text.t, selection, false);
438:         }
439:         if (original->text.image) text.image = original->text.image;
440:         if (original->grid) {
441:             auto gridclone = new Grid(original->grid->xs, original->grid->ys);
442:             gridclone->cell = this;
443:             original->grid->Clone(gridclone);
444:             // Note: deleting grid may invalidate c if its a child of grid, so clear it.
445:             original = nullptr;
446:             DELETEP(grid);  // FIXME: could merge instead?
447:             grid = gridclone;
448:             if (!HasText())
449:                 grid->MergeWithParent(parent->grid, selection, document);  // deletes grid/this.
450:         }
451:     }
452: 
453:     Cell *FindNextSearchMatch(const wxString &s, Cell *best, Cell *selected, bool &lastwasselected,
454:                               bool reverse) {
455:         if (reverse && grid)
456:             best = grid->FindNextSearchMatch(s, best, selected, lastwasselected, reverse);
457:         if ((sys->casesensitivesearch ? text.t.Find(s) : text.t.Lower().Find(s)) >= 0) {
458:             if (lastwasselected) best = this;
459:             lastwasselected = false;
460:         }
461:         if (selected == this) lastwasselected = true;
462:         if (!reverse && grid)
463:             best = grid->FindNextSearchMatch(s, best, selected, lastwasselected, reverse);
464:         return best;
465:     }
466: 
467:     Cell *FindNextFilterMatch(Cell *best, Cell *selected, bool &lastwasselected) {
468:         if (!text.filtered) {
469:             if (lastwasselected) best = this;
470:             lastwasselected = false;
471:         }
472:         if (selected == this) lastwasselected = true;
473:         if (grid) best = grid->FindNextFilterMatch(best, selected, lastwasselected);
474:         return best;
475:     }
476: 
477:     Cell *FindLink(const Selection &sel, Cell *link, Cell *best, bool &lastthis, bool &stylematch,
478:                    bool forward, bool image) {
479:         if (grid) best = grid->FindLink(sel, link, best, lastthis, stylematch, forward, image);
480:         if (link == this) {
481:             lastthis = true;
482:             return best;
483:         }
484:         if (image ? link->text.image == text.image
485:                   : link->text.ToText(0, sel, A_EXPTEXT) == text.t) {
486:             if (link->text.stylebits != text.stylebits || link->cellcolor != cellcolor ||
487:                 link->textcolor != textcolor) {
488:                 if (!stylematch) best = nullptr;
489:                 stylematch = true;
490:             } else if (stylematch) {
491:                 return best;
492:             }
493:             if (!best || lastthis) {
494:                 lastthis = false;
495:                 return this;
496:             }
497:         }
498:         return best;
499:     }
500: 
501:     void FindReplaceAll(const wxString &s, const wxString &ls) {
502:         if (grid) grid->FindReplaceAll(s, ls);
503:         text.ReplaceStr(s, ls);
504:     }
505: 
506:     Cell *FindExact(const wxString &s) {
507:         return text.t == s ? this : (grid ? grid->FindExact(s) : nullptr);
508:     }
509: 
510:     void ImageRefCount(bool includefolded) {
511:         if (grid) grid->ImageRefCount(includefolded);
512:         if (text.image) text.image->trefc++;
513:     }
514: 
515:     void ColorChange(Document *doc, int which, uint color) {
516:         switch (which) {
517:             case A_CELLCOLOR: cellcolor = color; break;
518:             case A_TEXTCOLOR:
519:                 if (IsTag(doc)) {
520:                     doc->tags[text.t] = color;
521:                 } else {
522:                     textcolor = color;
523:                 }
524:                 break;
525:             case A_BORDCOLOR:
526:                 if (parent && parent->grid) parent->grid->bordercolor = color;
527:                 break;
528:         }
529:         text.WasEdited();
530:     }
531: 
532:     void SetGridTextLayout(int ds, bool vert, bool noset) {
533:         if (!noset) verticaltextandgrid = vert;
534:         if (ds != -1) drawstyle = ds;
535:         if (grid) grid->SetGridTextLayout(ds, vert, noset, grid->SelectAll());
536:     }
537: 
538:     bool IsTag(Document *doc) { return doc->tags.contains(text.t); }
539:     void MaxDepthLeaves(int curdepth, int &maxdepth, int &leaves) {
540:         if (curdepth > maxdepth) maxdepth = curdepth;
541:         if (grid)
542:             grid->MaxDepthLeaves(curdepth + 1, maxdepth, leaves);
543:         else
544:             leaves++;
545:     }
546: 
547:     int ColWidth() {
548:         return parent ? parent->grid->colwidths[parent->grid->FindCell(this).x]
549:                       : sys->defaultmaxcolwidth;
550:     }
551: 
552:     void CollectCells(auto &itercells, bool recurse = true) {
553:         itercells.push_back(this);
554:         if (grid && recurse) grid->CollectCells(itercells);
555:     }
556: 
557:     Cell *Graph() {
558:         auto n = text.GetNum();
559:         text.t.Clear();
560:         text.t.Append(L'|', n);
561:         return this;
562:     }
563: };

================
File: src/document.h
================
   1: struct UndoItem {
   2:     vector<Selection> path;
   3:     vector<Selection> selpath;
   4:     Selection sel;
   5:     unique_ptr<Cell> clone;
   6:     size_t estimated_size {0};
   7:     uintptr_t cloned_from;  // May be dead.
   8:     int generation {0};
   9: };
  10: 
  11: struct Document {
  12:     TSCanvas *canvas {nullptr};
  13:     unique_ptr<Cell> root {nullptr};
  14:     Selection prev;
  15:     Selection hover;
  16:     Selection selected;
  17:     Selection begindrag;
  18:     int isctrlshiftdrag;
  19:     int scrollx;
  20:     int scrolly;
  21:     int maxx;
  22:     int maxy;
  23:     int centerx {0};
  24:     int centery {0};
  25:     int layoutxs;
  26:     int layoutys;
  27:     int hierarchysize;
  28:     int fgutter {6};
  29:     int lasttextsize;
  30:     int laststylebits;
  31:     Cell *currentdrawroot;  // for use during Render() calls
  32:     vector<unique_ptr<UndoItem>> undolist;
  33:     vector<unique_ptr<UndoItem>> redolist;
  34:     vector<Selection> drawpath;
  35:     int pathscalebias {0};
  36:     wxString filename {""};
  37:     long lastmodsinceautosave {0};
  38:     long undolistsizeatfullsave {0};
  39:     long lastsave {wxGetLocalTime()};
  40:     bool modified {false};
  41:     bool tmpsavesuccess {true};
  42:     wxDataObjectComposite *dndobjc {new wxDataObjectComposite()};
  43:     wxTextDataObject *dndobjt {new wxTextDataObject()};
  44:     wxBitmapDataObject *dndobji {new wxBitmapDataObject()};
  45:     wxFileDataObject *dndobjf {new wxFileDataObject()};
  46: 
  47:     struct Printout : wxPrintout {
  48:         Document *doc;
  49:         Printout(Document *d) : wxPrintout("printout"), doc(d) {}
  50: 
  51:         bool OnPrintPage(int page) {
  52:             auto dc = GetDC();
  53:             if (!dc) return false;
  54:             doc->Print(*dc, *this);
  55:             return true;
  56:         }
  57: 
  58:         bool OnBeginDocument(int startPage, int endPage) {
  59:             return wxPrintout::OnBeginDocument(startPage, endPage);
  60:         }
  61: 
  62:         void GetPageInfo(int *minPage, int *maxPage, int *selPageFrom, int *selPageTo) {
  63:             *minPage = 1;
  64:             *maxPage = 1;
  65:             *selPageFrom = 1;
  66:             *selPageTo = 1;
  67:         }
  68: 
  69:         bool HasPage(int pageNum) { return pageNum == 1; }
  70:     };
  71: 
  72:     bool while_printing {false};
  73:     wxPrintData printData;
  74:     wxPageSetupDialogData pageSetupData;
  75:     uint printscale {0};
  76:     bool scaledviewingmode {false};
  77:     bool paintscrolltoselection {true};
  78:     double currentviewscale {1.0};
  79:     bool searchfilter {false};
  80:     int editfilter {0};
  81:     wxDateTime lastmodificationtime;
  82:     map<wxString, uint> tags;
  83:     vector<Cell *> itercells;
  84: 
  85:     #define loopcellsin(par, c) \
  86:         CollectCells(par);      \
  87:         loopv(_i, itercells) for (auto c = itercells[_i]; c; c = nullptr)
  88:     #define loopallcells(c)     \
  89:         CollectCells(root.get()); \
  90:         for (auto c : itercells)
  91:     #define loopallcellssel(c, rec) \
  92:         CollectCellsSel(rec);     \
  93:         for (auto c : itercells)
  94: 
  95:     Document() {
  96:         ResetFont();
  97:         pageSetupData = printData;
  98:         pageSetupData.SetMarginTopLeft(wxPoint(15, 15));
  99:         pageSetupData.SetMarginBottomRight(wxPoint(15, 15));
 100:         dndobjc->Add(dndobjt);
 101:         dndobjc->Add(dndobji);
 102:         dndobjc->Add(dndobjf);
 103:     }
 104: 
 105:     uint Background() { return root ? root->cellcolor : 0xFFFFFF; }
 106: 
 107:     void InitCellSelect(Cell *initialselected, int xsize, int ysize) {
 108:         if (!initialselected) {
 109:             SetSelect(Selection(root->grid, 0, 0, 1, 1));
 110:             return;
 111:         }
 112:         SetSelect(initialselected->parent->grid->FindCell(initialselected));
 113:         selected.xs = xsize;
 114:         selected.ys = ysize;
 115:         sys->frame->UpdateStatus(selected, true);
 116:     }
 117: 
 118:     void InitWith(unique_ptr<Cell> root, const wxString &filename, Cell *initialselected, int xsize, int ysize) {
 119:         this->root = std::move(root);
 120:         InitCellSelect(initialselected, xsize, ysize);
 121:         ChangeFileName(filename, false);
 122:     }
 123: 
 124:     void UpdateFileName(int page = -1) {
 125:         sys->frame->SetPageTitle(filename, modified ? (lastmodsinceautosave ? "*" : "+") : "",
 126:                                  page);
 127:     }
 128: 
 129:     void ChangeFileName(const wxString &newfilename, bool checkext) {
 130:         filename = newfilename;
 131:         if (checkext) {
 132:             wxFileName wxfn(filename);
 133:             if (!wxfn.HasExt()) filename.Append(".cts");
 134:         }
 135:         UpdateFileName();
 136:     }
 137: 
 138:     wxString SaveDB(bool *success, bool istempfile = false, int page = -1) {
 139:         if (filename.empty()) return _("Save cancelled.");
 140:         Cell *ocs = selected.xs == 0 && selected.ys == 0
 141:                         ? nullptr
 142:                         : selected.grid->C(selected.x, selected.y).get();
 143:         auto start_saving_time = wxGetLocalTimeMillis();
 144: 
 145:         auto targetfilename = istempfile ? sys->TmpName(filename) : filename;
 146:         auto savefilename = sys->NewName(targetfilename);
 147: 
 148:         {  // limit destructors
 149:             wxBusyCursor wait;
 150:             wxFFileOutputStream fos(savefilename);
 151:             if (!fos.IsOk()) {
 152:                 if (!istempfile)
 153:                     wxMessageBox(
 154:                         _("Error writing TreeSheets file! (try saving under new filename)."),
 155:                         savefilename.wx_str(), wxOK, sys->frame);
 156:                 return _("Error writing to file.");
 157:             }
 158: 
 159:             wxDataOutputStream sos(fos);
 160:             fos.Write("TSFF", 4);
 161:             char vers = TS_VERSION;
 162:             fos.Write(&vers, 1);
 163:             sos.Write8(selected.xs);
 164:             sos.Write8(selected.ys);
 165:             sos.Write8(ocs ? drawpath.size() : 0);  // zoom level
 166:             RefreshImageRefCount(true);
 167:             int realindex = 0;
 168:             loopv(i, sys->imagelist) {
 169:                 if (auto &image = *sys->imagelist[i]; image.trefc) {
 170:                     fos.PutC(image.type);
 171:                     sos.WriteDouble(image.display_scale);
 172:                     wxInt64 imagelen(image.data.size());
 173:                     sos.Write64(imagelen);
 174:                     fos.Write(image.data.data(), imagelen);
 175:                     image.savedindex = realindex++;
 176:                 }
 177:             }
 178: 
 179:             fos.Write("D", 1);
 180:             wxZlibOutputStream zos(fos, 9);
 181:             if (!zos.IsOk()) return _("Zlib error while writing file.");
 182:             wxDataOutputStream dos(zos);
 183:             root->Save(dos, ocs);
 184:             for (auto &[tag, color] : tags) {
 185:                 dos.WriteString(tag);
 186:                 dos.Write32(color);
 187:             }
 188:             dos.WriteString(wxEmptyString);
 189:         }
 190: 
 191:         if (!istempfile && sys->makebaks && ::wxFileExists(filename)) {
 192:             ::wxRemoveFile(sys->BakName(filename));
 193:             ::wxRenameFile(filename, sys->BakName(filename));
 194:         }
 195: 
 196:         if (!::wxRenameFile(savefilename, targetfilename, true)) {
 197:             return _("Error renaming temporary file.");
 198:         }
 199: 
 200:         lastmodsinceautosave = 0;
 201:         lastsave = wxGetLocalTime();
 202:         auto end_saving_time = wxGetLocalTimeMillis();
 203: 
 204:         if (!istempfile) {
 205:             undolistsizeatfullsave = undolist.size();
 206:             modified = false;
 207:             tmpsavesuccess = true;
 208:             sys->FileUsed(filename, this);
 209:             if (::wxFileExists(sys->TmpName(filename))) ::wxRemoveFile(sys->TmpName(filename));
 210:         }
 211:         if (sys->autohtmlexport) {
 212:             ExportFile(sys->ExtName(filename, ".html"),
 213:                        sys->autohtmlexport == A_AUTOEXPORT_HTML_WITH_IMAGES - A_AUTOEXPORT_HTML_NONE
 214:                            ? A_EXPHTMLTE
 215:                            : A_EXPHTMLT,
 216:                        false);
 217:         }
 218:         UpdateFileName(page);
 219:         if (success) *success = true;
 220:         return wxString::Format(_("Saved %s successfully (in %lld milliseconds)."), filename,
 221:                                 end_saving_time - start_saving_time);
 222:     }
 223: 
 224:     void DrawSelect(wxDC &dc, Selection &s) {
 225:         if (!s.grid) return;
 226:         ResetFont();
 227:         s.grid->DrawSelect(this, dc, s);
 228:     }
 229: 
 230:     void UpdateHover(wxReadOnlyDC &dc, int mx, int my) {
 231:         ResetFont();
 232:         int x, y;
 233:         canvas->CalcUnscrolledPosition(mx, my, &x, &y);
 234:         prev = hover;
 235:         hover = Selection();
 236:         auto drawroot = WalkPath(drawpath);
 237:         if (drawroot->grid)
 238:             drawroot->grid->FindXY(
 239:                 this, x / currentviewscale - centerx / currentviewscale - hierarchysize,
 240:                 y / currentviewscale - centery / currentviewscale - hierarchysize, dc);
 241:     }
 242: 
 243:     void ScrollIfSelectionOutOfView(const Selection &sel, int sx, int sy, int mx, int my) {
 244:         if (!scaledviewingmode) {
 245:             // required, since sizes of things may have been reset by the last editing operation
 246:             int canvasw, canvash;
 247:             canvas->GetClientSize(&canvasw, &canvash);
 248:             if ((layoutys > canvash || layoutxs > canvasw) && sel.grid) {
 249:                 wxRect r = sel.grid->GetRect(this, sel);
 250:                 if (r.y < sy || r.y + r.height > my || r.x < sx || r.x + r.width > mx) {
 251:                     canvas->Scroll(r.width > canvasw || r.x < sx ? r.x
 252:                                    : r.x + r.width > mx             ? r.x + r.width - canvasw
 253:                                                                       : sx,
 254:                                    r.height > canvash || r.y < sy ? r.y
 255:                                    : r.y + r.height > my             ? r.y + r.height - canvash
 256:                                                                        : sy);
 257:                 }
 258:             }
 259:         }
 260:     }
 261: 
 262:     void ScrollOrZoom(bool zoomiftiny = false) {
 263:         if (!selected.grid) return;
 264:         auto drawroot = WalkPath(drawpath);
 265:         // If we jumped to a cell which may be insided a folded cell, we have to unfold it
 266:         // because the rest of the code doesn't deal with a selection that is invisible :)
 267:         for (auto cg = selected.grid->cell; cg; cg = cg->parent) {
 268:             // Unless we're under the drawroot, no need to unfold further.
 269:             if (cg == drawroot) break;
 270:             if (cg->grid->folded) {
 271:                 cg->grid->folded = false;
 272:                 cg->ResetLayout();
 273:                 cg->ResetChildren();
 274:             }
 275:         }
 276:         for (auto cg = selected.grid->cell; cg; cg = cg->parent)
 277:             if (cg == drawroot) {
 278:                 if (zoomiftiny) ZoomTiny();
 279:                 paintscrolltoselection = true;
 280:                 canvas->Refresh();
 281:                 return;
 282:             }
 283:         Zoom(-100, false);
 284:         if (zoomiftiny) ZoomTiny();
 285:     }
 286: 
 287:     void ZoomTiny() {
 288:         if (auto c = selected.GetCell(); c && c->tiny) {
 289:             Zoom(1);  // seems to leave selection box in a weird location?
 290:             if (selected.GetCell() != c) ZoomTiny();
 291:         }
 292:     }
 293: 
 294:     void ResetCursor() {
 295:         if (selected.grid) selected.SetCursorEdit(this, selected.TextEdit());
 296:     }
 297: 
 298:     void SetSelect(const Selection &sel = Selection()) {
 299:         selected = sel;
 300:         begindrag = sel;
 301:     }
 302: 
 303:     void SelectUp() {
 304:         if (!isctrlshiftdrag || isctrlshiftdrag == 3 || begindrag.EqLoc(selected)) return;
 305:         auto cell = selected.GetCell();
 306:         if (!cell) return;
 307:         auto targetcell = begindrag.ThinExpand(this);
 308:         selected = begindrag;
 309:         if (targetcell) {
 310:             auto is_parent = targetcell->IsParentOf(cell);
 311:             auto targetcell_parent = targetcell->parent;  // targetcell may be deleted.
 312:             targetcell->Paste(this, cell, begindrag);
 313:             // If is_parent, cell has been deleted already.
 314:             if (isctrlshiftdrag == 1 && !is_parent) {
 315:                 cell->parent->AddUndo(this);
 316:                 Selection cellselection = cell->parent->grid->FindCell(cell);
 317:                 cell->parent->grid->MultiCellDeleteSub(this, cellselection);
 318:             }
 319:             hover = targetcell_parent ? targetcell_parent->grid->FindCell(targetcell) : Selection();
 320:             SetSelect(hover);
 321:             wxInfoDC dc(canvas);
 322:             Layout(dc);
 323:         }
 324:     }
 325: 
 326:     void DoubleClick() {
 327:         SetSelect(hover);
 328:         if (selected.Thin() && selected.grid) {
 329:             selected.SelAll();
 330:         } else if (Cell *c = selected.GetCell()) {
 331:             selected.EnterEditOnly(this);
 332:             c->text.SelectWord(selected);
 333:             begindrag = selected;
 334:         }
 335:     }
 336: 
 337:     void Drop() {
 338:         switch (dndobjc->GetReceivedFormat().GetType()) {
 339:             case wxDF_BITMAP: PasteOrDrop(*dndobji); break;
 340:             case wxDF_FILENAME: PasteOrDrop(*dndobjf); break;
 341:             case wxDF_TEXT:
 342:             case wxDF_UNICODETEXT: PasteOrDrop(*dndobjt);
 343:             default:;
 344:         }
 345:     }
 346: 
 347:     auto CopyEntireCells(wxString &s, int action) {
 348:         sys->clipboardcopy = s;
 349:         auto html =
 350:             selected.grid->ConvertToText(selected, 0, action == A_COPYWI ? A_EXPHTMLTI : A_EXPHTMLT,
 351:                                          this, false, currentdrawroot);
 352:         return new wxHTMLDataObject(html);
 353:     }
 354: 
 355:     void Copy(int action) {
 356:         auto c = selected.GetCell();
 357:         sys->clipboardcopy = wxEmptyString;
 358: 
 359:         switch (action) {
 360:             case A_DRAGANDDROP: {
 361:                 sys->cellclipboard = c ? c->Clone(nullptr) : selected.grid->CloneSel(selected);
 362:                 wxDataObjectComposite dragdata;
 363:                 if (c && !c->text.t && c->text.image) {
 364:                     auto image = c->text.image;
 365:                     if (!image->data.empty()) {
 366:                         auto &[it, mime] = imagetypes.at(image->type);
 367:                         auto bitmap = ConvertBufferToWxBitmap(image->data, it);
 368:                         dragdata.Add(new wxBitmapDataObject(bitmap));
 369:                     }
 370:                 } else {
 371:                     auto s = selected.grid->ConvertToText(selected, 0, A_EXPTEXT, this, false,
 372:                                                           currentdrawroot);
 373:                     dragdata.Add(new wxTextDataObject(s));
 374:                     if (!selected.TextEdit()) {
 375:                         auto htmlobj = CopyEntireCells(s, wxID_COPY);
 376:                         dragdata.Add(htmlobj);
 377:                     }
 378:                 }
 379:                 wxDropSource dragsource(dragdata, canvas);
 380:                 dragsource.DoDragDrop(true);
 381:                 break;
 382:             }
 383:             case A_COPYCT: {
 384:                 sys->cellclipboard = nullptr;
 385:                 auto clipboardtextdata = new wxDataObjectComposite();
 386:                 wxString s = "";
 387:                 loopallcellssel(c, true) if (c->text.t.Len()) s += c->text.t + " ";
 388:                 if (!selected.TextEdit()) sys->clipboardcopy = s;
 389:                 clipboardtextdata->Add(new wxTextDataObject(s));
 390:                 if (wxTheClipboard->Open()) {
 391:                     wxTheClipboard->SetData(clipboardtextdata);
 392:                     wxTheClipboard->Close();
 393:                 }
 394:                 break;
 395:             }
 396:             case wxID_COPY:
 397:             case A_COPYWI:
 398:             default: {
 399:                 sys->cellclipboard = c ? c->Clone(nullptr) : selected.grid->CloneSel(selected);
 400:                 if (c && !c->text.t && c->text.image) {
 401:                     auto image = c->text.image;
 402:                     if (!image->data.empty() && wxTheClipboard->Open()) {
 403:                         auto &[it, mime] = imagetypes.at(image->type);
 404:                         auto bitmap = ConvertBufferToWxBitmap(image->data, it);
 405:                         wxTheClipboard->SetData(new wxBitmapDataObject(bitmap));
 406:                         wxTheClipboard->Close();
 407:                     }
 408:                 } else {
 409:                     auto clipboarddata = new wxDataObjectComposite();
 410:                     auto s = selected.grid->ConvertToText(selected, 0, A_EXPTEXT, this, false,
 411:                                                           currentdrawroot);
 412:                     clipboarddata->Add(new wxTextDataObject(s));
 413:                     if (!selected.TextEdit()) {
 414:                         auto htmlobj = CopyEntireCells(s, action);
 415:                         clipboarddata->Add(htmlobj);
 416:                     }
 417:                     if (wxTheClipboard->Open()) {
 418:                         wxTheClipboard->SetData(clipboarddata);
 419:                         wxTheClipboard->Close();
 420:                     }
 421:                 }
 422:                 break;
 423:             }
 424:         }
 425:         return;
 426:     }
 427: 
 428:     bool ZoomSetDrawPath(int dir, bool fromroot = true) {
 429:         int oldlen = drawpath.size();
 430:         int targetlen = max(0, (fromroot ? 0 : oldlen) + dir);
 431:         if (!targetlen && drawpath.empty()) return false;
 432:         if (dir > 0) {
 433:             if (!selected.grid) return false;
 434:             auto c = selected.GetCell();
 435:             CreatePath(c && c->grid ? c : selected.grid->cell, drawpath);
 436:         } else if (dir < 0) {
 437:             auto drawroot = WalkPath(drawpath);
 438:             if (drawroot->grid && drawroot->grid->folded)
 439:                 SetSelect(drawroot->parent->grid->FindCell(drawroot));
 440:         }
 441:         int tail = static_cast<int>(drawpath.size()) - targetlen;
 442:         if (tail > 0) drawpath.erase(drawpath.begin(), drawpath.begin() + tail);
 443:         return drawpath.size() != oldlen;
 444:     }
 445: 
 446:     void Zoom(int dir, bool fromroot = false) {
 447:         if (!ZoomSetDrawPath(dir, fromroot)) return;
 448:         auto drawroot = WalkPath(drawpath);
 449:         if (selected.GetCell() == drawroot && drawroot->grid) {
 450:             // We can't have the drawroot selected, so we must move the selection to the children.
 451:             SetSelect(Selection(drawroot->grid, 0, 0, drawroot->grid->xs, drawroot->grid->ys));
 452:         }
 453:         drawroot->ResetLayout();
 454:         drawroot->ResetChildren();
 455:         paintscrolltoselection = true;
 456:         canvas->Refresh();
 457:     }
 458: 
 459:     wxString NoSel() { return _("This operation requires a selection."); }
 460:     wxString OneCell() { return _("This operation works on a single selected cell only."); }
 461:     wxString NoThin() { return _("This operation doesn't work on thin selections."); }
 462:     wxString NoGrid() { return _("This operation requires a cell that contains a grid."); }
 463: 
 464:     wxString Wheel(int dir, bool alt, bool ctrl, bool shift, bool hierarchical = true) {
 465:         if (!dir) return wxEmptyString;
 466:         if (alt) {
 467:             if (!selected.grid) return NoSel();
 468:             if (selected.xs > 0) {
 469:                 if (!LastUndoSameCellAny(selected.grid->cell)) selected.grid->cell->AddUndo(this);
 470:                 selected.grid->ResizeColWidths(dir, selected, hierarchical);
 471:                 selected.grid->cell->ResetLayout();
 472:                 selected.grid->cell->ResetChildren();
 473:                 paintscrolltoselection = true;
 474:                 canvas->Refresh();
 475:                 sys->frame->UpdateStatus(selected, false);
 476:                 return dir > 0 ? _("Column width increased.") : _("Column width decreased.");
 477:             }
 478:             return _("nothing to resize");
 479:         } else if (shift) {
 480:             if (!selected.grid) return NoSel();
 481:             selected.grid->cell->AddUndo(this);
 482:             selected.grid->ResetChildren();
 483:             selected.grid->RelSize(-dir, selected, pathscalebias);
 484:             paintscrolltoselection = true;
 485:             canvas->Refresh();
 486:             return dir > 0 ? _("Text size increased.") : _("Text size decreased.");
 487:         } else if (ctrl) {
 488:             int steps = abs(dir);
 489:             dir = sign(dir);
 490:             loop(i, steps) Zoom(dir);
 491:             return dir > 0 ? _("Zoomed in.") : _("Zoomed out.");
 492:         } else {
 493:             ASSERT(0);
 494:             return wxEmptyString;
 495:         }
 496:     }
 497: 
 498:     void Layout(wxReadOnlyDC &dc) {
 499:         ResetFont();
 500:         dc.SetUserScale(1, 1);
 501:         currentdrawroot = WalkPath(drawpath);
 502:         int psb = currentdrawroot == root.get() ? 0 : currentdrawroot->MinRelsize();
 503:         if (psb < 0 || psb == INT_MAX) psb = 0;
 504:         if (psb != pathscalebias) currentdrawroot->ResetChildren();
 505:         pathscalebias = psb;
 506:         currentdrawroot->LazyLayout(this, dc, 0, currentdrawroot->ColWidth(), false);
 507:         ResetFont();
 508:         PickFont(dc, 0, 0, 0);
 509:         hierarchysize = 0;
 510:         for (Cell *p = currentdrawroot->parent; p; p = p->parent)
 511:             if (p->text.t.Len()) hierarchysize += dc.GetCharHeight();
 512:         hierarchysize += fgutter;
 513:         layoutxs = currentdrawroot->sx + hierarchysize + fgutter;
 514:         layoutys = currentdrawroot->sy + hierarchysize + fgutter;
 515:     }
 516: 
 517:     void ShiftToCenter(wxReadOnlyDC &dc) {
 518:         int dlx = dc.DeviceToLogicalX(0);
 519:         int dly = dc.DeviceToLogicalY(0);
 520:         dc.SetDeviceOrigin(dlx > 0 ? -dlx : centerx, dly > 0 ? -dly : centery);
 521:         dc.SetUserScale(currentviewscale, currentviewscale);
 522:     }
 523: 
 524:     void Render(wxDC &dc) {
 525:         ResetFont();
 526:         PickFont(dc, 0, 0, 0);
 527:         dc.SetTextForeground(*wxLIGHT_GREY);
 528:         int i = 0;
 529:         for (auto p = currentdrawroot->parent; p; p = p->parent)
 530:             if (p->text.t.Len()) {
 531:                 int off = hierarchysize - dc.GetCharHeight() * ++i;
 532:                 auto s = p->text.t;
 533:                 if (static_cast<int>(s.Len()) > sys->defaultmaxcolwidth) {
 534:                     // should take the width of these into account for layoutys, but really, the
 535:                     // worst that can happen on a thin window is that its rendering gets cut off
 536:                     s = s.Left(sys->defaultmaxcolwidth) + "...";
 537:                 }
 538:                 dc.DrawText(s, off, off);
 539:             }
 540:         dc.SetTextForeground(LightColor(0x000000));
 541:         currentdrawroot->Render(this, hierarchysize, hierarchysize, dc, 0, 0, 0, 0, 0,
 542:                                 currentdrawroot->ColWidth(), 0);
 543:     }
 544: 
 545:     void SelectClick(bool right = false) {
 546:         begindrag = Selection();
 547:         if (!(right && hover.IsInside(selected))) {
 548:             if (selected.GetCell() == hover.GetCell() && hover.GetCell())
 549:                 hover.EnterEditOnly(this);
 550:             else
 551:                 hover.ExitEdit(this);
 552:             SetSelect(hover);
 553:         }
 554:     }
 555: 
 556:     void Draw(wxDC &dc) {
 557:         if (!root) return;
 558:         canvas->GetClientSize(&maxx, &maxy);
 559:         Layout(dc);
 560:         dc.SetBackground(wxBrush(LightColor(Background())));
 561:         dc.Clear();
 562:         double xscale = maxx / static_cast<double>(layoutxs);
 563:         double yscale = maxy / static_cast<double>(layoutys);
 564:         currentviewscale = min(xscale, yscale);
 565:         if (currentviewscale > 5)
 566:             currentviewscale = 5;
 567:         else if (currentviewscale < 1)
 568:             currentviewscale = 1;
 569:         if (scaledviewingmode && currentviewscale > 1) {
 570:             dc.SetUserScale(currentviewscale, currentviewscale);
 571:             canvas->SetVirtualSize(maxx, maxy);
 572:             maxx /= currentviewscale;
 573:             maxy /= currentviewscale;
 574:             scrollx = scrolly = 0;
 575:         } else {
 576:             currentviewscale = 1;
 577:             dc.SetUserScale(1, 1);
 578:             canvas->SetVirtualSize(layoutxs, layoutys);
 579:             canvas->GetViewStart(&scrollx, &scrolly);
 580:             maxx += scrollx;
 581:             maxy += scrolly;
 582:         }
 583:         centerx = sys->centered && !scrollx && maxx > layoutxs
 584:                       ? (maxx - layoutxs) / 2 * currentviewscale
 585:                       : 0;
 586:         centery = sys->centered && !scrolly && maxy > layoutys
 587:                       ? (maxy - layoutys) / 2 * currentviewscale
 588:                       : 0;
 589:         ShiftToCenter(dc);
 590:         Render(dc);
 591:         DrawSelect(dc, selected);
 592:         if (paintscrolltoselection) {
 593:             paintscrolltoselection = false;
 594:                 canvas->CallAfter([this, sel = selected, sx = scrollx, sy = scrolly, mx = maxx, my = maxy](){
 595:                     ScrollIfSelectionOutOfView(sel, sx, sy, mx, my);
 596:                     #ifdef __WXMAC__
 597:                         canvas->Refresh();
 598:                     #endif
 599:                 });
 600:         }
 601:         if (scaledviewingmode) { dc.SetUserScale(1, 1); }
 602:     }
 603: 
 604:     void Print(wxDC &dc, wxPrintout &po) {
 605:         Layout(dc);
 606:         maxx = layoutxs;
 607:         maxy = layoutys;
 608:         scrollx = scrolly = 0;
 609:         po.FitThisSizeToPage(printscale ? wxSize(printscale, 1) : wxSize(maxx, maxy));
 610:         wxRect fitRect = po.GetLogicalPageRect();
 611:         wxCoord xoff = (fitRect.width - maxx) / 2;
 612:         wxCoord yoff = (fitRect.height - maxy) / 2;
 613:         po.OffsetLogicalOrigin(xoff, yoff);
 614:         while_printing = true;
 615:         Render(dc);
 616:         while_printing = false;
 617:     }
 618: 
 619:     int TextSize(int depth, int relsize) {
 620:         return max(g_mintextsize(), g_deftextsize - depth - relsize + pathscalebias);
 621:     }
 622: 
 623:     bool FontIsMini(int textsize) { return textsize == g_mintextsize(); }
 624: 
 625:     bool PickFont(wxReadOnlyDC &dc, int depth, int relsize, int stylebits) {
 626:         int textsize = TextSize(depth, relsize);
 627:         if (textsize != lasttextsize || stylebits != laststylebits) {
 628:             wxFont font(textsize - (while_printing || scaledviewingmode),
 629:                         stylebits & STYLE_FIXED ? wxFONTFAMILY_TELETYPE : wxFONTFAMILY_DEFAULT,
 630:                         stylebits & STYLE_ITALIC ? wxFONTSTYLE_ITALIC : wxFONTSTYLE_NORMAL,
 631:                         stylebits & STYLE_BOLD ? wxFONTWEIGHT_BOLD : wxFONTWEIGHT_NORMAL,
 632:                         (stylebits & STYLE_UNDERLINE) != 0,
 633:                         stylebits & STYLE_FIXED ? sys->defaultfixedfont : sys->defaultfont);
 634:             if (stylebits & STYLE_STRIKETHRU) font.SetStrikethrough(true);
 635:             dc.SetFont(font);
 636:             lasttextsize = textsize;
 637:             laststylebits = stylebits;
 638:         }
 639:         return FontIsMini(textsize);
 640:     }
 641: 
 642:     void ResetFont() {
 643:         lasttextsize = INT_MAX;
 644:         laststylebits = -1;
 645:     }
 646: 
 647:     bool CheckForChanges() {
 648:         if (modified) {
 649:             ThreeChoiceDialog tcd(sys->frame, filename,
 650:                                   _("Changes have been made, are you sure you wish to continue?"),
 651:                                   _("Save and Close"), _("Discard Changes"), _("Cancel"));
 652:             switch (tcd.Run()) {
 653:                 case 0: {
 654:                     bool success = false;
 655:                     Save(false, &success);
 656:                     return !success;
 657:                 }
 658:                 case 1: return false;
 659:                 default:
 660:                 case 2: return true;
 661:             }
 662:         }
 663:         return false;
 664:     }
 665: 
 666:     void RemoveTmpFile() {
 667:         if (!filename.empty() && ::wxFileExists(sys->TmpName(filename)))
 668:             ::wxRemoveFile(sys->TmpName(filename));
 669:     }
 670: 
 671:     bool CloseDocument() {
 672:         bool keep = CheckForChanges();
 673:         if (!keep) RemoveTmpFile();
 674:         return keep;
 675:     }
 676: 
 677:     wxString Export(const wxString &fmt, const wxString &pat, const wxString &message, int action) {
 678:         wxFileName tsfn(filename);
 679:         auto exportfilename = ::wxFileSelector(message, tsfn.GetPath(), tsfn.GetName(), fmt, pat,
 680:                                                wxFD_SAVE | wxFD_OVERWRITE_PROMPT | wxFD_CHANGE_DIR);
 681:         if (exportfilename.empty()) return _("Export cancelled.");
 682:         wxFileName expfn(exportfilename);
 683:         if (!expfn.HasExt()) {
 684:             expfn.SetExt(fmt);
 685:             exportfilename = expfn.GetFullPath();
 686:         }
 687:         return ExportFile(exportfilename, action, true);
 688:     }
 689: 
 690:     wxBitmap GetBitmap() {
 691:         maxx = layoutxs;
 692:         maxy = layoutys;
 693:         scrollx = scrolly = 0;
 694:         wxBitmap bm(maxx, maxy, 24);
 695:         wxMemoryDC mdc(bm);
 696:         DrawView(mdc);
 697:         return bm;
 698:     }
 699: 
 700:     bool DrawSVG(const wxString &filename) {
 701:         maxx = layoutxs;
 702:         maxy = layoutys;
 703:         scrollx = scrolly = 0;
 704:         wxSVGFileDC sdc(filename, maxx, maxy);
 705:         sdc.SetBitmapHandler(new wxSVGBitmapEmbedHandler());
 706:         DrawView(sdc);
 707:         return sdc.IsOk();
 708:     }
 709: 
 710:     void DrawView(wxDC &dc) {
 711:         DrawRectangle(dc, Background(), 0, 0, maxx, maxy);
 712:         Layout(dc);
 713:         Render(dc);
 714:     }
 715: 
 716:     wxBitmap GetSubBitmap(const Selection &sel) {
 717:         wxRect r = sel.grid->GetRect(this, sel, true);
 718:         return GetBitmap().GetSubBitmap(r);
 719:     }
 720: 
 721:     void RefreshImageRefCount(bool includefolded) {
 722:         loopv(i, sys->imagelist) sys->imagelist[i]->trefc = 0;
 723:         root->ImageRefCount(includefolded);
 724:     }
 725: 
 726:     wxString ExportFile(const wxString &filename, int action, bool currentview) {
 727:         Cell *exportroot = currentview ? currentdrawroot : root.get();
 728:         if (action == A_EXPIMAGE) {
 729:             auto bitmap = GetBitmap();
 730:             canvas->Refresh();
 731:             if (!bitmap.SaveFile(filename, wxBITMAP_TYPE_PNG)) return _("Error writing PNG file!");
 732:         } else if (action == A_EXPSVG) {
 733:             if (!DrawSVG(filename)) return _("Error exporting to SVG file!");
 734:             canvas->Refresh();
 735:         } else {
 736:             wxFFileOutputStream fos(filename, "w+b");
 737:             if (!fos.IsOk()) {
 738:                 wxMessageBox(_("Error exporting file!"), filename.wx_str(), wxOK, sys->frame);
 739:                 return _("Error writing to file!");
 740:             }
 741:             wxTextOutputStream dos(fos);
 742:             wxString content = exportroot->ToText(0, Selection(), action, this, true, exportroot);
 743:             switch (action) {
 744:                 case A_EXPXML:
 745:                     dos.WriteString(
 746:                         "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n"
 747:                         "<!DOCTYPE cell [\n"
 748:                         "<!ELEMENT cell (grid)>\n"
 749:                         "<!ELEMENT grid (row*)>\n"
 750:                         "<!ELEMENT row (cell*)>\n"
 751:                         "]>\n");
 752:                     dos.WriteString(content);
 753:                     break;
 754:                 case A_EXPHTMLT:
 755:                 case A_EXPHTMLTI:
 756:                 case A_EXPHTMLTE:
 757:                 case A_EXPHTMLB:
 758:                 case A_EXPHTMLO: {
 759:                     wxString output;
 760:                     output
 761:                         << "<!DOCTYPE html>\n"
 762:                         << "<html>\n<head>\n<style>\n"
 763:                         << "body { font-family: '" << sys->defaultfont << "', sans-serif; }\n"
 764:                         << "table, th, td { border: 1px solid #A0A0A0; border-collapse: collapse;"
 765:                         << " padding: 3px; vertical-align: top; }\n"
 766:                         << "@media (prefers-color-scheme: dark) {\n"
 767:                         << "  html { filter: invert(1); }\n"
 768:                         << "  img { filter: invert(1); }\n"
 769:                         << "}\n"
 770:                         << "li { }\n</style>\n"
 771:                         << "<title>export of TreeSheets file " << this->filename
 772:                         << "</title>\n<meta charset=\"UTF-8\" />\n"
 773:                         << "</head>\n<body style=\""
 774:                         << wxString::Format("background-color: #%06X;", SwapColor(root->cellcolor))
 775:                         << "\">" << content << "</body>\n</html>\n";
 776:                     dos.WriteString(output);
 777:                     break;
 778:                 }
 779:                 case A_EXPCSV:
 780:                 case A_EXPTEXT: dos.WriteString(content); break;
 781:             }
 782:             if (action == A_EXPHTMLTE) ExportAllImages(filename, exportroot);
 783:         }
 784:         return _("File exported successfully.");
 785:     }
 786: 
 787:     wxString Save(bool saveas, bool *success = nullptr) {
 788:         if (!saveas && !filename.empty()) { return SaveDB(success); }
 789:         auto filename = ::wxFileSelector(_("Choose TreeSheets file to save:"), "", "", "cts",
 790:                                          _("TreeSheets Files (*.cts)|*.cts|All Files (*.*)|*.*"),
 791:                                          wxFD_SAVE | wxFD_OVERWRITE_PROMPT | wxFD_CHANGE_DIR);
 792:         if (filename.empty()) return _("Save cancelled.");  // avoid name being set to ""
 793:         ChangeFileName(filename, true);
 794:         return SaveDB(success);
 795:     }
 796: 
 797:     void AutoSave(bool minimized, int page) {
 798:         if (sys->autosave && tmpsavesuccess && !filename.empty() && lastmodsinceautosave &&
 799:             (lastmodsinceautosave + 60 < wxGetLocalTime() || lastsave + 300 < wxGetLocalTime() ||
 800:              minimized)) {
 801:             tmpsavesuccess = false;
 802:             SaveDB(&tmpsavesuccess, true, page);
 803:         }
 804:     }
 805: 
 806:     wxString Key(int uk, int k, bool alt, bool ctrl, bool shift, bool &unprocessed) {
 807:         if (uk == WXK_NONE || k < ' ' && k || k == WXK_DELETE) {
 808:             switch (k) {
 809:                 case WXK_BACK:  // no menu shortcut available in wxwidgets
 810:                     if (!ctrl) return Action(A_BACKSPACE);
 811:                     break;  // Prevent Ctrl+H from being treated as Backspace
 812:                 case WXK_RETURN:
 813:                     return Action(shift  ? A_ENTERGRID
 814:                                   : ctrl ? A_ENTERCELL_JUMPTOSTART
 815:                                          : A_ENTERCELL);
 816:                 case WXK_ESCAPE:  // docs say it can be used as a menu accelerator, but it does not
 817:                                   // trigger from there?
 818:                     return Action(A_CANCELEDIT);
 819:                 #ifdef WIN32  // works fine on Linux, not sure OS X
 820:                 case WXK_PAGEDOWN: canvas->CursorScroll(0, g_scrollratecursor); return wxEmptyString;
 821:                 case WXK_PAGEUP: canvas->CursorScroll(0, -g_scrollratecursor); return wxEmptyString;
 822:                 #endif
 823:                 #ifdef __WXGTK__
 824:                 // Due to limitations within GTK, wxGTK does not support specific keycodes 
 825:                 // as accelerator keys for menu items. See wxWidgets documentation for the 
 826:                 // wxMenuItem class in order to obtain more details. This is why we implement 
 827:                 // the missing handling of these accelerator keys in the following section.
 828:                 // Please be aware that the custom implementation has the downside of these
 829:                 // "accelerator keys" being suppressed in the menu items on wxGTK.
 830:                     case WXK_DELETE: return Action(A_DELETE);
 831:                     case WXK_LEFT:
 832:                         return Action(shift ? (ctrl ? A_SCLEFT : A_SLEFT)
 833:                                             : (ctrl ? A_MLEFT : A_LEFT));
 834:                     case WXK_RIGHT:
 835:                         return Action(shift ? (ctrl ? A_SCRIGHT : A_SRIGHT)
 836:                                             : (ctrl ? A_MRIGHT : A_RIGHT));
 837:                     case WXK_UP:
 838:                         return Action(shift ? (ctrl ? A_SCUP : A_SUP) : (ctrl ? A_MUP : A_UP));
 839:                     case WXK_DOWN:
 840:                         return Action(shift ? (ctrl ? A_SCDOWN : A_SDOWN)
 841:                                             : (ctrl ? A_MDOWN : A_DOWN));
 842:                     case WXK_HOME:
 843:                         return Action(shift ? (ctrl ? A_SHOME : A_SHOME)
 844:                                             : (ctrl ? A_CHOME : A_HOME));
 845:                     case WXK_END:
 846:                         return Action(shift ? (ctrl ? A_SEND : A_SEND) : (ctrl ? A_CEND : A_END));
 847:                     case WXK_TAB:
 848:                         if (ctrl && !shift) {
 849:                             // WXK_CONTROL_I (italics) arrives as the same keycode as WXK_TAB + ctrl
 850:                             // on Linux?? They're both keycode 9 in defs.h We ignore it here, such
 851:                             // that CTRL+I works, but it means only CTRL+SHIFT+TAB works on Linux as
 852:                             // a way to switch tabs.
 853:                             // Also, even though we ignore CTRL+TAB, and it is not assigned in the
 854:                             // menus, it still has the
 855:                             // effect of de-selecting
 856:                             // the current tab (requires a click to re-activate). FIXME??
 857:                             break;
 858:                         }
 859:                         return Action(shift ? (ctrl ? A_PREVFILE : A_PREV)
 860:                                             : (ctrl ? A_NEXTFILE : A_NEXT));
 861:                     case WXK_PAGEUP:
 862:                         if (ctrl) return Action(alt ? A_INCWIDTHNH : A_ZOOMIN);
 863:                         if (shift) return Action(A_INCSIZE);
 864:                         if (!alt) canvas->CursorScroll(0, -g_scrollratecursor);
 865:                         return wxEmptyString;
 866:                     case WXK_PAGEDOWN:
 867:                         if (ctrl) return Action(alt ? A_DECWIDTHNH : A_ZOOMOUT);
 868:                         if (shift) return Action(A_DECSIZE);
 869:                         if (!alt) canvas->CursorScroll(0, g_scrollratecursor);
 870:                         return wxEmptyString;
 871:                 #endif
 872:             }
 873:         } else if (uk >= ' ') {
 874:             if (!selected.grid) return NoSel();
 875:             auto c = selected.ThinExpand(this);
 876:             if (!c) {
 877:                 selected.Wrap(this);
 878:                 c = selected.GetCell();
 879:             }
 880:             c->AddUndo(this);  // FIXME: not needed for all keystrokes, or at least, merge all
 881:                                // keystroke undos within same cell
 882:             c->text.Key(this, uk, selected);
 883:             paintscrolltoselection = true;
 884:             canvas->Refresh();
 885:             canvas->Update();
 886:             return wxEmptyString;
 887:         }
 888:         unprocessed = true;
 889:         return wxEmptyString;
 890:     }
 891: 
 892:     wxString Action(int action) {
 893:         switch (action) {
 894:             case wxID_EXECUTE:
 895:                 root->AddUndo(this);
 896:                 sys->evaluator.Eval(root.get());
 897:                 root->ResetChildren();
 898:                 selected = Selection();
 899:                 begindrag = Selection();
 900:                 canvas->Refresh();
 901:                 return _("Evaluation finished.");
 902: 
 903:             case wxID_UNDO:
 904:                 if (undolist.size()) {
 905:                     Undo(undolist, redolist);
 906:                     return wxEmptyString;
 907:                 } else {
 908:                     return _("Nothing more to undo.");
 909:                 }
 910: 
 911:             case wxID_REDO:
 912:                 if (redolist.size()) {
 913:                     Undo(redolist, undolist, true);
 914:                     return wxEmptyString;
 915:                 } else {
 916:                     return _("Nothing more to redo.");
 917:                 }
 918: 
 919:             case wxID_SAVE: return Save(false);
 920:             case wxID_SAVEAS: return Save(true);
 921:             case A_SAVEALL: sys->SaveAll(); return wxEmptyString;
 922: 
 923:             case A_EXPXML: return Export("xml", "*.xml", _("Choose XML file to write"), action);
 924:             case A_EXPHTMLT:
 925:             case A_EXPHTMLTE:
 926:             case A_EXPHTMLB:
 927:             case A_EXPHTMLO:
 928:                 return Export("html", "*.html", _("Choose HTML file to write"), action);
 929:             case A_EXPTEXT: return Export("txt", "*.txt", _("Choose Text file to write"), action);
 930:             case A_EXPIMAGE: return Export("png", "*.png", _("Choose PNG file to write"), action);
 931:             case A_EXPSVG: return Export("svg", "*.svg", _("Choose SVG file to write"), action);
 932:             case A_EXPCSV: {
 933:                 int maxdepth = 0, leaves = 0;
 934:                 currentdrawroot->MaxDepthLeaves(0, maxdepth, leaves);
 935:                 if (maxdepth > 1)
 936:                     return _(
 937:                         "Cannot export grid that is not flat (zoom the view to the desired grid, and/or use Flatten).");
 938:                 return Export("csv", "*.csv", _("Choose CSV file to write"), action);
 939:             }
 940: 
 941:             case A_IMPXML:
 942:             case A_IMPXMLA:
 943:             case A_IMPTXTI:
 944:             case A_IMPTXTC:
 945:             case A_IMPTXTS:
 946:             case A_IMPTXTT: {
 947:                 wxArrayString filenames;
 948:                 GetFilesFromUser(filenames, sys->frame, _("Please select file(s) to import:"),
 949:                                  _("*.*"));
 950:                 wxString message = "";
 951:                 for (auto &filename : filenames) message = sys->Import(filename, action);
 952:                 return message;
 953:             }
 954: 
 955:             case wxID_OPEN: {
 956:                 wxArrayString filenames;
 957:                 GetFilesFromUser(filenames, sys->frame,
 958:                                  _("Please select TreeSheets file(s) to load:"),
 959:                                  _("TreeSheets Files (*.cts)|*.cts|All Files (*.*)|*.*"));
 960:                 wxString message = "";
 961:                 for (auto &filename : filenames) message = sys->Open(filename);
 962:                 return message;
 963:             }
 964: 
 965:             case wxID_CLOSE: {
 966:                 if (sys->frame->notebook->GetPageCount() <= 1) {
 967:                     sys->frame->fromclosebox = false;
 968:                     sys->frame->Close();
 969:                     return wxEmptyString;
 970:                 }
 971: 
 972:                 if (!CloseDocument()) {
 973:                     int pagenumber = sys->frame->notebook->GetSelection();
 974:                     // sys->frame->notebook->AdvanceSelection();
 975:                     sys->frame->notebook->DeletePage(pagenumber);
 976:                 }
 977:                 return wxEmptyString;
 978:             }
 979: 
 980:             case wxID_NEW: {
 981:                 int size = static_cast<int>(
 982:                     ::wxGetNumberFromUser(_("What size grid would you like to start with?"),
 983:                                           _("size:"), _("New Sheet"), 10, 1, 25, sys->frame));
 984:                 if (size < 0) return _("New file cancelled.");
 985:                 sys->InitDB(size);
 986:                 sys->frame->GetCurrentTab()->Refresh();
 987:                 return wxEmptyString;
 988:             }
 989: 
 990:             case wxID_ABOUT: {
 991:                 wxAboutDialogInfo info;
 992:                 info.SetName("TreeSheets");
 993:                 info.SetVersion(wxT(PACKAGE_VERSION));
 994:                 info.SetCopyright("(C) 2026 Wouter van Oortmerssen and Tobias Predel");
 995:                 auto desc = wxString::Format("%s\n\n%s " wxVERSION_STRING,
 996:                                              _("The Free Form Hierarchical Information Organizer"),
 997:                                              _("Uses"));
 998:                 info.SetDescription(desc);
 999:                 wxAboutBox(info);
1000:                 return wxEmptyString;
1001:             }
1002: 
1003:             case wxID_HELP: sys->LoadTutorial(); return wxEmptyString;
1004: 
1005:             case A_HELP_OP_REF: sys->LoadOpRef(); return wxEmptyString;
1006: 
1007:             case A_TUTORIALWEBPAGE: {
1008:                 wxTranslations *trans = wxTranslations::Get();
1009:                 wxString lang = trans ? trans->GetBestTranslation("ts") : wxString("");
1010:                 wxString tutorialpath;
1011: 
1012:                 for (const wxString &suffix : {"-" + lang, "-" + lang.Left(2), wxString("")}) {
1013:                     if (suffix.Length() == 1) continue;
1014: 
1015:                     wxString candidate =
1016:                         sys->frame->app->GetDocPath("docs/tutorial" + suffix + ".html");
1017:                     if (::wxFileExists(candidate)) {
1018:                         tutorialpath = candidate;
1019:                         break;
1020:                     }
1021:                 }
1022: 
1023:                 if (!tutorialpath.IsEmpty()) {
1024:                     #ifdef __WXMAC__
1025:                         wxLaunchDefaultBrowser("file://" + tutorialpath);
1026:                     #else
1027:                         wxLaunchDefaultBrowser(tutorialpath);
1028:                     #endif
1029:                     return wxEmptyString;
1030:                 } else {
1031:                     return _("Tutorial web page could not be found.");
1032:                 }
1033:             }
1034: 
1035:             #ifdef ENABLE_LOBSTER
1036:                 case A_SCRIPTREFERENCE:
1037:                     #ifdef __WXMAC__
1038:                     wxLaunchDefaultBrowser(
1039:                         "file://" +
1040:                         sys->frame->app->GetDocPath("docs/script_reference.html"));  // RbrtPntn
1041:                     #else
1042:                     wxLaunchDefaultBrowser(
1043:                         sys->frame->app->GetDocPath("docs/script_reference.html"));
1044:                     #endif
1045:                         return wxEmptyString;
1046:             #endif
1047: 
1048:             case A_ZOOMIN:
1049:                 return Wheel(1, false, true,
1050:                              false);  // Zoom( 1, dc); return "zoomed in (menu)";
1051:             case A_ZOOMOUT:
1052:                 return Wheel(-1, false, true,
1053:                              false);  // Zoom(-1, dc); return "zoomed out (menu)";
1054:             case A_INCSIZE: return Wheel(1, false, false, true);
1055:             case A_DECSIZE: return Wheel(-1, false, false, true);
1056:             case A_INCWIDTH: return Wheel(1, true, false, false);
1057:             case A_DECWIDTH: return Wheel(-1, true, false, false);
1058:             case A_INCWIDTHNH: return Wheel(1, true, false, false, false);
1059:             case A_DECWIDTHNH: return Wheel(-1, true, false, false, false);
1060: 
1061:             case wxID_SELECT_FONT:
1062:             case A_SET_FIXED_FONT: {
1063:                 wxFontData fdat;
1064:                 fdat.SetInitialFont(wxFont(
1065:                     g_deftextsize,
1066:                     action == wxID_SELECT_FONT ? wxFONTFAMILY_DEFAULT : wxFONTFAMILY_TELETYPE,
1067:                     wxFONTSTYLE_NORMAL, wxFONTWEIGHT_NORMAL, false,
1068:                     action == wxID_SELECT_FONT ? sys->defaultfont : sys->defaultfixedfont));
1069:                 if (wxFontDialog fd(sys->frame, fdat); fd.ShowModal() == wxID_OK) {
1070:                     wxFont font = fd.GetFontData().GetChosenFont();
1071:                     g_deftextsize = min(20, max(10, font.GetPointSize()));
1072:                     sys->cfg->Write("defaultfontsize", g_deftextsize);
1073:                     switch (action) {
1074:                         case wxID_SELECT_FONT:
1075:                             sys->defaultfont = font.GetFaceName();
1076:                             sys->cfg->Write("defaultfont", sys->defaultfont);
1077:                             break;
1078:                         case A_SET_FIXED_FONT:
1079:                             sys->defaultfixedfont = font.GetFaceName();
1080:                             sys->cfg->Write("defaultfixedfont", sys->defaultfixedfont);
1081:                             break;
1082:                     }
1083:                     // root->ResetChildren();
1084:                     sys->frame->TabsReset();  // ResetChildren on all
1085:                     canvas->Refresh();
1086:                 }
1087:                 return wxEmptyString;
1088:             }
1089: 
1090:             case wxID_PRINT: {
1091:                 wxPrintDialogData printDialogData(printData);
1092:                 wxPrinter printer(&printDialogData);
1093:                 Printout printout(this);
1094:                 if (printer.Print(sys->frame, &printout, true)) {
1095:                     printData = printer.GetPrintDialogData().GetPrintData();
1096:                 }
1097:                 return wxEmptyString;
1098:             }
1099: 
1100:             case A_PRINTSCALE: {
1101:                 printscale = (uint)::wxGetNumberFromUser(
1102:                     _("How many pixels wide should a page be? (0 for auto fit)"), _("scale:"),
1103:                     _("Set Print Scale"), 0, 0, 5000, sys->frame);
1104:                 return wxEmptyString;
1105:             }
1106: 
1107:             case wxID_PREVIEW: {
1108:                 wxPrintDialogData printDialogData(printData);
1109:                 auto preview =
1110:                     new wxPrintPreview(new Printout(this), new Printout(this), &printDialogData);
1111:                 auto pframe = new wxPreviewFrame(preview, sys->frame, _("Print Preview"),
1112:                                                  wxPoint(100, 100), wxSize(600, 650));
1113:                 pframe->Centre(wxBOTH);
1114:                 pframe->Initialize();
1115:                 pframe->Show(true);
1116:                 return wxEmptyString;
1117:             }
1118: 
1119:             case A_PAGESETUP: {
1120:                 pageSetupData = printData;
1121:                 wxPageSetupDialog pageSetupDialog(sys->frame, &pageSetupData);
1122:                 pageSetupDialog.ShowModal();
1123:                 printData = pageSetupDialog.GetPageSetupDialogData().GetPrintData();
1124:                 pageSetupData = pageSetupDialog.GetPageSetupDialogData();
1125:                 return wxEmptyString;
1126:             }
1127: 
1128:             case A_NEXTFILE: sys->frame->CycleTabs(1); return wxEmptyString;
1129:             case A_PREVFILE: sys->frame->CycleTabs(-1); return wxEmptyString;
1130: 
1131:             case A_DEFBGCOL: {
1132:                 auto oldbg = Background();
1133:                 if (auto color = PickColor(sys->frame, oldbg); color != (uint)-1) {
1134:                     root->AddUndo(this);
1135:                     loopallcells(c) {
1136:                         if (c->cellcolor == oldbg && (!c->parent || c->parent->cellcolor == color))
1137:                             c->cellcolor = color;
1138:                     }
1139:                     canvas->Refresh();
1140:                 }
1141:                 return wxEmptyString;
1142:             }
1143: 
1144:             case A_DEFCURCOL: {
1145:                 if (auto color = PickColor(sys->frame, sys->cursorcolor); color != (uint)-1) {
1146:                     sys->cfg->Write("cursorcolor", sys->cursorcolor = color);
1147:                     canvas->Refresh();
1148:                 }
1149:                 return wxEmptyString;
1150:             }
1151: 
1152:             case A_SEARCHNEXT:
1153:             case A_SEARCHPREV: {
1154:                 if (sys->searchstring.Len()) return SearchNext(false, true, action == A_SEARCHPREV);
1155:                 if (auto c = selected.GetCell()) {
1156:                     auto s = c->text.ToText(0, selected, A_EXPTEXT);
1157:                     if (!s.Len()) return _("No text to search for.");
1158:                     sys->frame->filter->SetFocus();
1159:                     sys->frame->filter->SetValue(s);
1160:                     return wxEmptyString;
1161:                 } else {
1162:                     return _("You need to select one cell if you want to search for its text.");
1163:                 }
1164:             }
1165: 
1166:             case A_CASESENSITIVESEARCH: {
1167:                 sys->casesensitivesearch = !(sys->casesensitivesearch);
1168:                 sys->cfg->Write("casesensitivesearch", sys->casesensitivesearch);
1169:                 sys->searchstring = (sys->casesensitivesearch)
1170:                                         ? sys->frame->filter->GetValue()
1171:                                         : sys->frame->filter->GetValue().Lower();
1172:                 auto message = SearchNext(false, false, false);
1173:                 canvas->Refresh();
1174:                 return message;
1175:             }
1176: 
1177:             case A_ROUND0:
1178:             case A_ROUND1:
1179:             case A_ROUND2:
1180:             case A_ROUND3:
1181:             case A_ROUND4:
1182:             case A_ROUND5:
1183:             case A_ROUND6:
1184:                 sys->cfg->Write("roundness", long(sys->roundness = action - A_ROUND0));
1185:                 canvas->Refresh();
1186:                 return wxEmptyString;
1187: 
1188:             case A_OPENCELLCOLOR:
1189:                 if (sys->frame->cellcolordropdown) sys->frame->cellcolordropdown->ShowPopup();
1190:                 break;
1191:             case A_OPENTEXTCOLOR:
1192:                 if (sys->frame->textcolordropdown) sys->frame->textcolordropdown->ShowPopup();
1193:                 break;
1194:             case A_OPENBORDCOLOR:
1195:                 if (sys->frame->bordercolordropdown) sys->frame->bordercolordropdown->ShowPopup();
1196:                 break;
1197:             case A_OPENIMGDROPDOWN:
1198:                 if (sys->frame->imagedropdown) sys->frame->imagedropdown->ShowPopup();
1199:                 break;
1200: 
1201:             case A_REPLACEONCE:
1202:             case A_REPLACEONCEJ:
1203:             case A_REPLACEALL: {
1204:                 if (!sys->searchstring.Len()) return _("No search.");
1205:                 auto replaces = sys->frame->replaces->GetValue();
1206:                 auto lreplaces =
1207:                     sys->casesensitivesearch ? (wxString)wxEmptyString : replaces.Lower();
1208:                 if (action == A_REPLACEALL) {
1209:                     root->AddUndo(this);  // expensive?
1210:                     root->FindReplaceAll(replaces, lreplaces);
1211:                     root->ResetChildren();
1212:                     canvas->Refresh();
1213:                 } else {
1214:                     loopallcellssel(c, true) if (c->text.IsInSearch()) c->AddUndo(this);
1215:                     selected.grid->ReplaceStr(this, replaces, lreplaces, selected);
1216:                     if (action == A_REPLACEONCEJ) return SearchNext(false, true, false);
1217:                 }
1218:                 return _("Text has been replaced.");
1219:             }
1220: 
1221:             case A_CLEARREPLACE: {
1222:                 sys->frame->replaces->Clear();
1223:                 canvas->SetFocus();
1224:                 return wxEmptyString;
1225:             }
1226: 
1227:             case A_CLEARSEARCH: {
1228:                 sys->frame->filter->Clear();
1229:                 canvas->SetFocus();
1230:                 return wxEmptyString;
1231:             }
1232: 
1233:             case A_SCALED:
1234:                 scaledviewingmode = !scaledviewingmode;
1235:                 root->ResetChildren();
1236:                 canvas->Refresh();
1237:                 return scaledviewingmode ? _("Now viewing TreeSheet to fit to the screen exactly, press F12 to return to normal.")
1238:                                          : _("1:1 scale restored.");
1239: 
1240:             case A_FILTERRANGE: {
1241:                 DateTimeRangeDialog rd(sys->frame);
1242:                 if (rd.Run() == wxID_OK) ApplyEditRangeFilter(rd.begin, rd.end);
1243:                 return wxEmptyString;
1244:             }
1245: 
1246:             case A_FILTER5:
1247:                 editfilter = 5;
1248:                 ApplyEditFilter();
1249:                 return wxEmptyString;
1250:             case A_FILTER10:
1251:                 editfilter = 10;
1252:                 ApplyEditFilter();
1253:                 return wxEmptyString;
1254:             case A_FILTER20:
1255:                 editfilter = 20;
1256:                 ApplyEditFilter();
1257:                 return wxEmptyString;
1258:             case A_FILTER50:
1259:                 editfilter = 50;
1260:                 ApplyEditFilter();
1261:                 return wxEmptyString;
1262:             case A_FILTERM:
1263:                 editfilter++;
1264:                 ApplyEditFilter();
1265:                 return wxEmptyString;
1266:             case A_FILTERL:
1267:                 editfilter--;
1268:                 ApplyEditFilter();
1269:                 return wxEmptyString;
1270:             case A_FILTERS: SetSearchFilter(true); return wxEmptyString;
1271:             case A_FILTEROFF: SetSearchFilter(false); return wxEmptyString;
1272: 
1273:             case A_CUSTKEY: {
1274:                 wxArrayString strs, keys;
1275:                 for (auto &[s, k] : sys->frame->menustrings) {
1276:                     strs.push_back(s);
1277:                     keys.push_back(k);
1278:                 }
1279:                 wxSingleChoiceDialog choice(
1280:                     sys->frame, _("Please pick a menu item to change the key binding for"),
1281:                     _("Key binding"), strs);
1282:                 choice.SetSize(wxSize(500, 700));
1283:                 choice.Centre();
1284:                 if (choice.ShowModal() == wxID_OK) {
1285:                     int sel = choice.GetSelection();
1286:                     wxTextEntryDialog textentry(sys->frame,
1287:                                                 _("Please enter the new key binding string"),
1288:                                                 _("Key binding"), keys[sel]);
1289:                     if (textentry.ShowModal() == wxID_OK) {
1290:                         auto key = textentry.GetValue();
1291:                         sys->frame->menustrings[strs[sel]] = key;
1292:                         sys->cfg->Write(strs[sel], key);
1293:                         return _("NOTE: key binding will take effect next run of TreeSheets.");
1294:                     }
1295:                 }
1296:                 return _("Keybinding cancelled.");
1297:             }
1298: 
1299:             case A_SETLANG: {
1300:                 auto trans = wxTranslations::Get();
1301:                 if (!trans) return _("Failed to get translation.");
1302:                 wxArrayString langs = trans->GetAvailableTranslations("ts");
1303:                 langs.Insert(wxEmptyString, 0);
1304:                 wxSingleChoiceDialog choice(
1305:                     sys->frame, _("Please select the language for the interface (requires restart). Please select the empty row if you want to use the default language."),
1306:                     _("Available languages"), langs);
1307:                 if (choice.ShowModal() == wxID_OK) {
1308:                     sys->cfg->Write("defaultlang", choice.GetStringSelection());
1309:                 }
1310:                 return wxEmptyString;
1311:             }
1312:         }
1313: 
1314:         if (!selected.grid) return NoSel();
1315: 
1316:         auto cell = selected.GetCell();
1317: 
1318:         switch (action) {
1319:             case A_BACKSPACE:
1320:                 if (selected.Thin()) {
1321:                     if (selected.xs)
1322:                         DelRowCol(selected.y, 0, selected.grid->ys, 1, -1, selected.y - 1, 0, -1);
1323:                     else
1324:                         DelRowCol(selected.x, 0, selected.grid->xs, 1, selected.x - 1, -1, -1, 0);
1325:                 } else if (cell && selected.TextEdit()) {
1326:                     if (selected.cursorend == 0) return wxEmptyString;
1327:                     cell->AddUndo(this);
1328:                     cell->text.Backspace(selected);
1329:                     canvas->Refresh();
1330:                 } else {
1331:                     selected.grid->MultiCellDelete(this, selected);
1332:                     SetSelect(selected);
1333:                 }
1334:                 ZoomOutIfNoGrid();
1335:                 return wxEmptyString;
1336: 
1337:             case A_DELETE:
1338:                 if (selected.Thin()) {
1339:                     if (selected.xs)
1340:                         DelRowCol(selected.y, selected.grid->ys, selected.grid->ys, 0, -1,
1341:                                   selected.y, 0, -1);
1342:                     else
1343:                         DelRowCol(selected.x, selected.grid->xs, selected.grid->xs, 0, selected.x,
1344:                                   -1, -1, 0);
1345:                 } else if (cell && selected.TextEdit()) {
1346:                     if (selected.cursor == cell->text.t.Len()) return wxEmptyString;
1347:                     cell->AddUndo(this);
1348:                     cell->text.Delete(selected);
1349:                     canvas->Refresh();
1350:                 } else {
1351:                     selected.grid->MultiCellDelete(this, selected);
1352:                     SetSelect(selected);
1353:                 }
1354:                 ZoomOutIfNoGrid();
1355:                 return wxEmptyString;
1356: 
1357:             case A_DELETE_WORD:
1358:                 if (cell && selected.TextEdit()) {
1359:                     if (selected.cursor == cell->text.t.Len()) return wxEmptyString;
1360:                     cell->AddUndo(this);
1361:                     cell->text.DeleteWord(selected);
1362:                     canvas->Refresh();
1363:                 }
1364:                 ZoomOutIfNoGrid();
1365:                 return wxEmptyString;
1366: 
1367:             case wxID_CUT:
1368:             case wxID_COPY:
1369:             case A_COPYWI:
1370:             case A_COPYCT:
1371:                 if (selected.Thin()) return NoThin();
1372:                 if (selected.TextEdit()) {
1373:                     if (selected.cursor == selected.cursorend) return _("No text selected.");
1374:                 }
1375:                 Copy(action);
1376:                 if (action == wxID_CUT) {
1377:                     if (!selected.TextEdit()) {
1378:                         selected.grid->cell->AddUndo(this);
1379:                         selected.grid->MultiCellDelete(this, selected);
1380:                         SetSelect(selected);
1381:                     } else if (cell) {
1382:                         cell->AddUndo(this);
1383:                         cell->text.Backspace(selected);
1384:                     }
1385:                     canvas->Refresh();
1386:                 }
1387:                 ZoomOutIfNoGrid();
1388:                 return wxEmptyString;
1389: 
1390:             case A_COPYBM:
1391:                 if (wxTheClipboard->Open()) {
1392:                     wxTheClipboard->SetData(new wxBitmapDataObject(GetSubBitmap(selected)));
1393:                     wxTheClipboard->Close();
1394:                     return _("Bitmap copied to clipboard");
1395:                 }
1396:                 return wxEmptyString;
1397: 
1398:             case A_COLLAPSE: {
1399:                 if (selected.xs * selected.ys == 1)
1400:                     return _("More than one cell must be selected.");
1401:                 auto fc = selected.GetFirst();
1402:                 wxString ct = "";
1403:                 loopallcellssel(ci, true) if (ci != fc && ci->text.t.Len()) ct += " " + ci->text.t;
1404:                 if (!fc->HasContent() && !ct.Len()) return _("There is no content to collapse.");
1405:                 fc->parent->AddUndo(this);
1406:                 fc->text.t += ct;
1407:                 loopallcellssel(ci, false) if (ci != fc) ci->Clear();
1408:                 Selection deletesel(selected.grid,
1409:                                     selected.x + int(selected.xs > 1),  // sidestep is possible?
1410:                                     selected.y + int(selected.ys > 1),
1411:                                     selected.xs - int(selected.xs > 1),
1412:                                     selected.ys - int(selected.ys > 1));
1413:                 selected.grid->MultiCellDeleteSub(this, deletesel);
1414:                 SetSelect(Selection(selected.grid, selected.x, selected.y, 1, 1));
1415:                 fc->ResetLayout();
1416:                 canvas->Refresh();
1417:                 return wxEmptyString;
1418:             }
1419: 
1420:             case wxID_SELECTALL:
1421:                 selected.SelAll();
1422:                 canvas->Refresh();
1423:                 sys->frame->UpdateStatus(selected, true);
1424:                 return wxEmptyString;
1425: 
1426:             case A_UP:
1427:             case A_DOWN:
1428:             case A_LEFT:
1429:             case A_RIGHT: selected.Cursor(this, action, false, false); return wxEmptyString;
1430: 
1431:             case A_MUP:
1432:             case A_MDOWN:
1433:             case A_MLEFT:
1434:             case A_MRIGHT:
1435:                 selected.Cursor(this, action - A_MUP + A_UP, true, false);
1436:                 return wxEmptyString;
1437: 
1438:             case A_SUP:
1439:             case A_SDOWN:
1440:             case A_SLEFT:
1441:             case A_SRIGHT:
1442:                 selected.Cursor(this, action - A_SUP + A_UP, false, true);
1443:                 return wxEmptyString;
1444: 
1445:             case A_SCLEFT:
1446:             case A_SCRIGHT:
1447:             case A_SCUP:
1448:             case A_SCDOWN:
1449:                 if (!selected.TextEdit()) {
1450:                     bool horiz = (action == A_SCLEFT || action == A_SCRIGHT);
1451:                     bool ismin = (action == A_SCLEFT || action == A_SCUP);
1452: 
1453:                     int &pos = horiz ? selected.x : selected.y;
1454:                     int &ext = horiz ? selected.xs : selected.ys;
1455:                     int gridmax = horiz ? selected.grid->xs : selected.grid->ys;
1456: 
1457:                     if (ismin) {
1458:                         ext = pos + (selected.Thin() ? 0 : 1);
1459:                         pos = 0;
1460:                     } else {
1461:                         ext = gridmax - pos;
1462:                     }
1463: 
1464:                     sys->frame->UpdateStatus(selected, true);
1465:                     canvas->Refresh();
1466:                 } else if (action == A_SCLEFT || action == A_SCRIGHT) {
1467:                     selected.Cursor(this, action - A_SCUP + A_UP, true, true);
1468:                 }
1469:                 return wxEmptyString;
1470: 
1471:             case wxID_BOLD:
1472:                 selected.grid->SetStyle(this, selected, STYLE_BOLD);
1473:                 return wxEmptyString;
1474:             case wxID_ITALIC:
1475:                 selected.grid->SetStyle(this, selected, STYLE_ITALIC);
1476:                 return wxEmptyString;
1477:             case A_TT: selected.grid->SetStyle(this, selected, STYLE_FIXED); return wxEmptyString;
1478:             case wxID_UNDERLINE:
1479:                 selected.grid->SetStyle(this, selected, STYLE_UNDERLINE);
1480:                 return wxEmptyString;
1481:             case wxID_STRIKETHROUGH:
1482:                 selected.grid->SetStyle(this, selected, STYLE_STRIKETHRU);
1483:                 return wxEmptyString;
1484: 
1485:             case A_MARKDATA:
1486:             case A_MARKVARD:
1487:             case A_MARKVARU:
1488:             case A_MARKVIEWH:
1489:             case A_MARKVIEWV:
1490:             case A_MARKCODE: {
1491:                 int newcelltype;
1492:                 switch (action) {
1493:                     case A_MARKDATA: newcelltype = CT_DATA; break;
1494:                     case A_MARKVARD: newcelltype = CT_VARD; break;
1495:                     case A_MARKVARU: newcelltype = CT_VARU; break;
1496:                     case A_MARKVIEWH: newcelltype = CT_VIEWH; break;
1497:                     case A_MARKVIEWV: newcelltype = CT_VIEWV; break;
1498:                     case A_MARKCODE: newcelltype = CT_CODE; break;
1499:                 }
1500:                 selected.grid->cell->AddUndo(this);
1501:                 loopallcellssel(c, false) {
1502:                     c->celltype = (newcelltype == CT_CODE) ? sys->evaluator.InferCellType(c->text)
1503:                                                            : newcelltype;
1504:                     canvas->Refresh();
1505:                 }
1506:                 return wxEmptyString;
1507:             }
1508: 
1509:             case A_CANCELEDIT:
1510:                 if (selected.TextEdit()) break;
1511:                 if (selected.grid->cell->parent) {
1512:                     SetSelect(selected.grid->cell->parent->grid->FindCell(selected.grid->cell));
1513:                 } else {
1514:                     selected.SelAll();
1515:                 }
1516:                 ScrollOrZoom();
1517:                 return wxEmptyString;
1518: 
1519:             case A_NEWGRID:
1520:                 if (!(cell = selected.ThinExpand(this))) return OneCell();
1521:                 if (cell->grid) {
1522:                     SetSelect(Selection(cell->grid, 0, cell->grid->ys, 1, 0));
1523:                     ScrollOrZoom(true);
1524:                 } else {
1525:                     cell->AddUndo(this);
1526:                     cell->AddGrid();
1527:                     SetSelect(Selection(cell->grid, 0, 0, 1, 1));
1528:                     paintscrolltoselection = true;
1529:                     canvas->Refresh();
1530:                 }
1531:                 return wxEmptyString;
1532: 
1533:             case wxID_PASTE:
1534:                 if (!(cell = selected.ThinExpand(this))) return OneCell();
1535:                 if (wxTheClipboard->Open()) {
1536:                     if (wxTheClipboard->IsSupported(wxDF_BITMAP)) {
1537:                         wxBitmapDataObject bdo;
1538:                         wxTheClipboard->GetData(bdo);
1539:                         PasteOrDrop(bdo);
1540:                     } else if (wxTheClipboard->IsSupported(wxDF_FILENAME)) {
1541:                         wxFileDataObject fdo;
1542:                         wxTheClipboard->GetData(fdo);
1543:                         PasteOrDrop(fdo);
1544:                     } else if (wxTheClipboard->IsSupported(wxDF_TEXT) ||
1545:                                wxTheClipboard->IsSupported(wxDF_UNICODETEXT)) {
1546:                         wxTextDataObject tdo;
1547:                         wxTheClipboard->GetData(tdo);
1548:                         PasteOrDrop(tdo);
1549:                     }
1550:                     wxTheClipboard->Close();
1551:                     canvas->Refresh();
1552:                 } else if (sys->cellclipboard) {
1553:                     cell->Paste(this, sys->cellclipboard.get(), selected);
1554:                     canvas->Refresh();
1555:                 }
1556:                 return wxEmptyString;
1557: 
1558:             case A_EDITNOTE: {
1559:                 if (!(cell = selected.ThinExpand(this))) return OneCell();
1560: 
1561:                 wxDialog dlg(sys->frame, wxID_ANY, _("Note"), wxDefaultPosition,
1562:                              wxSize(sys->notesizex, sys->notesizey),
1563:                              wxDEFAULT_DIALOG_STYLE | wxRESIZE_BORDER);
1564:                 auto *sizer = new wxBoxSizer(wxVERTICAL);
1565:                 wxTextCtrl text(&dlg, wxID_ANY, cell->note, wxDefaultPosition, wxDefaultSize,
1566:                                 wxTE_MULTILINE);
1567:                 sizer->Add(&text, 1, wxEXPAND | wxALL, 10);
1568:                 auto *btns = dlg.CreateButtonSizer(wxOK | wxCANCEL);
1569:                 sizer->Add(btns, 0, wxALIGN_CENTER | wxBOTTOM, 10);
1570:                 dlg.SetSizer(sizer);
1571: 
1572:                 text.SetFocus();
1573: 
1574:                 if (dlg.ShowModal() == wxID_OK) {
1575:                     if (cell->note != text.GetValue()) {
1576:                         cell->AddUndo(this);
1577:                         cell->note = text.GetValue();
1578:                         canvas->Refresh();
1579:                     }
1580:                     sys->notesizex = dlg.GetSize().x;
1581:                     sys->notesizey = dlg.GetSize().y;
1582:                 }
1583:                 return wxEmptyString;
1584:             }
1585: 
1586:             case A_PASTESTYLE:
1587:                 if (!sys->cellclipboard) return _("No style to paste.");
1588:                 selected.grid->cell->AddUndo(this);
1589:                 selected.grid->SetStyles(selected, sys->cellclipboard.get());
1590:                 selected.grid->cell->ResetChildren();
1591:                 canvas->Refresh();
1592:                 return wxEmptyString;
1593: 
1594:             case A_ENTERCELL:
1595:             case A_ENTERCELL_JUMPTOEND:
1596:             case A_ENTERCELL_JUMPTOSTART:
1597:             case A_PROGRESSCELL: {
1598:                 if (!(cell = selected.ThinExpand(this, action == A_ENTERCELL_JUMPTOSTART)))
1599:                     return OneCell();
1600:                 if (selected.TextEdit()) {
1601:                     selected.Cursor(this, action == A_PROGRESSCELL ? A_RIGHT : A_DOWN, false, false,
1602:                                     true);
1603:                 } else {
1604:                     selected.EnterEdit(
1605:                         this,
1606:                         action == A_ENTERCELL_JUMPTOEND ? static_cast<int>(cell->text.t.Len()) : 0,
1607:                         static_cast<int>(cell->text.t.Len()));
1608:                     paintscrolltoselection = true;
1609:                     canvas->Refresh();
1610:                 }
1611:                 return wxEmptyString;
1612:             }
1613: 
1614:             case A_IMAGE: {
1615:                 if (!(cell = selected.ThinExpand(this))) return OneCell();
1616:                 auto filename =
1617:                     ::wxFileSelector(_("Please select an image file:"), "", "", "", "*.*",
1618:                                      wxFD_OPEN | wxFD_FILE_MUST_EXIST | wxFD_CHANGE_DIR);
1619:                 cell->AddUndo(this);
1620:                 LoadImageIntoCell(filename, cell, sys->frame->FromDIP(1.0));
1621:                 canvas->Refresh();
1622:                 return wxEmptyString;
1623:             }
1624: 
1625:             case A_IMAGER: {
1626:                 selected.grid->cell->AddUndo(this);
1627:                 selected.grid->ClearImages(selected);
1628:                 selected.grid->cell->ResetChildren();
1629:                 canvas->Refresh();
1630:                 return wxEmptyString;
1631:             }
1632: 
1633:             case A_SORTD: return Sort(true);
1634:             case A_SORT: return Sort(false);
1635: 
1636:             case A_SCOLS:
1637:                 selected.y = 0;
1638:                 selected.ys = selected.grid->ys;
1639:                 canvas->Refresh();
1640:                 return wxEmptyString;
1641: 
1642:             case A_SROWS:
1643:                 selected.x = 0;
1644:                 selected.xs = selected.grid->xs;
1645:                 canvas->Refresh();
1646:                 return wxEmptyString;
1647: 
1648:             case A_BORD0:
1649:             case A_BORD1:
1650:             case A_BORD2:
1651:             case A_BORD3:
1652:             case A_BORD4:
1653:             case A_BORD5:
1654:                 selected.grid->cell->AddUndo(this);
1655:                 selected.grid->SetBorder(action - A_BORD0 + 1);
1656:                 selected.grid->cell->ResetChildren();
1657:                 canvas->Refresh();
1658:                 return wxEmptyString;
1659: 
1660:             case A_TEXTGRID: return layrender(-1, true, true);
1661: 
1662:             case A_V_GS: return layrender(DS_GRID, true);
1663:             case A_V_BS: return layrender(DS_BLOBSHIER, true);
1664:             case A_V_LS: return layrender(DS_BLOBLINE, true);
1665:             case A_H_GS: return layrender(DS_GRID, false);
1666:             case A_H_BS: return layrender(DS_BLOBSHIER, false);
1667:             case A_H_LS: return layrender(DS_BLOBLINE, false);
1668:             case A_GS: return layrender(DS_GRID, true, false, true);
1669:             case A_BS: return layrender(DS_BLOBSHIER, true, false, true);
1670:             case A_LS: return layrender(DS_BLOBLINE, true, false, true);
1671: 
1672:             case A_WRAP: return selected.Wrap(this);
1673: 
1674:             case A_RESETSIZE:
1675:             case A_RESETWIDTH:
1676:             case A_RESETSTYLE:
1677:             case A_RESETCOLOR:
1678:             case A_LASTCELLCOLOR:
1679:             case A_LASTTEXTCOLOR:
1680:             case A_LASTBORDCOLOR:
1681:             case A_LASTIMAGE:
1682:                 selected.grid->cell->AddUndo(this);
1683:                 loopallcellssel(c, true) switch (action) {
1684:                     case A_RESETSIZE: c->text.relsize = 0; break;
1685:                     case A_RESETWIDTH:
1686:                         for (int x = selected.x; x < selected.x + selected.xs; x++)
1687:                             selected.grid->colwidths[x] = sys->defaultmaxcolwidth;
1688:                         selected.grid->cell->ResetLayout();
1689:                         break;
1690:                     case A_RESETSTYLE: c->text.stylebits = 0; break;
1691:                     case A_RESETCOLOR:
1692:                         if (c->IsTag(this)) {
1693:                             tags[c->text.t] = g_tagcolor_default;
1694:                         } else {
1695:                             c->textcolor = g_textcolor_default;
1696:                         }
1697:                         c->cellcolor = g_cellcolor_default;
1698:                         if (c->grid) c->grid->bordercolor = g_bordercolor_default;
1699:                         break;
1700:                     case A_LASTCELLCOLOR: c->cellcolor = sys->lastcellcolor; break;
1701:                     case A_LASTTEXTCOLOR: c->textcolor = sys->lasttextcolor; break;
1702:                     case A_LASTBORDCOLOR:
1703:                         if (c->parent && c->parent->grid)
1704:                             c->parent->grid->bordercolor = sys->lastbordcolor;
1705:                         break;
1706:                     case A_LASTIMAGE:
1707:                         if (sys->lastimage) c->text.image = sys->lastimage;
1708:                         break;
1709:                 }
1710:                 selected.grid->cell->ResetChildren();
1711:                 canvas->Refresh();
1712:                 sys->frame->UpdateStatus(selected, false);
1713:                 return wxEmptyString;
1714: 
1715:             case A_MINISIZE: {
1716:                 selected.grid->cell->AddUndo(this);
1717:                 CollectCellsSel(false);
1718:                 vector<Cell *> outer;
1719:                 outer.insert(outer.end(), itercells.begin(), itercells.end());
1720:                 for (auto o : outer) {
1721:                     if (o->grid) {
1722:                         loopcellsin(o, c) if (_i) {
1723:                             c->text.relsize = g_deftextsize - g_mintextsize() - c->Depth();
1724:                         }
1725:                     }
1726:                 }
1727:                 outer.clear();
1728:                 selected.grid->cell->ResetChildren();
1729:                 canvas->Refresh();
1730:                 return wxEmptyString;
1731:             }
1732: 
1733:             case A_FOLD:
1734:             case A_FOLDALL:
1735:             case A_UNFOLDALL:
1736:                 loopallcellssel(c, action != A_FOLD) if (c->grid) {
1737:                     c->AddUndo(this);
1738:                     c->grid->folded = action == A_FOLD ? !c->grid->folded : action == A_FOLDALL;
1739:                     c->ResetChildren();
1740:                 }
1741:                 canvas->Refresh();
1742:                 return wxEmptyString;
1743: 
1744:             case A_HOME:
1745:             case A_END:
1746:             case A_CHOME:
1747:             case A_CEND:
1748:                 if (selected.TextEdit()) break;
1749:                 selected.HomeEnd(this, action == A_HOME || action == A_CHOME);
1750:                 return wxEmptyString;
1751: 
1752:             case A_IMAGESCP:
1753:             case A_IMAGESCW:
1754:             case A_IMAGESCF: {
1755:                 std::set<Image *> imagestomanipulate;
1756:                 long v = 0.0;
1757:                 loopallcellssel(c, true) {
1758:                     if (c->text.image) { imagestomanipulate.insert(c->text.image); }
1759:                 }
1760:                 if (imagestomanipulate.empty()) return wxEmptyString;
1761:                 if (action == A_IMAGESCW) {
1762:                     v = wxGetNumberFromUser(_("Please enter the new image width:"), _("Width"),
1763:                                             _("Image Resize"), 500, 10, 4000, sys->frame);
1764:                 } else {
1765:                     v = wxGetNumberFromUser(
1766:                         _("Please enter the percentage you want the image scaled by:"), "%",
1767:                         _("Image Resize"), 50, 5, 400, sys->frame);
1768:                 }
1769:                 if (v < 0) return wxEmptyString;
1770:                 for (auto image : imagestomanipulate) {
1771:                     if (action == A_IMAGESCW) {
1772:                         int pw = image->pixel_width;
1773:                         if (pw)
1774:                             image->ImageRescale(static_cast<double>(v) / static_cast<double>(pw));
1775:                     } else if (action == A_IMAGESCP) {
1776:                         image->ImageRescale(v / 100.0);
1777:                     } else {
1778:                         image->DisplayScale(v / 100.0);
1779:                     }
1780:                 }
1781:                 currentdrawroot->ResetChildren();
1782:                 currentdrawroot->ResetLayout();
1783:                 canvas->Refresh();
1784:                 return wxEmptyString;
1785:             }
1786: 
1787:             case A_IMAGESCN: {
1788:                 loopallcellssel(c, true) if (c->text.image) {
1789:                     c->text.image->ResetScale(sys->frame->FromDIP(1.0));
1790:                 }
1791:                 currentdrawroot->ResetChildren();
1792:                 currentdrawroot->ResetLayout();
1793:                 canvas->Refresh();
1794:                 return wxEmptyString;
1795:             }
1796: 
1797:             case A_IMAGESVA: {
1798:                 set<Image *> imagestosave;
1799:                 loopallcellssel(c, true) if (auto image = c->text.image)
1800:                     imagestosave.insert(image);
1801:                 if (imagestosave.empty()) return _("There are no images in the selection.");
1802:                 wxString filename = ::wxFileSelector(
1803:                     _("Choose image file to save:"), "", "", "",
1804:                     _("PNG file (*.png)|*.png|JPEG file (*.jpg)|*.jpg|All Files (*.*)|*.*"),
1805:                     wxFD_SAVE | wxFD_OVERWRITE_PROMPT | wxFD_CHANGE_DIR);
1806:                 if (filename.empty()) return _("Save cancelled.");
1807:                 auto i = 0;
1808:                 for (auto image : imagestosave) {
1809:                     wxFileName fn(filename);
1810:                     wxString finalfilename = fn.GetPathWithSep() + fn.GetName() +
1811:                                              (i == 0 ? wxString() : wxString::Format("%d", i)) +
1812:                                              image->GetFileExtension();
1813:                     wxFFileOutputStream os(finalfilename, "w+b");
1814:                     if (!os.IsOk()) {
1815:                         wxMessageBox(
1816:                             _("Error writing image file! (try saving under new filename)."),
1817:                             finalfilename.wx_str(), wxOK, sys->frame);
1818:                         return _("Error writing to file.");
1819:                     }
1820:                     os.Write(image->data.data(), image->data.size());
1821:                     i++;
1822:                 }
1823:                 return _("Image(s) have been saved to disk.");
1824:             }
1825: 
1826:             case A_SAVE_AS_JPEG:
1827:             case A_SAVE_AS_PNG: {
1828:                 wxString returnmessage = _("No image found to convert");
1829:                 loopallcellssel(c, true) {
1830:                     auto image = c->text.image;
1831:                     if (action == A_SAVE_AS_JPEG && image && image->type == 'I') {
1832:                         auto transferimage = ConvertBufferToWxImage(image->data, wxBITMAP_TYPE_PNG);
1833:                         image->data = ConvertWxImageToBuffer(transferimage, wxBITMAP_TYPE_JPEG);
1834:                         image->type = 'J';
1835:                         returnmessage =
1836:                             _("Images in selected cells have been converted to JPEG format.");
1837:                     }
1838:                     if (action == A_SAVE_AS_PNG && image && image->type == 'J') {
1839:                         auto transferimage =
1840:                             ConvertBufferToWxImage(image->data, wxBITMAP_TYPE_JPEG);
1841:                         image->data = ConvertWxImageToBuffer(transferimage, wxBITMAP_TYPE_PNG);
1842:                         image->type = 'I';
1843:                         returnmessage =
1844:                             _("Images in selected cells have been converted to PNG format.");
1845:                     }
1846:                 }
1847:                 return returnmessage;
1848:             }
1849: 
1850:             case A_BROWSE: {
1851:                 wxString returnmessage = "";
1852:                 int counter = 0;
1853:                 loopallcellssel(c, false) {
1854:                     if (counter >= g_max_launches) {
1855:                         returnmessage = _("Maximum number of launches reached.");
1856:                         break;
1857:                     }
1858:                     if (!wxLaunchDefaultBrowser(c->text.ToText(0, selected, A_EXPTEXT))) {
1859:                         returnmessage = _("The browser could not open at least one link.");
1860:                     } else {
1861:                         counter++;
1862:                     }
1863:                 }
1864:                 return returnmessage;
1865:             }
1866: 
1867:             case A_BROWSEF: {
1868:                 wxString returnmessage = "";
1869:                 int counter = 0;
1870:                 loopallcellssel(c, false) {
1871:                     if (counter >= g_max_launches) {
1872:                         returnmessage = _("Maximum number of launches reached.");
1873:                         break;
1874:                     }
1875:                     auto f = c->text.ToText(0, selected, A_EXPTEXT);
1876:                     wxFileName fn(f);
1877:                     if (fn.IsRelative()) fn.MakeAbsolute(wxFileName(filename).GetPath());
1878:                     if (!wxLaunchDefaultApplication(fn.GetFullPath())) {
1879:                         returnmessage = _("At least one file could not be opened.");
1880:                     } else {
1881:                         counter++;
1882:                     }
1883:                 }
1884:                 return returnmessage;
1885:             }
1886: 
1887:             case A_TAGADD: {
1888:                 loopallcellssel(c, false) {
1889:                     if (!c->text.t.Len()) continue;
1890:                     tags[c->text.t] = g_tagcolor_default;
1891:                 }
1892:                 canvas->Refresh();
1893:                 return wxEmptyString;
1894:             }
1895: 
1896:             case A_TAGREMOVE: {
1897:                 loopallcellssel(c, false) tags.erase(c->text.t);
1898:                 canvas->Refresh();
1899:                 return wxEmptyString;
1900:             }
1901: 
1902:             case A_TRANSPOSE: {
1903:                 if (selected.Thin()) return NoThin();
1904:                 auto ac = selected.grid->cell;
1905:                 ac->AddUndo(this);
1906:                 if (selected.IsAll()) {
1907:                     ac->grid->Transpose();
1908:                     SetSelect(ac->parent ? ac->parent->grid->FindCell(ac) : Selection());
1909:                 } else {
1910:                     loopallcellssel(c, false) if (c->grid) c->grid->Transpose();
1911:                 }
1912:                 ac->ResetChildren();
1913:                 canvas->Refresh();
1914:                 return wxEmptyString;
1915:             }
1916:         }
1917: 
1918:         if (cell || (!cell && selected.IsAll())) {
1919:             auto ac = cell ? cell : selected.grid->cell;
1920:             switch (action) {
1921:                 case A_HIFY:
1922:                     if (!ac->grid) return NoGrid();
1923:                     if (!ac->grid->IsTable())
1924:                         return _(
1925:                             "Selected grid is not a table: cells must not already have sub-grids.");
1926:                     ac->AddUndo(this);
1927:                     ac->grid->Hierarchify(this);
1928:                     ac->ResetChildren();
1929:                     selected = Selection();
1930:                     begindrag = Selection();
1931:                     canvas->Refresh();
1932:                     return wxEmptyString;
1933: 
1934:                 case A_FLATTEN: {
1935:                     if (!ac->grid) return NoGrid();
1936:                     ac->AddUndo(this);
1937:                     int maxdepth = 0, leaves = 0;
1938:                     ac->MaxDepthLeaves(0, maxdepth, leaves);
1939:                     auto g = new Grid(maxdepth, leaves);
1940:                     g->InitCells();
1941:                     ac->grid->Flatten(0, 0, g);
1942:                     DELETEP(ac->grid);
1943:                     ac->grid = g;
1944:                     g->ReParent(ac);
1945:                     ac->ResetChildren();
1946:                     selected = Selection();
1947:                     begindrag = Selection();
1948:                     canvas->Refresh();
1949:                     return wxEmptyString;
1950:                 }
1951:             }
1952:         }
1953: 
1954:         if (!cell) return OneCell();
1955: 
1956:         switch (action) {
1957:             case A_NEXT: selected.Next(this, false); return wxEmptyString;
1958:             case A_PREV: selected.Next(this, true); return wxEmptyString;
1959: 
1960:             case A_ENTERGRID:
1961:                 if (!cell->grid) Action(A_NEWGRID);
1962:                 SetSelect(Selection(cell->grid, 0, 0, 1, 1));
1963:                 ScrollOrZoom(true);
1964:                 return wxEmptyString;
1965: 
1966:             case A_LINK:
1967:             case A_LINKIMG:
1968:             case A_LINKREV:
1969:             case A_LINKIMGREV: {
1970:                 if ((action == A_LINK || action == A_LINKREV) && !cell->text.t.Len())
1971:                     return _("No text in this cell.");
1972:                 if ((action == A_LINKIMG || action == A_LINKIMGREV) && !cell->text.image)
1973:                     return _("No image in this cell.");
1974:                 bool t1 = false, t2 = false;
1975:                 auto link = root->FindLink(selected, cell, nullptr, t1, t2,
1976:                                            action == A_LINK || action == A_LINKIMG,
1977:                                            action == A_LINKIMG || action == A_LINKIMGREV);
1978:                 if (!link || !link->parent) return _("No matching cell found!");
1979:                 SetSelect(link->parent->grid->FindCell(link));
1980:                 ScrollOrZoom(true);
1981:                 return wxEmptyString;
1982:             }
1983: 
1984:             case A_COLCELL: sys->customcolor = cell->cellcolor; return wxEmptyString;
1985: 
1986:             case A_HSWAP: {
1987:                 auto pp = cell->parent->parent;
1988:                 if (!pp) return _("Cannot move this cell up in the hierarchy.");
1989:                 if (pp->grid->xs != 1 && pp->grid->ys != 1)
1990:                     return _("Can only move this cell into a Nx1 or 1xN grid.");
1991:                 if (cell->parent->grid->xs != 1 && cell->parent->grid->ys != 1)
1992:                     return _("Can only move this cell from a Nx1 or 1xN grid.");
1993:                 pp->AddUndo(this);
1994:                 SetSelect(pp->grid->HierarchySwap(cell->text.t));
1995:                 pp->ResetChildren();
1996:                 pp->ResetLayout();
1997:                 canvas->Refresh();
1998:                 return wxEmptyString;
1999:             }
2000: 
2001:             case A_FILTERBYCELLBG:
2002:                 loopallcells(ci) ci->text.filtered = ci->cellcolor != cell->cellcolor;
2003:                 root->ResetChildren();
2004:                 canvas->Refresh();
2005:                 return wxEmptyString;
2006: 
2007:             case A_FILTERNOTE:
2008:                 loopallcells(ci) ci->text.filtered = ci->note.IsEmpty();
2009:                 root->ResetChildren();
2010:                 canvas->Refresh();
2011:                 return wxEmptyString;
2012: 
2013:             case A_FILTERMATCHNEXT:
2014:                 bool lastsel = true;
2015:                 Cell *next = root->FindNextFilterMatch(nullptr, selected.GetCell(), lastsel);
2016:                 if (!next) return _("No matches for filter.");
2017:                 if (next->parent) SetSelect(next->parent->grid->FindCell(next));
2018:                 canvas->SetFocus();
2019:                 ScrollOrZoom(true);
2020:                 return wxEmptyString;
2021:         }
2022: 
2023:         if (!selected.TextEdit()) return _("only works in cell text mode");
2024: 
2025:         switch (action) {
2026:             case A_CANCELEDIT:
2027:                 if (LastUndoSameCellTextEdit(cell))
2028:                     Undo(undolist, redolist);
2029:                 else
2030:                     canvas->Refresh();
2031:                 selected.ExitEdit(this);
2032:                 return wxEmptyString;
2033: 
2034:             case A_BACKSPACE_WORD:
2035:                 if (selected.cursorend == 0) return wxEmptyString;
2036:                 cell->AddUndo(this);
2037:                 cell->text.BackspaceWord(selected);
2038:                 canvas->Refresh();
2039:                 ZoomOutIfNoGrid();
2040:                 return wxEmptyString;
2041: 
2042:             case A_SHOME:
2043:             case A_SEND:
2044:             case A_CHOME:
2045:             case A_CEND:
2046:             case A_HOME:
2047:             case A_END: {
2048:                 switch (action) {
2049:                     case A_SHOME:  // FIXME: this functionality is really SCHOME, SHOME should be
2050:                                    // within line
2051:                         selected.cursor = 0;
2052:                         break;
2053:                     case A_SEND: selected.cursorend = static_cast<int>(cell->text.t.Len()); break;
2054:                     case A_CHOME: selected.cursor = selected.cursorend = 0; break;
2055:                     case A_CEND: selected.cursor = selected.cursorend = selected.MaxCursor(); break;
2056:                     case A_HOME: cell->text.HomeEnd(selected, true); break;
2057:                     case A_END: cell->text.HomeEnd(selected, false); break;
2058:                 }
2059:                 paintscrolltoselection = true;
2060:                 canvas->Refresh();
2061:                 return wxEmptyString;
2062:             }
2063:             default: return _("Internal error: unimplemented operation!");
2064:         }
2065:     }
2066: 
2067:     wxString SearchNext(bool focusmatch, bool jump, bool reverse) {
2068:         if (!root) return wxEmptyString;  // fix crash when opening new doc
2069:         if (!sys->searchstring.Len()) return _("No search string.");
2070:         bool lastsel = true;
2071:         Cell *next = root->FindNextSearchMatch(sys->searchstring, nullptr, selected.GetCell(),
2072:                                                lastsel, reverse);
2073:         if (!next) return _("No matches for search.");
2074:         if (!jump) return wxEmptyString;
2075:         SetSelect(next->parent->grid->FindCell(next));
2076:         if (focusmatch) canvas->SetFocus();
2077:         ScrollOrZoom(true);
2078:         return wxEmptyString;
2079:     }
2080: 
2081:     wxString layrender(int ds, bool vert, bool toggle = false, bool noset = false) {
2082:         if (selected.Thin()) return NoThin();
2083:         selected.grid->cell->AddUndo(this);
2084:         bool v = toggle ? !selected.GetFirst()->verticaltextandgrid : vert;
2085:         if (ds >= 0 && selected.IsAll()) selected.grid->cell->drawstyle = ds;
2086:         selected.grid->SetGridTextLayout(ds, v, noset, selected);
2087:         selected.grid->cell->ResetChildren();
2088:         canvas->Refresh();
2089:         return wxEmptyString;
2090:     }
2091: 
2092:     void ZoomOutIfNoGrid() {
2093:         if (!WalkPath(drawpath)->grid) Zoom(-1);
2094:     }
2095: 
2096:     void PasteSingleText(Cell *c, const wxString &s) { c->text.Insert(this, s, selected, false); }
2097: 
2098:     // Polymorphism with wxDataObjectSimple does not work on Windows; bitmap format seems to not be
2099:     // recognized.
2100: 
2101:     void PasteOrDrop(const wxFileDataObject &filedataobject) {
2102:         const wxArrayString &filenames = filedataobject.GetFilenames();
2103:         if (filenames.size() != 1) {
2104:             sys->frame->SetStatus(_("Can paste or drop only exactly one file."));
2105:             return;
2106:         }
2107:         Cell *cell = selected.ThinExpand(this);
2108:         wxString filename = filenames[0];
2109:         wxFFileInputStream fileinputstream(filename);
2110:         if (fileinputstream.IsOk()) {
2111:             char buffer[4];
2112:             fileinputstream.Read(buffer, 4);
2113:             if (!strncmp(buffer, "TSFF", 4)) {
2114:                 ThreeChoiceDialog askuser(
2115:                     sys->frame, filename,
2116:                     _("It seems that you are about to paste or drop a TreeSheets file. "
2117:                       "What would you like to do?"),
2118:                     _("Open TreeSheets file"), _("Paste file path"), _("Cancel"));
2119:                 switch (askuser.Run()) {
2120:                     case 0: sys->frame->SetStatus(sys->LoadDB(filename));
2121:                     case 2: return;
2122:                     default:
2123:                     case 1:;
2124:                 }
2125:             }
2126:         }
2127:         if (!cell) return;
2128:         cell->AddUndo(this);
2129:         if (!LoadImageIntoCell(filename, cell, sys->frame->FromDIP(1.0)))
2130:             PasteSingleText(cell, filename);
2131:     }
2132: 
2133:     void PasteOrDrop(const wxTextDataObject &textdataobject) {
2134:         if (textdataobject.GetText() != wxEmptyString) {
2135:             Cell *cell = selected.ThinExpand(this);
2136:             auto text = textdataobject.GetText();
2137:             if ((sys->clipboardcopy == text) && sys->cellclipboard) {
2138:                 cell->Paste(this, sys->cellclipboard.get(), selected);
2139:             } else {
2140:                 const wxArrayString &lines = wxStringTokenize(text, LINE_SEPARATOR);
2141:                 if (lines.size() == 1) {
2142:                     cell->AddUndo(this);
2143:                     cell->ResetLayout();
2144:                     PasteSingleText(cell, lines[0]);
2145:                 } else if (lines.size() > 1) {
2146:                     cell->parent->AddUndo(this);
2147:                     cell->ResetLayout();
2148:                     DELETEP(cell->grid);
2149:                     sys->FillRows(cell->AddGrid(), lines, sys->CountCol(lines[0]), 0, 0);
2150:                     if (!cell->HasText())
2151:                         cell->grid->MergeWithParent(cell->parent->grid, selected, this);
2152:                 }
2153:             }
2154:         }
2155:     }
2156: 
2157:     void PasteOrDrop(const wxBitmapDataObject &bitmapdataobject) {
2158:         if (bitmapdataobject.GetBitmap().GetRefData() != wxNullBitmap.GetRefData()) {
2159:             Cell *cell = selected.ThinExpand(this);
2160:             cell->AddUndo(this);
2161:             auto image = bitmapdataobject.GetBitmap().ConvertToImage();
2162:             vector<uint8_t> buffer = ConvertWxImageToBuffer(image, wxBITMAP_TYPE_PNG);
2163:             SetImageBM(cell, std::move(buffer), 'I', sys->frame->FromDIP(1.0));
2164:             cell->Reset();
2165:         }
2166:     }
2167: 
2168:     wxString Sort(bool descending) {
2169:         if (selected.xs != 1 && selected.ys <= 1)
2170:             return _(
2171:                 "Can't sort: make a 1xN selection to indicate what column to sort on, and what rows to affect");
2172:         selected.grid->cell->AddUndo(this);
2173:         selected.grid->Sort(selected, descending);
2174:         canvas->Refresh();
2175:         return wxEmptyString;
2176:     }
2177: 
2178:     void DelRowCol(int &v, int e, int gvs, int dec, int dx, int dy, int nxs, int nys) {
2179:         if (v != e) {
2180:             selected.grid->cell->AddUndo(this);
2181:             if (gvs == 1) {
2182:                 selected.grid->DelSelf(this, selected);
2183:             } else {
2184:                 selected.grid->DeleteCells(dx, dy, nxs, nys);
2185:                 v -= dec;
2186:             }
2187:             canvas->Refresh();
2188:         }
2189:     }
2190: 
2191:     void CreatePath(Cell *c, auto &path) {
2192:         path.clear();
2193:         while (c->parent) {
2194:             const Selection &s = c->parent->grid->FindCell(c);
2195:             ASSERT(s.grid);
2196:             path.push_back(s);
2197:             c = c->parent;
2198:         }
2199:     }
2200: 
2201:     Cell *WalkPath(auto &path) {
2202:         Cell *c = root.get();
2203:         loopvrev(i, path) {
2204:             Selection &s = path[i];
2205:             Grid *g = c->grid;
2206:             if (!g) return c;
2207:             ASSERT(g && s.x < g->xs && s.y < g->ys);
2208:             c = g->C(s.x, s.y).get();
2209:         }
2210:         return c;
2211:     }
2212: 
2213:     bool LastUndoSameCellAny(Cell *c) {
2214:         return undolist.size() && undolist.size() != undolistsizeatfullsave &&
2215:                undolist.back()->cloned_from == (uintptr_t)c;
2216:     }
2217: 
2218:     bool LastUndoSameCellTextEdit(Cell *c) {
2219:         // hacky way to detect word boundaries to stop coalescing, but works, and
2220:         // not a big deal if selected is not actually related to this cell
2221:         return undolist.size() && !c->grid && undolist.size() != undolistsizeatfullsave &&
2222:                undolist.back()->sel.EqLoc(c->parent->grid->FindCell(c)) &&
2223:                (!c->text.t.EndsWith(" ") || c->text.t.Len() != selected.cursor);
2224:     }
2225: 
2226:     void AddUndo(Cell *c, bool newgeneration = true) {
2227:         redolist.clear();
2228:         lastmodsinceautosave = wxGetLocalTime();
2229:         if (!modified) {
2230:             modified = true;
2231:             UpdateFileName();
2232:         }
2233:         if (LastUndoSameCellTextEdit(c)) return;
2234:         auto ui = make_unique<UndoItem>();
2235:         ui->clone = c->Clone(nullptr);
2236:         ui->estimated_size = c->EstimatedMemoryUse();
2237:         ui->sel = selected;
2238:         ui->cloned_from = (uintptr_t)c;
2239:         if (undolist.size()) ui->generation = undolist.back()->generation + (newgeneration ? 1 : 0);
2240:         CreatePath(c, ui->path);
2241:         if (selected.grid) CreatePath(selected.grid->cell, ui->selpath);
2242:         undolist.push_back(std::move(ui));
2243:         size_t total_usage = 0;
2244:         size_t old_list_size = undolist.size();
2245:         // Cull undolist. Always at least keeps last item.
2246:         for (auto i = static_cast<int>(undolist.size()) - 1; i >= 0; i--) {
2247:             // Cull old items if using more than 100MB or 1000 items, whichever comes first.
2248:             // TODO: make configurable?
2249:             if (total_usage < 100 * 1024 * 1024 && undolist.size() - i < 1000) {
2250:                 total_usage += undolist[i]->estimated_size;
2251:             } else {
2252:                 undolist.erase(undolist.begin(), undolist.begin() + i + 1);
2253:                 break;
2254:             }
2255:         }
2256:         size_t items_culled = old_list_size - undolist.size();
2257:         undolistsizeatfullsave -= items_culled;  // Allowed to go < 0
2258:     }
2259: 
2260:     void Undo(vector<unique_ptr<UndoItem>> &fromlist, vector<unique_ptr<UndoItem>> &tolist,
2261:               bool redo = false) {
2262:         for (bool next = true; next; ) {
2263:             UndoEach(fromlist, tolist, redo);
2264:             next = (fromlist.size() && tolist.size() &&
2265:                     fromlist.back()->generation == tolist.back()->generation)
2266:                        ? true
2267:                        : false;
2268:         }
2269:         if (selected.grid)
2270:             ScrollOrZoom();
2271:         else
2272:             canvas->Refresh();
2273:         UpdateFileName();
2274:     }
2275: 
2276:     void UndoEach(vector<unique_ptr<UndoItem>> &fromlist, vector<unique_ptr<UndoItem>> &tolist,
2277:                   bool redo = false) {
2278:         auto beforesel = selected;
2279:         vector<Selection> beforepath;
2280:         if (beforesel.grid) CreatePath(beforesel.grid->cell, beforepath);
2281: 
2282:         auto ui = std::move(fromlist.back());
2283:         fromlist.pop_back();
2284: 
2285:         Cell *c = WalkPath(ui->path);
2286: 
2287:         if (c->parent && c->parent->grid) {
2288:             Grid *g = c->parent->grid;
2289:             Selection s = g->FindCell(c);
2290:             std::swap(ui->clone, g->C(s.x, s.y));
2291:             c = g->C(s.x, s.y).get();
2292:             c->parent = ui->clone->parent;
2293:         } else {
2294:             std::swap(ui->clone, root);
2295:             c = root.get();
2296:             c->parent = nullptr;
2297:         }
2298:         c->ResetLayout();
2299: 
2300:         SetSelect(ui->sel);
2301:         if (selected.grid) { selected.grid = WalkPath(ui->selpath)->grid; }
2302: 
2303:         begindrag = selected;
2304:         ui->sel = beforesel;
2305:         ui->selpath = std::move(beforepath);
2306:         tolist.push_back(std::move(ui));
2307: 
2308:         if (undolistsizeatfullsave > (int)undolist.size()) undolistsizeatfullsave = -1;
2309:         modified = undolistsizeatfullsave != (int)undolist.size();
2310:     }
2311: 
2312:     void ColorChange(int which, int idx) {
2313:         if (!selected.grid) return;
2314:         auto col = idx == CUSTOMCOLORIDX ? sys->customcolor : celltextcolors[idx];
2315:         switch (which) {
2316:             case A_CELLCOLOR: sys->lastcellcolor = col; break;
2317:             case A_TEXTCOLOR: sys->lasttextcolor = col; break;
2318:             case A_BORDCOLOR: sys->lastbordcolor = col; break;
2319:         }
2320:         selected.grid->ColorChange(this, which, col, selected);
2321:     }
2322: 
2323:     void SetImageBM(Cell *c, auto &&data, char type, double scale) {
2324:         c->text.image = sys->lastimage =
2325:             sys->imagelist[sys->AddImageToList(scale, std::move(data), type)].get();
2326:     }
2327: 
2328:     bool LoadImageIntoCell(const wxString &filename, Cell *c, double scale) {
2329:         if (filename.empty()) return false;
2330:         auto pnghandler = wxImage::FindHandler(wxBITMAP_TYPE_PNG);
2331:         auto jpghandler = wxImage::FindHandler(wxBITMAP_TYPE_JPEG);
2332:         wxImageHandler *activeHandler = nullptr;
2333:         if (pnghandler && pnghandler->CanRead(filename)) {
2334:             activeHandler = pnghandler;
2335:         } else if (jpghandler && jpghandler->CanRead(filename)) {
2336:             activeHandler = jpghandler;
2337:         }
2338:         if (activeHandler) {
2339:             wxFile fn(filename);
2340:             if (!fn.IsOpened()) return false;
2341:             vector<uint8_t> buffer(fn.Length());
2342:             fn.Read(buffer.data(), buffer.size());
2343:             SetImageBM(c, std::move(buffer), (activeHandler == pnghandler) ? 'I' : 'J', scale);
2344:         } else {
2345:             wxImage image;
2346:             if (!image.LoadFile(filename)) return false;
2347:             auto buffer = ConvertWxImageToBuffer(image, wxBITMAP_TYPE_PNG);
2348:             SetImageBM(c, std::move(buffer), 'I', scale);
2349:         }
2350:         c->Reset();
2351:         return true;
2352:     }
2353: 
2354:     void ImageChange(const wxString &filename, double scale) {
2355:         if (!selected.grid) return;
2356:         selected.grid->cell->AddUndo(this);
2357:         loopallcellssel(c, false) LoadImageIntoCell(filename, c, scale);
2358:         canvas->Refresh();
2359:     }
2360: 
2361:     void RecreateTagMenu(wxMenu &menu) {
2362:         int i = A_TAGSET;
2363:         for (auto &[tag, color] : tags) { menu.Append(i++, tag); }
2364:         if (tags.size()) menu.AppendSeparator();
2365:         menu.Append(A_TAGADD, _("&Add Cell Text as Tag"));
2366:         menu.Append(A_TAGREMOVE, _("&Remove Cell Text from Tags"));
2367:     }
2368: 
2369:     wxString TagSet(int tagno) {
2370:         int i = 0;
2371:         for (auto &[tag, color] : tags)
2372:             if (i++ == tagno) {
2373:                 selected.grid->cell->AddUndo(this);
2374:                 loopallcellssel(c, false) {
2375:                     c->text.Clear(this, selected);
2376:                     c->text.Insert(this, tag, selected, true);
2377:                 }
2378:                 selected.grid->cell->ResetChildren();
2379:                 selected.grid->cell->ResetLayout();
2380:                 canvas->Refresh();
2381:                 return wxEmptyString;
2382:             }
2383:         ASSERT(0);
2384:         return wxEmptyString;
2385:     }
2386: 
2387:     void CollectCells(Cell *c) {
2388:         itercells.clear();
2389:         c->CollectCells(itercells);
2390:     }
2391: 
2392:     void CollectCellsSel(bool recurse) {
2393:         itercells.clear();
2394:         if (selected.grid) selected.grid->CollectCellsSel(itercells, selected, recurse);
2395:     }
2396: 
2397:     void ApplyEditFilter() {
2398:         searchfilter = false;
2399:         paintscrolltoselection = true;
2400:         editfilter = min(max(editfilter, 1), 99);
2401:         CollectCells(root.get());
2402:         ranges::sort(itercells, [](auto a, auto b) {
2403:             // sort in descending order
2404:             return a->text.lastedit > b->text.lastedit;
2405:         });
2406:         loopv(i, itercells) itercells[i]->text.filtered = i > itercells.size() * editfilter / 100;
2407:         root->ResetChildren();
2408:         canvas->Refresh();
2409:     }
2410: 
2411:     void ApplyEditRangeFilter(wxDateTime &rangebegin, wxDateTime &rangeend) {
2412:         searchfilter = false;
2413:         paintscrolltoselection = true;
2414:         CollectCells(root.get());
2415:         for (auto c : itercells) {
2416:             c->text.filtered = !c->text.lastedit.IsBetween(rangebegin, rangeend);
2417:         }
2418:         root->ResetChildren();
2419:         canvas->Refresh();
2420:     }
2421: 
2422:     wxDateTime ParseDateTimeString(const wxString &s) {
2423:         wxDateTime dt;
2424:         wxString::const_iterator end;
2425:         if (!dt.ParseDateTime(s, &end)) dt = wxInvalidDateTime;
2426:         return dt;
2427:     }
2428: 
2429:     void SetSearchFilter(bool on) {
2430:         searchfilter = on;
2431:         paintscrolltoselection = true;
2432:         loopallcells(c) c->text.filtered = on && !c->text.IsInSearch();
2433:         root->ResetChildren();
2434:         canvas->Refresh();
2435:     }
2436: 
2437:     void ExportAllImages(const wxString &filename, Cell *exportroot) {
2438:         std::set<Image *> exportimages;
2439:         CollectCells(exportroot);
2440:         for (auto c : itercells)
2441:             if (c->text.image) exportimages.insert(c->text.image);
2442:         wxFileName fn(filename);
2443:         auto directory = fn.GetPathWithSep();
2444:         for (auto image : exportimages)
2445:             if (!image->ExportToDirectory(directory)) break;
2446:     }
2447: };

================
File: src/evaluator.h
================
  1: /*
  2:     A structure describing an operation.
  3: */
  4: struct Operation {
  5:     virtual ~Operation() {};
  6:     const char *args;
  7: 
  8:     virtual unique_ptr<Cell> run() const { return nullptr; }
  9:     virtual double runn(double a) const { return 0; }
 10:     virtual unique_ptr<Cell> runl(Grid *l) const { return nullptr; }
 11:     virtual void rung(Grid *g) const {}
 12:     virtual unique_ptr<Cell> runc(unique_ptr<Cell> c) const { return nullptr; }
 13:     virtual double runnn(double a, double b) const { return 0; }
 14: };
 15: 
 16: typedef std::unordered_map<wxString, unique_ptr<Operation>> OperationMap;
 17: typedef std::unordered_map<wxString, unique_ptr<Cell>> VariableMap;
 18: 
 19: /*
 20:     Provides running evaluation of a grid.
 21: */
 22: struct Evaluator {
 23:     OperationMap ops;
 24:     VariableMap vars;
 25:     bool vert;
 26: 
 27:     ~Evaluator() {}
 28: 
 29:     void ClearVars() {
 30:         vars.clear();
 31:     }
 32: 
 33:     #define OP(_n, _c, _args, _f)           \
 34:         {                                   \
 35:             struct _op : Operation {        \
 36:                 _op() { args = _args; };    \
 37:                 _f const override { return _c; }; \
 38:             };                              \
 39:             ops[L## #_n] = make_unique<_op>();       \
 40:         }
 41: 
 42:     #define OPNN(_n, _c) OP(_n, _c, "nn", double runnn(double a, double b))
 43:     #define OPN(_n, _c) OP(_n, _c, "n", double runn(double a))
 44:     #define OPT(_n, _c) OP(_n, _c, "t", unique_ptr<Cell> runc(unique_ptr<Cell> c))
 45:     #define OPC(_n, _c) OP(_n, _c, "c", unique_ptr<Cell> runc(unique_ptr<Cell> c))
 46:     #define OPL(_n, _c) OP(_n, _c, "l", unique_ptr<Cell> runl(Grid *a))
 47:     #define OPG(_n, _c) OP(_n, _c, "g", void rung(Grid *a))
 48: 
 49:     void Init() {
 50:         OPNN(+, a + b);
 51:         OPNN(-, a - b);
 52:         OPNN(*, a * b);
 53:         OPNN(/, b != 0 ? a / b : 0);
 54:         OPNN(<, double(a < b));
 55:         OPNN(>, double(a > b));
 56:         OPNN(<=, double(a <= b));
 57:         OPNN(>=, double(a >= b));
 58:         OPNN(=, double(a == b));
 59:         OPNN(==, double(a == b));
 60:         OPNN(!=, double(a != b));
 61:         OPNN(<>, double(a != b));
 62:         OPN(inc, a + 1);
 63:         OPN(dec, a - 1);
 64:         OPN(neg, -a);
 65:         OPT(graph, (c->Graph(), std::move(c)));
 66:         OPL(sum, a->Sum())
 67:         OPG(transpose, a->Transpose())
 68:         struct _if : Operation {
 69:             _if() { args = "nLL"; };
 70:         };
 71:         ops[L"if"] = make_unique<_if>();
 72:     }
 73: 
 74:     int InferCellType(Text &t) {
 75:         if (ops[t.t])
 76:             return CT_CODE;
 77:         else
 78:             return CT_DATA;
 79:     }
 80: 
 81:     unique_ptr<Cell> Lookup(const wxString &name) {
 82:         auto it = vars.find(name);
 83:         return (it != vars.end()) ? it->second->Clone(nullptr) : nullptr;
 84:     }
 85: 
 86:     bool IsValidSymbol(wxString const &symbol) const { return !symbol.IsEmpty(); }
 87:     void SetSymbol(wxString const &symbol, unique_ptr<Cell> val) {
 88:         if (!this->IsValidSymbol(symbol)) { return; }
 89:         vars[symbol] = std::move(val);
 90:     }
 91: 
 92:     void Assign(const Cell *sym, const Cell *val) {
 93:         this->SetSymbol(sym->text.t, val->Clone(nullptr));
 94:         if (sym->grid && val->grid) this->DestructuringAssign(sym->grid, val->Clone(nullptr));
 95:     }
 96: 
 97:     void DestructuringAssign(Grid const *names, unique_ptr<Cell> val) {
 98:         Grid const *ng = names;
 99:         Grid const *vg = val->grid;
100:         if (ng->xs == vg->xs && ng->ys == vg->ys) {
101:             loop(x, ng->xs) loop(y, ng->ys) {
102:                 this->SetSymbol(ng->C(x, y)->text.t, vg->C(x, y)->Clone(nullptr));
103:             }
104:         }
105:     }
106: 
107:     Operation *FindOp(wxString &name) { return ops[name].get(); }
108: 
109:     unique_ptr<Cell> Execute(const Operation *op) { return op->run(); }
110: 
111:     unique_ptr<Cell> Execute(const Operation *op, unique_ptr<Cell> left) {
112:         Text &t = left->text;
113:         Grid *g = left->grid;
114:         switch (op->args[0]) {
115:             case 'n':
116:                 if (t.t.Len()) {
117:                     t.SetNum(op->runn(t.GetNum()));
118:                     return left;
119:                 } else if (g) {
120:                     foreachcellingrid(c, g) {
121:                         unique_ptr<Cell> temp = std::move(c);
122:                         c = Execute(op, std::move(temp));
123:                         c->SetParent(left.get());
124:                     }
125:                 }
126:                 break;
127:             case 't':
128:                 if (t.t.Len()) {
129:                     return op->runc(std::move(left));
130:                 } else if (g) {
131:                     foreachcellingrid(c, g) {
132:                         unique_ptr<Cell> temp = std::move(c);
133:                         c = Execute(op, std::move(temp));
134:                         c->SetParent(left.get());
135:                     }
136:                 }
137:                 break;
138:             case 'l':
139:                 if (g) {
140:                     if (g->xs == 1 || g->ys == 1) {
141:                         return op->runl(g);
142:                     } else {
143:                         vector<unique_ptr<Grid>> gs;
144:                         g->Split(gs, vert);
145:                         g = new Grid(vert ? gs.size() : 1, vert ? 1 : gs.size());
146:                         auto c = make_unique<Cell>(nullptr, left.get(), CT_DATA, g);
147:                         loopv(i, gs) {
148:                             auto res = op->runl(gs[i].get());
149:                             res->SetParent(c.get());
150:                             g->C(vert ? i : 0, vert ? 0 : i) = std::move(res);
151:                         }
152:                         return c;
153:                     }
154:                 }
155:                 break;
156:             case 'g':
157:                 if (g) op->rung(g);
158:                 break;
159:             case 'c': return op->runc(std::move(left));
160:         }
161:         return left;
162:     }
163: 
164:     unique_ptr<Cell> Execute(const Operation *op, unique_ptr<Cell> left, const Cell *_right) {
165:         auto right = _right->Eval(*this);
166:         if (!right) return left;
167:         Text &t1 = left->text;
168:         Text &t2 = right->text;
169:         Grid *g1 = left->grid;
170:         Grid *g2 = right->grid;
171:         switch (op->args[0]) {
172:             case 'n':
173:                 if (t1.t.Len() && t2.t.Len()) {
174:                     t1.SetNum(op->runnn(t1.GetNum(), t2.GetNum()));
175:                 } else if (g1 && g2 && g1->xs == g2->xs && g1->ys == g2->ys) {
176:                     Grid *g = new Grid(g1->xs, g1->ys);
177:                     auto c = make_unique<Cell>(nullptr, left.get(), CT_DATA, g);
178:                     loop(x, g->xs) loop(y, g->ys) {
179:                         unique_ptr<Cell> c1 = std::move(g1->C(x, y));
180:                         Cell *c2 = g2->C(x, y).get();
181:                         auto res = Execute(op, std::move(c1), c2);
182:                         res->SetParent(c.get());
183:                         g->C(x, y) = std::move(res);
184:                     }
185:                     return c;
186:                 } else if (g1 && t2.t.Len()) {
187:                     foreachcellingrid(c, g1) {
188:                         unique_ptr<Cell> res = Execute(op, std::move(c), right.get());
189:                         res->SetParent(left.get());
190:                         c = std::move(res);
191:                     }
192:                 }
193:                 break;
194:         }
195:         return left;
196:     }
197: 
198:     // IF is sofar the only ternary
199:     unique_ptr<Cell> Execute(const Operation *op, unique_ptr<Cell> left, const Cell *a,
200:                              const Cell *b) {
201:         Text &l = left->text;
202:         if (!l.t.Len()) return left;
203:         bool cond = l.GetNum() != 0;
204:         return (cond ? a : b)->Eval(*this);
205:     }
206: 
207:     void Eval(const Cell *root) {
208:         root->Eval(*this);
209:         ClearVars();
210:     }
211: };

================
File: src/events.h
================
 1: BEGIN_EVENT_TABLE(treesheets::TSFrame, wxFrame)
 2:   EVT_DPI_CHANGED(treesheets::TSFrame::OnDPIChanged)
 3:   EVT_SIZING(treesheets::TSFrame::OnSizing)
 4:   EVT_MENU(wxID_ANY, treesheets::TSFrame::OnMenu)
 5:   EVT_TEXT(A_SEARCH, treesheets::TSFrame::OnSearch)
 6:   EVT_TEXT_ENTER(A_SEARCH, treesheets::TSFrame::OnSearchReplaceEnter)
 7:   EVT_TEXT_ENTER(A_REPLACE, treesheets::TSFrame::OnSearchReplaceEnter)
 8:   EVT_CLOSE(treesheets::TSFrame::OnClosing)
 9:   EVT_MAXIMIZE(treesheets::TSFrame::OnMaximize)
10:   EVT_ACTIVATE_APP(treesheets::TSFrame::OnActivate)
11:   EVT_COMBOBOX(A_CELLCOLOR, treesheets::TSFrame::OnChangeColor)
12:   EVT_COMBOBOX(A_TEXTCOLOR, treesheets::TSFrame::OnChangeColor)
13:   EVT_COMBOBOX(A_BORDCOLOR, treesheets::TSFrame::OnChangeColor)
14:   EVT_COMBOBOX(A_DDIMAGE,   treesheets::TSFrame::OnDDImage)
15:   EVT_ICONIZE(treesheets::TSFrame::OnIconize)
16:   EVT_AUINOTEBOOK_PAGE_CHANGED(wxID_ANY, treesheets::TSFrame::OnTabChange)
17:   EVT_AUINOTEBOOK_PAGE_CLOSE(wxID_ANY, treesheets::TSFrame::OnTabClose)
18:   EVT_SYS_COLOUR_CHANGED(treesheets::TSFrame::OnSysColourChanged)
19: END_EVENT_TABLE()
20: 
21: BEGIN_EVENT_TABLE(treesheets::TSApp, wxApp)
22: END_EVENT_TABLE()
23: 
24: BEGIN_EVENT_TABLE(treesheets::TSCanvas, wxScrolledWindow)
25:   EVT_MOUSEWHEEL(treesheets::TSCanvas::OnMouseWheel)
26:   EVT_PAINT(treesheets::TSCanvas::OnPaint)
27:   EVT_MOTION(treesheets::TSCanvas::OnMotion)
28:   EVT_LEFT_DOWN(treesheets::TSCanvas::OnLeftDown)
29:   EVT_LEFT_UP(treesheets::TSCanvas::OnLeftUp)
30:   EVT_RIGHT_DOWN(treesheets::TSCanvas::OnRightDown)
31:   EVT_LEFT_DCLICK(treesheets::TSCanvas::OnLeftDoubleClick)
32:   EVT_CHAR(treesheets::TSCanvas::OnChar)
33:   EVT_KEY_DOWN(treesheets::TSCanvas::OnKeyDown)
34:   EVT_CONTEXT_MENU(treesheets::TSCanvas::OnContextMenuClick)
35:   EVT_SIZE(treesheets::TSCanvas::OnSize)
36:   EVT_SCROLLWIN(treesheets::TSCanvas::OnScrollWin)
37: END_EVENT_TABLE()
38: 
39: BEGIN_EVENT_TABLE(treesheets::ThreeChoiceDialog, wxDialog)
40:     EVT_BUTTON(wxID_ANY, treesheets::ThreeChoiceDialog::OnButton)
41: END_EVENT_TABLE()
42: 
43: BEGIN_EVENT_TABLE(treesheets::DateTimeRangeDialog, wxDialog)
44:     EVT_BUTTON(wxID_ANY, treesheets::DateTimeRangeDialog::OnButton)
45: END_EVENT_TABLE()

================
File: src/genpot.bat
================
1: xgettext --keyword=_ --sort-output --no-location -o ../TS/translations/ts.pot tsframe.h document.h system.h wxtools.h

================
File: src/grid.h
================
   1: struct Grid {
   2:     // owning cell.
   3:     Cell *cell;
   4:     // subcells
   5:     vector<unique_ptr<Cell>> cells;
   6:     // widths for each column
   7:     vector<int> colwidths;
   8:     // xsize, ysize
   9:     int xs;
  10:     int ys;
  11:     int view_margin;
  12:     int view_grid_outer_spacing;
  13:     int user_grid_outer_spacing {g_usergridouterspacing_default};
  14:     int cell_margin;
  15:     int bordercolor {g_bordercolor_default};
  16:     bool horiz {false};
  17:     bool tinyborder;
  18:     bool folded {false};
  19: 
  20:     unique_ptr<Cell> &C(int x, int y) {
  21:         ASSERT(x >= 0 && y >= 0 && x < xs && y < ys);
  22:         return cells[x + y * xs];
  23:     }
  24: 
  25:     Cell *C(int x, int y) const {
  26:         ASSERT(x >= 0 && y >= 0 && x < xs && y < ys);
  27:         return cells[x + y * xs].get();
  28:     }
  29: 
  30:     #define foreachcell(c)                \
  31:         for (int y = 0; y < ys; y++)      \
  32:             for (int x = 0; x < xs; x++)  \
  33:                 for (bool _f = true; _f;) \
  34:                     for (unique_ptr<Cell> &c = C(x, y); _f; _f = false)
  35: 
  36:     #define foreachcellconst(c)           \
  37:         for (int y = 0; y < ys; y++)      \
  38:             for (int x = 0; x < xs; x++)  \
  39:                 for (bool _f = true; _f;) \
  40:                     for (Cell *c = C(x, y); _f; _f = false)
  41: 
  42:     #define foreachcellrev(c)                 \
  43:         for (int y = ys - 1; y >= 0; y--)     \
  44:             for (int x = xs - 1; x >= 0; x--) \
  45:                 for (bool _f = true; _f;)     \
  46:                     for (unique_ptr<Cell> &c = C(x, y); _f; _f = false)
  47:     #define foreachcelly(c)           \
  48:         for (int y = 0; y < ys; y++)  \
  49:             for (bool _f = true; _f;) \
  50:                 for (unique_ptr<Cell> &c = C(0, y); _f; _f = false)
  51:     #define foreachcellcolumn(c)          \
  52:         for (int x = 0; x < xs; x++)      \
  53:             for (int y = 0; y < ys; y++)  \
  54:                 for (bool _f = true; _f;) \
  55:                     for (unique_ptr<Cell> &c = C(x, y); _f; _f = false)
  56:     #define foreachcellinsel(c, s)                 \
  57:         for (int y = s.y; y < s.y + s.ys; y++)     \
  58:             for (int x = s.x; x < s.x + s.xs; x++) \
  59:                 for (bool _f = true; _f;)          \
  60:                     for (unique_ptr<Cell> &c = C(x, y); _f; _f = false)
  61:     #define foreachcellinselrev(c, s)                   \
  62:         for (int y = s.y + s.ys - 1; y >= s.y; y--)     \
  63:             for (int x = s.x + s.xs - 1; x >= s.x; x--) \
  64:                 for (bool _f = true; _f;)               \
  65:                     for (unique_ptr<Cell> &c = C(x, y); _f; _f = false)
  66:     #define foreachcellingrid(c, g)         \
  67:         for (int y = 0; y < g->ys; y++)     \
  68:             for (int x = 0; x < g->xs; x++) \
  69:                 for (bool _f = true; _f;)   \
  70:                     for (unique_ptr<Cell> &c = g->C(x, y); _f; _f = false)
  71: 
  72:     Grid(int _xs, int _ys, Cell *_c = nullptr)
  73:         : xs(_xs), ys(_ys), cell(_c), cells(_xs * _ys) {
  74:         InitColWidths();
  75:         SetOrient();
  76:     }
  77: 
  78:     void InitCells(Cell *clonestylefrom = nullptr) {
  79:         foreachcell(c) c = make_unique<Cell>(cell, clonestylefrom);
  80:     }
  81:     void CloneStyleFrom(auto o) {
  82:         bordercolor = o->bordercolor;
  83:         // TODO: what others?
  84:     }
  85: 
  86:     void InitColWidths() {
  87:         colwidths.clear();
  88:         colwidths.resize(xs, cell ? cell->ColWidth() : sys->defaultmaxcolwidth);
  89:     }
  90: 
  91:     /* Clones g into this grid. This mutates the grid this function is called on. */
  92:     void Clone(Grid *g) {
  93:         g->bordercolor = bordercolor;
  94:         g->user_grid_outer_spacing = user_grid_outer_spacing;
  95:         g->folded = folded;
  96:         foreachcell(c) g->C(x, y) = c->Clone(g->cell);
  97:         loop(x, xs) g->colwidths[x] = colwidths[x];
  98:     }
  99: 
 100:     unique_ptr<Cell> CloneSel(const Selection &sel) {
 101:         auto cl = make_unique<Cell>(nullptr, sel.grid->cell, CT_DATA, new Grid(sel.xs, sel.ys));
 102:         foreachcellinsel(c, sel) cl->grid->C(x - sel.x, y - sel.y) = c->Clone(cl.get());
 103:         loop(i, sel.xs) cl->grid->colwidths[i] = sel.grid->colwidths[i];
 104:         return cl;
 105:     }
 106: 
 107:     size_t EstimatedMemoryUse() {
 108:         size_t sum = 0;
 109:         foreachcell(c) sum += c->EstimatedMemoryUse();
 110:         return sizeof(Grid) + xs * ys * sizeof(Cell *) + sum;
 111:     }
 112: 
 113:     void SetOrient() {
 114:         if (xs > ys) horiz = true;
 115:         if (ys > xs) horiz = false;
 116:     }
 117: 
 118:     bool Layout(Document *doc, wxReadOnlyDC &dc, int depth, int &sx, int &sy, int startx, int starty,
 119:                 bool forcetiny) {
 120:         auto xa = new int[xs];
 121:         auto ya = new int[ys];
 122:         loop(i, xs) xa[i] = 0;
 123:         loop(i, ys) ya[i] = 0;
 124:         tinyborder = true;
 125:         foreachcell(c) {
 126:             c->LazyLayout(doc, dc, depth + 1, colwidths[x], forcetiny);
 127:             tinyborder = c->tiny && tinyborder;
 128:             xa[x] = max(xa[x], c->sx);
 129:             ya[y] = max(ya[y], c->sy);
 130:         }
 131:         view_grid_outer_spacing =
 132:             tinyborder || cell->drawstyle != DS_GRID ? 0 : user_grid_outer_spacing;
 133:         view_margin = tinyborder || cell->drawstyle != DS_GRID ? 0 : g_grid_margin;
 134:         cell_margin = tinyborder ? 0 : (cell->drawstyle == DS_GRID ? 0 : g_cell_margin);
 135:         sx = (xs + 1) * g_line_width + xs * cell_margin * 2 +
 136:              2 * (view_grid_outer_spacing + view_margin) + startx;
 137:         sy = (ys + 1) * g_line_width + ys * cell_margin * 2 +
 138:              2 * (view_grid_outer_spacing + view_margin) + starty;
 139:         loop(i, xs) sx += xa[i];
 140:         loop(i, ys) sy += ya[i];
 141:         int cx = view_grid_outer_spacing + view_margin + g_line_width + cell_margin + startx;
 142:         int cy = view_grid_outer_spacing + view_margin + g_line_width + cell_margin + starty;
 143:         if (!cell->tiny) {
 144:             cx += g_margin_extra;
 145:             cy += g_margin_extra;
 146:         }
 147:         foreachcell(c) {
 148:             c->ox = cx;
 149:             c->oy = cy;
 150:             if (c->drawstyle == DS_BLOBLINE && !c->grid) {
 151:                 assert(c->sy <= ya[y]);
 152:                 c->ycenteroff = (ya[y] - c->sy) / 2;
 153:             }
 154:             c->sx = xa[x];
 155:             c->sy = ya[y];
 156:             cx += xa[x] + g_line_width + cell_margin * 2;
 157:             if (x == xs - 1) {
 158:                 cy += ya[y] + g_line_width + cell_margin * 2;
 159:                 cx = view_grid_outer_spacing + view_margin + g_line_width + cell_margin + startx;
 160:                 if (!cell->tiny) cx += g_margin_extra;
 161:             }
 162:         }
 163:         delete[] xa;
 164:         delete[] ya;
 165:         return tinyborder;
 166:     }
 167: 
 168:     void Render(Document *doc, int bx, int by, wxDC &dc, int depth, int sx, int sy, int xoff,
 169:                 int yoff) {
 170:         xoff = C(0, 0)->ox - view_margin - view_grid_outer_spacing - 1;
 171:         yoff = C(0, 0)->oy - view_margin - view_grid_outer_spacing - 1;
 172:         int maxx = C(xs - 1, 0)->ox + C(xs - 1, 0)->sx;
 173:         int maxy = C(0, ys - 1)->oy + C(0, ys - 1)->sy;
 174:         if (tinyborder || cell->drawstyle == DS_GRID) {
 175:             int ldelta = view_grid_outer_spacing != 0;
 176:             auto drawlines = [&]() {
 177:                 for (int x = ldelta; x <= xs - ldelta; x++) {
 178:                     int xl = (x == xs ? maxx : C(x, 0)->ox - g_line_width) + bx;
 179:                     if (xl >= doc->scrollx && xl <= doc->maxx) loop(line, g_line_width) {
 180:                             dc.DrawLine(
 181:                                 xl + line, max(doc->scrolly, by + yoff + view_grid_outer_spacing),
 182:                                 xl + line, min(doc->maxy, by + maxy + g_line_width) + view_margin);
 183:                         }
 184:                 }
 185:                 for (int y = ldelta; y <= ys - ldelta; y++) {
 186:                     int yl = (y == ys ? maxy : C(0, y)->oy - g_line_width) + by;
 187:                     if (yl >= doc->scrolly && yl <= doc->maxy) loop(line, g_line_width) {
 188:                             dc.DrawLine(max(doc->scrollx,
 189:                                             bx + xoff + view_grid_outer_spacing + g_line_width),
 190:                                         yl + line, min(doc->maxx, bx + maxx) + view_margin,
 191:                                         yl + line);
 192:                         }
 193:                 }
 194:             };
 195:             if (!sys->fastrender && view_grid_outer_spacing && cell->cellcolor != 0xFFFFFF) {
 196:                 dc.SetPen(wxPen(LightColor(0xFFFFFF)));
 197:                 drawlines();
 198:             }
 199:             // dotted lines result in very expensive drawline calls
 200:             dc.SetPen(view_grid_outer_spacing && !sys->fastrender ? sys->pen_gridlines
 201:                                                                   : sys->pen_tinygridlines);
 202:             drawlines();
 203:         }
 204: 
 205:         foreachcell(c) {
 206:             int cx = bx + c->ox;
 207:             int cy = by + c->oy;
 208:             if (cx < doc->maxx && cx + c->sx > doc->scrollx && cy < doc->maxy &&
 209:                 cy + c->sy > doc->scrolly) {
 210:                 c->Render(doc, cx, cy, dc, depth + 1, (x == 0) * view_margin,
 211:                           (x == xs - 1) * view_margin, (y == 0) * view_margin,
 212:                           (y == ys - 1) * view_margin, colwidths[x], cell_margin);
 213:             }
 214:         }
 215: 
 216:         if (cell->drawstyle == DS_BLOBLINE && !tinyborder && cell->HasHeader() && !cell->tiny) {
 217:             const int arcsize = 8;
 218:             int srcy = by + cell->ycenteroff +
 219:                        (cell->verticaltextandgrid ? cell->tys + 2 : cell->tys / 2) + g_margin_extra;
 220:             // fixme: the 8 is chosen to fit the smallest text size, not very portable
 221:             int srcx = bx + (cell->verticaltextandgrid ? 8 : cell->txs + 4) + g_margin_extra;
 222:             int destyfirst = -1, destylast = -1;
 223:             dc.SetPen(*wxGREY_PEN);
 224:             foreachcelly(c) if (c->HasContent() && !c->tiny) {
 225:                 int desty = c->ycenteroff + by + c->oy + c->tys / 2 + g_margin_extra;
 226:                 int destx = bx + c->ox - 2 + g_margin_extra;
 227:                 bool visible = srcx < doc->maxx && destx > doc->scrollx &&
 228:                                desty - arcsize < doc->maxy && desty + arcsize > doc->scrolly;
 229:                 if (abs(srcy - desty) < arcsize && !cell->verticaltextandgrid) {
 230:                     if (destyfirst < 0) destyfirst = desty;
 231:                     destylast = desty;
 232:                     if (visible) dc.DrawLine(srcx, desty, destx, desty);
 233:                 } else {
 234:                     if (desty < srcy) {
 235:                         if (destyfirst < 0) destyfirst = desty + arcsize;
 236:                         destylast = desty + arcsize;
 237:                         if (visible) dc.DrawBitmap(sys->frame->line_nw, srcx, desty, true);
 238:                     } else {
 239:                         destylast = desty - arcsize;
 240:                         if (visible)
 241:                             dc.DrawBitmap(sys->frame->line_sw, srcx, desty - arcsize, true);
 242:                         desty--;
 243:                     }
 244:                     if (visible) dc.DrawLine(srcx + arcsize, desty, destx, desty);
 245:                 }
 246:             }
 247:             if (cell->verticaltextandgrid) {
 248:                 if (destylast > 0) dc.DrawLine(srcx, srcy, srcx, destylast);
 249:             } else {
 250:                 if (destyfirst >= 0 && destylast >= 0 && destyfirst < destylast) {
 251:                     destyfirst = min(destyfirst, srcy);
 252:                     destylast = max(destylast, srcy);
 253:                     dc.DrawLine(srcx, destyfirst, srcx, destylast);
 254:                 }
 255:             }
 256:         }
 257:         if (view_grid_outer_spacing && cell->drawstyle == DS_GRID) {
 258:             dc.SetBrush(*wxTRANSPARENT_BRUSH);
 259:             dc.SetPen(wxPen(LightColor(bordercolor)));
 260:             loop(i, view_grid_outer_spacing - 1) {
 261:                 dc.DrawRoundedRectangle(
 262:                     bx + xoff + view_grid_outer_spacing - i,
 263:                     by + yoff + view_grid_outer_spacing - i,
 264:                     maxx - xoff - view_grid_outer_spacing + 1 + i * 2 + view_margin,
 265:                     maxy - yoff - view_grid_outer_spacing + 1 + i * 2 + view_margin,
 266:                     sys->roundness + i);
 267:             }
 268:         }
 269:     }
 270: 
 271:     void FindXY(Document *doc, int px, int py, wxReadOnlyDC &dc) {
 272:         foreachcell(c) {
 273:             int bx = px - c->ox;
 274:             int by = py - c->oy;
 275:             if (bx >= 0 && by >= -g_line_width - g_selmargin && bx < c->sx && by < g_selmargin) {
 276:                 doc->hover = Selection(this, x, y, 1, 0);
 277:                 return;
 278:             }
 279:             if (bx >= 0 && by >= c->sy - g_selmargin && bx < c->sx &&
 280:                 by < c->sy + g_line_width + g_selmargin) {
 281:                 doc->hover = Selection(this, x, y + 1, 1, 0);
 282:                 return;
 283:             }
 284:             if (bx >= -g_line_width - g_selmargin && by >= 0 && bx < g_selmargin && by < c->sy) {
 285:                 doc->hover = Selection(this, x, y, 0, 1);
 286:                 return;
 287:             }
 288:             if (bx >= c->sx - g_selmargin && by >= 0 && bx < c->sx + g_line_width + g_selmargin &&
 289:                 by < c->sy) {
 290:                 doc->hover = Selection(this, x + 1, y, 0, 1);
 291:                 return;
 292:             }
 293:             if (c->IsInside(bx, by)) {
 294:                 if (c->GridShown(doc)) c->grid->FindXY(doc, bx, by, dc);
 295:                 if (doc->hover.grid) return;
 296:                 doc->hover = Selection(this, x, y, 1, 1);
 297:                 if (c->HasText()) {
 298:                     c->text.FindCursor(doc, bx, by - c->ycenteroff, dc, doc->hover, colwidths[x]);
 299:                 }
 300:                 return;
 301:             }
 302:         }
 303:     }
 304: 
 305:     Cell *FindLink(const Selection &sel, Cell *link, Cell *best, bool &lastthis, bool &stylematch,
 306:                    bool forward, bool image) {
 307:         if (forward) {
 308:             foreachcell(c) best =
 309:                 c->FindLink(sel, link, best, lastthis, stylematch, forward, image);
 310:         } else {
 311:             foreachcellrev(c) best =
 312:                 c->FindLink(sel, link, best, lastthis, stylematch, forward, image);
 313:         }
 314:         return best;
 315:     }
 316: 
 317:     Cell *FindNextSearchMatch(const wxString &search, Cell *best, Cell *selected,
 318:                               bool &lastwasselected, bool reverse) {
 319:         if (reverse) {
 320:             foreachcellrev(c) best =
 321:                 c->FindNextSearchMatch(search, best, selected, lastwasselected, reverse);
 322:         } else {
 323:             foreachcell(c) best =
 324:                 c->FindNextSearchMatch(search, best, selected, lastwasselected, reverse);
 325:         }
 326:         return best;
 327:     }
 328: 
 329:     Cell *FindNextFilterMatch(Cell *best, Cell *selected, bool &lastwasselected) {
 330:         foreachcell(c) best = c->FindNextFilterMatch(best, selected, lastwasselected);
 331:         return best;
 332:     }
 333: 
 334:     void FindReplaceAll(const wxString &s, const wxString &ls) {
 335:         foreachcell(c) c->FindReplaceAll(s, ls);
 336:     }
 337: 
 338:     void ReplaceCell(Cell *o, Cell *n) { foreachcell(c) if (c.get() == o) c.reset(n); }
 339:     Selection FindCell(Cell *o) {
 340:         foreachcell(c) if (c.get() == o) return Selection(this, x, y, 1, 1);
 341:         return Selection();
 342:     }
 343: 
 344:     Selection SelectAll() { return Selection(this, 0, 0, xs, ys); }
 345:     void ImageRefCount(bool includefolded) {
 346:         if (includefolded || !folded) foreachcell(c) c->ImageRefCount(includefolded);
 347:     }
 348: 
 349:     void DrawCursor(Document *doc, wxDC &dc, Selection &sel, bool full, uint color) {
 350:         if (auto c = sel.GetCell(); c && !c->tiny && (c->HasText() || !c->grid))
 351:             c->text.DrawCursor(doc, dc, sel, full, color, colwidths[sel.x]);
 352:     }
 353: 
 354:     void DrawInsert(Document *doc, wxDC &dc, Selection &sel, uint colour) {
 355:         dc.SetPen(sys->pen_thinselect);
 356:         if (!sel.xs) {
 357:             auto c = C(sel.x - (sel.x == xs), sel.y).get();
 358:             int x = c->GetX(doc) + (c->sx + g_line_width + cell_margin) * (sel.x == xs) -
 359:                     g_line_width - cell_margin;
 360:             loop(line, g_line_width)
 361:                 dc.DrawLine(x + line, max(cell->GetY(doc), doc->scrolly), x + line,
 362:                             min(cell->GetY(doc) + cell->sy, doc->maxy));
 363:             DrawRectangle(dc, colour, x - 1, c->GetY(doc), g_line_width + 2, c->sy);
 364:         } else {
 365:             auto c = C(sel.x, sel.y - (sel.y == ys)).get();
 366:             int y = c->GetY(doc) + (c->sy + g_line_width + cell_margin) * (sel.y == ys) -
 367:                     g_line_width - cell_margin;
 368:             loop(line, g_line_width)
 369:                 dc.DrawLine(max(cell->GetX(doc), doc->scrollx), y + line,
 370:                             min(cell->GetX(doc) + cell->sx, doc->maxx), y + line);
 371:             DrawRectangle(dc, colour, c->GetX(doc), y - 1, c->sx, g_line_width + 2);
 372:         }
 373:     }
 374: 
 375:     wxRect GetRect(Document *doc, const Selection &sel, bool minimal = false) {
 376:         if (sel.Thin()) {
 377:             if (sel.xs) {
 378:                 if (sel.y < ys) {
 379:                     auto tl = C(sel.x, sel.y).get();
 380:                     return wxRect(tl->GetX(doc), tl->GetY(doc), tl->sx, 0);
 381:                 } else {
 382:                     auto br = C(sel.x, ys - 1).get();
 383:                     return wxRect(br->GetX(doc), br->GetY(doc) + br->sy, br->sx, 0);
 384:                 }
 385:             } else {
 386:                 if (sel.x < xs) {
 387:                     auto tl = C(sel.x, sel.y).get();
 388:                     return wxRect(tl->GetX(doc), tl->GetY(doc), 0, tl->sy);
 389:                 } else {
 390:                     auto br = C(xs - 1, sel.y).get();
 391:                     return wxRect(br->GetX(doc) + br->sx, br->GetY(doc), 0, br->sy);
 392:                 }
 393:             }
 394:         } else {
 395:             auto tl = C(sel.x, sel.y).get();
 396:             auto br = C(sel.x + sel.xs - 1, sel.y + sel.ys - 1).get();
 397:             wxRect r(tl->GetX(doc) - cell_margin, tl->GetY(doc) - cell_margin,
 398:                      br->GetX(doc) + br->sx - tl->GetX(doc) + cell_margin * 2,
 399:                      br->GetY(doc) + br->sy - tl->GetY(doc) + cell_margin * 2);
 400:             if (minimal && tl == br) r.width -= tl->sx - tl->minx;
 401:             return r;
 402:         }
 403:     }
 404: 
 405:     void DrawSelect(Document *doc, wxDC &dc, Selection &sel) {
 406:         if (sel.Thin()) {
 407:             DrawInsert(doc, dc, sel, 0);
 408:         } else {
 409:             dc.SetBrush(wxBrush(LightColor(0x000000)));
 410:             dc.SetPen(wxPen(LightColor(0x000000)));
 411:             wxRect g = GetRect(doc, sel);
 412:             int lw = g_line_width;
 413:             int te = sel.TextEdit();
 414:             dc.DrawRectangle(g.x - 1 - lw, g.y - 1 - lw, g.width + 2 + 2 * lw, 2 + lw - te);
 415:             dc.DrawRectangle(g.x - 1 - lw, g.y - 1 + g.height + te, g.width + 2 + 2 * lw - 5,
 416:                              2 + lw - te);
 417: 
 418:             dc.DrawRectangle(g.x - 1 - lw, g.y + 1 - te, 2 + lw - te, g.height - 2 + 2 * te);
 419:             dc.DrawRectangle(g.x - 1 + g.width + te, g.y + 1 - te, 2 + lw - te,
 420:                              g.height - 2 + 2 * te - 2 - te);
 421: 
 422:             dc.DrawRectangle(g.x + g.width, g.y + g.height - 2, lw + 2, lw + 4);
 423:             dc.DrawRectangle(g.x + g.width - lw - 1, g.y + g.height - 2 + 2 * te, lw + 1,
 424:                              lw + 4 - 2 * te);
 425:             if (sel.TextEdit()) {
 426:                 DrawCursor(doc, dc, sel, true, sys->cursorcolor);
 427:             } else {
 428:                 HintIMELocation(doc, g.x, g.y, g_deftextsize * 1.333, 0);
 429:             }
 430:         }
 431:     }
 432: 
 433:     void DeleteCells(int dx, int dy, int nxs, int nys) {
 434:         vector<unique_ptr<Cell>> ncells;
 435:         ncells.reserve((xs + nxs) * (ys + nys));
 436:         foreachcell(c) if (!(x == dx || y == dy)) ncells.push_back(std::move(c));
 437:         cells = std::move(ncells);
 438:         xs += nxs;
 439:         ys += nys;
 440:         if (dx >= 0) colwidths.erase(colwidths.begin() + dx);
 441:         SetOrient();
 442:     }
 443: 
 444:     void MultiCellDelete(Document *doc, Selection &sel) {
 445:         cell->AddUndo(doc);
 446:         MultiCellDeleteSub(doc, sel);
 447:         doc->canvas->Refresh();
 448:     }
 449: 
 450:     void MultiCellDeleteSub(Document *doc, Selection &sel) {
 451:         foreachcellinsel(c, sel) c->Clear();
 452:         bool delhoriz = true, delvert = true;
 453:         foreachcell(c) {
 454:             if (c->HasContent()) {
 455:                 if (y >= sel.y && y < sel.y + sel.ys) delhoriz = false;
 456:                 if (x >= sel.x && x < sel.x + sel.xs) delvert = false;
 457:             }
 458:         }
 459:         if (delhoriz && (!delvert || sel.xs >= sel.ys)) {
 460:             if (sel.ys == ys) {
 461:                 DelSelf(doc, sel);
 462:             } else {
 463:                 loop(i, sel.ys) DeleteCells(-1, sel.y, 0, -1);
 464:                 sel.ys = 0;
 465:                 sel.xs = 1;
 466:             }
 467:         } else if (delvert) {
 468:             if (sel.xs == xs) {
 469:                 DelSelf(doc, sel);
 470:             } else {
 471:                 loop(i, sel.xs) DeleteCells(sel.x, -1, -1, 0);
 472:                 sel.xs = 0;
 473:                 sel.ys = 1;
 474:             }
 475:         } else {
 476:             auto c = sel.GetCell();
 477:             if (c) sel.EnterEdit(doc);
 478:         }
 479:     }
 480: 
 481:     void DelSelf(Document *doc, Selection &s) {
 482:         if (!doc->drawpath.empty() && doc->drawpath.back().grid == this) {
 483:             doc->drawpath.pop_back();
 484:             doc->currentdrawroot = doc->WalkPath(doc->drawpath);
 485:         }
 486:         if (!cell->parent) return;  // FIXME: deletion of root cell, what would be better?
 487:         s = cell->parent->grid->FindCell(cell);
 488:         auto &pthis = cell->grid;
 489:         DELETEP(pthis);
 490:     }
 491: 
 492:     void InsertCells(int dx, int dy, int nxs, int nys, unique_ptr<Cell> nc = nullptr) {
 493:         vector<unique_ptr<Cell>> ocells = std::move(cells);
 494:         cells.clear();
 495:         cells.resize((xs + nxs) * (ys + nys));
 496: 
 497:         int oxs = xs;
 498:         int oys = ys;
 499:         xs += nxs;
 500:         ys += nys;
 501:         SetOrient();
 502:         int opos = 0;
 503:         foreachcell(c) {
 504:             if (x != dx && y != dy) { c = std::move(ocells[opos++]); }
 505:         }
 506:         foreachcell(c) {
 507:             if (x == dx || y == dy) {
 508:                 if (nc) {
 509:                     c = std::move(nc);
 510:                 } else {
 511:                     int sx = nxs ? max(0, min(dx - 1, xs - 1)) : x;
 512:                     int sy = nxs ? y : max(0, min(dy - 1, ys - 1));
 513:                     Cell *colcell = C(sx, sy).get();
 514:                     c = make_unique<Cell>(cell, colcell);
 515:                     if (colcell) c->text.relsize = colcell->text.relsize;
 516:                 }
 517:             }
 518:         }
 519: 
 520:         if (dx >= 0) colwidths.insert(colwidths.begin() + dx, cell->ColWidth());
 521:     }
 522: 
 523:     void Save(wxDataOutputStream &dos, Cell *ocs) const {
 524:         dos.Write32(xs);
 525:         dos.Write32(ys);
 526:         dos.Write32(bordercolor);
 527:         dos.Write32(user_grid_outer_spacing);
 528:         dos.Write8(cell->verticaltextandgrid);
 529:         dos.Write8(folded);
 530:         loop(x, xs) dos.Write32(colwidths[x]);
 531:         foreachcellconst(c) c->Save(dos, ocs);
 532:     }
 533: 
 534:     bool LoadContents(wxDataInputStream &dis, int &numcells, int &textbytes, Cell *&ics) {
 535:         if (sys->versionlastloaded >= 10) {
 536:             bordercolor = dis.Read32() & 0xFFFFFF;
 537:             user_grid_outer_spacing = dis.Read32();
 538:             if (sys->versionlastloaded >= 11) {
 539:                 cell->verticaltextandgrid = dis.Read8() != 0;
 540:                 if (sys->versionlastloaded >= 13) {
 541:                     if (sys->versionlastloaded >= 16) {
 542:                         folded = dis.Read8() != 0;
 543:                         if (folded && sys->versionlastloaded <= 17) {
 544:                             // Before v18, folding would use the image slot. So if this cell
 545:                             // contains an image, clear it.
 546:                             cell->text.image = nullptr;
 547:                         }
 548:                     }
 549:                     loop(x, xs) colwidths[x] = dis.Read32();
 550:                 }
 551:             }
 552:         }
 553:         foreachcell(c) {
 554:             Cell *rc = Cell::LoadWhich(dis, cell, numcells, textbytes, ics);
 555:             if (!rc) return false;
 556:             c.reset(rc);
 557:         }
 558:         return true;
 559:     }
 560: 
 561:     void Formatter(wxString &r, int format, int indent, const wxString &xml, const wxString &html,
 562:                    const wxString &htmlb) {
 563:         if (format == A_EXPXML) {
 564:             r.Append(' ', indent);
 565:             r.Append(xml);
 566:         } else if (format == A_EXPHTMLT || format == A_EXPHTMLTI || format == A_EXPHTMLTE) {
 567:             r.Append(' ', indent);
 568:             r.Append(html);
 569:         } else if (format == A_EXPHTMLB && !htmlb.IsEmpty()) {
 570:             r.Append(' ', indent);
 571:             r.Append(htmlb);
 572:         }
 573:     }
 574: 
 575:     wxString ToText(int indent, const Selection &sel, int format, Document *doc, bool inheritstyle,
 576:                     Cell *root) {
 577:         return ConvertToText(SelectAll(), indent + 2, format, doc, inheritstyle, root);
 578:     };
 579: 
 580:     wxString ConvertToText(const Selection &sel, int indent, int format, Document *doc,
 581:                            bool inheritstyle, Cell *root) {
 582:         wxString r;
 583:         const int root_grid_spacing = 2;  // Can't be adjusted in editor, so use a default.
 584:         const int font_size = 14 - indent / 2;
 585:         const int grid_border_width =
 586:             cell == doc->root.get() ? root_grid_spacing : user_grid_outer_spacing - 1;
 587: 
 588:         wxString xmlstr("<grid");
 589:         if (folded) xmlstr.Append(wxString::Format(" folded=\"%d\"", folded));
 590:         if (bordercolor != g_bordercolor_default) {
 591:             xmlstr.Append(wxString::Format(" bordercolor=\"0x%06X\"", bordercolor));
 592:         }
 593:         if (user_grid_outer_spacing != g_usergridouterspacing_default) {
 594:             xmlstr.Append(wxString::Format(" outerspacing=\"%d\"", user_grid_outer_spacing));
 595:         }
 596:         xmlstr.Append(">\n");
 597: 
 598:         Formatter(r, format, indent, xmlstr,
 599:                   wxString::Format("<table style=\"border-width: %dpt; font-size: %dpt;\">\n",
 600:                                    grid_border_width, font_size),
 601:                   wxString::Format("<ul style=\"font-size: %dpt;\">\n", font_size));
 602:         foreachcellinsel(c, sel) {
 603:             if (x == sel.x) Formatter(r, format, indent, "<row>\n", "<tr>\n", "");
 604:             r.Append(c->ToText(indent, sel, format, doc, inheritstyle, root));
 605:             if (format == A_EXPCSV) r.Append(x == sel.x + sel.xs - 1 ? '\n' : ',');
 606:             if (x == sel.x + sel.xs - 1) Formatter(r, format, indent, "</row>\n", "</tr>\n", "");
 607:         }
 608:         Formatter(r, format, indent, "</grid>\n", "</table>\n", "</ul>\n");
 609:         return r;
 610:     }
 611: 
 612:     void RelSize(int dir, int zoomdepth) { foreachcell(c) c->RelSize(dir, zoomdepth); }
 613:     void RelSize(int dir, const Selection &sel, int zoomdepth) {
 614:         foreachcellinsel(c, sel) c->RelSize(dir, zoomdepth);
 615:     }
 616:     void SetBorder(int width) {
 617:         user_grid_outer_spacing = width;
 618:     }
 619: 
 620:     int MinRelsize(int rs) {
 621:         foreachcell(c) {
 622:             int crs = c->MinRelsize();
 623:             rs = min(rs, crs);
 624:         }
 625:         return rs;
 626:     }
 627: 
 628:     void ResetChildren() {
 629:         cell->Reset();
 630:         foreachcell(c) c->ResetChildren();
 631:     }
 632: 
 633:     void Move(int dx, int dy, const Selection &sel) {
 634:         if (dx < 0 || dy < 0)
 635:             foreachcellinsel(c, sel) std::swap(c, C((x + dx + xs) % xs, (y + dy + ys) % ys));
 636:         else
 637:             foreachcellinselrev(c, sel) std::swap(c, C((x + dx + xs) % xs, (y + dy + ys) % ys));
 638:     }
 639: 
 640:     void Add(unique_ptr<Cell> c) {
 641:         c->parent = cell;
 642:         if (horiz)
 643:             InsertCells(xs, -1, 1, 0, std::move(c));
 644:         else
 645:             InsertCells(-1, ys, 0, 1, std::move(c));
 646:     }
 647: 
 648:     void MergeWithParent(Grid *p, Selection &sel, Document *doc) {
 649:         cell->grid = nullptr;
 650:         foreachcell(c) {
 651:             if (x + sel.x >= p->xs) p->InsertCells(p->xs, -1, 1, 0);
 652:             if (y + sel.y >= p->ys) p->InsertCells(-1, p->ys, 0, 1);
 653:             Cell *pc = p->C(x + sel.x, y + sel.y).get();
 654:             if (pc->HasContent()) {
 655:                 if (x) p->InsertCells(sel.x + x, -1, 1, 0);
 656:                 pc = p->C(x + sel.x, y + sel.y).get();
 657:                 if (pc->HasContent()) {
 658:                     if (y) p->InsertCells(-1, sel.y + y, 0, 1);
 659:                     pc = p->C(x + sel.x, y + sel.y).get();
 660:                 }
 661:             }
 662:             p->C(x + sel.x, y + sel.y) = std::move(c);
 663:             p->C(x + sel.x, y + sel.y)->parent = p->cell;
 664:         }
 665:         sel.grid = p;
 666:         sel.xs += xs - 1;
 667:         sel.ys += ys - 1;
 668:         sel.ExitEdit(doc);
 669:         delete this;
 670:     }
 671: 
 672:     void SetStyle(Document *doc, const Selection &sel, int sb) {
 673:         cell->AddUndo(doc);
 674:         cell->ResetChildren();
 675:         foreachcellinsel(c, sel) {
 676:             c->text.stylebits ^= sb;
 677:             c->text.WasEdited();
 678:         }
 679:         doc->canvas->Refresh();
 680:     }
 681: 
 682:     void ColorChange(Document *doc, int which, uint color, const Selection &sel) {
 683:         cell->AddUndo(doc);
 684:         cell->ResetChildren();
 685:         foreachcellinsel(c, sel) c->ColorChange(doc, which, color);
 686:         doc->canvas->Refresh();
 687:     }
 688: 
 689:     void ReplaceStr(Document *doc, const wxString &s, const wxString &ls, const Selection &sel) {
 690:         cell->AddUndo(doc);
 691:         cell->ResetChildren();
 692:         foreachcellinsel(c, sel) c->text.ReplaceStr(s, ls);
 693:         doc->canvas->Refresh();
 694:     }
 695: 
 696:     void CSVImport(const wxArrayString &as, const wxString &sep) {
 697:         int cy = 0;
 698:         loop(y, as.size()) {
 699:             auto s = as[y];
 700:             wxString word;
 701:             for (int x = 0; s[0]; x++) {
 702:                 if (s[0] == '\"') {
 703:                     word = "";
 704:                     for (int i = 1;; i++) {
 705:                         if (!s[i]) {
 706:                             if (y < static_cast<int>(as.size()) - 1) {
 707:                                 s = as[++y];
 708:                                 i = 0;
 709:                             } else {
 710:                                 s = L"";
 711:                                 break;
 712:                             }
 713:                         } else if (s[i] == '\"') {
 714:                             if (s[i + 1] == '\"')
 715:                                 word += s[++i];
 716:                             else {
 717:                                 s = s.size() == i + 1 ? wxString(L"") : s.Mid(i + 2);
 718:                                 break;
 719:                             }
 720:                         } else
 721:                             word += s[i];
 722:                     }
 723:                 } else {
 724:                     int pos = s.Find(sep);
 725:                     if (pos < 0) {
 726:                         word = s;
 727:                         s = "";
 728:                     } else {
 729:                         word = s.Left(pos);
 730:                         s = s.Mid(pos + 1);
 731:                     }
 732:                 }
 733:                 if (x >= xs) InsertCells(x, -1, 1, 0);
 734:                 Cell *c = C(x, cy).get();
 735:                 c->text.t = word;
 736:             }
 737:             cy++;
 738:         }
 739: 
 740:         ys = cy;  // throws memory away, but doesn't matter
 741:     }
 742: 
 743:     unique_ptr<Cell> EvalGridCell(auto &ev, unique_ptr<Cell> &c, unique_ptr<Cell> acc, int &x,
 744:                                   int &y, bool &alldata, bool vert) {
 745:         int ct = c->celltype;  // Type of subcell being evaluated
 746:         // Update alldata condition (variable reads act like data)
 747:         alldata = alldata && (ct == CT_DATA || ct == CT_VARU);
 748:         ev.vert = vert;  // Inform evaluatour of vert status. (?)
 749:         switch (ct) {
 750:             // Var assign
 751:             case CT_VARD: {
 752:                 if (vert) return acc;  // (Reject vertical assignments)
 753:                 // If we have no data, lets see if we can generate something useful from the
 754:                 // subgrid.
 755:                 if (!acc->grid && acc->text.t.IsEmpty()) {
 756:                     acc = c->Eval(ev);
 757:                     if (!acc) { return nullptr; }
 758:                 }
 759:                 // Assign the current data temporary to the text
 760:                 ev.Assign(c.get(), acc.get());
 761:                 // Pass the original data onwards
 762:                 return acc;
 763:             }
 764:             // View
 765:             case CT_VIEWV:
 766:             case CT_VIEWH:
 767:                 if (vert ? ct == CT_VIEWH : ct == CT_VIEWV) { return c->Clone(nullptr); }
 768:                 c = acc ? acc->Clone(cell) : make_unique<Cell>(cell);
 769:                 c->celltype = ct;
 770:                 return acc;
 771:             // Operation
 772:             case CT_CODE: {
 773:                 auto op = ev.FindOp(c->text.t);
 774:                 switch (op ? strlen(op->args) : -1) {
 775:                     default: return nullptr;
 776:                     case 0: return ev.Execute(op);
 777:                     case 1: return acc ? ev.Execute(op, std::move(acc)) : nullptr;
 778:                     case 2:
 779:                         if (vert) {
 780:                             if (acc && y + 1 < ys) {
 781:                                 return ev.Execute(op, std::move(acc), C(x, ++y).get());
 782:                             } else {
 783:                                 return nullptr;
 784:                             }
 785:                         } else {
 786:                             if (acc && x + 1 < xs) {
 787:                                 return ev.Execute(op, std::move(acc), C(++x, y).get());
 788:                             } else {
 789:                                 return nullptr;
 790:                             }
 791:                         }
 792:                     case 3:
 793:                         if (vert) {
 794:                             if (acc && y + 2 < ys) {
 795:                                 y += 2;
 796:                                 return ev.Execute(op, std::move(acc), C(x, y - 1).get(),
 797:                                                   C(x, y).get());
 798:                             } else {
 799:                                 return nullptr;
 800:                             }
 801:                         } else {
 802:                             if (acc && x + 2 < xs) {
 803:                                 x += 2;
 804:                                 return ev.Execute(op, std::move(acc), C(x - 1, y).get(),
 805:                                                   C(x, y).get());
 806:                             } else {
 807:                                 return nullptr;
 808:                             }
 809:                         }
 810:                 }
 811:             }
 812:             // Var read, Data
 813:             default: return c->Eval(ev);
 814:         }
 815:     }
 816: 
 817:     unique_ptr<Cell> Eval(auto &ev) {
 818:         unique_ptr<Cell> acc;  // Actual/Accumulating data temporary
 819:         bool alldata = true;   // Is the grid all data?
 820:         // Do left to right processing
 821:         if (xs > 1 || ys == 1) foreachcell(c) {
 822:                 if (x == 0) acc.reset();
 823:                 acc = EvalGridCell(ev, c, std::move(acc), x, y, alldata, false);
 824:             }
 825:         // Do top to bottom processing
 826:         if (ys > 1) foreachcellcolumn(c) {
 827:                 if (y == 0) acc.reset();
 828:                 acc = EvalGridCell(ev, c, std::move(acc), x, y, alldata, true);
 829:             }
 830:         // If all data is true then we can exit now.
 831:         if (alldata) {
 832:             auto result = cell->Clone(nullptr);  // Potential result if all data.
 833:             foreachcellingrid(rc, result->grid) { rc = rc->Eval(ev); }
 834:             return result;
 835:         }
 836:         return acc;
 837:     }
 838: 
 839:     void Split(vector<unique_ptr<Grid>> &gs, bool vert) {
 840:         loop(i, vert ? xs : ys) gs.push_back(make_unique<Grid>(vert ? 1 : xs, vert ? ys : 1));
 841:         foreachcell(c) {
 842:             auto g = gs[vert ? x : y].get();
 843:             c->SetParent(g->cell);
 844:             g->cells[vert ? y : x] = std::move(c);
 845:         }
 846:     }
 847: 
 848:     unique_ptr<Cell> Sum() {
 849:         double total = 0;
 850:         foreachcell(c) {
 851:             if (c->HasText()) total += c->text.GetNum();
 852:         }
 853:         auto c = make_unique<Cell>();
 854:         c->text.SetNum(total);
 855:         return c;
 856:     }
 857: 
 858:     void Transpose() {
 859:         vector<unique_ptr<Cell>> tr(xs * ys);
 860:         foreachcell(c) tr[y + x * ys] = std::move(c);
 861:         cells = std::move(tr);
 862:         swap_(xs, ys);
 863:         SetOrient();
 864:         InitColWidths();
 865:     }
 866: 
 867:     void Sort(Selection &sel, bool descending) {
 868:         vector<int> row_indices(sel.ys);
 869:         loop(i, sel.ys) row_indices[i] = i;
 870: 
 871:         std::stable_sort(row_indices.begin(), row_indices.end(), [&](int i, int j) {
 872:             loop(k, xs) {
 873:                 int col = (k + sel.x) % xs;
 874:                 int cmp = C(col, sel.y + i)->text.t.CmpNoCase(C(col, sel.y + j)->text.t);
 875:                 if (cmp) return descending ? cmp > 0 : cmp < 0;
 876:             }
 877:             return false;
 878:         });
 879: 
 880:         vector<unique_ptr<Cell>> new_cells;
 881:         new_cells.reserve(sel.ys * xs);
 882:         for (int i : row_indices) {
 883:             loop(c, xs) {
 884:                 new_cells.push_back(std::move(C(c, sel.y + i)));
 885:             }
 886:         }
 887: 
 888:         loop(i, sel.ys * xs) {
 889:             cells[sel.y * xs + i] = std::move(new_cells[i]);
 890:         }
 891:     }
 892: 
 893:     Cell *FindExact(const wxString &s) {
 894:         foreachcell(c) {
 895:             auto f = c->FindExact(s);
 896:             if (f) return f;
 897:         }
 898:         return nullptr;
 899:     }
 900: 
 901:     Selection HierarchySwap(wxString tag) {
 902:         Cell *selcell = nullptr;
 903:         bool done = false;
 904:     lookformore:
 905:         foreachcell(c) if (c->grid && !done) {
 906:             auto f = c->grid->FindExact(tag);
 907:             if (f) {
 908:                 // add all parent tags as extra hierarchy inside the cell
 909:                 for (auto p = f->parent; p != cell; p = p->parent) {
 910:                     // Special case check: if parents have same name, this would cause infinite
 911:                     // swapping.
 912:                     if (p->text.t == tag) done = true;
 913:                     auto t = make_unique<Cell>(f, p);
 914:                     t->text = p->text;
 915:                     t->text.cell = t.get();
 916:                     t->note = p->note;
 917:                     t->grid = f->grid;
 918:                     if (t->grid) t->grid->ReParent(t.get());
 919:                     f->grid = new Grid(1, 1);
 920:                     f->grid->cell = f;
 921:                     f->grid->cells[0] = std::move(t);
 922:                 }
 923:                 // remove cell from parent, recursively if parent becomes empty
 924:                 for (auto r = f; r && r != cell; r = r->parent->grid->DeleteTagParent(r, cell, f));
 925:                 // merge newly constructed hierarchy at this level
 926:                 if (!cells[0]) {
 927:                     cells[0].reset(f);
 928:                     f->parent = cell;
 929:                     selcell = f;
 930:                 } else {
 931:                     MergeTagCell(unique_ptr<Cell>(f), selcell);
 932:                 }
 933:                 goto lookformore;
 934:             }
 935:         }
 936:         ASSERT(selcell);
 937:         return FindCell(selcell);
 938:     }
 939: 
 940:     void ReParent(auto p) {
 941:         cell = p;
 942:         foreachcell(c) c->parent = p;
 943:     }
 944: 
 945:     Cell *DeleteTagParent(Cell *tag, Cell *basecell, Cell *found) {
 946:         Cell *next = tag->parent;
 947:         unique_ptr<Cell> detached_tag;
 948:         int found_x = -1, found_y = -1;
 949:         foreachcell(c) if (c.get() == tag) {
 950:             detached_tag = std::move(c);
 951:             found_x = x;
 952:             found_y = y;
 953:             break;
 954:         }
 955:         ASSERT(detached_tag);
 956:         if (xs * ys == 1) {
 957:             if (cell != basecell) {
 958:                 cell->grid = nullptr;
 959:                 delete this;
 960:             }
 961:             if (tag == found) detached_tag.release();
 962:             return next;
 963:         } else {
 964:             if (ys > 1)
 965:                 DeleteCells(-1, found_y, 0, -1);
 966:             else
 967:                 DeleteCells(found_x, -1, -1, 0);
 968:             if (tag == found) detached_tag.release();
 969:             return nullptr;
 970:         }
 971:     }
 972: 
 973:     void MergeTagCell(unique_ptr<Cell> f, Cell *&selcell) {
 974:         foreachcell(c) if (c->text.t == f->text.t) {
 975:             if (!selcell) selcell = c.get();
 976: 
 977:             if (f->grid) {
 978:                 if (c->grid) {
 979:                     f->grid->MergeTagAll(c.get());
 980:                 } else {
 981:                     c->grid = f->grid;
 982:                     c->grid->ReParent(c.get());
 983:                     f->grid = nullptr;
 984:                 }
 985:             }
 986:             return;
 987:         }
 988:         if (!selcell) selcell = f.get();
 989:         Add(std::move(f));
 990:     }
 991: 
 992:     void MergeTagAll(Cell *into) {
 993:         foreachcell(c) {
 994:             into->grid->MergeTagCell(std::move(c), into /*dummy*/);
 995:         }
 996:     }
 997: 
 998:     void SetGridTextLayout(int ds, bool vert, bool noset, const Selection &sel) {
 999:         foreachcellinsel(c, sel) c->SetGridTextLayout(ds, vert, noset);
1000:     }
1001: 
1002:     bool IsTable() {
1003:         foreachcell(c) if (c->grid) return false;
1004:         return true;
1005:     }
1006: 
1007:     void Hierarchify(Document *doc) {
1008:         loop(y, ys) {
1009:             unique_ptr<Cell> rest;
1010:             if (xs > 1) {
1011:                 Selection s(this, 1, y, xs - 1, 1);
1012:                 rest = CloneSel(s);
1013:             }
1014:             Cell *c = C(0, y).get();
1015:             loop(prevy, y) {
1016:                 if (Cell *prev = C(0, prevy).get(); prev->text.t == c->text.t) {
1017:                     if (rest) {
1018:                         ASSERT(prev->grid);
1019:                         prev->grid->MergeRow(rest->grid);
1020:                         rest = nullptr;
1021:                     }
1022: 
1023:                     Selection s(this, 0, y, xs, 1);
1024:                     MultiCellDeleteSub(doc, s);
1025:                     y--;
1026: 
1027:                     goto done;
1028:                 }
1029:             }
1030:             if (rest) {
1031:                 swap_(c->grid, rest->grid);
1032:                 c->grid->ReParent(c);
1033:             }
1034:         done:;
1035:         }
1036:         Selection s(this, 1, 0, xs - 1, ys);
1037:         MultiCellDeleteSub(doc, s);
1038:         foreachcell(c) if (c->grid && c->grid->xs > 1) c->grid->Hierarchify(doc);
1039:     }
1040: 
1041:     void MergeRow(Grid *tm) {
1042:         ASSERT(xs == tm->xs && tm->ys == 1);
1043:         InsertCells(-1, ys, 0, 1);
1044:         loop(x, xs) {
1045:             std::swap(C(x, ys - 1), tm->C(x, 0));
1046:             C(x, ys - 1)->parent = cell;
1047:         }
1048:     }
1049: 
1050:     void MaxDepthLeaves(int curdepth, int &maxdepth, int &leaves) {
1051:         foreachcell(c) c->MaxDepthLeaves(curdepth, maxdepth, leaves);
1052:     }
1053: 
1054:     int Flatten(int curdepth, int cury, Grid *g) {
1055:         foreachcell(c) if (c->grid) { cury = c->grid->Flatten(curdepth + 1, cury, g); }
1056:         else {
1057:             Cell *ic = c.get();
1058:             for (int i = curdepth; i >= 0; i--) {
1059:                 Cell *dest = g->C(i, cury).get();
1060:                 dest->text = ic->text;
1061:                 dest->text.cell = dest;
1062:                 ic = ic->parent;
1063:             }
1064:             cury++;
1065:         }
1066:         return cury;
1067:     }
1068: 
1069:     void ResizeColWidths(int dir, const Selection &sel, bool hierarchical) {
1070:         for (int x = sel.x; x < sel.x + sel.xs; x++) {
1071:             colwidths[x] += dir * 5;
1072:             if (colwidths[x] < 5) colwidths[x] = 5;
1073:             loop(y, ys) {
1074:                 if (C(x, y)->grid && hierarchical)
1075:                     C(x, y)->grid->ResizeColWidths(dir, C(x, y)->grid->SelectAll(), hierarchical);
1076:             }
1077:         }
1078:     }
1079: 
1080:     int GetColWidth(Cell *ct) {
1081:         foreachcell(c) {
1082:             if (c.get() == ct) return colwidths[x];
1083:         }
1084:         return 0;
1085:     }
1086: 
1087:     void SetColWidth(Cell *ct, int w) {
1088:         foreachcell(c) {
1089:             if (c.get() == ct) {
1090:                 colwidths[x] = w;
1091:                 return;
1092:             }
1093:         }
1094:     }
1095: 
1096:     void CollectCells(auto &itercells) { foreachcell(c) c->CollectCells(itercells); }
1097:     void CollectCellsSel(auto &itercells, const Selection &sel, bool recurse) {
1098:         foreachcellinsel(c, sel) c->CollectCells(itercells, recurse);
1099:     }
1100: 
1101:     void SetStyles(const Selection &sel, Cell *o) {
1102:         foreachcellinsel(c, sel) {
1103:             c->cellcolor = o->cellcolor;
1104:             c->textcolor = o->textcolor;
1105:             c->text.stylebits = o->text.stylebits;
1106:             c->text.image = o->text.image;
1107:             c->note = o->note;
1108:         }
1109:     }
1110: 
1111:     void ClearImages(const Selection &sel) { foreachcellinsel(c, sel) c->text.image = nullptr; }
1112: };

================
File: src/image.h
================
 1: struct Image {
 2:     vector<uint8_t> data;
 3:     char type;
 4:     wxBitmap bm_display;
 5:     int trefc {0};
 6:     int savedindex {-1};
 7:     uint64_t hash {0};
 8: 
 9:     // This indicates a relative scale, where 1.0 means bitmap pixels match display pixels on
10:     // a low res 96 dpi display. On a high dpi screen it will look scaled up. Higher values
11:     // look better on most screens.
12:     // This is all relative to GetContentScalingFactor.
13:     double display_scale;
14:     int pixel_width {0};
15: 
16:     Image(auto _hash, auto _sc, auto &&_data, auto _type)
17:         : hash(_hash), display_scale(_sc), data(std::move(_data)), type(_type) {}
18: 
19:     void ImageRescale(double scale) {
20:         auto &[it, mime] = imagetypes.at(type);
21:         auto im = ConvertBufferToWxImage(data, it);
22:         im.Rescale(im.GetWidth() * scale, im.GetHeight() * scale);
23:         data = ConvertWxImageToBuffer(im, it);
24:         hash = CalculateHash(data);
25:         bm_display = wxNullBitmap;
26:     }
27: 
28:     void DisplayScale(double scale) {
29:         display_scale /= scale;
30:         bm_display = wxNullBitmap;
31:     }
32: 
33:     void ResetScale(double scale) {
34:         display_scale = scale;
35:         bm_display = wxNullBitmap;
36:     }
37: 
38:     wxBitmap &Display() {
39:         // This might run in multiple threads in parallel
40:         // so this function must not touch any global resources
41:         // and callees must be thread-safe.
42:         if (!bm_display.IsOk()) {
43:             auto &[it, mime] = imagetypes.at(type);
44:             auto bm = ConvertBufferToWxBitmap(data, it);
45:             pixel_width = bm.GetWidth();
46:             ScaleBitmap(bm, sys->frame->FromDIP(1.0) / display_scale, bm_display);
47:         }
48:         return bm_display;
49:     }
50: 
51:     bool ExportToDirectory(const wxString &directory) {
52:         wxString targetname = directory + wxString::Format("%llu", hash) + GetFileExtension();
53:         wxFFileOutputStream os(targetname, "w+b");
54:         if (!os.IsOk()) {
55:             wxMessageBox(_("Error writing image file!"), targetname.wx_str(), wxOK, sys->frame);
56:             return false;
57:         }
58:         os.Write(data.data(), data.size());
59:         return true;
60:     }
61: 
62:     wxString GetFileExtension() {
63:         switch (type) {
64:             case 'J': return ".jpg";
65:             case 'I':
66:             default: return ".png";
67:         }
68:     }
69: };

================
File: src/lobster_impl.cpp
================
  1: #include "lobster/stdafx.h"
  2: 
  3: #include "script_interface.h"
  4: 
  5: #include "lobster/compiler.h"
  6: 
  7: using namespace lobster;
  8: 
  9: namespace script {
 10: 
 11: ScriptInterface *si = nullptr;
 12: 
 13: void AddTreeSheets(NativeRegistry &nfr) {
 14: nfr("goto_root", "", "", "",
 15:     "makes the root of the document the current cell. this is the default at the start "
 16:     "of any script, so this function is only needed to return there.",
 17:     [](StackPtr &, VM &) {
 18:         si->GoToRoot();
 19:         return NilVal();
 20:     });
 21: 
 22: nfr("goto_view", "", "", "", "makes what the user has zoomed into the current cell",
 23:     [](StackPtr &, VM &) {
 24:         si->GoToView();
 25:         return NilVal();
 26:     });
 27: 
 28: nfr("has_selection", "", "", "I", "whether there is a selection",
 29:     [](StackPtr &, VM &) { return Value(si->HasSelection()); });
 30: 
 31: nfr("goto_selection", "", "", "",
 32:     "makes the current cell the one selected, or the first of a selection",
 33:     [](StackPtr &, VM &) {
 34:         si->GoToSelection();
 35:         return NilVal();
 36:     });
 37: 
 38: nfr("has_parent", "", "", "I", "whether the current cell has a parent (is the root cell)",
 39:     [](StackPtr &, VM &) { return Value(si->HasParent()); });
 40: 
 41: nfr("goto_parent", "", "", "", "makes the current cell the parent of the current cell, if any",
 42:     [](StackPtr &, VM &) {
 43:         si->GoToParent();
 44:         return NilVal();
 45:     });
 46: 
 47: nfr("num_children", "", "", "I",
 48:     "returns the total number of children of the current cell (rows * columns). "
 49:     "returns 0 if this cell doesn't have a sub-grid at all.",
 50:     [](StackPtr &, VM &) { return Value(si->NumChildren()); });
 51: 
 52: nfr("num_columns_rows", "", "", "I}:2",
 53:     "returns the number of columns and rows in the current cell",
 54:     [](StackPtr &sp, VM &) { PushVec(sp, int2(si->NumColumnsRows())); });
 55: 
 56: nfr("selection", "", "", "I}:2I}:2",
 57:     "returns the (xs,ys) and (x,y) of the current selection, or zeroes if none",
 58:     [](StackPtr &sp, VM &) {
 59:         auto b = si->SelectionBox();
 60:         PushVec(sp, int2(b.second));
 61:         PushVec(sp, int2(b.first));
 62:     });
 63: 
 64: nfr("goto_child", "n", "I", "", "makes the current cell the nth child of the current cell",
 65:     [](StackPtr &, VM &, Value n) {
 66:         si->GoToChild(n.intval());
 67:         return NilVal();
 68:     });
 69: 
 70: nfr("goto_column_row", "col,row", "II", "", "makes the current cell the child at col / row",
 71:     [](StackPtr &, VM &, Value x, Value y) {
 72:         si->GoToColumnRow(x.intval(), y.intval());
 73:         return NilVal();
 74:     });
 75: 
 76: nfr("get_text", "", "", "S", "gets the text of the current cell.",
 77:     [](StackPtr &, VM &vm) { return Value(vm.NewString(si->GetText())); });
 78: 
 79: nfr("get_note", "", "", "S", "gets the note of the current cell.",
 80:     [](StackPtr &, VM &vm) { return Value(vm.NewString(si->GetNote())); });
 81: 
 82: nfr("set_text", "text", "S", "", "sets the text of the current cell",
 83:     [](StackPtr &, VM &, Value s) {
 84:         si->SetText(s.sval()->strv());
 85:         return NilVal();
 86:     });
 87: 
 88: nfr("set_note", "text", "S", "", "sets the note of the current cell",
 89:     [](StackPtr &, VM &, Value s) {
 90:         si->SetNote(s.sval()->strv());
 91:         return NilVal();
 92:     });
 93: 
 94: nfr("create_grid", "cols,rows", "II", "",
 95:     "creates a grid in the current cell if there is not one yet",
 96:     [](StackPtr &, VM &, Value x, Value y) {
 97:         si->CreateGrid(x.intval(), y.intval());
 98:         return NilVal();
 99:     });
100: 
101: nfr("insert_column", "c", "I", "", "inserts a column before column c in an existing grid",
102:     [](StackPtr &, VM &, Value x) {
103:         si->InsertColumn(x.intval());
104:         return NilVal();
105:     });
106: 
107: nfr("insert_row", "r", "I", "", "inserts a row before row r in an existing grid",
108:     [](StackPtr &, VM &, Value x) {
109:         si->InsertRow(x.intval());
110:         return NilVal();
111:     });
112: 
113: nfr("delete", "position,size", "I}:2I}:2", "",
114:     "clears the cells denoted by position/size. also removes columns/rows if they become "
115:     "completely empty, or the entire grid.",
116:     [](StackPtr &sp, VM &) {
117:         auto s = PopVec<int2>(sp);
118:         auto p = PopVec<int2>(sp);
119:         si->Delete(p.x, p.y, s.x, s.y);
120:     });
121: 
122: nfr("set_background_color", "color", "F}:4", "", "sets the background color of the current cell",
123:     [](StackPtr &sp, VM &) {
124:         auto col = PopVec<float3>(sp);
125:         si->SetBackgroundColor(*(uint32_t *)quantizec(col, 0.0f).data());
126:     });
127: 
128: nfr("set_text_color", "color", "F}:4", "", "sets the text color of the current cell",
129:     [](StackPtr &sp, VM &) {
130:         auto col = PopVec<float3>(sp);
131:         si->SetTextColor(*(uint32_t *)quantizec(col, 0.0f).data());
132:     });
133: 
134: nfr("set_text_filtered", "filtered", "B", "", "sets the text filtered of the current cell",
135:     [](StackPtr &, VM &, Value filtered) {
136:         si->SetTextFiltered(filtered.True());
137:         return NilVal();
138:     });
139: 
140: nfr("is_text_filtered", "", "", "B", "whether the text of the current cell is filtered",
141:     [](StackPtr &, VM &) { return Value(si->IsTextFiltered()); });
142: 
143: nfr("set_border_color", "color", "F}:4", "", "sets the border color of the current grid",
144:     [](StackPtr &sp, VM &) {
145:         auto col = PopVec<float3>(sp);
146:         si->SetBorderColor(*(uint32_t *)quantizec(col, 0.0f).data());
147:     });
148: 
149: nfr("get_relative_size", "", "", "I", "returns the relative text size of the current cell",
150:     [](StackPtr &, VM &) { return Value(si->GetRelativeSize()); });
151: 
152: nfr("set_relative_size", "size", "I", "",
153:     "sets the relative size (0 is normal, -1 is smaller etc.) of the current cell",
154:     [](StackPtr &, VM &, Value s) {
155:         si->SetRelativeSize(geom::clamp(s.intval(), -10, 10));
156:         return NilVal();
157:     });
158: 
159: nfr("set_style_bits", "stylebits", "I", "",
160:     "sets one or more styles (bold = 1, italic = 2, fixed = 4, underline = 8,"
161:     " strikethru = 16) on the current cell",
162:     [](StackPtr &, VM &, Value s) {
163:         si->SetStyle(s.intval());
164:         return NilVal();
165:     });
166: 
167: nfr("get_style_bits", "", "", "I", "returns the stylebits of the current cell",
168:     [](StackPtr &, VM &) { return Value(si->GetStyle()); });
169: 
170: nfr("set_status_message", "message", "S", "", "sets the status message in TreeSheets",
171:     [](StackPtr &, VM &, Value s) {
172:         si->SetStatusMessage(s.sval()->strv());
173:         return NilVal();
174:     });
175: 
176: nfr("get_filename_from_user", "is_save", "I", "S",
177:     "gets a filename using a file dialog. empty string if cancelled.",
178:     [](StackPtr &, VM &vm, Value is_save) {
179:         return Value(vm.NewString(si->GetFileNameFromUser(is_save.True())));
180:     });
181: 
182: nfr("get_filename", "", "", "S", "gets the current documents file name",
183:     [](StackPtr &, VM &vm) { return Value(vm.NewString(si->GetFileName())); });
184: 
185: nfr("load_document", "filename", "S", "B",
186:     "loads a document, and makes it the active one. returns false if failed.",
187:     [](StackPtr &, VM &, Value filename) {
188:         return Value(si->LoadDocument(filename.sval()->data()));
189:     });
190: 
191: nfr("set_window_size", "width,height", "II", "", "resizes the window",
192:     [](StackPtr &, VM &, Value w, Value h) {
193:         si->SetWindowSize(w.intval(), h.intval());
194:         return NilVal();
195:     });
196: 
197: nfr("get_last_edit", "", "", "I",
198:     "gets the timestamp of the last edit in milliseconds since the Unix/C epoch",
199:     [](StackPtr &, VM &) { return Value(si->GetLastEdit()); });
200: 
201: nfr("get_current_time", "", "", "I",
202:     "gets the current timestamp in milliseconds since the Unix/C epoch", [](StackPtr &, VM &) {
203:         auto now = std::chrono::system_clock::now();
204:         auto duration = now.time_since_epoch();
205:         auto milliseconds = std::chrono::duration_cast<std::chrono::milliseconds>(duration).count();
206:         return Value(milliseconds);
207:     });
208: 
209: nfr("is_tag", "", "", "B", "whether the current cell text is a tag",
210:     [](StackPtr &, VM &) {
211:         return Value(si->IsTag());
212:     });
213: 
214: nfr("has_image", "", "", "B", "whether the current cell has an image",
215:     [](StackPtr &, VM &) { return Value(si->HasImage()); });
216: 
217: nfr("get_column_width", "", "", "I", "get the column width of the current cell",
218:     [](StackPtr &, VM &) {
219:         return Value(si->GetColWidth());
220:     });
221: 
222: nfr("set_column_width", "width", "I", "", "set the column width of the current cell",
223:     [](StackPtr &, VM &, Value w) {
224:         si->SetColWidth(w.intval());
225:         return NilVal();
226:     });
227: 
228: nfr("remove_image", "", "", "", "remove image in the current cell",
229:     [](StackPtr &, VM &) {
230:         si->RemoveImage();
231:         return NilVal();
232:     });
233: 
234: nfr("set_image", "filename", "S", "B", "set image for the current cell",
235:     [](StackPtr &, VM &, Value filename) { return Value(si->SetImage(filename.sval()->data())); });
236: }
237: 
238: NativeRegistry natreg;  // FIXME: global.
239: 
240: string InitLobster(ScriptInterface *_si, const char *exefilepath, const char *auxfilepath,
241:                    bool from_bundle, FileLoader sl) {
242:     si = _si;
243:     min_output_level = OUTPUT_PROGRAM;
244:     string err;
245:     try {
246:         InitPlatform(exefilepath, auxfilepath, from_bundle, sl);
247:         RegisterBuiltin(natreg, "ts", "treesheets", AddTreeSheets);
248:         RegisterCoreLanguageBuiltins(natreg);
249:     } catch (string &s) { err = s; }
250:     return err;
251: }
252: 
253: string RunLobster(std::string_view filename, std::string_view code, bool dump_builtins) {
254:     string err;
255:     try {
256:         string bytecode;
257:         string codegen;
258:         Compile(natreg, filename, code, bytecode, nullptr, nullptr, false, RUNTIME_ASSERT, nullptr,
259:                 1, false, true, codegen, false, filename);
260:         auto ret = RunTCC(natreg, bytecode, filename, nullptr, {}, false, err, RUNTIME_ASSERT, true,
261:                           false, codegen);
262:     } catch (string &s) {
263:         err = s;
264:     }
265:     return err;
266: }
267: 
268: void TSDumpBuiltinDoc() { DumpBuiltinDoc(natreg, true); }
269: 
270: }  // namespace script
271: 
272: namespace lobster {
273: 
274: FileLoader EnginePreInit(NativeRegistry &nfr) {
275:     // nfr.DoneRegistering();
276:     return DefaultLoadFile;
277: }
278: 
279: }  // namespace lobster
280: 
281: extern "C" void GLFrame(StackPtr, VM &) {}
282: string BreakPoint(lobster::VM &vm, string_view reason) { return {}; }

================
File: src/main.cpp
================
  1: #include "stdafx.h"
  2: 
  3: static const auto TS_VERSION = 25;
  4: static const auto g_grid_margin = 1;
  5: static const auto g_cell_margin = 2;
  6: static const auto g_margin_extra = 2;  // TODO, could make this configurable: 0/2/4/6
  7: static const auto g_usergridouterspacing_default = 3;
  8: static const auto g_line_width = 1;
  9: static const auto g_selmargin = 2;
 10: static const auto g_scrollratecursor = 240;  // FIXME: must be configurable
 11: static const auto g_scrollratewheel = 2;  // relative to 1 step on a fixed wheel usually being 120
 12: static const auto g_max_launches = 20;
 13: static const auto g_deftextsize_default = 12;
 14: static const auto g_mintextsize_delta = 8;
 15: static const auto g_maxtextsize_delta = 32;
 16: static const auto BLINK_TIME = 400;
 17: static const auto CUSTOMCOLORIDX = 0;
 18: static const auto TS_SELECTION_MASK = 0x80;
 19: static const auto g_bordercolor_default = 0xA0A0A0;
 20: static const auto g_cellcolor_default = 0xFFFFFFU;
 21: static const auto g_textcolor_default = 0x000000U;
 22: static const auto g_tagcolor_default = 0xFF0000;
 23: static const std::array<uint, 42> celltextcolors = {
 24:     0xFFFFFF,  // CUSTOM COLOR!
 25:     0xFFFFFF, 0x000000, 0x202020, 0x404040, 0x606060, 0x808080, 0xA0A0A0, 0xC0C0C0, 0xD0D0D0,
 26:     0xE0E0E0, 0xE8E8E8, 0x000080, 0x0000FF, 0x8080FF, 0xC0C0FF, 0xC0C0E0, 0x008000, 0x00FF00,
 27:     0x80FF80, 0xC0FFC0, 0xC0E0C0, 0x800000, 0xFF0000, 0xFF8080, 0xFFC0C0, 0xE0C0C0, 0x800080,
 28:     0xFF00FF, 0xFF80FF, 0xFFC0FF, 0xE0C0E0, 0x008080, 0x00FFFF, 0x80FFFF, 0xC0FFFF, 0xC0E0E0,
 29:     0x808000, 0xFFFF00, 0xFFFF80, 0xFFFFC0, 0xE0E0C0,
 30: };
 31: 
 32: static const std::map<char, pair<wxBitmapType, wxString>> imagetypes = {
 33:     {'I', {wxBITMAP_TYPE_PNG, "image/png"}}, {'J', {wxBITMAP_TYPE_JPEG, "image/jpeg"}}};
 34: 
 35: static auto g_deftextsize = g_deftextsize_default;
 36: static int g_mintextsize() { return g_deftextsize - g_mintextsize_delta; }
 37: static int g_maxtextsize() { return g_deftextsize + g_maxtextsize_delta; }
 38: 
 39: enum { TS_TEXT = 0, TS_GRID = 1, TS_BOTH = 2, TS_NEITHER = 3 };
 40: 
 41: enum {
 42:     A_SAVEALL = 500,
 43:     A_COLLAPSE,
 44:     A_NEWGRID,
 45:     A_CLRVIEW,
 46:     A_MARKDATA,
 47:     A_MARKVIEWH,
 48:     A_MARKVIEWV,
 49:     A_MARKCODE,
 50:     A_IMAGE,
 51:     A_EXPIMAGE,
 52:     A_EXPSVG,
 53:     A_EXPXML,
 54:     A_EXPHTMLT,
 55:     A_EXPHTMLTI,
 56:     A_EXPHTMLTE,
 57:     A_EXPHTMLO,
 58:     A_EXPHTMLB,
 59:     A_EXPTEXT,
 60:     A_ZOOMIN,
 61:     A_ZOOMOUT,
 62:     A_TRANSPOSE,
 63:     A_DELETE,
 64:     A_BACKSPACE,
 65:     A_DELETE_WORD,
 66:     A_BACKSPACE_WORD,
 67:     A_LEFT,
 68:     A_RIGHT,
 69:     A_UP,
 70:     A_DOWN,
 71:     A_MLEFT,
 72:     A_MRIGHT,
 73:     A_MUP,
 74:     A_MDOWN,
 75:     A_SLEFT,
 76:     A_SRIGHT,
 77:     A_SUP,
 78:     A_SDOWN,
 79:     A_ALEFT,
 80:     A_ARIGHT,
 81:     A_AUP,
 82:     A_ADOWN,
 83:     A_SCLEFT,
 84:     A_SCRIGHT,
 85:     A_SCUP,
 86:     A_SCDOWN,
 87:     A_IMPXML,
 88:     A_IMPXMLA,
 89:     A_IMPTXTI,
 90:     A_IMPTXTC,
 91:     A_IMPTXTS,
 92:     A_IMPTXTT,
 93:     A_TUTORIALWEBPAGE,
 94:     #ifdef ENABLE_LOBSTER
 95:         A_SCRIPTREFERENCE,
 96:     #endif
 97:     A_MARKVARD,
 98:     A_MARKVARU,
 99:     A_SHOWSBAR,
100:     A_SHOWTBAR,
101:     A_LEFTTABS,
102:     A_TRADSCROLL,
103:     A_HOME,
104:     A_END,
105:     A_CHOME,
106:     A_CEND,
107:     A_PAGESETUP,
108:     A_PRINTSCALE,
109:     A_NEXT,
110:     A_PREV,
111:     A_TT,
112:     A_SEARCH,
113:     A_CASESENSITIVESEARCH,
114:     A_CLEARSEARCH,
115:     A_CLEARREPLACE,
116:     A_REPLACE,
117:     A_REPLACEONCE,
118:     A_REPLACEONCEJ,
119:     A_REPLACEALL,
120:     A_CANCELEDIT,
121:     A_BROWSE,
122:     A_ENTERCELL,
123:     A_ENTERCELL_JUMPTOSTART,
124:     A_ENTERCELL_JUMPTOEND,
125:     A_PROGRESSCELL,  // see
126:                      // https://github.com/aardappel/treesheets/issues/139#issuecomment-544167524
127:     A_CELLCOLOR,
128:     A_TEXTCOLOR,
129:     A_BORDCOLOR,
130:     A_INCSIZE,
131:     A_DECSIZE,
132:     A_INCWIDTH,
133:     A_DECWIDTH,
134:     A_ENTERGRID,
135:     A_LINK,
136:     A_LINKREV,
137:     A_LINKIMG,
138:     A_LINKIMGREV,
139:     A_SEARCHNEXT,
140:     A_SEARCHPREV,
141:     A_CUSTCOL,
142:     A_COLCELL,
143:     A_SORT,
144:     A_MAKEBAKS,
145:     A_TOTRAY,
146:     A_AUTOSAVE,
147:     A_FULLSCREEN,
148:     A_SCALED,
149:     A_SCOLS,
150:     A_SROWS,
151:     A_SHOME,
152:     A_SEND,
153:     A_BORD0,
154:     A_BORD1,
155:     A_BORD2,
156:     A_BORD3,
157:     A_BORD4,
158:     A_BORD5,
159:     A_HSWAP,
160:     A_TEXTGRID,
161:     A_TAGADD,
162:     A_TAGREMOVE,
163:     A_WRAP,
164:     A_HIFY,
165:     A_FLATTEN,
166:     A_BROWSEF,
167:     A_ROUND0,
168:     A_ROUND1,
169:     A_ROUND2,
170:     A_ROUND3,
171:     A_ROUND4,
172:     A_ROUND5,
173:     A_ROUND6,
174:     A_FILTER5,
175:     A_FILTER10,
176:     A_FILTER20,
177:     A_FILTER50,
178:     A_FILTERM,
179:     A_FILTERL,
180:     A_FILTERS,
181:     A_FILTEROFF,
182:     A_FILTERNOTE,
183:     A_FILTERBYCELLBG,
184:     A_FILTERMATCHNEXT,
185:     A_FILTERRANGE,
186:     A_FILTERDIALOG,
187:     A_FASTRENDER,
188:     A_INVERTRENDER,
189:     A_EXPCSV,
190:     A_PASTESTYLE,
191:     A_PREVFILE,
192:     A_NEXTFILE,
193:     A_IMAGER,
194:     A_INCWIDTHNH,
195:     A_DECWIDTHNH,
196:     A_ZOOMSCR,
197:     A_V_GS,
198:     A_V_BS,
199:     A_V_LS,
200:     A_H_GS,
201:     A_H_BS,
202:     A_H_LS,
203:     A_GS,
204:     A_BS,
205:     A_LS,
206:     A_RESETSIZE,
207:     A_RESETWIDTH,
208:     A_RESETSTYLE,
209:     A_RESETCOLOR,
210:     A_LASTCELLCOLOR,
211:     A_LASTTEXTCOLOR,
212:     A_LASTBORDCOLOR,
213:     A_LASTIMAGE,
214:     A_OPENCELLCOLOR,
215:     A_OPENTEXTCOLOR,
216:     A_OPENBORDCOLOR,
217:     A_OPENIMGDROPDOWN,
218:     A_DDIMAGE,
219:     A_MINCLOSE,
220:     A_SINGLETRAY,
221:     A_STARTMINIMIZED,
222:     A_CENTERED,
223:     A_SORTD,
224:     A_FOLD,
225:     A_FOLDALL,
226:     A_UNFOLDALL,
227:     A_IMAGESCP,
228:     A_IMAGESCW,
229:     A_IMAGESCF,
230:     A_IMAGESCN,
231:     A_IMAGESVA,
232:     A_SAVE_AS_JPEG,
233:     A_SAVE_AS_PNG,
234:     A_HELP_OP_REF,
235:     A_FSWATCH,
236:     A_DEFBGCOL,
237:     A_DEFCURCOL,
238:     A_RESETPERSPECTIVE,
239:     A_THINSELC,
240:     A_COPYCT,
241:     A_COPYBM,
242:     A_COPYWI,
243:     A_MINISIZE,
244:     A_CUSTKEY,
245:     A_SETLANG,
246:     A_AUTOEXPORT_HTML_NONE,
247:     A_AUTOEXPORT_HTML_WITH_IMAGES,
248:     A_AUTOEXPORT_HTML_WITHOUT_IMAGES,
249:     A_DRAGANDDROP,
250:     A_DEFAULTMAXCOLWIDTH,
251:     #ifdef ENABLE_LOBSTER
252:         A_ADDSCRIPT,
253:         A_DETSCRIPT,
254:     #endif
255:     A_SET_FIXED_FONT,
256:     A_EDITNOTE,
257:     A_NOP,
258:     A_TAGSET = 1000,  // and all values from here on
259:     #ifdef ENABLE_LOBSTER
260:         A_SCRIPT = 2000,  // and all values from here on
261:     #endif
262:     A_MAXACTION = 3000
263: };
264: 
265: enum {
266:     STYLE_BOLD = 1,
267:     STYLE_ITALIC = 2,
268:     STYLE_FIXED = 4,
269:     STYLE_UNDERLINE = 8,
270:     STYLE_STRIKETHRU = 16
271: };
272: 
273: enum { TEXT_SPACE = 3, TEXT_SEP = 2, TEXT_CHAR = 1 };
274: 
275: #ifdef ENABLE_LOBSTER
276: 
277:     // script_interface.h is both used by TreeSheets and lobster-impl
278:     // and uses data types that are already defined by lobster.
279: 
280:     // Define these data types separately on the TreeSheets side here
281:     // to avoid redefinitions.
282: 
283:     struct string_view_nt {
284:         string_view sv;
285:         string_view_nt(const string &s) : sv(s) {}
286:         explicit string_view_nt(const char *s) : sv(s) {}
287:         explicit string_view_nt(string_view osv) : sv(osv) { check_null_terminated(); }
288:         void check_null_terminated() const { assert(!sv.data()[sv.size()]); }
289:         size_t size() const { return sv.size(); }
290:         const char *data() const { return sv.data(); }
291:         const char *c_str() const {
292:             check_null_terminated();  // Catch appends to parent buffer since construction.
293:             return sv.data();
294:         }
295:     };
296: 
297:     using FileLoader = int64_t (*)(string_view_nt absfilename, std::string *dest, int64_t start,
298:                                    int64_t len);
299: 
300:     #include "script_interface.h"
301: 
302:     using namespace script;
303: 
304: #endif
305: 
306: struct treesheets {
307:     #ifdef ENABLE_LOBSTER
308:         struct TreeSheetsScriptImpl;
309:     #endif
310: 
311:     struct Image;
312:     struct Text;
313:     struct Cell;
314:     struct Grid;
315:     struct Selection;
316:     struct Document;
317:     struct Evaluator;
318: 
319:     struct System;
320:     struct TSCanvas;
321:     struct TSFrame;
322:     struct TSApp;
323: 
324:     static System *sys;
325: 
326:     #ifdef ENABLE_LOBSTER
327:         #include "treesheets_impl.h"
328:     #endif
329: 
330:     #include "image.h"
331:     #include "text.h"
332:     #include "cell.h"
333:     #include "grid.h"
334:     #include "selection.h"
335:     #include "document.h"
336:     #include "evaluator.h"
337: 
338:     #include "system.h"
339: 
340:     #include "wxtools.h"
341:     #include "tscanvas.h"
342:     #include "tsframe.h"
343:     #include "tsapp.h"
344: };
345: 
346: treesheets::System *treesheets::sys = nullptr;
347: #ifdef ENABLE_LOBSTER
348:     treesheets::TreeSheetsScriptImpl treesheets::tssi;
349: #endif
350: 
351: IMPLEMENT_APP(treesheets::TSApp)
352: 
353: #include "events.h"

================
File: src/pot_update.sh
================
1: #!/bin/sh
2: xgettext -j --keyword=_ --sort-output --no-location -o ../TS/translations/ts.pot tsframe.h document.h system.h wxtools.h

================
File: src/script_interface.h
================
 1: namespace script {
 2: 
 3: typedef std::pair<int, int> icoord;
 4: typedef std::pair<icoord, icoord> ibox;
 5: 
 6: struct ScriptInterface {
 7:     virtual bool LoadDocument(const char *filename) = 0;
 8:     virtual void GoToRoot() = 0;
 9:     virtual void GoToView() = 0;
10:     virtual bool HasSelection() = 0;
11:     virtual void GoToSelection() = 0;
12:     virtual bool HasParent() = 0;
13:     virtual void GoToParent() = 0;
14:     virtual int NumChildren() = 0;
15:     virtual icoord NumColumnsRows() = 0;
16:     virtual ibox SelectionBox() = 0;
17:     virtual void GoToChild(int n) = 0;
18:     virtual void GoToColumnRow(int x, int y) = 0;
19:     virtual std::string GetText() = 0;
20:     virtual std::string GetNote() = 0;
21:     virtual void SetText(std::string_view t) = 0;
22:     virtual void SetNote(std::string_view t) = 0;
23:     virtual void CreateGrid(int x, int n) = 0;
24:     virtual void InsertColumn(int x) = 0;
25:     virtual void InsertRow(int y) = 0;
26:     virtual void Delete(int x, int y, int xs, int ys) = 0;
27:     virtual void SetBackgroundColor(uint32_t col) = 0;
28:     virtual void SetTextColor(uint32_t col) = 0;
29:     virtual void SetTextFiltered(bool filtered) = 0;
30:     virtual bool IsTextFiltered() = 0;
31:     virtual void SetBorderColor(uint32_t col) = 0;
32:     virtual int GetRelativeSize() = 0;
33:     virtual void SetRelativeSize(int s) = 0;
34:     virtual void SetStyle(int s) = 0;
35:     virtual int GetStyle() = 0;
36:     virtual int GetColWidth() = 0;
37:     virtual void SetColWidth(int w) = 0;
38:     virtual void SetStatusMessage(std::string_view message) = 0;
39:     virtual void SetWindowSize(int width, int height) = 0;
40:     virtual std::string GetFileNameFromUser(bool is_save) = 0;
41:     virtual std::string GetFileName() = 0;
42:     virtual int64_t GetLastEdit() = 0;
43:     virtual bool IsTag() = 0;
44:     virtual bool HasImage() = 0;
45:     virtual void RemoveImage() = 0;
46:     virtual bool SetImage(std::string_view filename) = 0;
47:     virtual ~ScriptInterface() {};
48: };
49: 
50: extern std::string InitLobster(ScriptInterface *_si, const char *exefilepath,
51:                                const char *auxfilepath, bool from_bundle, FileLoader sl);
52: extern std::string RunLobster(std::string_view filename, std::string_view code, bool dump_builtins);
53: extern void TSDumpBuiltinDoc();
54: 
55: }  // namespace script

================
File: src/selection.h
================
  1: class Selection {
  2:     bool textedit {false};
  3: 
  4:     public:
  5:     Grid *grid;
  6:     int x;
  7:     int y;
  8:     int xs;
  9:     int ys;
 10:     int cursor {0};
 11:     int cursorend {0};
 12:     int firstdx {0};
 13:     int firstdy {0};
 14: 
 15:     Selection(Grid *_grid = nullptr, int _x = 0, int _y = 0, int _xs = 0, int _ys = 0)
 16:         : grid(_grid), x(_x), y(_y), xs(_xs), ys(_ys) {}
 17: 
 18:     void SelAll() {
 19:         if (textedit) {
 20:             cursor = 0;
 21:             cursorend = MaxCursor();
 22:         } else {
 23:             x = y = 0;
 24:             xs = grid->xs;
 25:             ys = grid->ys;
 26:         }
 27:     }
 28: 
 29:     Cell *GetCell() const { return grid && xs == 1 && ys == 1 ? grid->C(x, y).get() : nullptr; }
 30:     Cell *GetFirst() const { return grid && xs >= 1 && ys >= 1 ? grid->C(x, y).get() : nullptr; }
 31:     bool EqLoc(const Selection &s) {
 32:         return grid == s.grid && x == s.x && y == s.y && xs == s.xs && ys == s.ys;
 33:     }
 34:     bool operator==(Selection &s) {
 35:         return EqLoc(s) && cursor == s.cursor && cursorend == s.cursorend;
 36:     }
 37:     bool Thin() const { return !(xs * ys); }
 38:     bool IsAll() const { return xs == grid->xs && ys == grid->ys; }
 39:     void SetCursorEdit(Document *doc, bool edit) {
 40:         wxCursor c(edit ? wxCURSOR_IBEAM : wxCURSOR_ARROW);
 41:         #ifdef WIN32
 42:         // this changes the cursor instantly, but gets overridden by the local window cursor
 43:         ::SetCursor((HCURSOR)c.GetHCURSOR());
 44:         #endif
 45:         // this doesn't change the cursor immediately, only on mousemove:
 46:         doc->canvas->SetCursor(c);
 47: 
 48:         firstdx = firstdy = 0;
 49:     }
 50: 
 51:     bool TextEdit() { return textedit; }
 52:     void EnterEditOnly(Document *doc) {
 53:         textedit = true;
 54:         SetCursorEdit(doc, true);
 55:     }
 56:     void EnterEdit(Document *doc, int cursor = 0, int cursorend = 0) {
 57:         EnterEditOnly(doc);
 58:         this->cursor = cursor;
 59:         this->cursorend = cursorend;
 60:     }
 61:     void ExitEdit(Document *doc) {
 62:         textedit = false;
 63:         cursor = cursorend = 0;
 64:         SetCursorEdit(doc, false);
 65:     }
 66: 
 67:     bool IsInside(Selection &o) {
 68:         if (!o.grid || !grid) return false;
 69:         if (grid != o.grid)
 70:             return grid->cell->parent && grid->cell->parent->grid->FindCell(grid->cell).IsInside(o);
 71:         return x >= o.x && y >= o.y && x + xs <= o.x + o.xs && y + ys <= o.y + o.ys;
 72:     }
 73: 
 74:     void Merge(const Selection &a, const Selection &b) {
 75:         textedit = false;
 76:         if (a.grid == b.grid) {
 77:             if (a.GetCell() == b.GetCell() && a.GetCell() && (a.textedit || b.textedit)) {
 78:                 if (a.cursor != a.cursorend) {
 79:                     Selection c = b;
 80:                     a.GetCell()->text.SelectWord(c);
 81:                     cursor = min(a.cursor, c.cursor);
 82:                     cursorend = max(a.cursorend, c.cursorend);
 83:                 } else {
 84:                     cursor = min(a.cursor, b.cursor);
 85:                     cursorend = max(a.cursor, b.cursor);
 86:                 }
 87:                 textedit = true;
 88:             } else {
 89:                 cursor = cursorend = 0;
 90:             }
 91:         } else {
 92:             auto at = a.GetCell();
 93:             auto bt = b.GetCell();
 94:             int ad = at->Depth();
 95:             int bd = bt->Depth();
 96:             int i = 0;
 97:             while (i < ad && i < bd && at->Parent(ad - i) == bt->Parent(bd - i)) i++;
 98:             auto g = at->Parent(ad - i + 1)->grid;
 99:             Merge(g->FindCell(at->Parent(ad - i)), g->FindCell(bt->Parent(bd - i)));
100:             return;
101:         }
102:         grid = a.grid;
103:         x = min(a.x, b.x);
104:         y = min(a.y, b.y);
105:         xs = abs(a.x - b.x) + 1;
106:         ys = abs(a.y - b.y) + 1;
107:     }
108: 
109:     int MaxCursor() { return int(GetCell()->text.t.Len()); }
110: 
111:     inline bool IsWordSep(wxChar ch) {
112:         // represents: !"#$%&'()*+,-./    :;<=>?@    [\]^    {|}~    `
113:         return 32 < ch && ch < 48 || 57 < ch && ch < 65 || 90 < ch && ch < 95 ||
114:                122 < ch && ch < 127 || ch == 96;
115:     }
116: 
117:     inline int CharType(wxChar ch) {
118:         if (wxIsspace(ch)) return TEXT_SPACE;
119:         if (IsWordSep(ch)) return TEXT_SEP;
120:         return TEXT_CHAR;
121:     }
122: 
123:     void Dir(Document *doc, bool ctrl, bool shift, int dx, int dy, int &v, int &vs, int &ovs,
124:              bool notboundaryperp, bool notboundarypar, bool exitedit) {
125:         if (ctrl && !textedit) {
126:             grid->cell->AddUndo(doc);
127: 
128:             grid->Move(dx, dy, *this);
129:             x = (x + dx + grid->xs) % grid->xs;
130:             y = (y + dy + grid->ys) % grid->ys;
131:             if (x + xs > grid->xs || y + ys > grid->ys) grid = nullptr;
132: 
133:             // FIXME: this is null in the case of a whole column selection, and doesn't do the right
134:             // thing.
135:             if (grid) grid->cell->ResetChildren();
136:             doc->paintscrolltoselection = true;
137:             doc->canvas->Refresh();
138:         } else {
139:             if (ctrl && dx)  // implies textedit
140:             {
141:                 if (cursor == cursorend) firstdx = dx;
142:                 int &curs = firstdx < 0 ? cursor : cursorend;
143:                 int c = curs + dx;
144:                 wxChar ch;
145:                 if (c >= 0 && c <= MaxCursor()) {
146:                     ch = GetCell()->text.t[min(c, curs)];
147:                     // TEXT_SPACE > TEXT_SEP > TEXT_CHAR > 0.
148:                     // Accepts smaller or equal type when positive, only equal when negative.
149:                     // in regex terms (space/sep/char = s/p/c): match (s+p*|s+c*|p+c*|c+)
150:                     int allowed = CharType(ch);
151:                     curs = c;
152:                     for (;;) {
153:                         c += dx;
154:                         if (c < 0 || c > MaxCursor()) break;
155:                         ch = GetCell()->text.t[min(c, curs)];
156:                         int chtype = CharType(ch);
157:                         // type increase when positive or type change when negative => break
158:                         if (chtype > allowed && chtype != -allowed) break;
159:                         curs = c;
160:                         // type decrease when positive => negate
161:                         if (chtype < allowed) allowed = -chtype;
162:                     }
163:                 }
164:                 if (shift) {
165:                     if (cursorend < cursor) swap_(cursorend, cursor);
166:                 } else
167:                     cursorend = cursor = curs;
168:             } else if (shift) {
169:                 if (textedit) {
170:                     if (cursor == cursorend) firstdx = dx;
171:                     (firstdx < 0 ? cursor : cursorend) += dx;
172:                     if (cursor < 0) cursor = 0;
173:                     if (cursorend > MaxCursor()) cursorend = MaxCursor();
174:                 } else {
175:                     if (!xs) firstdx = 0;  // redundant: just in case someone else changed it
176:                     if (!ys) firstdy = 0;
177:                     if (!firstdx) firstdx = dx;
178:                     if (!firstdy) firstdy = dy;
179:                     if (firstdx < 0) {
180:                         x += dx;
181:                         xs += -dx;
182:                     } else
183:                         xs += dx;
184:                     if (firstdy < 0) {
185:                         y += dy;
186:                         ys += -dy;
187:                     } else
188:                         ys += dy;
189:                     if (x < 0) {
190:                         x = 0;
191:                         xs--;
192:                     }
193:                     if (y < 0) {
194:                         y = 0;
195:                         ys--;
196:                     }
197:                     if (x + xs > grid->xs) xs--;
198:                     if (y + ys > grid->ys) ys--;
199:                     if (!xs) firstdx = 0;
200:                     if (!ys) firstdy = 0;
201:                     if (!xs && !ys) grid = nullptr;
202:                 }
203:             } else {
204:                 if (vs) {
205:                     if (ovs)  // (multi) cell selection
206:                     {
207:                         bool intracell = true;
208:                         if (textedit && !exitedit && GetCell()) {
209:                             if (dy) {
210:                                 cursorend = cursor;
211:                                 auto &text = GetCell()->text;
212:                                 int maxcolwidth = GetCell()->parent->grid->colwidths[x];
213: 
214:                                 int i = 0;
215:                                 int laststart, lastlen;
216:                                 int nextoffset = -1;
217:                                 for (int l = 0;; l++) {
218:                                     int start = i;
219:                                     auto ls = text.GetLine(i, maxcolwidth);
220:                                     auto len = static_cast<int>(ls.Len());
221:                                     int end = start + len;
222: 
223:                                     if (len && nextoffset >= 0) {
224:                                         cursor = cursorend =
225:                                             start + (nextoffset > len ? len : nextoffset);
226:                                         intracell = false;
227:                                         break;
228:                                     }
229: 
230:                                     if (cursor >= start && cursor <= end) {
231:                                         if (dy < 0) {
232:                                             if (l != 0) {
233:                                                 cursor = cursorend =
234:                                                     laststart + (cursor - start > lastlen
235:                                                                      ? lastlen
236:                                                                      : cursor - start);
237:                                                 intracell = false;
238:                                             }
239:                                             break;
240:                                         } else {
241:                                             nextoffset = cursor - start;
242:                                         }
243:                                     }
244: 
245:                                     laststart = start;
246:                                     lastlen = len;
247: 
248:                                     if (!len) break;
249:                                 }
250:                             } else {
251:                                 intracell = false;
252:                                 if (cursor != cursorend) {
253:                                     if (dx < 0)
254:                                         cursorend = cursor;
255:                                     else
256:                                         cursor = cursorend;
257:                                 } else {
258:                                     if ((dx < 0 && cursor) || (dx > 0 && MaxCursor() > cursor))
259:                                         cursorend = cursor += dx;
260:                                 }
261:                             }
262:                         }
263: 
264:                         if (intracell) {
265:                             if (sys->thinselc) {
266:                                 if (dx + dy > 0) v += vs;
267:                                 vs = 0;  // make it a thin selection, in direction
268:                                 ovs = 1;
269:                             } else {
270:                                 if (x + dx >= 0 && x + dx + xs <= grid->xs && y + dy >= 0 &&
271:                                     y + dy + ys <= grid->ys) {
272:                                     x += dx;
273:                                     y += dy;
274:                                 }
275:                             }
276:                             ExitEdit(doc);
277:                         }
278:                     } else if (notboundarypar)  // thin selection, moving in parallel direction
279:                     {
280:                         v += dx + dy;
281:                     }
282:                 } else if (notboundaryperp)  // thin selection, moving in perpendicular direction
283:                 {
284:                     if (dx + dy < 0) v--;
285:                     vs = 1;  // make it a cell selection
286:                 } else {     // selection cycle, jump to the opposite side of the grid
287:                     if (y + dy > grid->ys) {
288:                         y = 0;
289:                         vs = 1;
290:                     } else if (y + dy < 0) {
291:                         y = grid->ys - 1;
292:                         vs = 1;
293:                     } else if (x + dx > grid->xs) {
294:                         x = 0;
295:                         vs = 1;
296:                     } else if (x + dx < 0) {
297:                         x = grid->xs - 1;
298:                         vs = 1;
299:                     }
300:                 };
301:             }
302:             doc->paintscrolltoselection = true;
303:             doc->canvas->Refresh();
304:         };
305:     }
306: 
307:     void Cursor(Document *doc, int action, bool ctrl, bool shift, bool exitedit = false) {
308:         switch (action) {
309:             case A_UP: Dir(doc, ctrl, shift, 0, -1, y, ys, xs, y != 0, y != 0, exitedit); break;
310:             case A_DOWN:
311:                 Dir(doc, ctrl, shift, 0, 1, y, ys, xs, y < grid->ys, y < grid->ys - 1, exitedit);
312:                 break;
313:             case A_LEFT: Dir(doc, ctrl, shift, -1, 0, x, xs, ys, x != 0, x != 0, exitedit); break;
314:             case A_RIGHT:
315:                 Dir(doc, ctrl, shift, 1, 0, x, xs, ys, x < grid->xs, x < grid->xs - 1, exitedit);
316:                 break;
317:         }
318:         sys->frame->UpdateStatus(doc->selected, true);
319:     }
320: 
321:     void Next(Document *doc, bool backwards) {
322:         ExitEdit(doc);
323:         if (backwards) {
324:             if (x > 0)
325:                 x--;
326:             else if (y > 0) {
327:                 y--;
328:                 x = grid->xs - 1;
329:             } else {
330:                 x = grid->xs - 1;
331:                 y = grid->ys - 1;
332:             }
333:         } else {
334:             if (x < grid->xs - 1)
335:                 x++;
336:             else if (y < grid->ys - 1) {
337:                 y++;
338:                 x = 0;
339:             } else
340:                 x = y = 0;
341:         }
342:         EnterEdit(doc, 0, MaxCursor());
343:         doc->paintscrolltoselection = true;
344:         doc->canvas->Refresh();
345:     }
346: 
347:     wxString Wrap(Document *doc) {
348:         if (Thin()) return doc->NoThin();
349:         grid->cell->AddUndo(doc);
350:         Cell *np = grid->CloneSel(*this).release();
351:         grid->C(x, y)->text.t = ".";  // avoid this cell getting deleted
352:         if (xs > 1) {
353:             Selection s(grid, x + 1, y, xs - 1, ys);
354:             grid->MultiCellDeleteSub(doc, s);
355:         }
356:         if (ys > 1) {
357:             Selection s(grid, x, y + 1, 1, ys - 1);
358:             grid->MultiCellDeleteSub(doc, s);
359:         }
360:         Cell *old = grid->C(x, y).get();
361:         np->text.relsize = old->text.relsize;
362:         np->CloneStyleFrom(old);
363:         grid->C(x, y).reset(np);
364:         np->parent = grid->cell;
365:         xs = ys = 1;
366:         EnterEdit(doc, MaxCursor(), MaxCursor());
367:         doc->canvas->Refresh();
368:         return wxEmptyString;
369:     }
370: 
371:     Cell *ThinExpand(Document *doc, bool jumptofirst = false) {
372:         if (Thin()) {
373:             if (xs) {
374:                 grid->cell->AddUndo(doc);
375:                 grid->InsertCells(-1, y, 0, 1);
376:                 ys = 1;
377:                 if (jumptofirst) x = 0;
378:             } else {
379:                 grid->cell->AddUndo(doc);
380:                 grid->InsertCells(x, -1, 1, 0);
381:                 xs = 1;
382:                 if (jumptofirst) y = 0;
383:             }
384:         }
385:         return GetCell();
386:     }
387: 
388:     void HomeEnd(Document *doc, bool ishome) {
389:         xs = ys = 1;
390:         if (ishome)
391:             x = y = 0;
392:         else {
393:             x = grid->xs - 1;
394:             y = grid->ys - 1;
395:         }
396:         doc->paintscrolltoselection = true;
397:         doc->canvas->Refresh();
398:     }
399: };

================
File: src/stdafx.cpp
================
1: #include "stdafx.h"

================
File: src/stdafx.h
================
 1: #pragma once
 2: 
 3: #ifdef _WIN32
 4:     #define _CRT_SECURE_NO_WARNINGS
 5:     #define _SCL_SECURE_NO_WARNINGS
 6: #endif
 7: 
 8: #include <ctype.h>
 9: #include <wx/aboutdlg.h>
10: #include <wx/clipbrd.h>
11: #include <wx/confbase.h>
12: #include <wx/config.h>
13: #include <wx/datstrm.h>
14: #include <wx/dcbuffer.h>
15: #include <wx/dir.h>
16: #include <wx/dnd.h>
17: #include <wx/fileconf.h>
18: #include <wx/numdlg.h>
19: #include <wx/tokenzr.h>
20: #include <wx/txtstrm.h>
21: #include <wx/wfstream.h>
22: #include <wx/wx.h>
23: #include <wx/zstream.h>
24: 
25: #ifdef _WIN32
26:     #include <WinUser.h>
27:     #include <imm.h>
28:     #ifdef _MSC_VER
29:         #pragma comment(lib, "imm32")
30:     #endif
31:     #include <wx/msw/dc.h>
32:     #include <wx/msw/regconf.h>
33: #endif
34: 
35: #include <wx/aui/aui.h>
36: #include <wx/base64.h>
37: #include <wx/bmpbndl.h>
38: #include <wx/colordlg.h>
39: #include <wx/datectrl.h>
40: #include <wx/dcsvg.h>
41: #include <wx/display.h>
42: #include <wx/docview.h>
43: #include <wx/filename.h>
44: #include <wx/fontdlg.h>
45: #include <wx/fswatcher.h>
46: #include <wx/ipc.h>
47: #include <wx/mstream.h>
48: #include <wx/notebook.h>
49: #include <wx/odcombo.h>
50: #include <wx/print.h>
51: #include <wx/printdlg.h>
52: #include <wx/sizer.h>
53: #include <wx/snglinst.h>
54: #include <wx/srchctrl.h>
55: #include <wx/stdpaths.h>
56: #include <wx/sysopt.h>
57: #include <wx/taskbar.h>
58: #include <wx/timectrl.h>
59: #include <wx/translation.h>
60: #include <wx/uilocale.h>
61: #include <wx/xml/xml.h>
62: 
63: #include <algorithm>
64: #include <array>
65: #include <clocale>
66: #include <condition_variable>
67: #include <filesystem>
68: #include <functional>
69: #include <future>
70: #include <locale>
71: #include <map>
72: #include <memory>
73: #include <mutex>
74: #include <new>
75: #include <queue>
76: #include <set>
77: #include <sstream>
78: #include <stdexcept>
79: #include <string>
80: #include <string_view>
81: #include <thread>
82: #include <utility>
83: #include <vector>
84: 
85: #include "threadpool.h"
86: #include "tools.h"
87: 
88: #ifdef __WXMAC__
89:     #include <mach-o/dyld.h>
90: #endif
91: 
92: using namespace std;

================
File: src/system.h
================
  1: struct System {
  2:     TSFrame *frame;
  3:     wxString defaultfont {
  4:         #ifdef WIN32
  5:             "Lucida Sans Unicode"
  6:         #else
  7:             "Verdana"
  8:         #endif
  9:     };
 10:     wxString defaultfixedfont {"Courier New"};
 11:     wxString defaultlang {wxEmptyString};
 12:     wxString searchstring;
 13:     unique_ptr<wxConfigBase> cfg;
 14:     Evaluator evaluator;
 15:     wxString clipboardcopy;
 16:     unique_ptr<Cell> cellclipboard;
 17:     vector<unique_ptr<Image>> imagelist;
 18:     vector<int> loadimageids;
 19:     uchar versionlastloaded {0};
 20:     wxLongLong fakelasteditonload;
 21:     wxPen pen_tinytext {wxColour(0x808080ul)};
 22:     wxPen pen_gridborder {wxColour(0xb5a6a4)};
 23:     wxPen pen_tinygridlines {wxColour(0xf2dcd8)};
 24:     wxPen pen_gridlines {wxColour(0xe5b7b0)};
 25:     wxPen pen_thinselect {*wxLIGHT_GREY};
 26:     int roundness {3};
 27:     int defaultmaxcolwidth {80};
 28:     bool makebaks {true};
 29:     bool totray {false};
 30:     bool autosave {true};
 31:     bool zoomscroll {false};
 32:     bool thinselc {true};
 33:     bool minclose {false};
 34:     bool singletray {false};
 35:     bool startminimized {false};
 36:     bool centered {true};
 37:     bool fswatch {true};
 38:     int autohtmlexport {0};
 39:     bool casesensitivesearch {true};
 40:     bool darkennonmatchingcells {false};
 41:     bool fastrender {true};
 42:     bool showtoolbar {true};
 43:     bool showstatusbar {true};
 44:     bool followdarkmode {false};
 45:     uint colormask {0};
 46:     int notesizex {300};
 47:     int notesizey {255};
 48:     std::set<wxString> watchedpaths;
 49:     bool insidefiledialog {false};
 50:     struct TimerStruct : wxTimer {
 51:         void Notify() {
 52:             sys->SaveCheck();
 53:             sys->cfg->Flush();
 54:         }
 55:     } every_second_timer;
 56:     int lastcellcolor {0xFFFFFF};
 57:     int lasttextcolor {0};
 58:     int lastbordcolor {0xA0A0A0};
 59:     Image *lastimage {nullptr};
 60:     int customcolor {0xFFFFFF};
 61:     int cursorcolor {0x00FF00};
 62: 
 63:     System(bool portable)
 64:         : cfg(portable
 65:                   ? (wxConfigBase *)new wxFileConfig("", "", wxGetCwd() + "/TreeSheets.ini", "", 0)
 66:                   : (wxConfigBase *)new wxConfig("TreeSheets")) {
 67:         static const wxDash glpattern[] = {1, 3};
 68:         pen_gridlines.SetDashes(2, glpattern);
 69:         pen_gridlines.SetStyle(wxPENSTYLE_USER_DASH);
 70:         static const wxDash tspattern[] = {2, 4};
 71:         pen_thinselect.SetDashes(2, tspattern);
 72:         pen_thinselect.SetStyle(wxPENSTYLE_USER_DASH);
 73: 
 74:         roundness = static_cast<int>(cfg->Read("roundness", roundness));
 75:         autohtmlexport = static_cast<int>(cfg->Read("autohtmlexport", autohtmlexport));
 76:         defaultfont = cfg->Read("defaultfont", defaultfont);
 77:         defaultfixedfont = cfg->Read("defaultfixedfont", defaultfixedfont);
 78:         defaultlang = cfg->Read("defaultlang", defaultlang);
 79:         cfg->Read("defaultmaxcolwidth", &defaultmaxcolwidth, defaultmaxcolwidth);
 80:         cfg->Read("makebaks", &makebaks, makebaks);
 81:         cfg->Read("totray", &totray, totray);
 82:         cfg->Read("zoomscroll", &zoomscroll, zoomscroll);
 83:         cfg->Read("thinselc", &thinselc, thinselc);
 84:         cfg->Read("autosave", &autosave, autosave);
 85:         cfg->Read("fastrender", &fastrender, fastrender);
 86:         cfg->Read("followdarkmode", &followdarkmode, followdarkmode);
 87:         cfg->Read("minclose", &minclose, minclose);
 88:         cfg->Read("singletray", &singletray, singletray);
 89:         cfg->Read("startminimized", &startminimized, startminimized);
 90:         cfg->Read("centered", &centered, centered);
 91:         cfg->Read("fswatch", &fswatch, fswatch);
 92:         cfg->Read("casesensitivesearch", &casesensitivesearch, casesensitivesearch);
 93:         cfg->Read("defaultfontsize", &g_deftextsize, g_deftextsize);
 94:         cfg->Read("customcolor", &customcolor, customcolor);
 95:         cfg->Read("cursorcolor", &cursorcolor, cursorcolor);
 96:         cfg->Read("showtoolbar", &showtoolbar, showtoolbar);
 97:         cfg->Read("showstatusbar", &showstatusbar, showstatusbar);
 98:         cfg->Read("notesizex", &notesizex, notesizex);
 99:         cfg->Read("notesizey", &notesizey, notesizey);
100:         cfg->Read("lastcellcolor", &lastcellcolor, lastcellcolor);
101:         cfg->Read("lasttextcolor", &lasttextcolor, lasttextcolor);
102:         cfg->Read("lastbordcolor", &lastbordcolor, lastbordcolor);
103:         // fsw.Connect(wxID_ANY, wxID_ANY, wxEVT_FSWATCHER,
104:         // wxFileSystemWatcherEventHandler(System::OnFileChanged));
105:         colormask = (followdarkmode && wxSystemSettings::GetAppearance().IsDark()) ? 0x00FFFFFF : 0;
106:     }
107: 
108:     Document *NewTabDoc(bool append = false, int insert_at = -1) {
109:         auto doc = make_unique<Document>();
110:         auto canvas = frame->NewTab(std::move(doc), append, insert_at);
111:         return canvas->doc.get();
112:     }
113: 
114:     void TabChange(Document *newdoc) {
115:         // SetSelect(hover = Selection());
116:         newdoc->canvas->SetFocus();
117:         newdoc->UpdateFileName();
118:     }
119: 
120:     void Init(const wxString &filename) {
121:         evaluator.Init();
122: 
123:         auto numfiles = static_cast<int>(cfg->Read("numopenfiles", static_cast<long>(0)));
124:         wxString focusfile = cfg->Read("lastopenfile", "");
125:         int selection = -1;
126:         loop(i, numfiles) {
127:             wxString filename;
128:             cfg->Read(wxString::Format("lastopenfile_%d", i), &filename);
129:             if (!LoadDB(filename) && filename == focusfile) selection = i;
130:         }
131: 
132:         if (!filename.IsEmpty()) {
133:             LoadDB(filename);
134:         } else if (selection >= 0) {
135:             frame->notebook->SetSelection(selection);
136:         }
137: 
138:         if (!frame->notebook->GetPageCount()) LoadTutorial();
139: 
140:         if (!frame->notebook->GetPageCount()) InitDB(10);
141: 
142:         // Refresh();
143:         every_second_timer.Start(1000);
144:     }
145: 
146:     void LoadTutorial() {
147:         auto trans = wxTranslations::Get();
148:         auto language = trans ? trans->GetBestTranslation("ts") : wxString("");
149: 
150:         if (language.Len() == 5 &&
151:             !LoadDB(frame->app->GetDocPath("examples/tutorial-" + language + ".cts"))[0]) {
152:             return;
153:         }
154: 
155:         language.Truncate(2);
156:         if (language.Len() == 2 &&
157:             !LoadDB(frame->app->GetDocPath("examples/tutorial-" + language + ".cts"))[0]) {
158:             return;
159:         }
160: 
161:         LoadDB(frame->app->GetDocPath("examples/tutorial.cts"));
162:     }
163: 
164:     void LoadOpRef() { LoadDB(frame->app->GetDocPath("examples/operation-reference.cts")); }
165: 
166:     unique_ptr<Cell> &InitDB(int sizex, int sizey = 0) {
167:         unique_ptr<Cell> c = make_unique<Cell>(nullptr, nullptr, CT_DATA, new Grid(sizex, sizey ? sizey : sizex));
168:         c->cellcolor = 0xCCDCE2;
169:         c->grid->InitCells();
170:         auto doc = NewTabDoc();
171:         doc->InitWith(std::move(c), "", nullptr, 1, 1);
172:         return doc->root;
173:     }
174: 
175:     wxString BakName(const wxString &filename) { return ExtName(filename, ".bak"); }
176:     wxString TmpName(const wxString &filename) { return ExtName(filename, ".tmp"); }
177:     wxString NewName(const wxString &filename) { return filename + ".new"; }
178:     wxString ExtName(const wxString &filename, auto ext) {
179:         wxFileName fn(filename);
180:         return fn.GetPathWithSep() + fn.GetName() + ext;
181:     }
182: 
183:     wxString LoadDB(const wxString &filename, bool fromreload = false, int insert_at = -1) {
184:         auto fn = filename;
185:         auto loadedfromtmp = false;
186: 
187:         if (!fromreload) {
188:             if (frame->GetTabByFileName(filename))
189:                 return wxEmptyString;  //"this file is already loaded";
190: 
191:             if (::wxFileExists(TmpName(filename))) {
192:                 if (::wxMessageBox(
193:                         _("A temporary autosave file exists, would you like to load it instead?"),
194:                         _("Autosave load"), wxYES_NO, frame) == wxYES) {
195:                     fn = TmpName(filename);
196:                     loadedfromtmp = true;
197:                 }
198:             }
199:         }
200: 
201:         Document *doc = nullptr;
202:         auto anyimagesfailed = false;
203:         auto start_loading_time = wxGetLocalTimeMillis();
204:         int zoomlevel = 0;
205: 
206:         {  // limit destructors
207:             wxBusyCursor wait;
208:             Cell *ics = nullptr;
209:             wxFFileInputStream fis(fn);
210:             wxDataInputStream dis(fis);
211:             if (!fis.IsOk()) {
212:                 for (int i = 0, n = frame->filehistory.GetCount(); i < n; i++) {
213:                     if (frame->filehistory.GetHistoryFile(i) == filename)
214:                         frame->filehistory.RemoveFileFromHistory(i);
215:                 }
216:                 return _("Cannot open file.");
217:             }
218: 
219:             char buf[4];
220:             fis.Read(buf, 4);
221:             if (strncmp(buf, "TSFF", 4)) return _("Not a TreeSheets file.");
222:             fis.Read(&versionlastloaded, 1);
223:             if (versionlastloaded > TS_VERSION) return _("File of newer version.");
224:             auto xs = versionlastloaded >= 21 ? dis.Read8() : 1;
225:             auto ys = versionlastloaded >= 21 ? dis.Read8() : 1;
226:             zoomlevel = versionlastloaded >= 23 ? dis.Read8() : 0;
227:             fakelasteditonload = wxDateTime::Now().GetValue();
228: 
229:             loadimageids.clear();
230: 
231:             for (;;) {
232:                 fis.Read(buf, 1);
233:                 switch (*buf) {
234:                     case 'I':
235:                     case 'J': {
236:                         char iti = *buf;
237:                         if (!imagetypes.contains(iti))
238:                             return _("Found an image type that is not defined in this program.");
239:                         if (versionlastloaded < 9) dis.ReadString();
240:                         auto sc = versionlastloaded >= 19 ? dis.ReadDouble() : 1.0;
241:                         vector<uint8_t> image_data;
242:                         if (versionlastloaded >= 22) {
243:                             auto imagelen = (size_t)dis.Read64();
244:                             image_data.resize(imagelen);
245:                             fis.Read(image_data.data(), imagelen);
246:                         } else {
247:                             off_t beforeimage = fis.TellI();
248: 
249:                             if (iti == 'I') {
250:                                 uchar header[8];
251:                                 fis.Read(header, 8);
252:                                 uchar expected[] = {0x89, 'P', 'N', 'G', '\r', '\n', 0x1A, '\n'};
253:                                 if (memcmp(header, expected, 8)) return _("Corrupt PNG header.");
254:                                 dis.BigEndianOrdered(true);
255:                                 for (;;) {  // Skip all chunks.
256:                                     wxInt32 len = dis.Read32();
257:                                     char fourcc[4];
258:                                     fis.Read(fourcc, 4);
259:                                     fis.SeekI(len, wxFromCurrent);  // skip data
260:                                     dis.Read32();                   // skip CRC
261:                                     if (memcmp(fourcc, "IEND", 4) == 0) break;
262:                                 }
263:                             } else if (iti == 'J') {
264:                                 wxImage im;
265:                                 im.LoadFile(fis);
266:                                 if (!im.IsOk()) { return _("JPEG file is corrupted!"); }
267:                             }
268: 
269:                             off_t afterimage = fis.TellI();
270:                             fis.SeekI(beforeimage);
271:                             auto sz = afterimage - beforeimage;
272:                             image_data.resize(sz);
273:                             fis.Read(image_data.data(), sz);
274:                             fis.SeekI(afterimage);
275:                         }
276:                         if (!fis.IsOk()) image_data.clear();
277: 
278:                         loadimageids.push_back(AddImageToList(sc, std::move(image_data), iti));
279:                         break;
280:                     }
281: 
282:                     case 'D': {
283:                         wxZlibInputStream zis(fis);
284:                         if (!zis.IsOk()) return _("Cannot decompress file.");
285:                         wxDataInputStream dis(zis);
286:                         auto numcells = 0, textbytes = 0;
287:                         unique_ptr<Cell> root(Cell::LoadWhich(dis, nullptr, numcells, textbytes, ics));
288:                         if (!root) return _("File corrupted!");
289: 
290:                         doc = NewTabDoc(true, insert_at);
291:                         if (loadedfromtmp) {
292:                             doc->undolistsizeatfullsave =
293:                                 -1;  // if not, user will lose tmp without warning when he closes
294:                             doc->modified = true;
295:                         }
296:                         doc->InitWith(std::move(root), filename, ics, xs, ys);
297: 
298:                         if (versionlastloaded >= 11) {
299:                             for (;;) {
300:                                 auto tag = dis.ReadString();
301:                                 if (!tag.Len()) break;
302:                                 doc->tags[tag] =
303:                                     versionlastloaded >= 24 ? dis.Read32() : g_tagcolor_default;
304:                             }
305:                         }
306: 
307:                         auto end_loading_time = wxGetLocalTimeMillis();
308: 
309:                         frame->SetStatus(wxString::Format(
310:                             _("Loaded %s (%d cells, %d characters) in %lld milliseconds."),
311:                             filename, numcells, textbytes, end_loading_time - start_loading_time));
312: 
313:                         goto done;
314:                     }
315: 
316:                     default: return _("Corrupt block header.");
317:                 }
318:             }
319:         }
320: 
321:     done:
322: 
323:         doc->RefreshImageRefCount(false);
324:         {
325:             ThreadPool pool(std::thread::hardware_concurrency());
326:             for (const auto &image : sys->imagelist) {
327:                 pool.enqueue(
328:                     [](auto img) {
329:                         if (img->trefc) img->Display();
330:                     },
331:                     image.get());
332:             }
333:         }  // wait until all tasks are finished
334: 
335:         FileUsed(filename, doc);
336:         doc->Zoom(zoomlevel, true);
337:         if (anyimagesfailed)
338:             wxMessageBox(_("PNG decode failed on some images in this document\nThey have been replaced by red squares."),
339:                          _("PNG decoder failure"), wxOK, frame);
340: 
341:         return wxEmptyString;
342:     }
343: 
344:     void FileUsed(const wxString &filename, Document *doc) {
345:         frame->filehistory.AddFileToHistory(filename);
346:         if (fswatch) {
347:             doc->lastmodificationtime = wxFileName(filename).GetModificationTime();
348:             const auto &directorypath = wxFileName(filename).GetPath(wxPATH_GET_VOLUME | wxPATH_GET_SEPARATOR);
349:             if (watchedpaths.insert(directorypath).second) {
350:                 frame->watcher->Add(wxFileName(directorypath), wxFSW_EVENT_ALL);
351:             }
352:         }
353:     }
354: 
355:     wxString Open(const wxString &filename) {
356:         if (!filename.empty()) {
357:             auto msg = LoadDB(filename);
358:             if (!msg.IsEmpty()) wxMessageBox(msg, filename, wxOK, frame);
359:             return msg;
360:         }
361:         return _("Open file cancelled.");
362:     }
363: 
364:     void RememberOpenFiles() {
365:         cfg->Write("lastopenfile", frame->GetCurrentTab()->doc->filename);
366:         auto namedfiles = 0;
367:         for(auto i: frame->notebook->GetPagesInDisplayOrder(frame->notebook->GetActiveTabCtrl())) {
368:             auto canvas = static_cast<TSCanvas *>(frame->notebook->GetPage(i));
369:             if (canvas->doc->filename.Len()) {
370:                 cfg->Write(wxString::Format("lastopenfile_%d", namedfiles), canvas->doc->filename);
371:                 namedfiles++;
372:             }
373:         }
374: 
375:         cfg->Write("numopenfiles", namedfiles);
376:         cfg->Flush();
377:     }
378: 
379:     void SaveCheck() {
380:         loop(i, frame->notebook->GetPageCount()) {
381:             static_cast<TSCanvas *>(frame->notebook->GetPage(i))
382:                 ->doc->AutoSave(!frame->IsActive(), i);
383:         }
384:     }
385: 
386:     void SaveAll() {
387:         loop(i, frame->notebook->GetPageCount()) {
388:             frame->GetCurrentTab()->doc->Save(false);
389:             frame->CycleTabs(1);
390:         }
391:     }
392: 
393:     wxString Import(const wxString &filename, int action) {
394:         if (!filename.empty()) {
395:             wxBusyCursor wait;
396:             switch (action) {
397:                 case A_IMPXML:
398:                 case A_IMPXMLA: {
399:                     wxXmlDocument doc;
400:                     if (!doc.Load(filename)) goto problem;
401:                     unique_ptr<Cell> &root = InitDB(1);
402:                     Cell *c = root->grid->cells[0].get();
403:                     FillXML(c, doc.GetRoot(), action == A_IMPXMLA);
404:                     if (!c->HasText() && c->grid) {
405:                         unique_ptr<Cell> child = std::move(root->grid->cells[0]);
406:                         root = std::move(child);
407:                         root->parent = nullptr;
408:                     }
409:                     break;
410:                 }
411:                 case A_IMPTXTI:
412:                 case A_IMPTXTC:
413:                 case A_IMPTXTS:
414:                 case A_IMPTXTT: {
415:                     wxFFile file(filename);
416:                     if (!file.IsOpened()) goto problem;
417:                     wxString content;
418:                     if (!file.ReadAll(&content)) goto problem;
419:                     const auto &lines = wxStringTokenize(content, LINE_SEPARATOR);
420: 
421:                     if (lines.size()) switch (action) {
422:                             case A_IMPTXTI: {
423:                                 FillRows(InitDB(1)->grid, lines, CountCol(lines[0]), 0, 0);
424:                             }; break;
425:                             case A_IMPTXTC:
426:                                 InitDB(1, static_cast<int>(lines.size()))
427:                                     ->grid->CSVImport(lines, L',');
428:                                 break;
429:                             case A_IMPTXTS:
430:                                 InitDB(1, static_cast<int>(lines.size()))
431:                                     ->grid->CSVImport(lines, L';');
432:                                 break;
433:                             case A_IMPTXTT:
434:                                 InitDB(1, static_cast<int>(lines.size()))
435:                                     ->grid->CSVImport(lines, L'\t');
436:                                 break;
437:                         }
438:                     break;
439:                 }
440:             }
441:             Document *doc = frame->GetCurrentTab()->doc.get();
442:             doc->modified = true;
443:             doc->UpdateFileName();
444:             doc->selected = Selection();
445:             doc->begindrag = Selection();
446:             doc->canvas->Refresh();
447:         }
448:         return wxEmptyString;
449:     problem:
450:         wxMessageBox(_("couldn't import file!"), filename, wxOK, frame);
451:         return _("File load error.");
452:     }
453: 
454:     int GetXMLNodes(wxXmlNode *node, auto &nodes, vector<wxXmlAttribute *> *attributes = nullptr,
455:                     bool attributestoo = false) {
456:         for (auto child = node->GetChildren(); child; child = child->GetNext()) {
457:             if (child->GetType() == wxXML_ELEMENT_NODE) nodes.push_back(child);
458:         }
459:         if (attributestoo && attributes)
460:             for (auto attribute = node->GetAttributes(); attribute;
461:                  attribute = attribute->GetNext()) {
462:                 attributes->push_back(attribute);
463:             }
464:         return nodes.size() + (attributes ? attributes->size() : 0);
465:     }
466: 
467:     void FillXML(Cell *c, wxXmlNode *node, bool attributestoo) {
468:         const auto &words = wxStringTokenize(
469:             node->GetType() == wxXML_ELEMENT_NODE ? node->GetNodeContent() : node->GetContent());
470:         loop(i, words.GetCount()) {
471:             if (c->text.t.Len()) c->text.t.Append(L' ');
472:             c->text.t.Append(words[i]);
473:         }
474: 
475:         if (node->GetName() == "cell") {
476:             c->text.relsize = -wxAtoi(node->GetAttribute("relsize", "0"));
477:             c->text.stylebits = wxAtoi(node->GetAttribute("stylebits", "0"));
478:             c->cellcolor =
479:                 std::stoi(node->GetAttribute("colorbg", "0xFFFFFF").ToStdString(), nullptr, 0);
480:             c->textcolor =
481:                 std::stoi(node->GetAttribute("colorfg", "0x000000").ToStdString(), nullptr, 0);
482:             c->celltype = wxAtoi(node->GetAttribute("type", "0"));
483:         }
484: 
485:         vector<wxXmlNode *> nodes;
486:         vector<wxXmlAttribute *> attributes;
487:         auto numrows = GetXMLNodes(node, nodes, &attributes, attributestoo);
488:         if (!numrows) return;
489: 
490:         if (nodes.size() == 1 && (!c->text.t.Len() || nodes[0]->IsWhitespaceOnly()) &&
491:             nodes[0]->GetName() != "row") {
492:             FillXML(c, nodes[0], attributestoo);
493:         } else {
494:             auto allrow = node->GetName() == "grid";
495:             for (auto node : nodes)
496:                 if (node->GetName() != "row") {
497:                     allrow = false;
498:                     break;
499:                 }
500:             if (allrow) {
501:                 int desiredxs;
502:                 loopv(i, nodes) {
503:                     vector<wxXmlNode *> ins;
504:                     auto xs = GetXMLNodes(nodes[i], ins);
505:                     if (!i) {
506:                         desiredxs = xs ? xs : 1;
507:                         c->AddGrid(desiredxs, nodes.size());
508:                         SetGridSettingsFromXML(c, node);
509:                     }
510:                     loop(j, desiredxs) if (ins.size() > j)
511:                         FillXML(c->grid->C(j, i).get(), ins[j], attributestoo);
512:                 }
513:             } else {
514:                 c->AddGrid(1, numrows);
515:                 SetGridSettingsFromXML(c, node);
516:                 loopv(i, attributes) c->grid->C(0, i)->text.t = attributes[i]->GetValue();
517:                 loopv(i, nodes)
518:                     FillXML(c->grid->C(0, i + attributes.size()).get(), nodes[i], attributestoo);
519:             }
520:         }
521:     }
522: 
523:     void SetGridSettingsFromXML(Cell *c, wxXmlNode *node) {
524:         c->grid->folded = wxAtoi(node->GetAttribute("folded", "0"));
525:         c->grid->bordercolor = std::stoi(
526:             node->GetAttribute("bordercolor", wxString() << g_bordercolor_default).ToStdString(),
527:             nullptr, 0);
528:         c->grid->user_grid_outer_spacing = wxAtoi(
529:             node->GetAttribute("outerspacing", wxString() << g_usergridouterspacing_default));
530:     }
531: 
532:     int CountCol(const auto &s) {
533:         auto col = 0;
534:         while (s[col] == ' ' || s[col] == '\t') col++;
535:         return col;
536:     }
537: 
538:     int FillRows(Grid *g, const wxArrayString &as, int column, int startrow, int starty) {
539:         auto y = starty;
540:         for (int i = startrow, n = as.size(); i < n; i++) {
541:             auto s = as[i];
542:             auto col = CountCol(s);
543:             if (col < column && startrow != 0) return i;
544:             if (col > column) {
545:                 auto c = g->C(0, y - 1).get();
546:                 auto sg = c->grid;
547:                 i = FillRows(sg ? sg : c->AddGrid(), as, col, i, sg ? sg->ys : 0) - 1;
548:             } else {
549:                 if (g->ys <= y) g->InsertCells(-1, y, 0, 1);
550:                 auto &t = g->C(0, y)->text;
551:                 t.t = s.Trim(false);
552:                 y++;
553:             }
554:         }
555:         return static_cast<int>(as.size());
556:     }
557: 
558:     int AddImageToList(double scale, auto &&data, char type) {
559:         auto hash = CalculateHash(data);
560:         loopv(i, imagelist) {
561:             if (imagelist[i]->hash == hash && imagelist[i]->type == type) return i;
562:         }
563:         imagelist.push_back(make_unique<Image>(hash, scale, std::move(data), type));
564:         return imagelist.size() - 1;
565:     }
566: 
567:     void ImageSize(wxBitmap *bm, int &xs, int &ys) {
568:         if (!bm) return;
569:         xs = bm->GetWidth();
570:         ys = bm->GetHeight();
571:     }
572: 
573:     void ImageDraw(wxBitmap *bm, wxDC &dc, int x, int y) { dc.DrawBitmap(*bm, x, y); }
574: };

================
File: src/text.h
================
  1: struct Text {
  2:     Cell *cell {nullptr};
  3:     Image *image {nullptr};
  4:     wxString t {wxEmptyString};
  5:     int relsize {0};
  6:     int stylebits {0};
  7:     int extent {0};
  8:     wxDateTime lastedit;
  9:     bool filtered {false};
 10: 
 11:     void WasEdited() { lastedit = wxDateTime::Now(); }
 12: 
 13:     Text() { WasEdited(); }
 14: 
 15:     wxBitmap *DisplayImage() {
 16:         return cell->grid && cell->grid->folded ? &sys->frame->foldicon
 17:                                                 : (image ? &image->Display() : nullptr);
 18:     }
 19: 
 20:     size_t EstimatedMemoryUse() {
 21:         ASSERT(wxUSE_UNICODE);
 22:         return sizeof(Text) + t.Length() * sizeof(wchar_t);
 23:     }
 24: 
 25:     double GetNum() {
 26:         std::wstringstream ss(t.ToStdWstring());
 27:         double r;
 28:         ss >> r;
 29:         return r;
 30:     }
 31: 
 32:     void SetNum(double d) {
 33:         std::wstringstream ss;
 34:         ss << std::fixed;
 35: 
 36:         // We're going to use at most 19 digits after '.'. Add small value round remainder.
 37:         size_t max_significant = 10;
 38:         d += 0.00000000005;
 39: 
 40:         ss << d;
 41: 
 42:         auto s = ss.str();
 43:         // First trim whatever lies beyond the precision to avoid garbage digits.
 44:         max_significant += 2;  // "0."
 45:         if (s[0] == '-') max_significant++;
 46:         if (s.length() > max_significant) s.erase(max_significant);
 47:         // Now strip unnecessary trailing zeroes.
 48:         while (s.back() == '0') s.pop_back();
 49:         // If there were only zeroes, remove '.'.
 50:         if (s.back() == '.') s.pop_back();
 51: 
 52:         t = s;
 53:     }
 54: 
 55:     wxString htmlify(wxString &str) {
 56:         wxString r;
 57:         for (auto cref : str) {
 58:             switch (wxChar c = cref.GetValue()) {
 59:                 case '&': r += "&amp;"; break;
 60:                 case '<': r += "&lt;"; break;
 61:                 case '>': r += "&gt;"; break;
 62:                 default: r += c;
 63:             }
 64:         }
 65:         return r;
 66:     }
 67: 
 68:     wxString ToText(int indent, const Selection &s, int format) {
 69:         wxString str = s.cursor != s.cursorend ? t.Mid(s.cursor, s.cursorend - s.cursor) : t;
 70:         if (format == A_EXPXML || format == A_EXPHTMLT || format == A_EXPHTMLTI ||
 71:             format == A_EXPHTMLTE || format == A_EXPHTMLO || format == A_EXPHTMLB)
 72:             str = htmlify(str);
 73:         if (format == A_EXPHTMLTI && image)
 74:             str.Prepend("<img src=\"data:" + imagetypes.at(image->type).second + ";base64," +
 75:                         wxBase64Encode(image->data.data(), image->data.size()) + "\" />");
 76:         else if (format == A_EXPHTMLTE && image) {
 77:             wxString relsize = wxString::Format(
 78:                 "%d%%", static_cast<int>(100.0 * sys->frame->FromDIP(1.0) / image->display_scale));
 79:             str.Prepend("<img src=\"" + wxString::Format("%llu", image->hash) +
 80:                         image->GetFileExtension() + "\" width=\"" + relsize + "\" height=\"" +
 81:                         relsize + "\" />");
 82:         }
 83:         return str;
 84:     };
 85: 
 86:     auto MinRelsize(int rs) { return min(relsize, rs); }
 87:     auto RelSize(int dir, int zoomdepth) {
 88:         relsize = max(min(relsize + dir, g_deftextsize - g_mintextsize() + zoomdepth),
 89:                       g_deftextsize - g_maxtextsize() - zoomdepth);
 90:     }
 91: 
 92:     auto IsWord(wxChar c) { return wxIsalnum(c) || wxStrchr(L"_\"\'()", c) || wxIspunct(c); }
 93:     auto GetLinePart(int &currentpos, int breakpos, int limitpos) {
 94:         auto startpos = currentpos;
 95:         currentpos = breakpos;
 96: 
 97:         for (auto j = t.begin() + startpos; (j != t.end()) && !wxIsspace(*j) && !IsWord(*j); j++) {
 98:             currentpos++;
 99:             breakpos++;
100:         }
101:         // gobble up any trailing punctuation
102:         if (currentpos != startpos && currentpos < limitpos &&
103:             (t[currentpos] == '\"' || t[currentpos] == '\'')) {
104:             currentpos++;
105:             breakpos++;
106:         }  // special case: if punctuation followed by quote, quote is meant to be part of word
107: 
108:         for (auto k = t.begin() + currentpos; (k != t.end()) && wxIsspace(*k); k++) {
109:             // gobble spaces, but do not copy them
110:             currentpos++;
111:             if (currentpos == limitpos)
112:                 breakpos = currentpos;  // happens with a space at the last line, user is most
113:                                         // likely about to type another word, so
114:             // need to show space. Alternatively could check if the cursor is actually on this spot.
115:             // Simply
116:             // showing a blank new line would not be a good idea, unless the cursor is here for
117:             // sure, and
118:             // even then, placing the cursor there again after deselect may be hard.
119:         }
120: 
121:         ASSERT(startpos != currentpos);
122: 
123:         return t.Mid(startpos, breakpos - startpos);
124:     }
125: 
126:     wxString GetLine(auto &i, auto maxcolwidth) {
127:         auto l = static_cast<int>(t.Len());
128: 
129:         if (i >= l) return wxEmptyString;
130: 
131:         if (!i && l <= maxcolwidth) {
132:             i = l;
133:             return t;
134:         }  // subsumed by the case below, but this case happens 90% of the time, so more optimal
135:         if (l - i <= maxcolwidth) return GetLinePart(i, l, l);
136: 
137:         for (auto p = i + maxcolwidth; p >= i; p--)
138:             if (!IsWord(t[p])) return GetLinePart(i, p, l);
139: 
140:         // A single word is > maxcolwidth. We split it up anyway.
141:         // This happens with long urls and e.g. Japanese text without spaces.
142:         // Should really do proper unicode linebreaking instead (see libunibreak),
143:         // but for now this is better than the old code below which allowed for arbitrary long
144:         // words.
145:         return GetLinePart(i, min(i + maxcolwidth, l), l);
146: 
147:         // for(int p = i+maxcolwidth; p<l;  p++) if (!IsWord(t[p])) return GetLinePart(i, p, l);  //
148:         // we arrive here only
149:         // if a single word is too big for maxcolwidth, so simply return that word
150:         // return GetLinePart(i, l, l);     // big word was the last one
151:     }
152: 
153:     void TextSize(wxReadOnlyDC &dc, int &sx, int &sy, int tiny, int &leftoffset, int maxcolwidth) {
154:         sx = sy = 0;
155:         auto i = 0;
156:         for (;;) {
157:             auto curl = GetLine(i, maxcolwidth);
158:             if (!curl.Len()) break;
159:             int x, y;
160:             if (tiny) {
161:                 x = static_cast<int>(curl.Len());
162:                 y = 1;
163:             } else
164:                 dc.GetTextExtent(curl, &x, &y);
165:             sx = max(x, sx);
166:             sy += y;
167:             leftoffset = y;
168:         }
169:         if (!tiny) sx += 4;
170:     }
171: 
172:     bool IsInSearch() {
173:         return sys->searchstring.Len() &&
174:                (sys->casesensitivesearch ? t.Find(sys->searchstring)
175:                                          : t.Lower().Find(sys->searchstring)) >= 0;
176:     }
177: 
178:     int Render(Document *doc, int bx, int by, int depth, wxDC &dc, int &leftoffset,
179:                int maxcolwidth) {
180:         auto ixs = 0, iys = 0;
181:         if (!cell->tiny) sys->ImageSize(DisplayImage(), ixs, iys);
182: 
183:         if (ixs && iys) {
184:             sys->ImageDraw(DisplayImage(), dc, bx + 1 + g_margin_extra,
185:                            by + (cell->tys - iys) / 2 + g_margin_extra);
186:             ixs += 2;
187:             iys += 2;
188:         }
189: 
190:         if (t.empty()) return iys;
191: 
192:         doc->PickFont(dc, depth, relsize, stylebits);
193: 
194:         auto h = cell->tiny ? 1 : dc.GetCharHeight();
195:         leftoffset = h;
196:         auto i = 0;
197:         auto lines = 0;
198:         auto searchfound = IsInSearch();
199:         auto istag = cell->IsTag(doc);
200:         if (cell->tiny) {
201:             if (searchfound)
202:                 dc.SetPen(*wxRED_PEN);
203:             else if (filtered)
204:                 dc.SetPen(*wxLIGHT_GREY_PEN);
205:             else if (istag)
206:                 dc.SetPen(wxPen(LightColor(doc->tags[t])));
207:             else
208:                 dc.SetPen(sys->pen_tinytext);
209:         }
210:         for (;;) {
211:             auto curl = GetLine(i, maxcolwidth);
212:             if (!curl.Len()) break;
213:             if (cell->tiny) {
214:                 if (sys->fastrender) {
215:                     dc.DrawLine(bx + ixs, by + lines * h, bx + ixs + static_cast<int>(curl.Len()),
216:                                 by + lines * h);
217:                     /*
218:                     wxPoint points[] = { wxPoint(bx + ixs, by + lines * h), wxPoint(bx + ixs +
219:                     curl.Len(), by + lines * h) }; dc.DrawLines(1, points, 0, 0);
220:                      */
221:                 } else {
222:                     auto word = 0;
223:                     loop(p, static_cast<int>(curl.Len()) + 1) {
224:                         if (static_cast<int>(curl.Len()) <= p || curl[p] == ' ') {
225:                             if (word)
226:                                 dc.DrawLine(bx + p - word + ixs, by + lines * h, bx + p,
227:                                             by + lines * h);
228:                             word = 0;
229:                         } else
230:                             word++;
231:                     }
232:                 }
233:             } else {
234:                 if (searchfound)
235:                     dc.SetTextForeground(*wxRED);
236:                 else if (filtered)
237:                     dc.SetTextForeground(*wxLIGHT_GREY);
238:                 else if (istag)
239:                     dc.SetTextForeground(LightColor(doc->tags[t]));
240:                 else if (cell->textcolor)
241:                     dc.SetTextForeground(LightColor(cell->textcolor));  // FIXME: clean up
242:                 auto tx = bx + 2 + ixs;
243:                 auto ty = by + lines * h;
244:                 dc.DrawText(curl, tx + g_margin_extra, ty + g_margin_extra);
245:                 if (searchfound || filtered || istag || cell->textcolor)
246:                     dc.SetTextForeground(LightColor(0x000000));
247:             }
248:             lines++;
249:         }
250: 
251:         return max(lines * h, iys);
252:     }
253: 
254:     void FindCursor(Document *doc, int bx, int by, wxReadOnlyDC &dc, Selection &s, int maxcolwidth) {
255:         bx -= g_margin_extra;
256:         by -= g_margin_extra;
257: 
258:         auto ixs = 0, iys = 0;
259:         if (!cell->tiny) sys->ImageSize(DisplayImage(), ixs, iys);
260:         if (ixs) ixs += 2;
261: 
262:         doc->PickFont(dc, cell->Depth() - doc->drawpath.size(), relsize, stylebits);
263: 
264:         auto i = 0, linestart = 0;
265:         auto line = by / dc.GetCharHeight();
266:         wxString ls;
267: 
268:         loop(l, line + 1) {
269:             linestart = i;
270:             ls = GetLine(i, maxcolwidth);
271:         }
272: 
273:         for (;;) {
274:             auto x = 0, y = 0;
275:             dc.GetTextExtent(ls, &x, &y);  // FIXME: can we do this more intelligently?
276:             if (x <= bx - ixs + 2 || !x) break;
277:             ls.Truncate(ls.Len() - 1);
278:         }
279: 
280:         s.cursor = s.cursorend = linestart + static_cast<int>(ls.Len());
281:         ASSERT(s.cursor >= 0 && s.cursor <= static_cast<int>(t.Len()));
282:     }
283: 
284:     void DrawCursor(Document *doc, wxDC &dc, Selection &s, bool full, uint color, int maxcolwidth) {
285:         auto ixs = 0, iys = 0;
286:         if (!cell->tiny) sys->ImageSize(DisplayImage(), ixs, iys);
287:         if (ixs) ixs += 2;
288:         doc->PickFont(dc, cell->Depth() - doc->drawpath.size(), relsize, stylebits);
289:         auto h = dc.GetCharHeight();
290:         {
291:             auto i = 0;
292:             for (auto l = 0;; l++) {
293:                 auto start = i;
294:                 auto ls = GetLine(i, maxcolwidth);
295:                 auto len = static_cast<int>(ls.Len());
296:                 auto end = start + len;
297: 
298:                 if (s.cursor != s.cursorend) {
299:                     if (s.cursor <= end && s.cursorend >= start) {
300:                         ls.Truncate(min(s.cursorend, end) - start);
301:                         auto x1 = 0, x2 = 0;
302:                         dc.GetTextExtent(ls, &x2, nullptr);
303:                         ls.Truncate(max(s.cursor, start) - start);
304:                         dc.GetTextExtent(ls, &x1, nullptr);
305:                         if (x1 != x2) {
306:                             int startx = cell->GetX(doc) + x1 + 2 + ixs + g_margin_extra;
307:                             int starty =
308:                                 cell->GetY(doc) + l * h + 1 + cell->ycenteroff + g_margin_extra;
309:                             DrawRectangle(dc, color, startx, starty, x2 - x1, h - 1, true);
310:                             HintIMELocation(doc, startx, starty, h - 1, stylebits);
311:                         }
312:                     }
313:                 } else if (s.cursor >= start && s.cursor <= end) {
314:                     ls.Truncate(s.cursor - start);
315:                     auto x = 0;
316:                     dc.GetTextExtent(ls, &x, nullptr);
317:                     int startx = cell->GetX(doc) + x + 1 + ixs + g_margin_extra;
318:                     int starty = cell->GetY(doc) + l * h + 1 + cell->ycenteroff + g_margin_extra;
319:                     DrawRectangle(dc, color, startx, starty, 2, h - 2);
320:                     HintIMELocation(doc, startx, starty, h - 2, stylebits);
321:                     break;
322:                 }
323: 
324:                 if (!len) break;
325:             }
326:         }
327:     }
328: 
329:     void ExpandToWord(Selection &s) {
330:         if (!wxIsalnum(t[s.cursor])) return;
331:         while (s.cursor > 0 && wxIsalnum(t[s.cursor - 1])) s.cursor--;
332:         while (s.cursorend < static_cast<int>(t.Len()) && wxIsalnum(t[s.cursorend])) s.cursorend++;
333:     }
334: 
335:     void SelectWord(Selection &s) {
336:         if (s.cursor >= static_cast<int>(t.Len())) return;
337:         s.cursorend = s.cursor + 1;
338:         ExpandToWord(s);
339:     }
340: 
341:     void SelectWordBefore(Selection &s) {
342:         if (s.cursor <= 1) return;
343:         s.cursorend = s.cursor--;
344:         ExpandToWord(s);
345:     }
346: 
347:     bool RangeSelRemove(Selection &s) {
348:         WasEdited();
349:         if (s.cursor != s.cursorend) {
350:             t.Remove(s.cursor, s.cursorend - s.cursor);
351:             s.cursorend = s.cursor;
352:             return true;
353:         }
354:         return false;
355:     }
356: 
357:     void SetRelSize(Selection &s) {
358:         if (t.Len() || !cell->parent) return;
359:         int dd[] = {0, 1, 1, 0, 0, -1, -1, 0};
360:         for (auto i = 0; i < 4; i++) {
361:             auto x = max(0, min(s.x + dd[i * 2], s.grid->xs - 1));
362:             auto y = max(0, min(s.y + dd[i * 2 + 1], s.grid->ys - 1));
363:             auto c = s.grid->C(x, y).get();
364:             if (c->text.t.Len()) {
365:                 relsize = c->text.relsize;
366:                 break;
367:             }
368:         }
369:     }
370: 
371:     auto Insert(Document *doc, const auto &ins, Selection &s, bool keeprelsize) {
372:         auto prevl = t.Len();
373:         if (!s.TextEdit()) Clear(doc, s);
374:         RangeSelRemove(s);
375:         if (!prevl && !keeprelsize) SetRelSize(s);
376:         t.insert(s.cursor, ins);
377:         s.cursor = s.cursorend = s.cursor + static_cast<int>(ins.Len());
378:     }
379:     void Key(Document *doc, int k, Selection &s) {
380:         wxString ins;
381:         ins += k;
382:         Insert(doc, ins, s, false);
383:     }
384: 
385:     void Delete(Selection &s) {
386:         if (!RangeSelRemove(s))
387:             if (s.cursor < static_cast<int>(t.Len())) { t.Remove(s.cursor, 1); };
388:     }
389:     void Backspace(Selection &s) {
390:         if (!RangeSelRemove(s))
391:             if (s.cursor > 0) {
392:                 t.Remove(--s.cursor, 1);
393:                 --s.cursorend;
394:             };
395:     }
396:     void DeleteWord(Selection &s) {
397:         SelectWord(s);
398:         Delete(s);
399:     }
400:     void BackspaceWord(Selection &s) {
401:         SelectWordBefore(s);
402:         Backspace(s);
403:     }
404: 
405:     void ReplaceStr(const wxString &str, const wxString &lstr) {
406:         if (sys->casesensitivesearch) {
407:             for (auto i = 0, j = 0; (j = t.Mid(i).Find(sys->searchstring)) >= 0;) {
408:                 // does this need WasEdited()?
409:                 i += j;
410:                 t.Remove(i, sys->searchstring.Len());
411:                 t.insert(i, str);
412:                 i += str.Len();
413:             }
414:         } else {
415:             auto lowert = t.Lower();
416:             for (auto i = 0, j = 0; (j = lowert.Mid(i).Find(sys->searchstring)) >= 0;) {
417:                 // does this need WasEdited()?
418:                 i += j;
419:                 lowert.Remove(i, sys->searchstring.Len());
420:                 t.Remove(i, sys->searchstring.Len());
421:                 lowert.insert(i, lstr);
422:                 t.insert(i, str);
423:                 i += str.Len();
424:             }
425:         }
426:     }
427: 
428:     void Clear(Document *doc, Selection &s) {
429:         t.Clear();
430:         s.EnterEdit(doc);
431:     }
432: 
433:     void HomeEnd(Selection &s, bool home) {
434:         auto i = 0;
435:         auto cw = cell->ColWidth();
436:         auto findwhere = home ? s.cursor : s.cursorend;
437:         for (;;) {
438:             auto start = i;
439:             auto curl = GetLine(i, cw);
440:             if (!curl.Len()) break;
441:             auto end = i == t.Len() ? i : i - 1;
442:             if (findwhere >= start && findwhere <= end) {
443:                 s.cursor = s.cursorend = home ? start : end;
444:                 break;
445:             }
446:         }
447:     }
448: 
449:     void Save(wxDataOutputStream &dos) const {
450:         dos.WriteString(t.wx_str());
451:         dos.Write32(relsize);
452:         dos.Write32(image ? image->savedindex : -1);
453:         dos.Write32(stylebits);
454:         wxLongLong le = lastedit.GetValue();
455:         dos.Write64(&le, 1);
456:     }
457: 
458:     void Load(wxDataInputStream &dis) {
459:         t = dis.ReadString();
460: 
461:         // if (t.length() > 10000)
462:         //    printf("");
463: 
464:         if (sys->versionlastloaded <= 11) dis.Read32();  // numlines
465: 
466:         relsize = dis.Read32();
467: 
468:         int i = dis.Read32();
469:         image = i >= 0 ? sys->imagelist[sys->loadimageids[i]].get() : nullptr;
470: 
471:         if (sys->versionlastloaded >= 7) stylebits = dis.Read32();
472: 
473:         wxLongLong time;
474:         if (sys->versionlastloaded >= 14) {
475:             dis.Read64(&time, 1);
476:         } else {
477:             time = sys->fakelasteditonload--;
478:         }
479:         lastedit = wxDateTime(time);
480:     }
481: 
482:     auto Eval(auto &ev) const {
483:         switch (cell->celltype) {
484:             // Load variable's data.
485:             case CT_VARU: {
486:                 auto v = ev.Lookup(t);
487:                 if (!v) {
488:                     v = cell->Clone(nullptr);
489:                     v->celltype = CT_DATA;
490:                     v->text.t = "**Variable Load Error**";
491:                 }
492:                 return v;
493:             }
494: 
495:             // Return our current data.
496:             case CT_DATA: return cell->Clone(nullptr);
497: 
498:             default: return unique_ptr<Cell>();
499:         }
500:     }
501: };

================
File: src/threadpool.h
================
 1: class ThreadPool {
 2:     public:
 3:     ThreadPool(size_t);
 4:     template<class F, class... Args>
 5:     auto enqueue(F&& f, Args&&... args)
 6:         -> std::future<typename std::invoke_result<F, Args...>::type>;
 7:     ~ThreadPool();
 8: 
 9:     private:
10:     // need to keep track of threads so we can join them
11:     std::vector<std::thread> workers;
12:     // the task queue
13:     std::queue<std::function<void()>> tasks;
14: 
15:     // synchronization
16:     std::mutex queue_mutex;
17:     std::condition_variable condition;
18:     bool stop;
19: };
20: 
21: // the constructor just launches some amount of workers
22: inline ThreadPool::ThreadPool(size_t threads) : stop(false) {
23:     for (size_t i = 0; i < threads; ++i)
24:         workers.emplace_back([this] {
25:             for (;;) {
26:                 std::function<void()> task;
27: 
28:                 {
29:                     std::unique_lock<std::mutex> lock(this->queue_mutex);
30:                     this->condition.wait(lock,
31:                                          [this] { return this->stop || !this->tasks.empty(); });
32:                     if (this->stop && this->tasks.empty()) return;
33:                     task = std::move(this->tasks.front());
34:                     this->tasks.pop();
35:                 }
36: 
37:                 task();
38:             }
39:         });
40: }
41: 
42: // add new work item to the pool
43: template<class F, class... Args>
44: auto ThreadPool::enqueue(F&& f, Args&&... args)
45:     -> std::future<typename std::invoke_result<F, Args...>::type> {
46:     using return_type = typename std::invoke_result<F, Args...>::type;
47: 
48:     auto task = std::make_shared<std::packaged_task<return_type()>>(
49:         std::bind(std::forward<F>(f), std::forward<Args>(args)...));
50: 
51:     std::future<return_type> res = task->get_future();
52:     {
53:         std::unique_lock<std::mutex> lock(queue_mutex);
54: 
55:         // don't allow enqueueing after stopping the pool
56:         assert(!stop);
57: 
58:         tasks.emplace([task]() { (*task)(); });
59:     }
60:     condition.notify_one();
61:     return res;
62: }
63: 
64: // the destructor joins all threads
65: inline ThreadPool::~ThreadPool() {
66:     {
67:         std::unique_lock<std::mutex> lock(queue_mutex);
68:         stop = true;
69:     }
70:     condition.notify_all();
71:     for (std::thread& worker : workers) worker.join();
72: }

================
File: src/tools.h
================
  1: typedef unsigned char uchar;
  2: typedef unsigned short ushort;
  3: typedef unsigned int uint;
  4: 
  5: #ifdef _DEBUG
  6: #define ASSERT(c) assert(c)
  7: #else
  8: #define ASSERT(c) \
  9:     if (c) {}
 10: #endif
 11: 
 12: #define loop(i, m) for (int i = 0; i < int(m); i++)
 13: #define loopv(i, v) for (int i = 0; i < (v).size(); i++)
 14: #define loopvrev(i, v) for (int i = (v).size() - 1; i >= 0; i--)
 15: 
 16: #define max(a, b) ((a) < (b) ? (b) : (a))
 17: #define min(a, b) ((a) > (b) ? (b) : (a))
 18: #define sign(x) ((x) < 0 ? -1 : 1)
 19: 
 20: #define varargs(v, fmt, body) \
 21:     {                         \
 22:         va_list v;            \
 23:         va_start(v, fmt);     \
 24:         body;                 \
 25:         va_end(v);            \
 26:     }
 27: 
 28: #define DELETEP(p)       \
 29:     {                    \
 30:         if (p) {         \
 31:             delete p;    \
 32:             p = nullptr; \
 33:         }                \
 34:     }
 35: #define DELETEA(a)       \
 36:     {                    \
 37:         if (a) {         \
 38:             delete[] a;  \
 39:             a = nullptr; \
 40:         }                \
 41:     }
 42: 
 43: #define bound(v, a, s, e)     \
 44:     {                         \
 45:         v += a;               \
 46:         if (v > (e)) v = (e); \
 47:         if (v < (s)) v = (s); \
 48:     }
 49: 
 50: // Use the same on all platforms, because:
 51: // Win32: usually contains both.
 52: // Macos: older versions use \r and newer \n in clipboard?
 53: // Linux: should only ever be \n but if we encounter \r we want to strip it.
 54: #ifdef __WXGTK__
 55:     #define LINE_SEPARATOR "\r\n"
 56: #else
 57:     #define LINE_SEPARATOR "\n"
 58: #endif
 59: 
 60: #ifdef WIN32
 61: #define PATH_SEPARATOR "\\"
 62: #else
 63: #define PATH_SEPARATOR "/"
 64: #define __cdecl
 65: #define _vsnprintf vsnprintf
 66: #endif
 67: 
 68: template<class T> inline void swap_(T &a, T &b) {
 69:     T c = a;
 70:     a = b;
 71:     b = c;
 72: };
 73: 
 74: #ifdef WIN32
 75: #pragma warning(3 : 4189)        // local variable is initialized but not referenced
 76: #pragma warning(disable : 4244)  // conversion from 'int' to 'float', possible loss of data
 77: #pragma warning(disable : 4355)  // 'this' : used in base member initializer list
 78: #pragma warning(disable : 4996)  // 'strncpy' was declared deprecated
 79: #endif
 80: 
 81: inline uchar *loadfile(const char *fn, size_t *lenret = nullptr) {
 82:     FILE *f = fopen(fn, "rb");
 83:     if (!f) return nullptr;
 84:     fseek(f, 0, SEEK_END);
 85:     size_t len = ftell(f);
 86:     fseek(f, 0, SEEK_SET);
 87:     uchar *buf = (uchar *)malloc(len + 1);
 88:     if (!buf) {
 89:         fclose(f);
 90:         return nullptr;
 91:     }
 92:     buf[len] = 0;
 93:     size_t rlen = fread(buf, 1, len, f);
 94:     fclose(f);
 95:     if (len != rlen || len <= 0) {
 96:         free(buf);
 97:         return nullptr;
 98:     }
 99:     if (lenret) *lenret = len;
100:     return buf;
101: }
102: 
103: // for use with vc++ crtdbg
104: 
105: #if defined(_DEBUG) && defined(_WIN32)
106: inline void *__cdecl operator new(size_t n, const char *fn, int l) {
107:     return ::operator new(n, 1, fn, l);
108: }
109: inline void *__cdecl operator new[](size_t n, const char *fn, int l) {
110:     return ::operator new[](n, 1, fn, l);
111: }
112: inline void __cdecl operator delete(void *p, const char *fn, int l) {
113:     ::operator delete(p, 1, fn, l);
114: }
115: inline void __cdecl operator delete[](void *p, const char *fn, int l) {
116:     ::operator delete[](p, 1, fn, l);
117: }
118: #define new new (__FILE__, __LINE__)
119: #endif
120: 
121: inline uint64_t FNV1A64(uint8_t *data, size_t size) {
122:     uint64_t hash = 0xCBF29CE484222325;
123:     for (size_t i = 0; i < size; ++i) {
124:         hash ^= data[i];
125:         hash *= 0x100000001B3;
126:     }
127:     return hash;
128: }

================
File: src/treesheets_impl.h
================
  1: struct TreeSheetsScriptImpl : public ScriptInterface {
  2:     Document *document = nullptr;
  3:     Cell *current = nullptr;
  4:     Cell *lowestcommonancestor = nullptr;
  5: 
  6:     enum { max_new_grid_cells = 256 * 256 };  // Don't allow crazy sizes.
  7: 
  8:     void SwitchToCurrentDocument() {
  9:         document = sys->frame->GetCurrentTab()->doc.get();
 10:         current = document->root.get();
 11:         lowestcommonancestor = nullptr;
 12:     }
 13: 
 14:     void AddUndoIfNecessary() {
 15:         if (!lowestcommonancestor) {
 16:             UpdateLowestCommonAncestor(true);
 17:         } else {
 18:             for (auto p = current; p; p = p->parent) {
 19:                 if (p == lowestcommonancestor) {
 20:                     // There is no need to add current to the undo stack as
 21:                     // lowestcommonancestor including subordinated current
 22:                     // is already in there.
 23:                     return;
 24:                 }
 25:             }
 26:             UpdateLowestCommonAncestor(false);
 27:         }
 28:     }
 29: 
 30:     void UpdateLowestCommonAncestor(bool newgeneration) {
 31:         // Use parent as lowestcommonancestor so changes to siblings are already covered
 32:         lowestcommonancestor = current->parent;
 33:         document->AddUndo(lowestcommonancestor, newgeneration);
 34:     }
 35: 
 36:     std::string ScriptRun(const char *filename) {
 37:         SwitchToCurrentDocument();
 38: 
 39:         bool dump_builtins = false;
 40:         #ifdef _DEBUG
 41:             //dump_builtins = true;
 42:         #endif
 43: 
 44:         auto errormessage = RunLobster(filename, {}, dump_builtins);
 45: 
 46:         document->root->ResetChildren();
 47:         document->canvas->Refresh();
 48: 
 49:         document = nullptr;
 50:         current = nullptr;
 51: 
 52:         return errormessage;
 53:     }
 54: 
 55:     bool LoadDocument(const char *filename) {
 56:         auto message = sys->LoadDB(filename);
 57:         if (message.IsEmpty()) return false;
 58: 
 59:         SwitchToCurrentDocument();
 60:         return true;
 61:     }
 62: 
 63:     void GoToRoot() { current = document->root.get(); }
 64:     void GoToView() { current = document->currentdrawroot; }
 65:     bool HasSelection() { return document->selected.grid; }
 66:     void GoToSelection() {
 67:         auto cell = document->selected.GetFirst();
 68:         if (cell) current = cell;
 69:     }
 70:     bool HasParent() { return current->parent; }
 71:     void GoToParent() {
 72:         if (current->parent) current = current->parent;
 73:     }
 74:     int NumChildren() { return current->grid ? current->grid->xs * current->grid->ys : 0; }
 75: 
 76:     icoord NumColumnsRows() {
 77:         return current->grid ? icoord(current->grid->xs, current->grid->ys) : icoord(0, 0);
 78:     }
 79: 
 80:     int GetColWidth() { return current->parent ? current->parent->grid->GetColWidth(current) : 0; }
 81: 
 82:     void SetColWidth(int w) {
 83:         if (current->parent) { current->parent->grid->SetColWidth(current, w); }
 84:     }
 85: 
 86:     ibox SelectionBox() {
 87:         auto &selection = document->selected;
 88:         return selection.grid ? ibox(icoord(selection.x, selection.y), icoord(selection.xs, selection.ys))
 89:                       : ibox(icoord(0, 0), icoord(0, 0));
 90:     }
 91: 
 92:     void GoToChild(int n) {
 93:         if (current->grid && n < current->grid->xs * current->grid->ys)
 94:             current = current->grid->cells[n].get();
 95:     }
 96: 
 97:     void GoToColumnRow(int x, int y) {
 98:         if (current->grid && x < current->grid->xs && y < current->grid->ys)
 99:             current = current->grid->C(x, y).get();
100:     }
101: 
102:     std::string GetText() { return current->text.t.utf8_string(); }
103: 
104:     std::string GetNote() { return current->note.utf8_string(); }
105: 
106:     void SetText(std::string_view t) {
107:         if (current->parent) {
108:             AddUndoIfNecessary();
109:             current->text.t = wxString::FromUTF8(t.data(), t.size());
110:         }
111:     }
112: 
113:     void SetNote(std::string_view t) {
114:         if (current->parent) {
115:             AddUndoIfNecessary();
116:             current->note = wxString::FromUTF8(t.data(), t.size());
117:         }
118:     }
119: 
120:     void CreateGrid(int x, int y) {
121:         if (x > 0 && y > 0 && x * y < max_new_grid_cells) {
122:             AddUndoIfNecessary();
123:             current->AddGrid(x, y);
124:         }
125:     }
126: 
127:     void InsertColumn(int x) {
128:         if (current->grid && x >= 0 && x <= current->grid->xs) {
129:             AddUndoIfNecessary();
130:             current->grid->InsertCells(x, -1, 1, 0);
131:         }
132:     }
133: 
134:     void InsertRow(int y) {
135:         if (current->grid && y >= 0 && y <= current->grid->ys) {
136:             AddUndoIfNecessary();
137:             current->grid->InsertCells(-1, y, 0, 1);
138:         }
139:     }
140: 
141:     void Delete(int x, int y, int xs, int ys) {
142:         if (current->grid && x >= 0 && x + xs <= current->grid->xs && y >= 0 &&
143:             y + ys <= current->grid->ys) {
144:             AddUndoIfNecessary();
145:             Selection s(current->grid, x, y, xs, ys);
146:             current->grid->MultiCellDeleteSub(document, s);
147:             document->SetSelect(Selection());
148:             document->Zoom(-100);
149:         }
150:     }
151: 
152:     void SetBackgroundColor(uint color) {
153:         AddUndoIfNecessary();
154:         current->cellcolor = color;
155:     }
156: 
157:     void SetTextColor(uint color) {
158:         AddUndoIfNecessary();
159:         current->textcolor = color;
160:     }
161: 
162:     void SetTextFiltered(bool filtered) {
163:         if (current->parent) {
164:             AddUndoIfNecessary();
165:             current->text.filtered = filtered;
166:         }
167:     }
168: 
169:     bool IsTextFiltered() { return current->text.filtered; }
170: 
171:     void SetBorderColor(uint color) {
172:         if (current->grid) {
173:             AddUndoIfNecessary();
174:             current->grid->bordercolor = color;
175:         }
176:     }
177: 
178:     int GetRelativeSize() { return -current->text.relsize; }
179: 
180:     void SetRelativeSize(int relsize) {
181:         AddUndoIfNecessary();
182:         current->text.relsize = -relsize;
183:     }
184: 
185:     void SetStyle(int stylebits) {
186:         AddUndoIfNecessary();
187:         current->text.stylebits = stylebits;
188:     }
189: 
190:     int GetStyle() { return current->text.stylebits; }
191: 
192:     void RemoveImage() {
193:         AddUndoIfNecessary();
194:         current->text.image = nullptr;
195:     }
196: 
197:     void SetStatusMessage(std::string_view message) {
198:         auto ws = wxString(message.data(), message.size());
199:         sys->frame->SetStatus(ws);
200:     }
201: 
202:     void SetWindowSize(int width, int height) { sys->frame->SetSize(width, height); }
203: 
204:     std::string GetFileNameFromUser(bool is_save) {
205:         int flags = wxFD_CHANGE_DIR;
206:         if (is_save)
207:             flags |= wxFD_OVERWRITE_PROMPT | wxFD_SAVE;
208:         else
209:             flags |= wxFD_OPEN | wxFD_FILE_MUST_EXIST;
210:         wxString fn = ::wxFileSelector(_("Choose file:"), "", "", "", "*.*", flags);
211:         return fn.utf8_string();
212:     }
213: 
214:     std::string GetFileName() { return document->filename.utf8_string(); }
215: 
216:     int64_t GetLastEdit() { return current->text.lastedit.GetValue().GetValue(); }
217: 
218:     bool IsTag() { return current->IsTag(document); }
219: 
220:     bool HasImage() { return current->text.image; }
221:     bool SetImage(std::string_view fn) {
222:         AddUndoIfNecessary();
223:         return document->LoadImageIntoCell(wxString::FromUTF8(fn.data(), fn.size()), current,
224:                                            sys->frame->FromDIP(1.0));
225:     }
226: };
227: 
228: static int64_t TreeSheetsLoader(string_view_nt absfilename, std::string *dest, int64_t start,
229:                                 int64_t len) {
230:     size_t l = 0;
231:     auto buf = (char *)loadfile(absfilename.c_str(), &l);
232:     if (!buf) return -1;
233:     dest->assign(buf, l);
234:     free(buf);
235:     return l;
236: }
237: 
238: static TreeSheetsScriptImpl tssi;
239: 
240: static string ScriptInit(const wxString &datapath) {
241:     return InitLobster(&tssi, datapath, "", false, TreeSheetsLoader);
242: }

================
File: src/tsapp.h
================
  1: struct IPCServer : wxServer {
  2:     wxConnectionBase *OnAcceptConnection(const wxString &topic) {
  3:         sys->frame->DeIconize();
  4:         if (topic.Len() && topic != "*") sys->Open(topic);
  5:         return new wxConnection();
  6:     }
  7: };
  8: 
  9: struct TSApp : wxApp {
 10:     TSFrame *frame {nullptr};
 11:     unique_ptr<IPCServer> serv {make_unique<IPCServer>()};
 12:     wxString service {
 13:         #ifdef __WXMSW__
 14:                 "4242"
 15:         #else
 16:                 "/tmp/TreeSheets-socket"
 17:         #endif
 18:     };
 19:     wxString filename;
 20:     bool initiateventloop {false};
 21:     wxString exename;
 22:     wxString exepath;
 23:     unique_ptr<wxSingleInstanceChecker> instance_checker {nullptr};
 24: 
 25:     bool OnInit() override {
 26:         #if wxUSE_UNICODE == 0
 27:             #error "must use unicode version of wx libs to ensure data integrity of .cts files"
 28:         #endif
 29:         ASSERT(wxUSE_UNICODE);
 30: 
 31:         exename = GetExecutablePath();
 32:         exepath = wxFileName(exename).GetPath();
 33: 
 34:         #ifdef __WXMAC__
 35:             int cut = exepath.Find("/MacOS");
 36:             if (cut > 0) { exepath = exepath.SubString(0, cut) + "/Resources"; }
 37:             wxDisableAsserts();
 38:         #endif
 39: 
 40:         bool portable = false;
 41:         bool single_instance = true;
 42:         bool dump_builtins = false;
 43:         bool start_minimized = false;
 44:         for (int i = 1; i < argc; i++) {
 45:             if (argv[i][0] == '-') {
 46:                 switch (static_cast<int>(argv[i][1])) {
 47:                     case 'p': portable = true; break;
 48:                     case 'i': single_instance = false; break;
 49:                     case 'm': start_minimized = true; break;
 50:                     case 'd':
 51:                         dump_builtins = true;
 52:                         single_instance = false;
 53:                         break;
 54:                 }
 55:             } else {
 56:                 filename = argv[i];
 57:             }
 58:         }
 59: 
 60:         if (single_instance) {
 61:             instance_checker.reset(new wxSingleInstanceChecker(
 62:                 wxTheApp->GetAppName() + '-' + wxGetUserId(), wxStandardPaths::Get().GetTempDir()));
 63:             if (instance_checker->IsAnotherRunning()) {
 64:                 wxClient client;
 65:                 client.MakeConnection("localhost", service,
 66:                                       filename.Len() ? filename : wxString("*"));  // fire and forget
 67:                 return false;
 68:             }
 69:         }
 70: 
 71:         wxStandardPaths::Get().SetFileLayout(wxStandardPathsBase::FileLayout_XDG);
 72:         sys = new System(portable);
 73:         if (start_minimized) sys->startminimized = true;
 74:         SetupInternationalization();
 75:         #ifdef __WXMSW__
 76:             DeclareHiDpiAwareOnWindows();
 77:             MSWEnableDarkMode();
 78:         #endif
 79:         frame = new TSFrame(this);
 80: 
 81:         #ifdef ENABLE_LOBSTER
 82:             auto serr = ScriptInit(GetDataPath("scripts/"));
 83:             if (!serr.empty()) {
 84:                 wxLogFatalError("Script system could not initialize: %s", serr);
 85:                 return false;
 86:             }
 87:         #endif
 88: 
 89:         if (dump_builtins) {
 90:             #ifdef ENABLE_LOBSTER
 91:                 TSDumpBuiltinDoc();
 92:             #endif
 93:             return false;
 94:         }
 95: 
 96:         SetTopWindow(frame);
 97: 
 98:         serv->Create(service);
 99:         return true;
100:     }
101: 
102:     void OnEventLoopEnter(wxEventLoopBase *WXUNUSED(loop)) override {
103:         if (!initiateventloop) {
104:             initiateventloop = true;
105:             frame->AppOnEventLoopEnter();
106:             sys->Init(filename);
107:         }
108:     }
109: 
110:     #ifdef __WXMAC__
111:         void MacOpenFiles(const wxArrayString &filenames) override {
112:             if (!sys) return;
113:             // MacOpenFiles does not trigger OnEventLoopEnter so we need
114:             // to do this manually
115:             if (!initiateventloop) {
116:                 initiateventloop = true;
117:                 frame->AppOnEventLoopEnter();
118:             }
119:             for (auto &fn : filenames) { sys->Init(fn); }
120:         }
121:     #endif
122: 
123:     int OnExit() override {
124:         DELETEP(sys);
125:         return 0;
126:     }
127: 
128:     wxString GetExecutablePath() {
129:         wxString executablepath = argv[0];
130:         #if defined(__WXMAC__)
131:             char path[PATH_MAX];
132:             uint32_t size = sizeof(path);
133:             if(_NSGetExecutablePath(path, &size) == 0) executablepath = path;
134:         #elif defined(__WXGTK__)
135:             // argv[0] could be relative, this is apparently a more robust way to get the
136:             // full path.
137:             char path[PATH_MAX];
138:             auto len = readlink("/proc/self/exe", path, PATH_MAX - 1);
139:             if (len >= 0) {
140:                 path[len] = 0;
141:                 executablepath = path;
142:             }
143:         #endif
144:         return executablepath;
145:     }
146: 
147:     void SetupInternationalization() {
148:         wxUILocale::UseDefault();
149: 
150:         #ifdef __WXGTK__
151:             wxFileTranslationsLoader::AddCatalogLookupPathPrefix("/usr");
152:             wxFileTranslationsLoader::AddCatalogLookupPathPrefix("/usr/local");
153:             #ifdef LOCALEDIR
154:                 wxFileTranslationsLoader::AddCatalogLookupPathPrefix(LOCALEDIR);
155:             #endif
156:             wxString prefix = wxStandardPaths::Get().GetInstallPrefix();
157:             wxFileTranslationsLoader::AddCatalogLookupPathPrefix(prefix);
158:         #endif
159:         wxFileTranslationsLoader::AddCatalogLookupPathPrefix(GetDataPath("translations"));
160: 
161:         auto trans = new wxTranslations();
162:         trans->SetLanguage(sys->defaultlang);
163:         trans->AddCatalog("ts");
164: 
165:         wxTranslations::Set(trans);
166:     }
167: 
168:     wxString GetDataPath(const wxString &relpath) {
169:         std::filesystem::path candidatePaths[] = {
170:             std::filesystem::path(exepath.Length()
171:                                       ? exepath.ToStdString() + "/" + relpath.ToStdString()
172:                                       : relpath.ToStdString()),
173:             #ifdef TREESHEETS_DATADIR
174:                 std::filesystem::path(TREESHEETS_DATADIR "/" + relpath.ToStdString()),
175:             #endif
176:         };
177:         std::filesystem::path relativePath;
178:         for (auto path : candidatePaths) {
179:             relativePath = path;
180:             if (std::filesystem::exists(relativePath)) { break; }
181:         }
182: 
183:         return wxString(relativePath);
184:     }
185: 
186:     wxString GetDocPath(const wxString &relpath) {
187:         std::filesystem::path candidatePaths[] = {
188:             std::filesystem::path(exepath.Length()
189:                                       ? exepath.ToStdString() + "/" + relpath.ToStdString()
190:                                       : relpath.ToStdString()),
191:             #ifdef TREESHEETS_DOCDIR
192:                 std::filesystem::path(TREESHEETS_DOCDIR "/" + relpath.ToStdString()),
193:             #endif
194:         };
195:         std::filesystem::path relativePath;
196:         for (auto path : candidatePaths) {
197:             relativePath = path;
198:             if (std::filesystem::exists(relativePath)) { break; }
199:         }
200: 
201:         return wxString(relativePath);
202:     }
203: 
204:     #ifdef __WXMSW__
205:         void DeclareHiDpiAwareOnWindows() {
206:             // wxWidgets should really be doing this itself, but it does not (or expects you to
207:             // want to use a manifest), so we try to use the most recent Windows API to declare
208:             // ourselves as HiDPI compatible.
209: 
210:             #ifndef DPI_ENUMS_DECLARED
211:                 typedef enum PROCESS_DPI_AWARENESS {
212:                     PROCESS_DPI_UNAWARE = 0,
213:                     PROCESS_SYSTEM_DPI_AWARE = 1,
214:                     PROCESS_PER_MONITOR_DPI_AWARE = 2
215:                 } PROCESS_DPI_AWARENESS;
216:             #endif
217: 
218:             using SetProcessDPIAware_T = BOOL(WINAPI *)(void);
219:             using SetProcessDpiAwareness_T = HRESULT(WINAPI *)(PROCESS_DPI_AWARENESS);
220:             using SetProcessDpiAwarenessContext_T = BOOL(WINAPI *)(DPI_AWARENESS_CONTEXT);
221: 
222:             SetProcessDPIAware_T SetProcessDPIAware = nullptr;
223:             SetProcessDpiAwareness_T SetProcessDpiAwareness = nullptr;
224:             SetProcessDpiAwarenessContext_T SetProcessDpiAwarenessContext = nullptr;
225: 
226:             HMODULE user32 = LoadLibraryA("User32.dll");
227:             HMODULE shcore = LoadLibraryA("Shcore.dll");
228: 
229:             if (user32) {
230:                 SetProcessDPIAware = (SetProcessDPIAware_T)GetProcAddress(user32, "SetProcessDPIAware");
231:                 SetProcessDpiAwarenessContext = (SetProcessDpiAwarenessContext_T)GetProcAddress(
232:                     user32, "SetProcessDpiAwarenessContext");
233:             }
234:             if (shcore) {
235:                 SetProcessDpiAwareness =
236:                     (SetProcessDpiAwareness_T)GetProcAddress(shcore, "SetProcessDpiAwareness");
237:             }
238: 
239:             if (SetProcessDpiAwarenessContext) {
240:                 SetProcessDpiAwarenessContext(DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2);
241:             } else if (SetProcessDpiAwareness) {
242:                 SetProcessDpiAwareness(PROCESS_PER_MONITOR_DPI_AWARE);
243:             } else if (SetProcessDPIAware) {
244:                 SetProcessDPIAware();
245:             }
246: 
247:             if (user32) FreeLibrary(user32);
248:             if (shcore) FreeLibrary(shcore);
249:         }
250:     #endif
251: 
252:     DECLARE_EVENT_TABLE()
253: };

================
File: src/tscanvas.h
================
  1: struct TSCanvas : public wxScrolledCanvas {
  2:     TSFrame *frame;
  3:     unique_ptr<Document> doc {nullptr};
  4:     int mousewheelaccum {0};
  5:     bool lastrmbwaswithctrl {false};
  6:     wxPoint lastmousepos;
  7: 
  8:     TSCanvas(TSFrame *fr, wxWindow *parent, const wxSize &size = wxDefaultSize)
  9:         : wxScrolledCanvas(parent, wxID_ANY, wxDefaultPosition, size,
 10:                            wxScrolledWindowStyle | wxWANTS_CHARS | wxFULL_REPAINT_ON_RESIZE),
 11:           frame(fr) {
 12:         SetBackgroundStyle(wxBG_STYLE_PAINT);
 13:         SetBackgroundColour(*wxWHITE);
 14:         DisableKeyboardScrolling();
 15:         // Without this, canvas does its own scrolling upon mousewheel events, which
 16:         // interferes with our own.
 17:         EnableScrolling(false, false);
 18:     }
 19: 
 20:     ~TSCanvas() { frame = nullptr; }
 21: 
 22:     void OnPaint(wxPaintEvent &event) {
 23:         #ifdef __WXMSW__
 24:             auto sz = GetClientSize();
 25:             if (sz.GetX() <= 0 || sz.GetY() <= 0) return;
 26:             wxBitmap bmp;
 27:             auto sf = GetDPIScaleFactor();
 28:             bmp.CreateWithDIPSize(sz, sf, 24);
 29:             wxBufferedPaintDC dc(this, bmp);
 30:         #else
 31:             wxPaintDC dc(this);
 32:         #endif
 33:         DoPrepareDC(dc);
 34:         doc->Draw(dc);
 35:     };
 36: 
 37:     void OnMotion(wxMouseEvent &me) {
 38:         wxInfoDC dc(this);
 39:         doc->UpdateHover(dc, me.GetX(), me.GetY());
 40:         if (me.LeftIsDown() || me.RightIsDown()) {
 41:             if (me.AltDown() && me.ShiftDown()) {
 42:                 doc->Copy(A_DRAGANDDROP);
 43:                 Refresh();
 44:             } else {
 45:                 if (doc->isctrlshiftdrag) {
 46:                     doc->begindrag = doc->hover;
 47:                 } else if (!doc->hover.Thin()) {
 48:                     if (doc->begindrag.Thin() || doc->selected.Thin()) {
 49:                         doc->SetSelect(doc->hover);
 50:                         doc->ResetCursor();
 51:                         Refresh();
 52:                     } else {
 53:                         Selection old = doc->selected;
 54:                         doc->selected.Merge(doc->begindrag, doc->hover);
 55:                         if (!(old == doc->selected)) {
 56:                             doc->ResetCursor();
 57:                             Refresh();
 58:                         }
 59:                     }
 60:                 }
 61:             }
 62:             sys->frame->UpdateStatus(doc->selected, true);
 63:         } else if (me.MiddleIsDown()) {
 64:             wxPoint p = me.GetPosition() - lastmousepos;
 65:             CursorScroll(-p.x, -p.y);
 66:         } else {
 67:             if (doc->hover != doc->prev && !doc->hover.Thin()) sys->frame->UpdateStatus(doc->hover, false);
 68:         }
 69:         lastmousepos = me.GetPosition();
 70:     }
 71: 
 72:     void SelectClick(int mx, int my, bool right, int isctrlshift) {
 73:         wxInfoDC dc(this);
 74:         if (mx < 0 || my < 0)
 75:             return;  // for some reason, using just the "menu" key sends a right-click at (-1, -1)
 76:         doc->isctrlshiftdrag = isctrlshift;
 77:         doc->UpdateHover(dc, mx, my);
 78:         doc->SelectClick(right);
 79:         sys->frame->UpdateStatus(doc->selected, true);
 80:         Refresh();
 81:     }
 82: 
 83:     void OnLeftDown(wxMouseEvent &me) {
 84:         #ifndef __WXMSW__
 85:         // seems to not want to give the canvas focus otherwise (thinks its already in focus
 86:         // when its not?)
 87:         if (frame->filter) frame->filter->SetFocus();
 88:         #endif
 89:         SetFocus();
 90:         if (me.ShiftDown())
 91:             OnMotion(me);
 92:         else
 93:             SelectClick(me.GetX(), me.GetY(), false, me.CmdDown() + me.AltDown() * 2);
 94:     }
 95: 
 96:     void OnLeftUp(wxMouseEvent &me) {
 97:         if (me.CmdDown() || me.AltDown()) {
 98:             wxInfoDC dc(this);
 99:             doc->UpdateHover(dc, me.GetX(), me.GetY());
100:             doc->SelectUp();
101:             sys->frame->UpdateStatus(doc->selected, true);
102:             Refresh();
103:         }
104:     }
105: 
106:     void OnRightDown(wxMouseEvent &me) {
107:         SetFocus();
108:         SelectClick(me.GetX(), me.GetY(), true, 0);
109:         lastrmbwaswithctrl = me.CmdDown();
110:         #ifndef __WXMSW__
111:         me.Skip();  // otherwise EVT_CONTEXT_MENU won't be triggered?
112:         #endif
113:     }
114: 
115:     void OnLeftDoubleClick(wxMouseEvent &me) {
116:         wxInfoDC dc(this);
117:         doc->UpdateHover(dc, me.GetX(), me.GetY());
118:         doc->DoubleClick();
119:         sys->frame->UpdateStatus(doc->selected, true);
120:         Refresh();
121:     }
122: 
123:     void OnKeyDown(wxKeyEvent &ce) { ce.Skip(); }
124:     void OnChar(wxKeyEvent &ce) {
125:         /*
126:         if (sys->insidefiledialog)
127:         {
128:             ce.Skip();
129:             return;
130:         }
131:         */
132:         #ifndef __WXMAC__
133:             // Without this check, Alt+[Alphanumericals], Alt+Shift+[Alphanumericals] and
134:             // Alt+[Shift]+cursor (scrolling) don't work. The 128 makes sure unicode entry on e.g.
135:             // Polish keyboards still works. (on Linux in particular).
136:             if ((ce.GetModifiers() == wxMOD_ALT || ce.GetModifiers() == (wxMOD_ALT | wxMOD_SHIFT)) &&
137:                 (ce.GetUnicodeKey() < 128)) {
138:                 ce.Skip();
139:                 return;
140:             }
141:         #endif
142: 
143:         bool unprocessed = false;
144:         sys->frame->SetStatus(doc->Key(ce.GetUnicodeKey(), ce.GetKeyCode(), ce.AltDown(),
145:                                        ce.CmdDown(), ce.ShiftDown(), unprocessed));
146:         if (unprocessed) ce.Skip();
147:     }
148: 
149:     void OnMouseWheel(wxMouseEvent &me) {
150:         bool ctrl = me.CmdDown();
151:         if (sys->zoomscroll) ctrl = !ctrl;
152:         if (me.AltDown() || ctrl || me.ShiftDown()) {
153:             mousewheelaccum += me.GetWheelRotation();
154:             int steps = mousewheelaccum / me.GetWheelDelta();
155:             if (!steps) return;
156:             mousewheelaccum -= steps * me.GetWheelDelta();
157:             sys->frame->SetStatus(doc->Wheel(steps, me.AltDown(), ctrl, me.ShiftDown()));
158:         } else if (me.GetWheelAxis()) {
159:             CursorScroll(me.GetWheelRotation() * g_scrollratewheel, 0);
160:         } else {
161:             CursorScroll(0, -me.GetWheelRotation() * g_scrollratewheel);
162:         }
163:     }
164: 
165:     void OnSize(wxSizeEvent &se) {}
166:     void OnContextMenuClick(wxContextMenuEvent &cme) {
167:         if (lastrmbwaswithctrl) {
168:             auto tagmenu = make_unique<wxMenu>();
169:             doc->RecreateTagMenu(*tagmenu);
170:             PopupMenu(tagmenu.get());
171:         } else {
172:             PopupMenu(frame->editmenupopup);
173:         }
174:     }
175: 
176:     void OnScrollWin(wxScrollWinEvent &swe) {
177:         // This only gets called when scrolling using the scroll bar, not with mousewheel.
178:         swe.Skip();  // Use default scrolling behavior.
179:     }
180: 
181:     void CursorScroll(int dx, int dy) {
182:         int x, y;
183:         GetViewStart(&x, &y);
184:         x += dx;
185:         y += dy;
186:         // EnableScrolling(true, true);
187:         Scroll(x, y);
188:         // EnableScrolling(false, false);
189:     }
190: 
191:     DECLARE_EVENT_TABLE()
192: };

================
File: src/tsframe.h
================
   1: struct TSFrame : wxFrame {
   2:     TSApp *app;
   3:     wxIcon icon;
   4:     wxTaskBarIcon taskbaricon;
   5:     wxMenu *editmenupopup;
   6:     wxFileHistory filehistory;
   7:     #ifdef ENABLE_LOBSTER
   8:         wxFileHistory scripts {A_MAXACTION - A_SCRIPT, A_SCRIPT};
   9:     #endif
  10:     wxFileSystemWatcher *watcher;
  11:     wxAuiNotebook *notebook {nullptr};
  12:     wxAuiManager aui {this};
  13:     wxBitmap line_nw;
  14:     wxBitmap line_sw;
  15:     wxBitmap foldicon;
  16:     bool fromclosebox {true};
  17:     bool watcherwaitingforuser {false};
  18:     wxColour toolbarbackgroundcolor {0xD8C7BC};
  19:     wxTextCtrl *filter {nullptr};
  20:     wxTextCtrl *replaces {nullptr};
  21:     ColorDropdown *cellcolordropdown {nullptr};
  22:     ColorDropdown *textcolordropdown {nullptr};
  23:     ColorDropdown *bordercolordropdown {nullptr};
  24:     ImageDropdown *imagedropdown {nullptr};
  25:     wxString imagepath;
  26:     int refreshhack {0};
  27:     int refreshhackinstances {0};
  28:     std::map<wxString, wxString> menustrings;
  29: 
  30:     TSFrame(TSApp *_app)
  31:         : wxFrame((wxFrame *)nullptr, wxID_ANY, "TreeSheets", wxDefaultPosition, wxDefaultSize,
  32:                   wxDEFAULT_FRAME_STYLE),
  33:           app(_app) {
  34:         sys->frame = this;
  35: 
  36:         class MyLog : public wxLog {
  37:             void DoLogString(const wxChar *message, time_t timestamp) { DoLogText(*message); }
  38:             void DoLogText(const wxString &message) {
  39:                 #ifdef WIN32
  40:                 OutputDebugString(message.c_str());
  41:                 OutputDebugString(L"\n");
  42:                 #else
  43:                 fputws(message.c_str(), stderr);
  44:                 fputws(L"\n", stderr);
  45:                 #endif
  46:             }
  47:         };
  48: 
  49:         wxLog::SetActiveTarget(new MyLog());
  50: 
  51:         wxLogMessage("%s", wxVERSION_STRING);
  52: 
  53:         aui.SetManagedWindow(this);
  54: 
  55:         wxInitAllImageHandlers();
  56: 
  57:         wxIconBundle icons;
  58:         wxIcon iconbig;
  59:         #ifdef WIN32
  60:             int iconsmall = ::GetSystemMetrics(SM_CXSMICON);
  61:             int iconlarge = ::GetSystemMetrics(SM_CXICON);
  62:         #endif
  63:         icon.LoadFile(app->GetDataPath("images/icon16.png"), wxBITMAP_TYPE_PNG
  64:             #ifdef WIN32
  65:                 , iconsmall, iconsmall
  66:             #endif
  67:         );
  68:         iconbig.LoadFile(app->GetDataPath("images/icon32.png"), wxBITMAP_TYPE_PNG
  69:             #ifdef WIN32
  70:                 , iconlarge, iconlarge
  71:             #endif
  72:         );
  73:         if (!icon.IsOk() || !iconbig.IsOk()) {
  74:             wxMessageBox(_("Error loading core data file (TreeSheets not installed correctly?)"),
  75:                          _("Initialization Error"), wxOK, this);
  76:             exit(1);
  77:         }
  78:         icons.AddIcon(icon);
  79:         icons.AddIcon(iconbig);
  80:         SetIcons(icons);
  81: 
  82:         RenderFolderIcon();
  83:         line_nw.LoadFile(app->GetDataPath("images/render/line_nw.png"), wxBITMAP_TYPE_PNG);
  84:         line_sw.LoadFile(app->GetDataPath("images/render/line_sw.png"), wxBITMAP_TYPE_PNG);
  85: 
  86:         imagepath = app->GetDataPath("images/nuvola/dropdown/");
  87: 
  88:         if (sys->singletray)
  89:             taskbaricon.Connect(wxID_ANY, wxEVT_TASKBAR_LEFT_UP,
  90:                         wxTaskBarIconEventHandler(TSFrame::OnTBIDBLClick), nullptr, this);
  91:         else
  92:             taskbaricon.Connect(wxID_ANY, wxEVT_TASKBAR_LEFT_DCLICK,
  93:                         wxTaskBarIconEventHandler(TSFrame::OnTBIDBLClick), nullptr, this);
  94: 
  95:         bool showtbar, showsbar, lefttabs;
  96: 
  97:         sys->cfg->Read("showtbar", &showtbar, true);
  98:         sys->cfg->Read("showsbar", &showsbar, true);
  99:         sys->cfg->Read("lefttabs", &lefttabs, true);
 100: 
 101:         filehistory.Load(*sys->cfg);
 102:         #ifdef ENABLE_LOBSTER
 103:             auto oldpath = sys->cfg->GetPath();
 104:             sys->cfg->SetPath("/scripts");
 105:             scripts.Load(*sys->cfg);
 106:             sys->cfg->SetPath(oldpath);
 107:         #endif
 108: 
 109:         #ifdef __WXMAC__
 110:             #define CTRLORALT "CTRL"
 111:         #else
 112:             #define CTRLORALT "ALT"
 113:         #endif
 114: 
 115:         #ifdef __WXMAC__
 116:             #define ALTORCTRL "ALT"
 117:         #else
 118:             #define ALTORCTRL "CTRL"
 119:         #endif
 120: 
 121:         auto expmenu = new wxMenu();
 122:         MyAppend(expmenu, A_EXPXML, _("&XML..."),
 123:                  _("Export the current view as XML (which can also be reimported without losing structure)"));
 124:         expmenu->AppendSeparator();
 125:         MyAppend(expmenu, A_EXPHTMLT, _("&HTML (Tables+Styling)..."),
 126:                  _("Export the current view as HTML using nested tables, that will look somewhat like the TreeSheet"));
 127:         MyAppend(expmenu, A_EXPHTMLTE, _("&HTML (Tables+Styling+Images)..."),
 128:                  _("Export the curent view as HTML using nested tables and exported images"));
 129:         MyAppend(expmenu, A_EXPHTMLB, _("HTML (&Bullet points)..."),
 130:                  _("Export the current view as HTML as nested bullet points."));
 131:         MyAppend(expmenu, A_EXPHTMLO, _("HTML (&Outline)..."),
 132:                  _("Export the current view as HTML as nested headers, suitable for importing into Word's outline mode"));
 133:         expmenu->AppendSeparator();
 134:         MyAppend(
 135:             expmenu, A_EXPTEXT, _("Indented &Text..."),
 136:             _("Export the current view as tree structured text, using spaces for each indentation level. Suitable for importing into mindmanagers and general text programs"));
 137:         MyAppend(
 138:             expmenu, A_EXPCSV, _("&Comma delimited text (CSV)..."),
 139:             _("Export the current view as CSV. Good for spreadsheets and databases. Only works on grids with no sub-grids (use the Flatten operation first if need be)"));
 140:         expmenu->AppendSeparator();
 141:         MyAppend(expmenu, A_EXPIMAGE, _("&Image..."),
 142:                  _("Export the current view as an image. Useful for faithful renderings of the TreeSheet, and programs that don't accept any of the above options"));
 143:         MyAppend(expmenu, A_EXPSVG, _("&Vector graphics..."),
 144:                 _("Export the current view to a SVG vector file."));
 145: 
 146:         auto impmenu = new wxMenu();
 147:         MyAppend(impmenu, A_IMPXML, _("XML..."));
 148:         MyAppend(impmenu, A_IMPXMLA, _("XML (attributes too, for OPML etc)..."));
 149:         MyAppend(impmenu, A_IMPTXTI, _("Indented text..."));
 150:         MyAppend(impmenu, A_IMPTXTC, _("Comma delimited text (CSV)..."));
 151:         MyAppend(impmenu, A_IMPTXTS, _("Semi-Colon delimited text (CSV)..."));
 152:         MyAppend(impmenu, A_IMPTXTT, _("Tab delimited text..."));
 153: 
 154:         auto recentmenu = new wxMenu();
 155:         filehistory.UseMenu(recentmenu);
 156:         filehistory.AddFilesToMenu();
 157: 
 158:         auto filemenu = new wxMenu();
 159:         MyAppend(filemenu, wxID_NEW, _("&New") + "\tCTRL+N", _("Create a new document"));
 160:         MyAppend(filemenu, wxID_OPEN, _("&Open...") + "\tCTRL+O", _("Open an existing document"));
 161:         MyAppend(filemenu, wxID_CLOSE, _("&Close") + "\tCTRL+W", _("Close current document"));
 162:         filemenu->AppendSubMenu(recentmenu, _("&Recent files"));
 163:         MyAppend(filemenu, wxID_SAVE, _("&Save") + "\tCTRL+S", _("Save current document"));
 164:         MyAppend(filemenu, wxID_SAVEAS, _("Save &As..."),
 165:                  _("Save current document with a different filename"));
 166:         MyAppend(filemenu, A_SAVEALL, _("Save All"));
 167:         filemenu->AppendSeparator();
 168:         MyAppend(filemenu, A_PAGESETUP, _("Page Setup..."));
 169:         MyAppend(filemenu, A_PRINTSCALE, _("Set Print Scale..."));
 170:         MyAppend(filemenu, wxID_PREVIEW, _("Print preview..."));
 171:         MyAppend(filemenu, wxID_PRINT, _("&Print...") + "\tCTRL+P");
 172:         filemenu->AppendSeparator();
 173:         filemenu->AppendSubMenu(expmenu, _("Export &view as"));
 174:         filemenu->AppendSubMenu(impmenu, _("Import from"));
 175:         filemenu->AppendSeparator();
 176:         MyAppend(filemenu, wxID_EXIT, _("&Exit") + "\tCTRL+Q", _("Quit this program"));
 177: 
 178:         wxMenu *editmenu;
 179:         loop(twoeditmenus, 2) {
 180:             auto sizemenu = new wxMenu();
 181:             MyAppend(sizemenu, A_INCSIZE,
 182:                      _("&Increase text size (SHIFT+mousewheel)") + "\tSHIFT+PGUP");
 183:             MyAppend(sizemenu, A_DECSIZE,
 184:                      _("&Decrease text size (SHIFT+mousewheel)") + "\tSHIFT+PGDN");
 185:             MyAppend(sizemenu, A_RESETSIZE, _("&Reset text sizes") + "\tCTRL+SHIFT+S");
 186:             MyAppend(sizemenu, A_MINISIZE, _("&Shrink text of all sub-grids") + "\tCTRL+SHIFT+M");
 187:             sizemenu->AppendSeparator();
 188:             MyAppend(sizemenu, A_INCWIDTH,
 189:                      _("Increase column width (ALT+mousewheel)") + "\tALT+PGUP");
 190:             MyAppend(sizemenu, A_DECWIDTH,
 191:                      _("Decrease column width (ALT+mousewheel)") + "\tALT+PGDN");
 192:             MyAppend(sizemenu, A_INCWIDTHNH,
 193:                      _("Increase column width (no sub grids)") + "\tCTRL+ALT+PGUP");
 194:             MyAppend(sizemenu, A_DECWIDTHNH,
 195:                      _("Decrease column width (no sub grids)") + "\tCTRL+ALT+PGDN");
 196:             MyAppend(sizemenu, A_RESETWIDTH, _("Reset column widths") + "\tCTRL+R",
 197:                      _("Reset the column widths in the selection to the default column width"));
 198: 
 199:             auto bordmenu = new wxMenu();
 200:             MyAppend(bordmenu, A_BORD0, _("Border &0") + "\tCTRL+SHIFT+9");
 201:             MyAppend(bordmenu, A_BORD1, _("Border &1") + "\tCTRL+SHIFT+1");
 202:             MyAppend(bordmenu, A_BORD2, _("Border &2") + "\tCTRL+SHIFT+2");
 203:             MyAppend(bordmenu, A_BORD3, _("Border &3") + "\tCTRL+SHIFT+3");
 204:             MyAppend(bordmenu, A_BORD4, _("Border &4") + "\tCTRL+SHIFT+4");
 205:             MyAppend(bordmenu, A_BORD5, _("Border &5") + "\tCTRL+SHIFT+5");
 206: 
 207:             auto selmenu = new wxMenu();
 208:             MyAppend(selmenu, A_NEXT,
 209:                 #ifdef __WXGTK__
 210:                     _("Move to next cell (TAB)")
 211:                 #else
 212:                      _("Move to next cell") + "\tTAB"
 213:                 #endif
 214:             );
 215:             MyAppend(selmenu, A_PREV,
 216:                 #ifdef __WXGTK__
 217:                     _("Move to previous cell (SHIFT+TAB)")
 218:                 #else
 219:                      _("Move to previous cell") + "\tSHIFT+TAB"
 220:                 #endif
 221:             );
 222:             selmenu->AppendSeparator();
 223:             MyAppend(selmenu, wxID_SELECTALL, _("Select &all in current grid/cell") + "\tCTRL+A");
 224:             selmenu->AppendSeparator();
 225:             MyAppend(selmenu, A_LEFT,
 226:                 #ifdef __WXGTK__
 227:                     _("Move Selection Left (LEFT)")
 228:                 #else
 229:                      _("Move Selection Left") + "\tLEFT"
 230:                 #endif
 231:             );
 232:             MyAppend(selmenu, A_RIGHT,
 233:                 #ifdef __WXGTK__
 234:                     _("Move Selection Right (RIGHT)")
 235:                 #else
 236:                      _("Move Selection Right") + "\tRIGHT"
 237:                 #endif
 238:             );
 239:             MyAppend(selmenu, A_UP,
 240:                 #ifdef __WXGTK__
 241:                     _("Move Selection Up (UP)")
 242:                 #else
 243:                      _("Move Selection Up") + "\tUP"
 244:                 #endif
 245:             );
 246:             MyAppend(selmenu, A_DOWN,
 247:                 #ifdef __WXGTK__
 248:                     _("Move Selection Down (DOWN)")
 249:                 #else
 250:                      _("Move Selection Down") + "\tDOWN"
 251:                 #endif
 252:             );
 253:             selmenu->AppendSeparator();
 254:             MyAppend(selmenu, A_MLEFT, _("Move Cells Left") + "\tCTRL+LEFT");
 255:             MyAppend(selmenu, A_MRIGHT, _("Move Cells Right") + "\tCTRL+RIGHT");
 256:             MyAppend(selmenu, A_MUP, _("Move Cells Up") + "\tCTRL+UP");
 257:             MyAppend(selmenu, A_MDOWN, _("Move Cells Down") + "\tCTRL+DOWN");
 258:             selmenu->AppendSeparator();
 259:             MyAppend(selmenu, A_SLEFT, _("Extend Selection Left") + "\tSHIFT+LEFT");
 260:             MyAppend(selmenu, A_SRIGHT, _("Extend Selection Right") + "\tSHIFT+RIGHT");
 261:             MyAppend(selmenu, A_SUP, _("Extend Selection Up") + "\tSHIFT+UP");
 262:             MyAppend(selmenu, A_SDOWN, _("Extend Selection Down") + "\tSHIFT+DOWN");
 263:             selmenu->AppendSeparator();
 264:             MyAppend(selmenu, A_SROWS, _("Extend Selection Full Rows") + "\tCTRL+SHIFT+B");
 265:             MyAppend(selmenu, A_SCLEFT, _("Extend Selection Rows Left") + "\tCTRL+SHIFT+LEFT");
 266:             MyAppend(selmenu, A_SCRIGHT, _("Extend Selection Rows Right") + "\tCTRL+SHIFT+RIGHT");
 267:             selmenu->AppendSeparator();
 268:             MyAppend(selmenu, A_SCOLS, _("Extend Selection Full Columns") + "\tCTRL+SHIFT+A");
 269:             MyAppend(selmenu, A_SCUP, _("Extend Selection Columns Up") + "\tCTRL+SHIFT+UP");
 270:             MyAppend(selmenu, A_SCDOWN, _("Extend Selection Columns Down") + "\tCTRL+SHIFT+DOWN");
 271:             selmenu->AppendSeparator();
 272:             MyAppend(selmenu, A_CANCELEDIT, _("Select &Parent") + "\tESC");
 273:             MyAppend(selmenu, A_ENTERGRID, _("Select First &Child") + "\tSHIFT+ENTER");
 274:             selmenu->AppendSeparator();
 275:             MyAppend(selmenu, A_LINK, _("Go To &Matching Cell (Text)") + "\tF6");
 276:             MyAppend(selmenu, A_LINKREV, _("Go To Matching Cell (Text, Reverse)") + "\tSHIFT+F6");
 277:             MyAppend(selmenu, A_LINKIMG, _("Go To Matching Cell (Image)") + "\tF7");
 278:             MyAppend(selmenu, A_LINKIMGREV,
 279:                      _("Go To Matching Cell (Image, Reverse)") + "\tSHIFT+F7");
 280: 
 281:             auto temenu = new wxMenu();
 282:             MyAppend(temenu, A_LEFT, _("Cursor Left") + "\tLEFT");
 283:             MyAppend(temenu, A_RIGHT, _("Cursor Right") + "\tRIGHT");
 284:             MyAppend(temenu, A_MLEFT, _("Word Left") + "\tCTRL+LEFT");
 285:             MyAppend(temenu, A_MRIGHT, _("Word Right") + "\tCTRL+RIGHT");
 286:             temenu->AppendSeparator();
 287:             MyAppend(temenu, A_SLEFT, _("Extend Selection Left") + "\tSHIFT+LEFT");
 288:             MyAppend(temenu, A_SRIGHT, _("Extend Selection Right") + "\tSHIFT+RIGHT");
 289:             MyAppend(temenu, A_SCLEFT, _("Extend Selection Word Left") + "\tCTRL+SHIFT+LEFT");
 290:             MyAppend(temenu, A_SCRIGHT, _("Extend Selection Word Right") + "\tCTRL+SHIFT+RIGHT");
 291:             MyAppend(temenu, A_SHOME, _("Extend Selection to Start") + "\tSHIFT+HOME");
 292:             MyAppend(temenu, A_SEND, _("Extend Selection to End") + "\tSHIFT+END");
 293:             temenu->AppendSeparator();
 294:             MyAppend(temenu, A_HOME, _("Start of line of text") + "\tHOME");
 295:             MyAppend(temenu, A_END, _("End of line of text") + "\tEND");
 296:             MyAppend(temenu, A_CHOME, _("Start of text") + "\tCTRL+HOME");
 297:             MyAppend(temenu, A_CEND, _("End of text") + "\tCTRL+END");
 298:             temenu->AppendSeparator();
 299:             MyAppend(temenu, A_ENTERCELL, _("Enter/exit text edit mode") + "\tENTER");
 300:             MyAppend(temenu, A_ENTERCELL_JUMPTOEND,
 301:                      _("...and jump to the end of the text") + "\tF2");
 302:             MyAppend(
 303:                 temenu, A_ENTERCELL_JUMPTOSTART,
 304:                 _("...and progress to the first cell in the new row") + "\t" ALTORCTRL "+ENTER");
 305:             MyAppend(temenu, A_PROGRESSCELL,
 306:                      _("...and progress to the next cell on the right") + "\t" CTRLORALT "+ENTER");
 307:             MyAppend(temenu, A_CANCELEDIT, _("Cancel text edits") + "\tESC");
 308: 
 309:             auto stmenu = new wxMenu();
 310:             MyAppend(stmenu, wxID_BOLD, _("Toggle cell &BOLD") + "\tCTRL+B");
 311:             MyAppend(stmenu, wxID_ITALIC, _("Toggle cell &ITALIC") + "\tCTRL+I");
 312:             MyAppend(stmenu, A_TT, _("Toggle cell &typewriter") + "\tCTRL+ALT+T");
 313:             MyAppend(stmenu, wxID_UNDERLINE, _("Toggle cell &underlined") + "\tCTRL+U");
 314:             MyAppend(stmenu, wxID_STRIKETHROUGH, _("Toggle cell &strikethrough") + "\tCTRL+T");
 315:             stmenu->AppendSeparator();
 316:             MyAppend(stmenu, A_RESETSTYLE, _("&Reset text styles") + "\tCTRL+SHIFT+R");
 317:             MyAppend(stmenu, A_RESETCOLOR, _("Reset &colors") + "\tCTRL+SHIFT+C");
 318:             stmenu->AppendSeparator();
 319:             MyAppend(stmenu, A_LASTCELLCOLOR, _("Apply last cell color") + "\tSHIFT+ALT+C");
 320:             MyAppend(stmenu, A_LASTTEXTCOLOR, _("Apply last text color") + "\tSHIFT+ALT+T");
 321:             MyAppend(stmenu, A_LASTBORDCOLOR, _("Apply last border color") + "\tSHIFT+ALT+B");
 322:             MyAppend(stmenu, A_OPENCELLCOLOR, _("Open cell colors") + "\tSHIFT+ALT+F9");
 323:             MyAppend(stmenu, A_OPENTEXTCOLOR, _("Open text colors") + "\tSHIFT+ALT+F10");
 324:             MyAppend(stmenu, A_OPENBORDCOLOR, _("Open border colors") + "\tSHIFT+ALT+F11");
 325:             MyAppend(stmenu, A_OPENIMGDROPDOWN, _("Open image dropdown") + "\tSHIFT+ALT+F12");
 326: 
 327:             auto tagmenu = new wxMenu();
 328:             MyAppend(tagmenu, A_TAGADD, _("&Add Cell Text as Tag"));
 329:             MyAppend(tagmenu, A_TAGREMOVE, _("&Remove Cell Text from Tags"));
 330:             MyAppend(tagmenu, A_NOP, _("&Set Cell Text to tag (use CTRL+RMB)"),
 331:                      _("Hold CTRL while pressing right mouse button to quickly set a tag for the current cell using a popup menu"));
 332: 
 333:             auto orgmenu = new wxMenu();
 334:             MyAppend(orgmenu, A_TRANSPOSE, _("&Transpose") + "\tCTRL+SHIFT+T",
 335:                      _("changes the orientation of a grid"));
 336:             MyAppend(orgmenu, A_SORT, _("Sort &Ascending"),
 337:                      _("Make a 1xN selection to indicate which column to sort on, and which rows to affect"));
 338:             MyAppend(orgmenu, A_SORTD, _("Sort &Descending"),
 339:                      _("Make a 1xN selection to indicate which column to sort on, and which rows to affect"));
 340:             MyAppend(orgmenu, A_HSWAP, _("Hierarchy &Swap") + "\tF8",
 341:                      _("Swap all cells with this text at this level (or above) with the parent"));
 342:             MyAppend(orgmenu, A_HIFY, _("&Hierarchify"),
 343:                      _("Convert an NxN grid with repeating elements per column into an 1xN grid with hierarchy, useful to convert data from spreadsheets"));
 344:             MyAppend(orgmenu, A_FLATTEN, _("&Flatten"),
 345:                      _("Takes a hierarchy (nested 1xN or Nx1 grids) and converts it into a flat NxN grid, useful for export to spreadsheets"));
 346: 
 347:             auto imgmenu = new wxMenu();
 348:             MyAppend(imgmenu, A_IMAGE, _("&Add..."), _("Add an image to the selected cell"));
 349:             MyAppend(imgmenu, A_IMAGESVA, _("&Save as..."),
 350:                      _("Save image(s) from selected cell(s) to disk. Multiple images will be saved with a counter appended to each file name."));
 351:             imgmenu->AppendSeparator();
 352:             MyAppend(
 353:                 imgmenu, A_IMAGESCP, _("Scale (re-sa&mple pixels, by %)"),
 354:                 _("Change the image(s) size if it is too big, by reducing the amount of pixels"));
 355:             MyAppend(
 356:                 imgmenu, A_IMAGESCW, _("Scale (re-sample pixels, by &width)"),
 357:                 _("Change the image(s) size if it is too big, by reducing the amount of pixels"));
 358:             MyAppend(imgmenu, A_IMAGESCF, _("Scale (&display only)"),
 359:                      _("Change the image(s) size if it is too big or too small, by changing the size shown on screen. Applies to all uses of this image."));
 360:             MyAppend(imgmenu, A_IMAGESCN, _("&Reset Scale (display only)"),
 361:                      _("Change the image(s) scale to match DPI of the current display. Applies to all uses of this image."));
 362:             imgmenu->AppendSeparator();
 363:             MyAppend(
 364:                 imgmenu, A_SAVE_AS_JPEG, _("Embed as &JPEG"),
 365:                 _("Embed the image(s) in the selected cells in JPEG format (reduces data size)"));
 366:             MyAppend(imgmenu, A_SAVE_AS_PNG, _("Embed as &PNG"),
 367:                      _("Embed the image(s) in the selected cells in PNG format (default)"));
 368:             imgmenu->AppendSeparator();
 369:             MyAppend(imgmenu, A_LASTIMAGE, _("Insert last image") + "\tSHIFT+ALT+i",
 370:                      _("Insert the last image that has been inserted before in TreeSheets."));
 371:             MyAppend(imgmenu, A_IMAGER, _("Remo&ve"),
 372:                      _("Remove image(s) from the selected cells"));
 373: 
 374:             auto navmenu = new wxMenu();
 375:             MyAppend(navmenu, A_BROWSE, _("Open link in &browser") + "\tF5",
 376:                      _("Opens up the text from the selected cell in browser (should start be a valid URL)"));
 377:             MyAppend(navmenu, A_BROWSEF, _("Open &file") + "\tF4",
 378:                      _("Opens up the text from the selected cell in default application for the file type"));
 379: 
 380:             auto laymenu = new wxMenu();
 381:             MyAppend(laymenu, A_V_GS,
 382:                      _("Vertical Layout with Grid Style Rendering") + "\t" CTRLORALT "+1");
 383:             MyAppend(laymenu, A_V_BS,
 384:                      _("Vertical Layout with Bubble Style Rendering") + "\t" CTRLORALT "+2");
 385:             MyAppend(laymenu, A_V_LS,
 386:                      _("Vertical Layout with Line Style Rendering") + "\t" CTRLORALT "+3");
 387:             laymenu->AppendSeparator();
 388:             MyAppend(laymenu, A_H_GS,
 389:                      _("Horizontal Layout with Grid Style Rendering") + "\t" CTRLORALT "+4");
 390:             MyAppend(laymenu, A_H_BS,
 391:                      _("Horizontal Layout with Bubble Style Rendering") + "\t" CTRLORALT "+5");
 392:             MyAppend(laymenu, A_H_LS,
 393:                      _("Horizontal Layout with Line Style Rendering") + "\t" CTRLORALT "+6");
 394:             laymenu->AppendSeparator();
 395:             MyAppend(laymenu, A_GS, _("Grid Style Rendering") + "\t" CTRLORALT "+7");
 396:             MyAppend(laymenu, A_BS, _("Bubble Style Rendering") + "\t" CTRLORALT "+8");
 397:             MyAppend(laymenu, A_LS, _("Line Style Rendering") + "\t" CTRLORALT "+9");
 398:             laymenu->AppendSeparator();
 399:             MyAppend(laymenu, A_TEXTGRID, _("Toggle Vertical Layout") + "\t" CTRLORALT "+0",
 400:                      _("Make a hierarchy layout more vertical (default) or more horizontal"));
 401: 
 402:             editmenu = new wxMenu();
 403:             MyAppend(editmenu, wxID_CUT, _("Cu&t") + "\tCTRL+X", _("Cut selection"));
 404:             MyAppend(editmenu, wxID_COPY, _("&Copy") + "\tCTRL+C", _("Copy selection"));
 405:             editmenu->AppendSeparator();
 406:             MyAppend(editmenu, A_COPYWI, _("Copy with &Images") + "\tCTRL+ALT+C");
 407:             MyAppend(editmenu, A_COPYBM, _("&Copy as Bitmap"));
 408:             MyAppend(editmenu, A_COPYCT, _("Copy As Continuous Text"));
 409:             editmenu->AppendSeparator();
 410:             MyAppend(editmenu, wxID_PASTE, _("&Paste") + "\tCTRL+V", _("Paste clipboard contents"));
 411:             MyAppend(editmenu, A_PASTESTYLE, _("Paste Style Only") + "\tCTRL+SHIFT+V",
 412:                      _("only sets the colors and style of the copied cell, and keeps the text"));
 413:             MyAppend(editmenu, A_COLLAPSE, _("Collapse Ce&lls") + "\tCTRL+L");
 414:             editmenu->AppendSeparator();
 415: 
 416:             MyAppend(editmenu, A_EDITNOTE, _("Edit &Note") + "\tCTRL+E",
 417:                      _("Edit the note of the selected cell"));
 418:             MyAppend(editmenu, wxID_UNDO, _("&Undo") + "\tCTRL+Z",
 419:                      _("revert the changes, one step at a time"));
 420:             MyAppend(editmenu, wxID_REDO, _("&Redo") + "\tCTRL+Y",
 421:                      _("redo any undo steps, if you haven't made changes since"));
 422:             editmenu->AppendSeparator();
 423:             MyAppend(
 424:                 editmenu, A_DELETE, _("&Delete After") + "\tDEL",
 425:                 _("Deletes the column of cells after the selected grid line, or the row below"));
 426:             MyAppend(
 427:                 editmenu, A_BACKSPACE, _("Delete Before") + "\tBACK",
 428:                 _("Deletes the column of cells before the selected grid line, or the row above"));
 429:             MyAppend(editmenu, A_DELETE_WORD, _("Delete Word After") + "\tCTRL+DEL",
 430:                      _("Deletes the entire word after the cursor"));
 431:             MyAppend(editmenu, A_BACKSPACE_WORD, _("Delete Word Before") + "\tCTRL+BACK",
 432:                      _("Deletes the entire word before the cursor"));
 433:             editmenu->AppendSeparator();
 434:             MyAppend(editmenu, A_NEWGRID,
 435:                      #ifdef __WXMAC__
 436:                      _("&Insert New Grid") + "\tCTRL+G",
 437:                      #else
 438:                      _("&Insert New Grid") + "\tINS",
 439:                      #endif
 440:                      _("Adds a grid to the selected cell"));
 441:             MyAppend(editmenu, A_WRAP, _("&Wrap in new parent") + "\tF9",
 442:                      _("Creates a new level of hierarchy around the current selection"));
 443:             editmenu->AppendSeparator();
 444:             // F10 is tied to the OS on both Ubuntu and OS X, and SHIFT+F10 is now right
 445:             // click on all platforms?
 446:             MyAppend(editmenu, A_FOLD,
 447:                      #ifndef WIN32
 448:                      _("Toggle Fold") + "\tCTRL+F10",
 449:                      #else
 450:                      _("Toggle Fold") + "\tF10",
 451:                      #endif
 452:                      _("Toggles showing the grid of the selected cell(s)"));
 453:             MyAppend(editmenu, A_FOLDALL, _("Fold All") + "\tCTRL+SHIFT+F10",
 454:                      _("Folds the grid of the selected cell(s) recursively"));
 455:             MyAppend(editmenu, A_UNFOLDALL, _("Unfold All") + "\tCTRL+ALT+F10",
 456:                      _("Unfolds the grid of the selected cell(s) recursively"));
 457:             editmenu->AppendSeparator();
 458:             editmenu->AppendSubMenu(selmenu, _("&Selection"));
 459:             editmenu->AppendSubMenu(orgmenu, _("&Grid Reorganization"));
 460:             editmenu->AppendSubMenu(laymenu, _("&Layout && Render Style"));
 461:             editmenu->AppendSubMenu(imgmenu, _("&Images"));
 462:             editmenu->AppendSubMenu(navmenu, _("&Browsing"));
 463:             editmenu->AppendSubMenu(temenu, _("Text &Editing"));
 464:             editmenu->AppendSubMenu(sizemenu, _("Text Sizing"));
 465:             editmenu->AppendSubMenu(stmenu, _("Text Style"));
 466:             editmenu->AppendSubMenu(bordmenu, _("Set Grid Border Width"));
 467:             editmenu->AppendSubMenu(tagmenu, _("Tag"));
 468: 
 469:             if (!twoeditmenus) editmenupopup = editmenu;
 470:         }
 471: 
 472:         auto semenu = new wxMenu();
 473:         MyAppend(semenu, wxID_FIND, _("&Search") + "\tCTRL+F", _("Find in document"));
 474:         semenu->AppendCheckItem(A_CASESENSITIVESEARCH, _("Case-sensitive search"));
 475:         semenu->Check(A_CASESENSITIVESEARCH, sys->casesensitivesearch);
 476:         semenu->AppendSeparator();
 477:         MyAppend(semenu, A_SEARCHNEXT, _("&Next Match") + "\tF3", _("Go to next search match"));
 478:         MyAppend(semenu, A_SEARCHPREV, _("&Previous Match") + "\tSHIFT+F3",
 479:                  _("Go to previous search match"));
 480:         semenu->AppendSeparator();
 481:         MyAppend(semenu, wxID_REPLACE, _("&Replace") + "\tCTRL+H",
 482:                  _("Find and replace in document"));
 483:         MyAppend(semenu, A_REPLACEONCE, _("Replace in Current &Selection") + "\tCTRL+K");
 484:         MyAppend(semenu, A_REPLACEONCEJ,
 485:                  _("Replace in Current Selection && &Jump Next") + "\tCTRL+J");
 486:         MyAppend(semenu, A_REPLACEALL, _("Replace &All"));
 487: 
 488:         auto scrollmenu = new wxMenu();
 489:         MyAppend(scrollmenu, A_AUP, _("Scroll Up (mousewheel)") + "\tPGUP");
 490:         MyAppend(scrollmenu, A_AUP, _("Scroll Up (mousewheel)") + "\tALT+UP");
 491:         MyAppend(scrollmenu, A_ADOWN, _("Scroll Down (mousewheel)") + "\tPGDN");
 492:         MyAppend(scrollmenu, A_ADOWN, _("Scroll Down (mousewheel)") + "\tALT+DOWN");
 493:         MyAppend(scrollmenu, A_ALEFT, _("Scroll Left") + "\tALT+LEFT");
 494:         MyAppend(scrollmenu, A_ARIGHT, _("Scroll Right") + "\tALT+RIGHT");
 495: 
 496:         auto filtermenu = new wxMenu();
 497:         MyAppend(filtermenu, A_FILTEROFF, _("Turn filter &off") + "\tCTRL+SHIFT+F");
 498:         MyAppend(filtermenu, A_FILTERS, _("Show only cells in current search"));
 499:         MyAppend(filtermenu, A_FILTERRANGE, _("Show last edits in specific date range"));
 500:         // xgettext:no-c-format
 501:         MyAppend(filtermenu, A_FILTER5, _("Show 5% of last edits"));
 502:         // xgettext:no-c-format
 503:         MyAppend(filtermenu, A_FILTER10, _("Show 10% of last edits"));
 504:         // xgettext:no-c-format
 505:         MyAppend(filtermenu, A_FILTER20, _("Show 20% of last edits"));
 506:         // xgettext:no-c-format
 507:         MyAppend(filtermenu, A_FILTER50, _("Show 50% of last edits"));
 508:         // xgettext:no-c-format
 509:         MyAppend(filtermenu, A_FILTERM, _("Show 1% more than the last filter"));
 510:         // xgettext:no-c-format
 511:         MyAppend(filtermenu, A_FILTERL, _("Show 1% less than the last filter"));
 512:         MyAppend(filtermenu, A_FILTERNOTE, _("Show cells with notes"));
 513:         MyAppend(filtermenu, A_FILTERBYCELLBG, _("Filter by the same cell color"));
 514:         MyAppend(filtermenu, A_FILTERMATCHNEXT, _("Go to next filter match") + "\tCTRL+F3");
 515: 
 516:         auto viewmenu = new wxMenu();
 517:         MyAppend(viewmenu, A_ZOOMIN, _("Zoom &In (CTRL+mousewheel)") + "\tCTRL+PGUP");
 518:         MyAppend(viewmenu, A_ZOOMOUT, _("Zoom &Out (CTRL+mousewheel)") + "\tCTRL+PGDN");
 519:         viewmenu->AppendSeparator();
 520:         MyAppend(
 521:             viewmenu, A_NEXTFILE,
 522:             _("&Next tab")
 523:                  #ifndef __WXGTK__
 524:                     // On Linux, this conflicts with CTRL+I, see Document::Key()
 525:                     // CTRL+SHIFT+TAB below still works, so that will have to be used to switch tabs.
 526:                      + "\tCTRL+TAB"
 527:                  #endif
 528:             ,
 529:             _("Go to the document in the next tab"));
 530:         MyAppend(viewmenu, A_PREVFILE, _("Previous tab") + "\tCTRL+SHIFT+TAB",
 531:                  _("Go to the document in the previous tab"));
 532:         viewmenu->AppendSeparator();
 533:         MyAppend(viewmenu, A_FULLSCREEN,
 534:                  #ifdef __WXMAC__
 535:                  _("Toggle &Fullscreen View") + "\tCTRL+F11");
 536:                  #else
 537:                  _("Toggle &Fullscreen View") + "\tF11");
 538:                  #endif
 539:         MyAppend(viewmenu, A_SCALED,
 540:                  #ifdef __WXMAC__
 541:                  _("Toggle &Scaled Presentation View") + "\tCTRL+F12");
 542:                  #else
 543:                  _("Toggle &Scaled Presentation View") + "\tF12");
 544:                  #endif
 545:         viewmenu->AppendSeparator();
 546:         viewmenu->AppendSubMenu(scrollmenu, _("Scroll Sheet"));
 547:         viewmenu->AppendSubMenu(filtermenu, _("Filter"));
 548: 
 549:         auto roundmenu = new wxMenu();
 550:         roundmenu->AppendRadioItem(A_ROUND0, _("Radius &0"));
 551:         roundmenu->AppendRadioItem(A_ROUND1, _("Radius &1"));
 552:         roundmenu->AppendRadioItem(A_ROUND2, _("Radius &2"));
 553:         roundmenu->AppendRadioItem(A_ROUND3, _("Radius &3"));
 554:         roundmenu->AppendRadioItem(A_ROUND4, _("Radius &4"));
 555:         roundmenu->AppendRadioItem(A_ROUND5, _("Radius &5"));
 556:         roundmenu->AppendRadioItem(A_ROUND6, _("Radius &6"));
 557:         roundmenu->Check(sys->roundness + A_ROUND0, true);
 558: 
 559:         auto autoexportmenu = new wxMenu();
 560:         autoexportmenu->AppendRadioItem(A_AUTOEXPORT_HTML_NONE, _("No autoexport"));
 561:         autoexportmenu->AppendRadioItem(A_AUTOEXPORT_HTML_WITH_IMAGES, _("Export with images"),
 562:                                         _("Export to a HTML file with exported images alongside "
 563:                                           "the original TreeSheets file when document is saved"));
 564:         autoexportmenu->AppendRadioItem(A_AUTOEXPORT_HTML_WITHOUT_IMAGES,
 565:                                         _("Export without images"),
 566:                                         _("Export to a HTML file alongside the original "
 567:                                           "TreeSheets file when document is saved"));
 568:         autoexportmenu->Check(sys->autohtmlexport + A_AUTOEXPORT_HTML_NONE, true);
 569: 
 570:         auto optmenu = new wxMenu();
 571:         MyAppend(optmenu, wxID_SELECT_FONT, _("Font..."),
 572:                  _("Set the font the document text is displayed with"));
 573:         MyAppend(optmenu, A_SET_FIXED_FONT, _("Typewriter font..."),
 574:                  _("Set the font the typewriter text is displayed with."));
 575:         MyAppend(optmenu, A_CUSTKEY, _("Key bindings..."),
 576:                  _("Change the key binding of a menu item"));
 577:         MyAppend(optmenu, A_SETLANG, _("Change language..."), _("Change interface language"));
 578:         MyAppend(optmenu, A_DEFAULTMAXCOLWIDTH, _("Default column width..."),
 579:                  _("Set the default column width for a new grid"));
 580:         optmenu->AppendSeparator();
 581:         MyAppend(optmenu, A_CUSTCOL, _("Custom &color..."),
 582:                  _("Set a custom color for the color dropdowns"));
 583:         MyAppend(
 584:             optmenu, A_COLCELL, _("&Set custom color from cell background"),
 585:             _("Set a custom color for the color dropdowns from the selected cell background"));
 586:         MyAppend(optmenu, A_DEFBGCOL, _("Background color..."),
 587:                  _("Set the color for the document background"));
 588:         MyAppend(optmenu, A_DEFCURCOL, _("Cu&rsor color..."),
 589:                  _("Set the color for the text cursor"));
 590:         optmenu->AppendSeparator();
 591:         MyAppend(optmenu, A_RESETPERSPECTIVE, _("Reset toolbar"),
 592:                  _("Reset the toolbar appearance"));
 593:         optmenu->AppendCheckItem(
 594:             A_SHOWTBAR, _("Toolbar"),
 595:             _("Toggle whether toolbar is shown between menu bar and documents"));
 596:         optmenu->Check(A_SHOWTBAR, sys->showtoolbar);
 597:         optmenu->AppendCheckItem(A_SHOWSBAR, _("Statusbar"),
 598:                                  _("Toggle whether statusbar is shown below the documents"));
 599:         optmenu->Check(A_SHOWSBAR, sys->showstatusbar);
 600:         optmenu->AppendCheckItem(
 601:             A_LEFTTABS, _("File Tabs on the bottom"),
 602:             _("Toggle whether file tabs are shown on top or on bottom of the documents"));
 603:         optmenu->Check(A_LEFTTABS, lefttabs);
 604:         optmenu->AppendCheckItem(A_TOTRAY, _("Minimize to tray"),
 605:                                  _("Toogle whether window is minimized to system tray"));
 606:         optmenu->Check(A_TOTRAY, sys->totray);
 607:         optmenu->AppendCheckItem(A_MINCLOSE, _("Minimize on close"),
 608:                                  _("Toggle whether the window is minimized instead of closed"));
 609:         optmenu->Check(A_MINCLOSE, sys->minclose);
 610:         optmenu->AppendCheckItem(
 611:             A_SINGLETRAY, _("Single click maximize from tray"),
 612:             _("Toggle whether only one click is required to maximize from system tray"));
 613:         optmenu->Check(A_SINGLETRAY, sys->singletray);
 614:         optmenu->AppendCheckItem(A_STARTMINIMIZED, _("Start minimized"),
 615:                                  _("Start the application minimized"));
 616:         optmenu->Check(A_STARTMINIMIZED, sys->startminimized);
 617:         optmenu->AppendSeparator();
 618:         optmenu->AppendCheckItem(A_ZOOMSCR, _("Swap mousewheel scrolling and zooming"));
 619:         optmenu->Check(A_ZOOMSCR, sys->zoomscroll);
 620:         optmenu->AppendCheckItem(A_THINSELC, _("Navigate in between cells with cursor keys"),
 621:                                  _("Toggle whether the cursor keys are used for navigation in addition to text editing"));
 622:         optmenu->Check(A_THINSELC, sys->thinselc);
 623:         optmenu->AppendSeparator();
 624:         optmenu->AppendCheckItem(A_MAKEBAKS, _("Backup files"),
 625:                                  _("Create backup file before document is saved to file"));
 626:         optmenu->Check(A_MAKEBAKS, sys->makebaks);
 627:         optmenu->AppendCheckItem(A_AUTOSAVE, _("Autosave"),
 628:                                  _("Save open documents periodically to temporary files"));
 629:         optmenu->Check(A_AUTOSAVE, sys->autosave);
 630:         optmenu->AppendCheckItem(
 631:             A_FSWATCH, _("Autoreload documents"),
 632:             _("Reload when another computer has changed a file (if you have made changes, asks)"));
 633:         optmenu->Check(A_FSWATCH, sys->fswatch);
 634:         optmenu->AppendSubMenu(autoexportmenu, _("Autoexport to HTML"));
 635:         optmenu->AppendSeparator();
 636:         optmenu->AppendCheckItem(
 637:             A_CENTERED, _("Render document centered"),
 638:             _("Toggle whether documents are rendered centered or left aligned"));
 639:         optmenu->Check(A_CENTERED, sys->centered);
 640:         optmenu->AppendCheckItem(
 641:             A_FASTRENDER, _("Faster line rendering"),
 642:             _("Toggle whether lines are drawn solid (faster rendering) or dashed"));
 643:         optmenu->Check(A_FASTRENDER, sys->fastrender);
 644:         optmenu->AppendCheckItem(A_INVERTRENDER, _("Invert in dark mode"),
 645:                                  _("Invert the document in dark mode"));
 646:         optmenu->Check(A_INVERTRENDER, sys->followdarkmode);
 647:         optmenu->AppendSubMenu(roundmenu, _("&Roundness of grid borders"));
 648: 
 649:         #ifdef ENABLE_LOBSTER
 650:             auto scriptmenu = new wxMenu();
 651:             MyAppend(scriptmenu, A_ADDSCRIPT, _("Add...") + "\tCTRL+ALT+L",
 652:                      _("Add Lobster scripts to the menu"));
 653:             MyAppend(scriptmenu, A_DETSCRIPT, _("Remove...") + "\tCTRL+SHIFT+ALT+L",
 654:                      _("Remove script from list in the menu"));
 655:             scripts.UseMenu(scriptmenu);
 656:             scripts.SetMenuPathStyle(wxFH_PATH_SHOW_NEVER);
 657:             scripts.AddFilesToMenu();
 658: 
 659:             auto scriptpath = app->GetDataPath("scripts/");
 660:             auto sf = wxFindFirstFile(scriptpath + "*.lobster");
 661:             int sidx = 0;
 662:             while (!sf.empty()) {
 663:                 auto fn = wxFileName::FileName(sf).GetFullName();
 664:                 scripts.AddFileToHistory(fn);
 665:                 sf = wxFindNextFile();
 666:             }
 667:         #endif
 668: 
 669:         auto markmenu = new wxMenu();
 670:         MyAppend(markmenu, A_MARKDATA, _("&Data") + "\tCTRL+ALT+D");
 671:         MyAppend(markmenu, A_MARKCODE, _("&Operation") + "\tCTRL+ALT+O");
 672:         MyAppend(markmenu, A_MARKVARD, _("Variable &Assign") + "\tCTRL+ALT+A");
 673:         MyAppend(markmenu, A_MARKVARU, _("Variable &Read") + "\tCTRL+ALT+R");
 674:         MyAppend(markmenu, A_MARKVIEWH, _("&Horizontal View") + "\tCTRL+ALT+.");
 675:         MyAppend(markmenu, A_MARKVIEWV, _("&Vertical View") + "\tCTRL+ALT+,");
 676: 
 677:         auto langmenu = new wxMenu();
 678:         MyAppend(langmenu, wxID_EXECUTE, _("&Run") + "\tCTRL+ALT+F5");
 679:         langmenu->AppendSubMenu(markmenu, _("&Mark as"));
 680:         MyAppend(langmenu, A_CLRVIEW, _("&Clear Views"));
 681: 
 682:         auto helpmenu = new wxMenu();
 683:         MyAppend(helpmenu, wxID_ABOUT, _("&About..."), _("Show About dialog"));
 684:         helpmenu->AppendSeparator();
 685:         MyAppend(helpmenu, wxID_HELP, _("Interactive &tutorial") + "\tF1",
 686:                  _("Load an interactive tutorial in TreeSheets"));
 687:         MyAppend(helpmenu, A_HELP_OP_REF, _("Operation reference") + "\tCTRL+ALT+F1",
 688:                  _("Load an interactive program operation reference in TreeSheets"));
 689:         helpmenu->AppendSeparator();
 690:         MyAppend(helpmenu, A_TUTORIALWEBPAGE, _("Tutorial &web page"),
 691:                  _("Open the tutorial web page in browser"));
 692:         #ifdef ENABLE_LOBSTER
 693:             MyAppend(helpmenu, A_SCRIPTREFERENCE, _("&Script reference"),
 694:                  _("Open the Lobster script reference in browser"));
 695:         #endif
 696: 
 697:         wxAcceleratorEntry entries[3];
 698:         entries[0].Set(wxACCEL_SHIFT, WXK_DELETE, wxID_CUT);
 699:         entries[1].Set(wxACCEL_SHIFT, WXK_INSERT, wxID_PASTE);
 700:         entries[2].Set(wxACCEL_CTRL, WXK_INSERT, wxID_COPY);
 701:         wxAcceleratorTable accel(3, entries);
 702:         SetAcceleratorTable(accel);
 703: 
 704:         auto menubar = new wxMenuBar();
 705:         menubar->Append(filemenu, _("&File"));
 706:         menubar->Append(editmenu, _("&Edit"));
 707:         menubar->Append(semenu, _("&Search"));
 708:         menubar->Append(viewmenu, _("&View"));
 709:         menubar->Append(optmenu, _("&Options"));
 710:         #ifdef ENABLE_LOBSTER
 711:             menubar->Append(scriptmenu, _("S&cript"));
 712:         #endif
 713:         menubar->Append(langmenu, _("&Program"));
 714:         menubar->Append(helpmenu,
 715:                         #ifdef __WXMAC__
 716:                         wxApp::s_macHelpMenuTitleName  // so merges with osx provided help
 717:                         #else
 718:                         _("&Help")
 719:                         #endif
 720:                         );
 721:         #ifdef __WXMAC__
 722:         // these don't seem to work anymore in the newer wxWidgets, handled in the menu event
 723:         // handler below instead
 724:         wxApp::s_macAboutMenuItemId = wxID_ABOUT;
 725:         wxApp::s_macExitMenuItemId = wxID_EXIT;
 726:         wxApp::s_macPreferencesMenuItemId =
 727:             wxID_SELECT_FONT;  // we have no prefs, so for now just select the font
 728:         #endif
 729:         SetMenuBar(menubar);
 730: 
 731:         RefreshToolBar();
 732: 
 733:         auto sb = CreateStatusBar(5);
 734:         SetStatusBarPane(0);
 735:         SetDPIAwareStatusWidths();
 736:         sb->Show(sys->showstatusbar);
 737: 
 738:         notebook =
 739:             new wxAuiNotebook(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 740:                               wxAUI_NB_TAB_MOVE | wxAUI_NB_TAB_SPLIT | wxAUI_NB_SCROLL_BUTTONS |
 741:                                   wxAUI_NB_WINDOWLIST_BUTTON | wxAUI_NB_CLOSE_ON_ALL_TABS |
 742:                                   (lefttabs ? wxAUI_NB_BOTTOM : wxAUI_NB_TOP));
 743: 
 744:         int display_id = wxDisplay::GetFromWindow(this);
 745:         wxRect disprect = wxDisplay(display_id == wxNOT_FOUND ? 0 : display_id).GetClientArea();
 746:         const int boundary = 64;
 747:         const int defx = disprect.width - 2 * boundary;
 748:         const int defy = disprect.height - 2 * boundary;
 749:         int resx, resy, posx, posy;
 750:         sys->cfg->Read("resx", &resx, defx);
 751:         sys->cfg->Read("resy", &resy, defy);
 752:         sys->cfg->Read("posx", &posx, boundary + disprect.x);
 753:         sys->cfg->Read("posy", &posy, boundary + disprect.y);
 754:         #ifndef __WXGTK__
 755:         // On X11, disprect only refers to the primary screen. Thus, for a multi-head display,
 756:         // the conditions below might be fulfilled (e.g. large window spanning multiple screens
 757:         // or being on the secondary screen), so just ignore them.
 758:         if (resx > disprect.width || resy > disprect.height || posx < disprect.x ||
 759:             posy < disprect.y || posx + resx > disprect.width + disprect.x ||
 760:             posy + resy > disprect.height + disprect.y) {
 761:             // Screen res has been resized since we last ran, set sizes to default to avoid being
 762:             // off-screen.
 763:             resx = defx;
 764:             resy = defy;
 765:             posx = posy = boundary;
 766:             posx += disprect.x;
 767:             posy += disprect.y;
 768:         }
 769:         #endif
 770:         SetSize(resx, resy);
 771:         Move(posx, posy);
 772: 
 773:         bool ismax;
 774:         sys->cfg->Read("maximized", &ismax, true);
 775: 
 776:         aui.AddPane(
 777:             notebook,
 778:             wxAuiPaneInfo().Name("notebook").Caption("Notebook").CenterPane().PaneBorder(false));
 779:         aui.LoadPerspective(sys->cfg->Read("perspective", ""));
 780:         aui.Update();
 781: 
 782:         Show(!IsIconized());
 783: 
 784:         // needs to be after Show() to avoid scrollbars rendered in the wrong place?
 785:         if (ismax && !IsIconized()) Maximize(true);
 786: 
 787:         if (sys->startminimized)
 788:             #ifdef __WXGTK__
 789:                 CallAfter([this]() { Iconize(true); });
 790:             #else
 791:                 Iconize(true);
 792:             #endif
 793: 
 794:         SetFileAssoc(app->exename);
 795: 
 796:         wxSafeYield();
 797:     }
 798: 
 799:     wxArrayString GetToolbarPaneNames() {
 800:         wxArrayString toolbarNames;
 801:         wxAuiPaneInfoArray &all_panes = aui.GetAllPanes();
 802:         for (size_t i = 0; i < all_panes.GetCount(); ++i) {
 803:             wxAuiPaneInfo &pane = all_panes.Item(i);
 804:             if (pane.IsToolbar()) { toolbarNames.Add(pane.name); }
 805:         }
 806:         return toolbarNames;
 807:     }
 808: 
 809:     void DestroyToolbarPane(const wxString &name) {
 810:         wxAuiPaneInfo &pane = aui.GetPane(name);
 811:         if (pane.IsOk()) {
 812:             wxWindow *wnd = pane.window;
 813:             aui.DetachPane(wnd);
 814:             if (wnd) { wnd->Destroy(); }
 815:         }
 816:     }
 817: 
 818:     void RefreshToolBar() {
 819:         for (const auto &name : GetToolbarPaneNames()) { DestroyToolbarPane(name); }
 820:         auto iconpath = app->GetDataPath("images/material/toolbar/");
 821:         auto AddToolbarIcon = [&](wxAuiToolBar *tb, const wxChar *name, int action,
 822:                                   wxString iconpath, wxString lighticon, wxString darkicon) {
 823:             tb->AddTool(
 824:                 action, name,
 825:                 wxBitmapBundle::FromSVGFile(
 826:                     iconpath + (wxSystemSettings::GetAppearance().IsDark() ? darkicon : lighticon),
 827:                     wxSize(24, 24)),
 828:                 name, wxITEM_NORMAL);
 829:         };
 830: 
 831:         auto filetb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 832:                                        wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
 833:         AddToolbarIcon(filetb, _("New (CTRL+n)"), wxID_NEW, iconpath, "filenew.svg",
 834:                        "filenew_dark.svg");
 835:         AddToolbarIcon(filetb, _("Open (CTRL+o)"), wxID_OPEN, iconpath, "fileopen.svg",
 836:                        "fileopen_dark.svg");
 837:         AddToolbarIcon(filetb, _("Save (CTRL+s)"), wxID_SAVE, iconpath, "filesave.svg",
 838:                        "filesave_dark.svg");
 839:         AddToolbarIcon(filetb, _("Save as..."), wxID_SAVEAS, iconpath, "filesaveas.svg",
 840:                        "filesaveas_dark.svg");
 841:         filetb->Realize();
 842: 
 843:         auto edittb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 844:                                        wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
 845:         AddToolbarIcon(edittb, _("Undo (CTRL+z)"), wxID_UNDO, iconpath, "undo.svg",
 846:                        "undo_dark.svg");
 847:         AddToolbarIcon(edittb, _("Copy (CTRL+c)"), wxID_COPY, iconpath, "editcopy.svg",
 848:                        "editcopy_dark.svg");
 849:         AddToolbarIcon(edittb, _("Paste (CTRL+v)"), wxID_PASTE, iconpath, "editpaste.svg",
 850:                        "editpaste_dark.svg");
 851:         edittb->Realize();
 852: 
 853:         auto zoomtb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 854:                                        wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
 855:         AddToolbarIcon(zoomtb, _("Zoom In (CTRL+mousewheel)"), A_ZOOMIN, iconpath, "zoomin.svg",
 856:                        "zoomin_dark.svg");
 857:         AddToolbarIcon(zoomtb, _("Zoom Out (CTRL+mousewheel)"), A_ZOOMOUT, iconpath, "zoomout.svg",
 858:                        "zoomout_dark.svg");
 859:         zoomtb->Realize();
 860: 
 861:         auto celltb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 862:                                        wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
 863:         AddToolbarIcon(celltb, _("New Grid (INS)"), A_NEWGRID, iconpath, "newgrid.svg",
 864:                        "newgrid_dark.svg");
 865:         AddToolbarIcon(celltb, _("Add Image"), A_IMAGE, iconpath, "image.svg", "image_dark.svg");
 866:         AddToolbarIcon(celltb, _("Run"), wxID_EXECUTE, iconpath, "run.svg", "run_dark.svg");
 867:         celltb->Realize();
 868: 
 869:         auto findtb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 870:                                        wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
 871:         findtb->AddControl(new wxStaticText(findtb, wxID_ANY, _("Search ")));
 872:         findtb->AddControl(filter = new wxTextCtrl(findtb, A_SEARCH, "", wxDefaultPosition,
 873:                                                    FromDIP(wxSize(80, 22)),
 874:                                                    wxWANTS_CHARS | wxTE_PROCESS_ENTER));
 875:         AddToolbarIcon(findtb, _("Clear search"), A_CLEARSEARCH, iconpath, "cancel.svg",
 876:                        "cancel_dark.svg");
 877:         AddToolbarIcon(findtb, _("Go to Next Search Result"), A_SEARCHNEXT, iconpath, "search.svg",
 878:                        "search_dark.svg");
 879:         findtb->Realize();
 880: 
 881:         auto repltb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 882:                                        wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
 883:         repltb->AddControl(new wxStaticText(repltb, wxID_ANY, _("Replace ")));
 884:         repltb->AddControl(replaces = new wxTextCtrl(repltb, A_REPLACE, "", wxDefaultPosition,
 885:                                                      FromDIP(wxSize(80, 22)),
 886:                                                      wxWANTS_CHARS | wxTE_PROCESS_ENTER));
 887:         AddToolbarIcon(repltb, _("Clear replace"), A_CLEARREPLACE, iconpath, "cancel.svg",
 888:                        "cancel_dark.svg");
 889:         AddToolbarIcon(repltb, _("Replace in selection"), A_REPLACEONCE, iconpath, "replace.svg",
 890:                        "replace_dark.svg");
 891:         AddToolbarIcon(repltb, _("Replace All"), A_REPLACEALL, iconpath, "replaceall.svg",
 892:                        "replaceall_dark.svg");
 893:         repltb->Realize();
 894: 
 895:         auto GetColorIndex = [&](int targetcolor, int defaultindex) {
 896:             for (auto i = 1; i < celltextcolors.size(); ++i) {
 897:                 if (celltextcolors[i] == targetcolor) return i;
 898:             }
 899:             if (sys->customcolor == targetcolor) return 0;
 900:             return defaultindex;
 901:         };
 902: 
 903:         auto cellcolortb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 904:                                             wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
 905:         cellcolortb->AddControl(new wxStaticText(cellcolortb, wxID_ANY, _("Cell ")));
 906: 
 907:         cellcolordropdown =
 908:             new ColorDropdown(cellcolortb, A_CELLCOLOR, GetColorIndex(sys->lastcellcolor, 1));
 909:         cellcolortb->AddControl(cellcolordropdown);
 910:         cellcolortb->Realize();
 911: 
 912:         auto textcolortb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 913:                                             wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
 914:         textcolortb->AddControl(new wxStaticText(textcolortb, wxID_ANY, _("Text ")));
 915:         textcolordropdown =
 916:             new ColorDropdown(textcolortb, A_TEXTCOLOR, GetColorIndex(sys->lasttextcolor, 2));
 917:         textcolortb->AddControl(textcolordropdown);
 918:         textcolortb->Realize();
 919: 
 920:         auto bordercolortb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 921:                                               wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
 922:         bordercolortb->AddControl(new wxStaticText(bordercolortb, wxID_ANY, _("Border ")));
 923:         bordercolordropdown =
 924:             new ColorDropdown(bordercolortb, A_BORDCOLOR, GetColorIndex(sys->lastbordcolor, 7));
 925:         bordercolortb->AddControl(bordercolordropdown);
 926:         bordercolortb->Realize();
 927: 
 928:         auto imagetb = new wxAuiToolBar(this, wxID_ANY, wxDefaultPosition, wxDefaultSize,
 929:                                         wxAUI_TB_DEFAULT_STYLE | wxAUI_TB_PLAIN_BACKGROUND);
 930:         imagetb->AddControl(new wxStaticText(imagetb, wxID_ANY, _("Image ")));
 931:         imagedropdown = new ImageDropdown(imagetb, imagepath);
 932:         imagetb->AddControl(imagedropdown);
 933:         imagetb->Realize();
 934: 
 935:         aui.AddPane(filetb, wxAuiPaneInfo()
 936:                                 .Name("filetb")
 937:                                 .Caption("File operations")
 938:                                 .ToolbarPane()
 939:                                 .Top()
 940:                                 .Row(0)
 941:                                 .LeftDockable(false)
 942:                                 .RightDockable(false)
 943:                                 .Gripper(true));
 944:         aui.AddPane(edittb, wxAuiPaneInfo()
 945:                                 .Name("edittb")
 946:                                 .Caption("Edit operations")
 947:                                 .ToolbarPane()
 948:                                 .Top()
 949:                                 .Row(0)
 950:                                 .LeftDockable(false)
 951:                                 .RightDockable(false)
 952:                                 .Gripper(true));
 953:         aui.AddPane(zoomtb, wxAuiPaneInfo()
 954:                                 .Name("zoomtb")
 955:                                 .Caption("Zoom operations")
 956:                                 .ToolbarPane()
 957:                                 .Top()
 958:                                 .Row(0)
 959:                                 .LeftDockable(false)
 960:                                 .RightDockable(false)
 961:                                 .Gripper(true));
 962:         aui.AddPane(celltb, wxAuiPaneInfo()
 963:                                 .Name("celltb")
 964:                                 .Caption("Cell operations")
 965:                                 .ToolbarPane()
 966:                                 .Top()
 967:                                 .Row(0)
 968:                                 .LeftDockable(false)
 969:                                 .RightDockable(false)
 970:                                 .Gripper(true));
 971:         aui.AddPane(findtb, wxAuiPaneInfo()
 972:                                 .Name("findtb")
 973:                                 .Caption("Find operations")
 974:                                 .ToolbarPane()
 975:                                 .Top()
 976:                                 .Row(0)
 977:                                 .LeftDockable(false)
 978:                                 .RightDockable(false)
 979:                                 .Gripper(true));
 980:         aui.AddPane(repltb, wxAuiPaneInfo()
 981:                                 .Name("repltb")
 982:                                 .Caption("Replace operations")
 983:                                 .ToolbarPane()
 984:                                 .Top()
 985:                                 .Row(0)
 986:                                 .LeftDockable(false)
 987:                                 .RightDockable(false)
 988:                                 .Gripper(true));
 989:         aui.AddPane(cellcolortb, wxAuiPaneInfo()
 990:                                      .Name("cellcolortb")
 991:                                      .Caption("Cell color operations")
 992:                                      .ToolbarPane()
 993:                                      .Top()
 994:                                      .Row(0)
 995:                                      .LeftDockable(false)
 996:                                      .RightDockable(false)
 997:                                      .Gripper(true));
 998:         aui.AddPane(textcolortb, wxAuiPaneInfo()
 999:                                      .Name("textcolortb")
1000:                                      .Caption("Text color operations")
1001:                                      .ToolbarPane()
1002:                                      .Top()
1003:                                      .Row(0)
1004:                                      .LeftDockable(false)
1005:                                      .RightDockable(false)
1006:                                      .Gripper(true));
1007:         aui.AddPane(bordercolortb, wxAuiPaneInfo()
1008:                                        .Name("bordercolortb")
1009:                                        .Caption("Border color operations")
1010:                                        .ToolbarPane()
1011:                                        .Top()
1012:                                        .Row(0)
1013:                                        .LeftDockable(false)
1014:                                        .RightDockable(false)
1015:                                        .Gripper(true));
1016:         aui.AddPane(imagetb, wxAuiPaneInfo()
1017:                                  .Name("imagetb")
1018:                                  .Caption("Image operations")
1019:                                  .ToolbarPane()
1020:                                  .Top()
1021:                                  .Row(0)
1022:                                  .LeftDockable(false)
1023:                                  .RightDockable(false)
1024:                                  .Gripper(true));
1025:         auto artprovider = aui.GetArtProvider();
1026:         artprovider->SetColour(wxAUI_DOCKART_BACKGROUND_COLOUR, wxSystemSettings::GetColour(wxSYS_COLOUR_BTNFACE));
1027:         artprovider->SetMetric(wxAUI_DOCKART_PANE_BORDER_SIZE, 0);
1028:     }
1029: 
1030:     void AppOnEventLoopEnter() {
1031:         watcher = new wxFileSystemWatcher();
1032:         watcher->SetOwner(this);
1033:         Connect(wxEVT_FSWATCHER, wxFileSystemWatcherEventHandler(TSFrame::OnFileSystemEvent));
1034:     }
1035: 
1036:     // event handling functions
1037: 
1038:     void OnMenu(wxCommandEvent &ce) {
1039:         wxTextCtrl *tc;
1040:         auto canvas = GetCurrentTab();
1041:         if ((tc = filter) && filter == wxWindow::FindFocus() ||
1042:             (tc = replaces) && replaces == wxWindow::FindFocus()) {
1043:             long from, to;
1044:             tc->GetSelection(&from, &to);
1045:             switch (ce.GetId()) {
1046:                 #if defined(__WXMSW__) || defined(__WXMAC__)
1047:                 // FIXME: have to emulate this behavior on Windows and Mac because menu always captures these events (??)
1048:                 case A_MLEFT:
1049:                 case A_LEFT:
1050:                     if (from != to)
1051:                         tc->SetInsertionPoint(from);
1052:                     else if (from)
1053:                         tc->SetInsertionPoint(from - 1);
1054:                     return;
1055:                 case A_MRIGHT:
1056:                 case A_RIGHT:
1057:                     if (from != to)
1058:                         tc->SetInsertionPoint(to);
1059:                     else if (to < tc->GetLineLength(0))
1060:                         tc->SetInsertionPoint(to + 1);
1061:                     return;
1062: 
1063:                 case A_SHOME: tc->SetSelection(0, to); return;
1064:                 case A_SEND: tc->SetSelection(from, 1000); return;
1065: 
1066:                 case A_SCLEFT:
1067:                 case A_SLEFT:
1068:                     if (from) tc->SetSelection(from - 1, to);
1069:                     return;
1070:                 case A_SCRIGHT:
1071:                 case A_SRIGHT:
1072:                     if (to < tc->GetLineLength(0)) tc->SetSelection(from, to + 1);
1073:                     return;
1074: 
1075:                 case A_BACKSPACE: tc->Remove(from - (from == to), to); return;
1076:                 case A_DELETE: tc->Remove(from, to + (from == to)); return;
1077:                 case A_HOME: tc->SetSelection(0, 0); return;
1078:                 case A_END: tc->SetSelection(1000, 1000); return;
1079:                 case wxID_SELECTALL: tc->SetSelection(0, 1000); return;
1080:                 #endif
1081:                 #ifdef __WXMSW__
1082:                 case A_ENTERCELL: {
1083:                     if (tc == filter) {
1084:                         // OnSearchEnter equivalent implementation for MSW
1085:                         // as EVT_TEXT_ENTER event is not generated.
1086:                         if (sys->searchstring.IsEmpty()) {
1087:                             canvas->SetFocus();
1088:                         } else {
1089:                             canvas->doc->Action(A_SEARCHNEXT);
1090:                         }
1091:                     } else if (tc == replaces) {
1092:                         // OnReplaceEnter equivalent implementation for MSW
1093:                         // as EVT_TEXT_ENTER event is not generated.
1094:                         canvas->doc->Action(A_REPLACEONCEJ);
1095:                     }
1096:                     return;
1097:                 }
1098:                 #endif
1099:                 case A_CANCELEDIT:
1100:                     tc->Clear();
1101:                     canvas->SetFocus();
1102:                     return;
1103:             }
1104:         }
1105:         auto Check = [&](const wxString &cfg) {
1106:             sys->cfg->Write(cfg, ce.IsChecked());
1107:             SetStatus(_("change will take effect next run of TreeSheets"));
1108:         };
1109:         switch (ce.GetId()) {
1110:             case A_NOP: break;
1111: 
1112:             case A_ALEFT: canvas->CursorScroll(-g_scrollratecursor, 0); break;
1113:             case A_ARIGHT: canvas->CursorScroll(g_scrollratecursor, 0); break;
1114:             case A_AUP: canvas->CursorScroll(0, -g_scrollratecursor); break;
1115:             case A_ADOWN: canvas->CursorScroll(0, g_scrollratecursor); break;
1116:             case A_RESETPERSPECTIVE:
1117:                 RefreshToolBar();
1118:                 sys->showtoolbar = true;
1119:                 aui.Update();
1120:                 break;
1121:             case A_SHOWSBAR:
1122:                 if (!IsFullScreen()) {
1123:                     sys->cfg->Write("showstatusbar", sys->showstatusbar = ce.IsChecked());
1124:                     auto wsb = GetStatusBar();
1125:                     wsb->Show(sys->showstatusbar);
1126:                     SendSizeEvent();
1127:                     Refresh();
1128:                     wsb->Refresh();
1129:                 }
1130:                 break;
1131:             case A_SHOWTBAR:
1132:                 if (!IsFullScreen()) {
1133:                     sys->cfg->Write("showtoolbar", sys->showtoolbar = ce.IsChecked());
1134:                     for (const auto &name : GetToolbarPaneNames()) {
1135:                         if (sys->showtoolbar)
1136:                             aui.GetPane(name).Show();
1137:                         else
1138:                             aui.GetPane(name).Hide();
1139:                     }
1140:                     aui.Update();
1141:                 }
1142:                 break;
1143:             case A_CUSTCOL: {
1144:                 if (auto color = PickColor(sys->frame, sys->customcolor); color != (uint)-1)
1145:                     sys->cfg->Write("customcolor", sys->customcolor = color);
1146:                 break;
1147:             }
1148: 
1149:             #ifdef ENABLE_LOBSTER
1150:                 case A_ADDSCRIPT: {
1151:                     wxArrayString filenames;
1152:                     GetFilesFromUser(filenames, this, _("Please select Lobster script file(s):"),
1153:                                      _("Lobster Files (*.lobster)|*.lobster|All Files (*.*)|*.*"));
1154:                     for (auto &filename : filenames) scripts.AddFileToHistory(filename);
1155:                     break;
1156:                 }
1157: 
1158:                 case A_DETSCRIPT: {
1159:                     wxArrayString filenames;
1160:                     for (int i = 0, n = scripts.GetCount(); i < n; i++) {
1161:                         filenames.Add(scripts.GetHistoryFile(i));
1162:                     }
1163:                     auto dialog = wxSingleChoiceDialog(
1164:                         this, _("Please select the script you want to remove from the list:"),
1165:                         _("Remove script from list..."), filenames);
1166:                     if (dialog.ShowModal() == wxID_OK)
1167:                         scripts.RemoveFileFromHistory(dialog.GetSelection());
1168:                     break;
1169:                 }
1170:             #endif
1171: 
1172:             case A_DEFAULTMAXCOLWIDTH: {
1173:                 int w = wxGetNumberFromUser(_("Please enter the default column width:"),
1174:                                             _("Width"), _("Default column width"),
1175:                                             sys->defaultmaxcolwidth, 1, 1000, sys->frame);
1176:                 if (w > 0) sys->cfg->Write("defaultmaxcolwidth", sys->defaultmaxcolwidth = w);
1177:                 break;
1178:             }
1179: 
1180:             case A_LEFTTABS: Check("lefttabs"); break;
1181:             case A_SINGLETRAY: Check("singletray"); break;
1182:             case A_MAKEBAKS: sys->cfg->Write("makebaks", sys->makebaks = ce.IsChecked()); break;
1183:             case A_TOTRAY: sys->cfg->Write("totray", sys->totray = ce.IsChecked()); break;
1184:             case A_MINCLOSE: sys->cfg->Write("minclose", sys->minclose = ce.IsChecked()); break;
1185:             case A_STARTMINIMIZED:
1186:                 sys->cfg->Write("startminimized", sys->startminimized = ce.IsChecked());
1187:                 break;
1188:             case A_ZOOMSCR: sys->cfg->Write("zoomscroll", sys->zoomscroll = ce.IsChecked()); break;
1189:             case A_THINSELC: sys->cfg->Write("thinselc", sys->thinselc = ce.IsChecked()); break;
1190:             case A_AUTOSAVE: sys->cfg->Write("autosave", sys->autosave = ce.IsChecked()); break;
1191:             case A_CENTERED:
1192:                 sys->cfg->Write("centered", sys->centered = ce.IsChecked());
1193:                 Refresh();
1194:                 break;
1195:             case A_FSWATCH:
1196:                 Check("fswatch");
1197:                 sys->fswatch = ce.IsChecked();
1198:                 break;
1199:             case A_AUTOEXPORT_HTML_NONE:
1200:             case A_AUTOEXPORT_HTML_WITH_IMAGES:
1201:             case A_AUTOEXPORT_HTML_WITHOUT_IMAGES:
1202:                 sys->cfg->Write(
1203:                     "autohtmlexport",
1204:                     static_cast<long>(sys->autohtmlexport = ce.GetId() - A_AUTOEXPORT_HTML_NONE));
1205:                 break;
1206:             case A_FASTRENDER:
1207:                 sys->cfg->Write("fastrender", sys->fastrender = ce.IsChecked());
1208:                 Refresh();
1209:                 break;
1210:             case A_INVERTRENDER:
1211:                 sys->cfg->Write("followdarkmode", sys->followdarkmode = ce.IsChecked());
1212:                 sys->colormask = (sys->followdarkmode && wxSystemSettings::GetAppearance().IsDark())
1213:                                      ? 0x00FFFFFF
1214:                                      : 0;
1215:                 Refresh();
1216:                 break;
1217:             case A_FULLSCREEN:
1218:                 ShowFullScreen(!IsFullScreen());
1219:                 if (IsFullScreen()) SetStatus(_("Press F11 to exit fullscreen mode."));
1220:                 break;
1221:             case wxID_FIND:
1222:                 if (filter) {
1223:                     filter->SetFocus();
1224:                     filter->SetSelection(0, 1000);
1225:                 } else {
1226:                     SetStatus(_("Please enable (Options -> Show Toolbar) to use search."));
1227:                 }
1228:                 break;
1229:             case wxID_REPLACE:
1230:                 if (replaces) {
1231:                     replaces->SetFocus();
1232:                     replaces->SetSelection(0, 1000);
1233:                 } else {
1234:                     SetStatus(_("Please enable (Options -> Show Toolbar) to use replace."));
1235:                 }
1236:                 break;
1237:             #ifdef __WXMAC__
1238:                 case wxID_OSX_HIDE: Iconize(true); break;
1239:                 case wxID_OSX_HIDEOTHERS: SetStatus("NOT IMPLEMENTED"); break;
1240:                 case wxID_OSX_SHOWALL: Iconize(false); break;
1241:                 case wxID_ABOUT: canvas->doc->Action(wxID_ABOUT); break;
1242:                 case wxID_PREFERENCES: canvas->doc->Action(wxID_SELECT_FONT); break;
1243:             #endif
1244:             case wxID_EXIT:
1245:                 fromclosebox = false;
1246:                 Close();
1247:                 break;
1248:             case wxID_CLOSE:
1249:                 canvas->doc->Action(ce.GetId());
1250:                 break;  // canvas dangling pointer on return
1251:             default:
1252:                 if (ce.GetId() >= wxID_FILE1 && ce.GetId() <= wxID_FILE9) {
1253:                     wxString filename(filehistory.GetHistoryFile(ce.GetId() - wxID_FILE1));
1254:                     SetStatus(sys->Open(filename));
1255:                 #ifdef ENABLE_LOBSTER
1256:                     } else if (ce.GetId() >= A_TAGSET && ce.GetId() < A_SCRIPT) {
1257:                         SetStatus(canvas->doc->TagSet(ce.GetId() - A_TAGSET));
1258:                     } else if (ce.GetId() >= A_SCRIPT && ce.GetId() < A_MAXACTION) {
1259:                         auto message =
1260:                             tssi.ScriptRun(scripts.GetHistoryFile(ce.GetId() - A_SCRIPT).c_str());
1261:                         message.erase(std::remove(message.begin(), message.end(), '\n'), message.end());
1262:                         SetStatus(wxString(message));
1263:                 #else
1264:                     } else if (ce.GetId() >= A_TAGSET && ce.GetId() < A_MAXACTION) {
1265:                         SetStatus(canvas->doc->TagSet(ce.GetId() - A_TAGSET));
1266:                 #endif
1267:                 } else {
1268:                     SetStatus(canvas->doc->Action(ce.GetId()));
1269:                     break;
1270:                 }
1271:         }
1272:     }
1273: 
1274:     void OnTabChange(wxAuiNotebookEvent &nbe) {
1275:         auto canvas = static_cast<TSCanvas *>(notebook->GetPage(nbe.GetSelection()));
1276:         ClearStatus();
1277:         sys->TabChange(canvas->doc.get());
1278:         nbe.Skip();
1279:     }
1280: 
1281:     void OnTabClose(wxAuiNotebookEvent &nbe) {
1282:         auto canvas = static_cast<TSCanvas *>(notebook->GetPage(nbe.GetSelection()));
1283:         if (notebook->GetPageCount() <= 1) {
1284:             nbe.Veto();
1285:             Close();
1286:         } else if (canvas->doc->CloseDocument()) {
1287:             nbe.Veto();
1288:         } else {
1289:             nbe.Skip();
1290:         }
1291:     }
1292: 
1293:     void OnSearch(wxCommandEvent &ce) {
1294:         auto searchstring = ce.GetString();
1295:         sys->darkennonmatchingcells = searchstring.Len() != 0;
1296:         sys->searchstring = sys->casesensitivesearch ? searchstring : searchstring.Lower();
1297:         TSCanvas *canvas = GetCurrentTab();
1298:         Document *doc = canvas->doc.get();
1299:         if (doc->searchfilter) {
1300:             doc->SetSearchFilter(sys->searchstring.Len() != 0);
1301:             doc->searchfilter = true;
1302:         }
1303:         canvas->Refresh();
1304:     }
1305: 
1306:     void OnSearchReplaceEnter(wxCommandEvent &ce) {
1307:         auto canvas = GetCurrentTab();
1308:         if (ce.GetId() == A_SEARCH && ce.GetString().IsEmpty())
1309:             canvas->SetFocus();
1310:         else
1311:             canvas->doc->Action(ce.GetId() == A_SEARCH ? A_SEARCHNEXT : A_REPLACEONCEJ);
1312:     }
1313: 
1314:     void OnChangeColor(wxCommandEvent &ce) {
1315:         GetCurrentTab()->doc->ColorChange(ce.GetId(), ce.GetInt());
1316:         ReFocus();
1317:     }
1318: 
1319:     void OnDDImage(wxCommandEvent &ce) {
1320:         GetCurrentTab()->doc->ImageChange(imagedropdown->filenames[ce.GetInt()], dd_icon_res_scale);
1321:         ReFocus();
1322:     }
1323: 
1324:     void OnActivate(wxActivateEvent &ae) {
1325:         // This causes warnings in the debug log, but without it keyboard entry upon window select
1326:         // doesn't work.
1327:         ReFocus();
1328:     }
1329: 
1330:     void OnSizing(wxSizeEvent &se) { se.Skip(); }
1331: 
1332:     void OnMaximize(wxMaximizeEvent &me) {
1333:         ReFocus();
1334:         me.Skip();
1335:     }
1336: 
1337:     void OnIconize(wxIconizeEvent &me) {
1338:         if (me.IsIconized()) {
1339:             #ifndef __WXMAC__
1340:             if (sys->totray) {
1341:                 taskbaricon.SetIcon(icon, "TreeSheets");
1342:                 Show(false);
1343:                 Iconize();
1344:             }
1345:             #endif
1346:         } else {
1347:             #ifdef __WXGTK__
1348:             if (sys->totray) {
1349:                 Show(true);
1350:             }
1351:             #endif
1352:             if (TSCanvas *canvas = GetCurrentTab()) canvas->SetFocus();
1353:         }
1354:     }
1355: 
1356:     void OnTBIDBLClick(wxTaskBarIconEvent &e) { DeIconize(); }
1357: 
1358:     void OnClosing(wxCloseEvent &ce) {
1359:         bool fcb = fromclosebox;
1360:         fromclosebox = true;
1361:         if (fcb && sys->minclose) {
1362:             ce.Veto();
1363:             Iconize();
1364:             return;
1365:         }
1366:         sys->RememberOpenFiles();
1367:         if (ce.CanVeto()) {
1368:             // ask to save/discard all files before closing any
1369:             loop(i, notebook->GetPageCount()) {
1370:                 auto canvas = static_cast<TSCanvas *>(notebook->GetPage(i));
1371:                 if (canvas->doc->modified) {
1372:                     notebook->SetSelection(i);
1373:                     if (canvas->doc->CheckForChanges()) {
1374:                         ce.Veto();
1375:                         return;
1376:                     }
1377:                 }
1378:             }
1379:             // all files have been saved/discarded
1380:             while (notebook->GetPageCount()) {
1381:                 GetCurrentTab()->doc->RemoveTmpFile();
1382:                 notebook->DeletePage(notebook->GetSelection());
1383:             }
1384:         }
1385:         sys->every_second_timer.Stop();
1386:         filehistory.Save(*sys->cfg);
1387:         #ifdef ENABLE_LOBSTER
1388:             auto oldpath = sys->cfg->GetPath();
1389:             sys->cfg->SetPath("/scripts");
1390:             scripts.Save(*sys->cfg);
1391:             sys->cfg->SetPath(oldpath);
1392:         #endif
1393:         if (!IsIconized()) {
1394:             sys->cfg->Write("maximized", IsMaximized());
1395:             if (!IsMaximized()) {
1396:                 sys->cfg->Write("resx", GetSize().x);
1397:                 sys->cfg->Write("resy", GetSize().y);
1398:                 sys->cfg->Write("posx", GetPosition().x);
1399:                 sys->cfg->Write("posy", GetPosition().y);
1400:             }
1401:         }
1402:         sys->cfg->Write("notesizex", sys->notesizex);
1403:         sys->cfg->Write("notesizey", sys->notesizey);
1404:         sys->cfg->Write("perspective", aui.SavePerspective());
1405:         sys->cfg->Write("lastcellcolor", sys->lastcellcolor);
1406:         sys->cfg->Write("lasttextcolor", sys->lasttextcolor);
1407:         sys->cfg->Write("lastbordcolor", sys->lastbordcolor);
1408:         aui.ClearEventHashTable();
1409:         aui.UnInit();
1410:         DELETEP(editmenupopup);
1411:         DELETEP(watcher);
1412:         Destroy();
1413:     }
1414: 
1415:     void OnFileSystemEvent(wxFileSystemWatcherEvent &event) {
1416:         // 0xF == create/delete/rename/modify
1417:         if ((event.GetChangeType() & 0xF) == 0 || watcherwaitingforuser || !notebook) return;
1418:         const auto &modfile = event.GetPath().GetFullPath();
1419:         loop(i, notebook->GetPageCount()) {
1420:             Document *doc = static_cast<TSCanvas *>(notebook->GetPage(i))->doc.get();
1421:             if (modfile == doc->filename) {
1422:                 auto modtime = wxFileName(modfile).GetModificationTime();
1423:                 // Compare with last modified to trigger multiple times.
1424:                 if (!modtime.IsValid() || !doc->lastmodificationtime.IsValid() ||
1425:                     modtime == doc->lastmodificationtime) {
1426:                     return;
1427:                 }
1428:                 if (doc->modified) {
1429:                     // TODO: this dialog is problematic since it may be on an unattended
1430:                     // computer and more of these events may fire. since the occurrence of this
1431:                     // situation is rare, it may be better to just take the most
1432:                     // recently changed version (which is the one that has just been modified
1433:                     // on disk) this potentially throws away local changes, but this can only
1434:                     // happen if the user left changes unsaved, then decided to go edit an older
1435:                     // version on another computer.
1436:                     // for now, we leave this code active, and guard it with
1437:                     // watcherwaitingforuser
1438:                     auto message = wxString::Format(
1439:                         _("%s\nhas been modified on disk by another program / computer:\nWould you like to discard your changes and re-load from disk?"),
1440:                         doc->filename);
1441:                     watcherwaitingforuser = true;
1442:                     int res = wxMessageBox(message, _("File modification conflict!"),
1443:                                            wxYES_NO | wxICON_QUESTION, this);
1444:                     watcherwaitingforuser = false;
1445:                     if (res != wxYES) return;
1446:                 }
1447:                 auto message = sys->LoadDB(doc->filename, true, i);
1448:                 if (!message.IsEmpty()) {
1449:                     SetStatus(message);
1450:                 } else {
1451:                     notebook->DeletePage(i + 1);
1452:                     ::wxRemoveFile(sys->TmpName(modfile));
1453:                     SetStatus(
1454:                         _("File has been re-loaded because of modifications of another program / computer"));
1455:                 }
1456:                 return;
1457:             }
1458:         }
1459:     }
1460: 
1461:     void OnDPIChanged(wxDPIChangedEvent &dce) {
1462:         // block all other events until we finished preparing
1463:         wxEventBlocker blocker(this);
1464:         wxBusyCursor wait;
1465:         {
1466:             ThreadPool pool(std::thread::hardware_concurrency());
1467:             for (const auto &image : sys->imagelist) {
1468:                 pool.enqueue(
1469:                     [](auto img) {
1470:                         img->bm_display = wxNullBitmap;
1471:                         img->Display();
1472:                     },
1473:                     image.get());
1474:             }
1475:         }  // wait until all tasks are finished
1476:         RenderFolderIcon();
1477:         dce.Skip();
1478:     }
1479: 
1480:     void OnSysColourChanged(wxSysColourChangedEvent &se) {
1481:         sys->colormask =
1482:             (sys->followdarkmode && wxSystemSettings::GetAppearance().IsDark()) ? 0x00FFFFFF : 0;
1483:         auto perspective = aui.SavePerspective();
1484:         RefreshToolBar();
1485:         aui.LoadPerspective(perspective);
1486:         aui.Update();
1487:         se.Skip();
1488:     }
1489: 
1490:     // helper functions
1491: 
1492:     void CycleTabs(int offset = 1) {
1493:         auto numtabs = static_cast<int>(notebook->GetPageCount());
1494:         offset = offset >= 0 ? 1 : numtabs - 1;  // normalize to non-negative wrt modulo
1495:         notebook->SetSelection((notebook->GetSelection() + offset) % numtabs);
1496:     }
1497: 
1498:     void DeIconize() {
1499:         if (!IsIconized()) {
1500:             RequestUserAttention();
1501:             return;
1502:         }
1503:         Show(true);
1504:         Iconize(false);
1505:         taskbaricon.RemoveIcon();
1506:     }
1507: 
1508:     TSCanvas *GetCurrentTab() {
1509:         return notebook ? static_cast<TSCanvas *>(notebook->GetCurrentPage()) : nullptr;
1510:     }
1511: 
1512:     TSCanvas *GetTabByFileName(const wxString &filename) {
1513:         if (notebook) loop(i, notebook->GetPageCount()) {
1514:                 auto canvas = static_cast<TSCanvas *>(notebook->GetPage(i));
1515:                 if (canvas->doc->filename == filename) {
1516:                     notebook->SetSelection(i);
1517:                     return canvas;
1518:                 }
1519:             }
1520:         return nullptr;
1521:     }
1522: 
1523:     void MyAppend(wxMenu *menu, int tag, const wxString &contents, const wxString &help = "") {
1524:         auto item = contents;
1525:         wxString key = "";
1526:         if (int pos = contents.Find("\t"); pos >= 0) {
1527:             item = contents.Mid(0, pos);
1528:             key = contents.Mid(pos + 1);
1529:         }
1530:         key = sys->cfg->Read(item, key);
1531:         auto newcontents = item;
1532:         if (key.Length()) newcontents += "\t" + key;
1533:         menu->Append(tag, newcontents, help);
1534:         menustrings[item] = key;
1535:     }
1536: 
1537:     TSCanvas *NewTab(unique_ptr<Document> doc, bool append = false, int insert_at = -1) {
1538:         TSCanvas *canvas = new TSCanvas(this, notebook);
1539:         doc->canvas = canvas;
1540:         canvas->doc = std::move(doc);
1541:         canvas->SetScrollRate(1, 1);
1542:         if (insert_at >= 0)
1543:             notebook->InsertPage(insert_at, canvas, _("<unnamed>"), true, wxNullBitmap);
1544:         else if (append)
1545:             notebook->AddPage(canvas, _("<unnamed>"), true, wxNullBitmap);
1546:         else
1547:             notebook->InsertPage(0, canvas, _("<unnamed>"), true, wxNullBitmap);
1548:         canvas->SetDropTarget(new DropTarget(canvas->doc->dndobjc));
1549:         canvas->SetFocus();
1550:         return canvas;
1551:     }
1552: 
1553:     void ReFocus() {
1554:         if (TSCanvas *canvas = GetCurrentTab()) canvas->SetFocus();
1555:     }
1556: 
1557:     void RenderFolderIcon() {
1558:         wxImage foldiconi;
1559:         foldiconi.LoadFile(app->GetDataPath("images/nuvola/fold.png"));
1560:         foldicon = wxBitmap(foldiconi);
1561:         ScaleBitmap(foldicon, FromDIP(1.0) / 3.0, foldicon);
1562:     }
1563: 
1564:     void SetDPIAwareStatusWidths() {
1565:         int statusbarfieldwidths[] = {-1, FromDIP(300), FromDIP(120), FromDIP(100), FromDIP(150)};
1566:         SetStatusWidths(5, statusbarfieldwidths);
1567:     }
1568: 
1569:     void SetFileAssoc(const wxString &exename) {
1570:         #ifdef WIN32
1571:         SetRegistryKey("HKEY_CURRENT_USER\\Software\\Classes\\.cts", "TreeSheets");
1572:         SetRegistryKey("HKEY_CURRENT_USER\\Software\\Classes\\TreeSheets", "TreeSheets file");
1573:         SetRegistryKey("HKEY_CURRENT_USER\\Software\\Classes\\TreeSheets\\Shell\\Open\\Command",
1574:                        "\"" + exename + "\" \"%1\"");
1575:         SetRegistryKey("HKEY_CURRENT_USER\\Software\\Classes\\TreeSheets\\DefaultIcon",
1576:                        "\"" + exename + "\",0");
1577:         #else
1578:         // TODO: do something similar for mac/kde/gnome?
1579:         #endif
1580:     }
1581: 
1582:     void SetPageTitle(const wxString &filename, wxString mods, int page = -1) {
1583:         if (page < 0) page = notebook->GetSelection();
1584:         if (page < 0) return;
1585:         if (page == notebook->GetSelection()) SetTitle("TreeSheets - " + filename + mods);
1586:         notebook->SetPageText(
1587:             page,
1588:             (filename.empty() ? wxString(_("<unnamed>")) : wxFileName(filename).GetName()) + mods);
1589:     }
1590: 
1591:     #ifdef WIN32
1592:     void SetRegistryKey(const wxString &key, wxString value) {
1593:         wxRegKey registrykey(key);
1594:         registrykey.Create();
1595:         registrykey.SetValue("", value);
1596:     }
1597:     #endif
1598: 
1599:     void SetStatus(const wxString &message) {
1600:         if (GetStatusBar() && !message.IsEmpty()) SetStatusText(message, 0);
1601:     }
1602: 
1603:     void ClearStatus() {
1604:         if (GetStatusBar()) SetStatusText("", 0);
1605:     }
1606: 
1607:     void TabsReset() {
1608:         if (notebook) loop(i, notebook->GetPageCount()) {
1609:                 auto canvas = static_cast<TSCanvas *>(notebook->GetPage(i));
1610:                 canvas->doc->root->ResetChildren();
1611:             }
1612:     }
1613: 
1614:     void UpdateStatus(const Selection &s, bool updateamount) {
1615:         if (GetStatusBar()) {
1616:             if (Cell *c = s.GetCell(); c && s.xs) {
1617:                 SetStatusText(wxString::Format(_("Size %d"), -c->text.relsize), 3);
1618:                 SetStatusText(wxString::Format(_("Width %d"), s.grid->colwidths[s.x]), 2);
1619:                 SetStatusText(wxString::Format(_("Edited %s %s"), c->text.lastedit.FormatDate(),
1620:                                                c->text.lastedit.FormatTime()),
1621:                               1);
1622:             } else
1623:                 for (int field : {1, 2, 3}) SetStatusText("", field);
1624:             if (updateamount) SetStatusText(wxString::Format(_("%d cell(s)"), s.xs * s.ys), 4);
1625:         }
1626:     }
1627: 
1628:     DECLARE_EVENT_TABLE()
1629: };

================
File: src/wxtools.h
================
  1: static void DrawRectangle(wxDC &dc, uint color, int x, int y, int xs, int ys,
  2:                           bool outline = false) {
  3:     if (outline)
  4:         dc.SetBrush(*wxTRANSPARENT_BRUSH);
  5:     else
  6:         dc.SetBrush(wxBrush(LightColor(color)));
  7:     dc.SetPen(wxPen(LightColor(color)));
  8:     dc.DrawRectangle(x, y, xs, ys);
  9: }
 10: 
 11: static uint SwapColor(uint c) { return ((c & 0xFF) << 16) | (c & 0xFF00) | ((c & 0xFF0000) >> 16); }
 12: 
 13: struct DropTarget : wxDropTarget {
 14:     DropTarget(wxDataObject *data) : wxDropTarget(data) {};
 15: 
 16:     wxDragResult OnDragOver(wxCoord x, wxCoord y, wxDragResult def) {
 17:         auto canvas = sys->frame->GetCurrentTab();
 18:         wxInfoDC dc(canvas);
 19:         canvas->doc->UpdateHover(dc, x, y);
 20:         return canvas->doc->hover.grid ? wxDragCopy : wxDragNone;
 21:     }
 22: 
 23:     bool OnDrop(wxCoord x, wxCoord y) {
 24:         return sys->frame->GetCurrentTab()->doc->hover.grid != nullptr;
 25:     }
 26:     wxDragResult OnData(wxCoord x, wxCoord y, wxDragResult def) {
 27:         GetData();
 28:         auto canvas = sys->frame->GetCurrentTab();
 29:         wxInfoDC dc(canvas);
 30:         canvas->doc->UpdateHover(dc, x, y);
 31:         canvas->doc->SelectClick();
 32:         canvas->doc->Drop();
 33:         canvas->Refresh();
 34:         return wxDragCopy;
 35:     }
 36: };
 37: 
 38: struct ThreeChoiceDialog : public wxDialog {
 39:     ThreeChoiceDialog(wxWindow *parent, const wxString &title, const wxString &msg,
 40:                       const wxString &ch1, const wxString &ch2, const wxString &ch3)
 41:         : wxDialog(parent, wxID_ANY, title) {
 42:         auto bsv = new wxBoxSizer(wxVERTICAL);
 43:         bsv->Add(new wxStaticText(this, -1, msg), 0, wxALL, 5);
 44:         auto bsb = new wxBoxSizer(wxHORIZONTAL);
 45:         bsb->Prepend(new wxButton(this, 2, ch3), 0, wxALL, 5);
 46:         bsb->PrependStretchSpacer(1);
 47:         bsb->Prepend(new wxButton(this, 1, ch2), 0, wxALL, 5);
 48:         bsb->PrependStretchSpacer(1);
 49:         bsb->Prepend(new wxButton(this, 0, ch1), 0, wxALL, 5);
 50:         bsv->Add(bsb, 1, wxEXPAND);
 51:         SetSizer(bsv);
 52:         bsv->SetSizeHints(this);
 53:     }
 54: 
 55:     void OnButton(wxCommandEvent &ce) { EndModal(ce.GetId()); }
 56:     int Run() { return ShowModal(); }
 57:     DECLARE_EVENT_TABLE()
 58: };
 59: 
 60: struct DateTimeRangeDialog : public wxDialog {
 61:     wxStaticText introtext {this, wxID_ANY, _("Please select the datetime range.")};
 62:     wxStaticText starttext {this, wxID_ANY, _("Start date and time")};
 63:     wxDatePickerCtrl startdate {this, wxID_ANY};
 64:     wxTimePickerCtrl starttime {this, wxID_ANY};
 65:     wxStaticText endtext {this, wxID_ANY, _("End date and time")};
 66:     wxDatePickerCtrl enddate {this, wxID_ANY};
 67:     wxTimePickerCtrl endtime {this, wxID_ANY};
 68:     wxButton okbtn {this, wxID_OK, _("Filter")};
 69:     wxButton cancelbtn {this, wxID_CANCEL, _("Cancel")};
 70:     wxDateTime begin;
 71:     wxDateTime end;
 72:     DateTimeRangeDialog(wxWindow *parent) : wxDialog(parent, wxID_ANY, _("Date and time range")) {
 73:         wxSizerFlags sizerflags(1);
 74:         auto startsizer = new wxFlexGridSizer(2, wxSize(5, 5));
 75:         startsizer->Add(&startdate, 0, wxALL, 5);
 76:         startsizer->Add(&starttime, 0, wxALL, 5);
 77:         auto endsizer = new wxFlexGridSizer(2, wxSize(5, 5));
 78:         endsizer->Add(&enddate, 0, wxALL, 5);
 79:         endsizer->Add(&endtime, 0, wxALL, 5);
 80:         auto btnsizer = new wxFlexGridSizer(2, wxSize(5, 5));
 81:         btnsizer->Add(&okbtn, 0, wxALL, 5);
 82:         btnsizer->Add(&cancelbtn, 0, wxALL, 5);
 83:         auto topsizer = new wxFlexGridSizer(1);
 84:         topsizer->Add(&introtext, 0, wxALL, 5);
 85:         topsizer->Add(&starttext, 0, wxALL, 5);
 86:         topsizer->Add(startsizer, sizerflags);
 87:         topsizer->Add(&endtext, 0, wxALL, 5);
 88:         topsizer->Add(endsizer, sizerflags);
 89:         topsizer->Add(btnsizer, sizerflags);
 90:         SetSizerAndFit(topsizer);
 91:         topsizer->SetSizeHints(this);
 92:     }
 93:     void OnButton(wxCommandEvent &ce) {
 94:         if (ce.GetId() == wxID_OK) {
 95:             int starthour, startmin, startsec;
 96:             starttime.GetTime(&starthour, &startmin, &startsec);
 97:             wxTimeSpan starttimespan(starthour, startmin, startsec);
 98:             int endhour, endmin, endsec;
 99:             endtime.GetTime(&endhour, &endmin, &endsec);
100:             wxTimeSpan endtimespan(endhour, endmin, endsec);
101:             begin = startdate.GetValue().Add(starttimespan);
102:             end = enddate.GetValue().Add(endtimespan);
103:         }
104:         EndModal(ce.GetId());
105:     }
106:     int Run() { return ShowModal(); }
107:     DECLARE_EVENT_TABLE()
108: };
109: 
110: struct ColorPopup : wxVListBoxComboPopup {
111:     ColorPopup(wxWindow *parent) {}
112: 
113:     void OnComboDoubleClick() {
114:         sys->frame->GetCurrentTab()->doc->ColorChange(m_combo->GetId(), GetSelection());
115:     }
116: };
117: 
118: struct ColorDropdown : wxOwnerDrawnComboBox {
119:     ColorDropdown(wxWindow *parent, wxWindowID id, int sel) {
120:         wxArrayString as;
121:         as.Add("", sizeof(celltextcolors) / sizeof(uint));
122:         Create(parent, id, "", wxDefaultPosition, FromDIP(wxSize(44, 22)), as,
123:                wxCB_READONLY | wxCC_SPECIAL_DCLICK);
124:         SetPopupControl(new ColorPopup(this));
125:         SetSelection(sel);
126:         SetPopupMaxHeight(wxDisplay().GetGeometry().GetHeight() * 3 / 4);
127:     }
128: 
129:     wxCoord OnMeasureItem(size_t item) const { return FromDIP(22); }
130:     wxCoord OnMeasureItemWidth(size_t item) const { return FromDIP(40); }
131:     void OnDrawBackground(wxDC &dc, const wxRect &rect, int item, int flags) const {
132:         DrawRectangle(dc, flags & wxODCB_PAINTING_SELECTED ? 0xA9A9A9 : 0xFFFFFF, rect.x, rect.y,
133:                       rect.width, rect.height);
134:     }
135: 
136:     void OnDrawItem(wxDC &dc, const wxRect &rect, int item, int flags) const {
137:         DrawRectangle(dc, item == CUSTOMCOLORIDX ? sys->customcolor : celltextcolors[item],
138:                       rect.x + 1, rect.y + 1, rect.width - 2, rect.height - 2);
139:         if (item == CUSTOMCOLORIDX) {
140:             dc.SetTextForeground(LightColor(0x000000));
141:             dc.SetFont(wxFont(9, wxFONTFAMILY_DEFAULT, wxFONTSTYLE_NORMAL, wxFONTWEIGHT_NORMAL,
142:                               false, ""));
143:             dc.DrawText(_("Custom"), rect.x + 1, rect.y + 1);
144:         }
145:     }
146: };
147: 
148: static uint PickColor(wxWindow *parent, uint defaultcolor) {
149:     auto color = wxGetColourFromUser(parent, wxColour(defaultcolor));
150:     if (color.IsOk()) return (color.Blue() << 16) + (color.Green() << 8) + color.Red();
151:     return -1;
152: }
153: 
154: inline static uint LightColor(uint color) { return color ^ sys->colormask; }
155: 
156: #define dd_icon_res_scale 3.0
157: 
158: struct ImagePopup : wxVListBoxComboPopup {
159:     void OnComboDoubleClick() {
160:         auto filename = GetString(GetSelection());
161:         sys->frame->GetCurrentTab()->doc->ImageChange(filename, dd_icon_res_scale);
162:     }
163: };
164: 
165: struct ImageDropdown : wxOwnerDrawnComboBox {
166:     vector<unique_ptr<wxBitmap>> bitmaps_display;
167:     wxArrayString filenames;
168:     const int image_space = 22;
169: 
170:     ImageDropdown(wxWindow *parent, const wxString &directory) {
171:         FillBitmapVector(directory);
172:         Create(parent, A_DDIMAGE, "", wxDefaultPosition,
173:                FromDIP(wxSize(image_space * 2, image_space)), filenames,
174:                wxCB_READONLY | wxCC_SPECIAL_DCLICK);
175:         SetPopupControl(new ImagePopup());
176:         SetSelection(0);
177:         SetPopupMaxHeight(wxDisplay().GetGeometry().GetHeight() * 3 / 4);
178:     }
179: 
180:     wxCoord OnMeasureItem(size_t item) const { return FromDIP(image_space); }
181:     wxCoord OnMeasureItemWidth(size_t item) const { return FromDIP(image_space); }
182:     void OnDrawBackground(wxDC &dc, const wxRect &rect, int item, int flags) const {
183:         DrawRectangle(dc, 0xFFFFFF, rect.x, rect.y, rect.width, rect.height);
184:     }
185: 
186:     void OnDrawItem(wxDC &dc, const wxRect &rect, int item, int flags) const {
187:         sys->ImageDraw(bitmaps_display[item].get(), dc, rect.x + FromDIP(3), rect.y + FromDIP(3));
188:     }
189: 
190:     void FillBitmapVector(const wxString &directory) {
191:         if (!bitmaps_display.empty()) bitmaps_display.resize(0);
192:         auto filename = wxFindFirstFile(directory + "*.*");
193:         while (!filename.empty()) {
194:             wxBitmap bitmap;
195:             if (bitmap.LoadFile(filename, wxBITMAP_TYPE_PNG)) {
196:                 auto scaledbitmap = make_unique<wxBitmap>();
197:                 ScaleBitmap(bitmap, FromDIP(1.0) / dd_icon_res_scale, *scaledbitmap);
198:                 bitmaps_display.push_back(std::move(scaledbitmap));
199:                 filenames.Add(filename);
200:             }
201:             filename = wxFindNextFile();
202:         }
203:     }
204: };
205: 
206: static void ScaleBitmap(const wxBitmap &source, double scale, wxBitmap &destination) {
207:     destination = wxBitmap(source.ConvertToImage().Scale(
208:         source.GetWidth() * scale, source.GetHeight() * scale, wxIMAGE_QUALITY_HIGH));
209: }
210: 
211: static vector<uint8_t> ConvertWxImageToBuffer(const wxImage &image, wxBitmapType bitmaptype) {
212:     wxMemoryOutputStream imageoutputstream(NULL, 0);
213:     image.SaveFile(imageoutputstream, bitmaptype);
214:     auto size = imageoutputstream.TellO();
215:     vector<uint8_t> buffer(size);
216:     imageoutputstream.CopyTo(buffer.data(), size);
217:     return buffer;
218: }
219: 
220: static wxImage ConvertBufferToWxImage(const vector<uint8_t> &buffer, wxBitmapType bitmaptype) {
221:     wxMemoryInputStream imageinputstream(buffer.data(), buffer.size());
222:     wxImage image(imageinputstream, bitmaptype);
223:     if (!image.IsOk()) {
224:         int size = 32;
225:         image.Create(size, size, false);
226:         image.SetRGB(wxRect(0, 0, size, size), 0xFF, 0, 0);
227:         // Set to red to indicate error.
228:     }
229:     return image;
230: }
231: 
232: static wxBitmap ConvertBufferToWxBitmap(const vector<uint8_t> &buffer, wxBitmapType bmt) {
233:     auto image = ConvertBufferToWxImage(buffer, bmt);
234:     wxBitmap bitmap(image, 32);
235:     return bitmap;
236: }
237: 
238: static uint64_t CalculateHash(vector<uint8_t> &buffer) {
239:     return FNV1A64(buffer.data(), buffer.size());
240: }
241: 
242: static void GetFilesFromUser(wxArrayString &filenames, wxWindow *parent, const wxString &title,
243:                              const wxString &filter) {
244:     wxFileDialog filedialog(parent, title, "", "", filter,
245:                             wxFD_OPEN | wxFD_FILE_MUST_EXIST | wxFD_CHANGE_DIR | wxFD_MULTIPLE);
246:     if (filedialog.ShowModal() == wxID_OK) filedialog.GetPaths(filenames);
247: }
248: 
249: static void HintIMELocation(Document *doc, int bx, int by, int bh, int stylebits) {
250:     // TODO: implement on other platforms
251:     #ifdef __WXMSW__
252:         HWND hwnd = doc->canvas->GetHandle();
253:         if (hwnd == 0) return;
254:         int scrollx, scrolly;
255:         doc->canvas->GetViewStart(&scrollx, &scrolly);
256:         int imx = doc->centerx + (bx + doc->hierarchysize) * doc->currentviewscale - scrollx;
257:         int imy = doc->centery + (by + doc->hierarchysize) * doc->currentviewscale - scrolly;
258:         if (HIMC himc = ImmGetContext(hwnd)) {
259:             COMPOSITIONFORM cof = {.dwStyle = CFS_FORCE_POSITION,
260:                                    .ptCurrentPos = {.x = imx, .y = imy}};
261:             ImmSetCompositionWindow(himc, &cof);
262:             LOGFONT lf = {.lfHeight = static_cast<LONG>(-bh * doc->currentviewscale),
263:                           .lfWeight = stylebits & STYLE_BOLD ? FW_BOLD : FW_REGULAR,
264:                           .lfItalic = static_cast<BYTE>(stylebits & STYLE_ITALIC),
265:                           .lfUnderline = static_cast<BYTE>(stylebits & STYLE_UNDERLINE),
266:                           .lfStrikeOut = static_cast<BYTE>(stylebits & STYLE_STRIKETHRU),
267:                           .lfPitchAndFamily = static_cast<BYTE>(stylebits & STYLE_FIXED
268:                                                                     ? FIXED_PITCH | FF_MODERN
269:                                                                     : VARIABLE_PITCH | FF_SWISS)};
270:             ImmSetCompositionFont(himc, &lf);
271:             CANDIDATEFORM caf = {.dwStyle = CFS_CANDIDATEPOS, .ptCurrentPos = {.x = imx, .y = imy}};
272:             ImmSetCandidateWindow(himc, &caf);
273:             ImmReleaseContext(hwnd, himc);
274:         }
275:     #endif
276: }

================
File: TS/docs/script_reference.html
================
  1: <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
  2: <html>
  3: <head>
  4: <title>lobster builtin function reference</title>
  5: <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
  6: <style type="text/css">table.a, tr.a, td.a {font-size: 10pt;border: 1pt solid #DDDDDD; border-Collapse: collapse; max-width: 88em}</style>
  7: </head>
  8: <body><center><table border=0><tr><td>
  9: <p>lobster builtin functions:(file auto generated by compiler, do not modify)</p></td></tr><tr class="a" valign=top><td><h3>treesheets</h3></td></tr>
 10: <tr><td><table class="a" border=1 cellspacing=0 cellpadding=4><tr class="a" valign=top><td class="a"><tt><b>ts.goto_root</b>()</tt></td><td class="a">makes the root of the document the current cell. this is the default at the start of any script, so this function is only needed to return there.</td>
 11: 
 12: </tr>
 13: <tr class="a" valign=top><td class="a"><tt><b>ts.goto_view</b>()</tt></td><td class="a">makes what the user has zoomed into the current cell</td>
 14: 
 15: </tr>
 16: <tr class="a" valign=top><td class="a"><tt><b>ts.has_selection</b>() -> <font color="#666666">int</font></tt></td><td class="a">whether there is a selection</td>
 17: 
 18: </tr>
 19: <tr class="a" valign=top><td class="a"><tt><b>ts.goto_selection</b>()</tt></td><td class="a">makes the current cell the one selected, or the first of a selection</td>
 20: 
 21: </tr>
 22: <tr class="a" valign=top><td class="a"><tt><b>ts.has_parent</b>() -> <font color="#666666">int</font></tt></td><td class="a">whether the current cell has a parent (is the root cell)</td>
 23: 
 24: </tr>
 25: <tr class="a" valign=top><td class="a"><tt><b>ts.goto_parent</b>()</tt></td><td class="a">makes the current cell the parent of the current cell, if any</td>
 26: 
 27: </tr>
 28: <tr class="a" valign=top><td class="a"><tt><b>ts.num_children</b>() -> <font color="#666666">int</font></tt></td><td class="a">returns the total number of children of the current cell (rows * columns). returns 0 if this cell doesn't have a sub-grid at all.</td>
 29: 
 30: </tr>
 31: <tr class="a" valign=top><td class="a"><tt><b>ts.num_columns_rows</b>() -> <font color="#666666">int2</font></tt></td><td class="a">returns the number of columns and rows in the current cell</td>
 32: 
 33: </tr>
 34: <tr class="a" valign=top><td class="a"><tt><b>ts.selection</b>() -> <font color="#666666">int2</font>, <font color="#666666">int2</font></tt></td><td class="a">returns the (xs,ys) and (x,y) of the current selection, or zeroes if none</td>
 35: 
 36: </tr>
 37: <tr class="a" valign=top><td class="a"><tt><b>ts.goto_child</b>(n<font color="#666666">: int</font>)</tt></td><td class="a">makes the current cell the nth child of the current cell</td>
 38: 
 39: </tr>
 40: <tr class="a" valign=top><td class="a"><tt><b>ts.goto_column_row</b>(col<font color="#666666">: int</font>, row<font color="#666666">: int</font>)</tt></td><td class="a">makes the current cell the child at col / row</td>
 41: 
 42: </tr>
 43: <tr class="a" valign=top><td class="a"><tt><b>ts.get_text</b>() -> <font color="#666666">string</font></tt></td><td class="a">gets the text of the current cell.</td>
 44: 
 45: </tr>
 46: <tr class="a" valign=top><td class="a"><tt><b>ts.get_note</b>() -> <font color="#666666">string</font></tt></td><td class="a">gets the note of the current cell.</td>
 47: 
 48: </tr>
 49: <tr class="a" valign=top><td class="a"><tt><b>ts.set_text</b>(text<font color="#666666">: string</font>)</tt></td><td class="a">sets the text of the current cell</td>
 50: 
 51: </tr>
 52: <tr class="a" valign=top><td class="a"><tt><b>ts.set_note</b>(text<font color="#666666">: string</font>)</tt></td><td class="a">sets the note of the current cell</td>
 53: 
 54: </tr>
 55: <tr class="a" valign=top><td class="a"><tt><b>ts.create_grid</b>(cols<font color="#666666">: int</font>, rows<font color="#666666">: int</font>)</tt></td><td class="a">creates a grid in the current cell if there is not one yet</td>
 56: 
 57: </tr>
 58: <tr class="a" valign=top><td class="a"><tt><b>ts.insert_column</b>(c<font color="#666666">: int</font>)</tt></td><td class="a">inserts a column before column c in an existing grid</td>
 59: 
 60: </tr>
 61: <tr class="a" valign=top><td class="a"><tt><b>ts.insert_row</b>(r<font color="#666666">: int</font>)</tt></td><td class="a">inserts a row before row r in an existing grid</td>
 62: 
 63: </tr>
 64: <tr class="a" valign=top><td class="a"><tt><b>ts.delete</b>(position<font color="#666666">: int2</font>, size<font color="#666666">: int2</font>)</tt></td><td class="a">clears the cells denoted by position/size. also removes columns/rows if they become completely empty, or the entire grid.</td>
 65: 
 66: </tr>
 67: <tr class="a" valign=top><td class="a"><tt><b>ts.set_background_color</b>(color<font color="#666666">: float4</font>)</tt></td><td class="a">sets the background color of the current cell</td>
 68: 
 69: </tr>
 70: <tr class="a" valign=top><td class="a"><tt><b>ts.set_text_color</b>(color<font color="#666666">: float4</font>)</tt></td><td class="a">sets the text color of the current cell</td>
 71: 
 72: </tr>
 73: <tr class="a" valign=top><td class="a"><tt><b>ts.set_text_filtered</b>(filtered<font color="#666666">: bool</font>)</tt></td><td class="a">sets the text filtered of the current cell</td>
 74: 
 75: </tr>
 76: <tr class="a" valign=top><td class="a"><tt><b>ts.is_text_filtered</b>() -> <font color="#666666">int</font></tt></td><td class="a">whether the text of the current cell is filtered</td>
 77: 
 78: </tr>
 79: <tr class="a" valign=top><td class="a"><tt><b>ts.set_border_color</b>(color<font color="#666666">: float4</font>)</tt></td><td class="a">sets the border color of the current grid</td>
 80: 
 81: </tr>
 82: <tr class="a" valign=top><td class="a"><tt><b>ts.get_relative_size</b>() -> <font color="#666666">int</font></tt></td><td class="a">returns the relative text size of the current cell</td>
 83: 
 84: </tr>
 85: <tr class="a" valign=top><td class="a"><tt><b>ts.set_relative_size</b>(size<font color="#666666">: int</font>)</tt></td><td class="a">sets the relative size (0 is normal, -1 is smaller etc.) of the current cell</td>
 86: 
 87: </tr>
 88: <tr class="a" valign=top><td class="a"><tt><b>ts.set_style_bits</b>(stylebits<font color="#666666">: int</font>)</tt></td><td class="a">sets one or more styles (bold = 1, italic = 2, fixed = 4, underline = 8, strikethru = 16) on the current cell</td>
 89: 
 90: </tr>
 91: <tr class="a" valign=top><td class="a"><tt><b>ts.get_style_bits</b>() -> <font color="#666666">int</font></tt></td><td class="a">returns the stylebits of the current cell</td>
 92: 
 93: </tr>
 94: <tr class="a" valign=top><td class="a"><tt><b>ts.set_status_message</b>(message<font color="#666666">: string</font>)</tt></td><td class="a">sets the status message in TreeSheets</td>
 95: 
 96: </tr>
 97: <tr class="a" valign=top><td class="a"><tt><b>ts.get_filename_from_user</b>(is_save<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">gets a filename using a file dialog. empty string if cancelled.</td>
 98: 
 99: </tr>
100: <tr class="a" valign=top><td class="a"><tt><b>ts.get_filename</b>() -> <font color="#666666">string</font></tt></td><td class="a">gets the current documents file name</td>
101: 
102: </tr>
103: <tr class="a" valign=top><td class="a"><tt><b>ts.load_document</b>(filename<font color="#666666">: string</font>) -> <font color="#666666">int</font></tt></td><td class="a">loads a document, and makes it the active one. returns false if failed.</td>
104: 
105: </tr>
106: <tr class="a" valign=top><td class="a"><tt><b>ts.set_window_size</b>(width<font color="#666666">: int</font>, height<font color="#666666">: int</font>)</tt></td><td class="a">resizes the window</td>
107: 
108: </tr>
109: <tr class="a" valign=top><td class="a"><tt><b>ts.get_last_edit</b>() -> <font color="#666666">int</font></tt></td><td class="a">gets the timestamp of the last edit in milliseconds since the Unix/C epoch</td>
110: 
111: </tr>
112: <tr class="a" valign=top><td class="a"><tt><b>ts.get_current_time</b>() -> <font color="#666666">int</font></tt></td><td class="a">gets the current timestamp in milliseconds since the Unix/C epoch</td>
113: 
114: </tr>
115: <tr class="a" valign=top><td class="a"><tt><b>ts.is_tag</b>() -> <font color="#666666">int</font></tt></td><td class="a">whether the current cell text is a tag</td>
116: 
117: </tr>
118: <tr class="a" valign=top><td class="a"><tt><b>ts.has_image</b>() -> <font color="#666666">int</font></tt></td><td class="a">whether the current cell has an image</td>
119: 
120: </tr>
121: <tr class="a" valign=top><td class="a"><tt><b>ts.get_column_width</b>() -> <font color="#666666">int</font></tt></td><td class="a">get the column width of the current cell</td>
122: 
123: </tr>
124: <tr class="a" valign=top><td class="a"><tt><b>ts.set_column_width</b>(width<font color="#666666">: int</font>)</tt></td><td class="a">set the column width of the current cell</td>
125: 
126: </tr>
127: <tr class="a" valign=top><td class="a"><tt><b>ts.remove_image</b>()</tt></td><td class="a">remove image in the current cell</td>
128: 
129: </tr>
130: <tr class="a" valign=top><td class="a"><tt><b>ts.set_image</b>(filename<font color="#666666">: string</font>) -> <font color="#666666">int</font></tt></td><td class="a">set image for the current cell</td>
131: 
132: </tr>
133: </table></td></tr>
134: <tr class="a" valign=top><td><h3>builtin</h3></td></tr>
135: <tr><td><table class="a" border=1 cellspacing=0 cellpadding=4><tr class="a" valign=top><td class="a"><tt><b>print</b>(x<font color="#666666">: string</font>)</tt></td><td class="a">output any value to the console (with linefeed).</td>
136: 
137: </tr>
138: <tr class="a" valign=top><td class="a"><tt><b>string</b>(x<font color="#666666">: string</font>) -> <font color="#666666">string</font></tt></td><td class="a">convert any value to string</td>
139: 
140: </tr>
141: <tr class="a" valign=top><td class="a"><tt><b>set_print_depth</b>(depth<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">for printing / string conversion: sets max vectors/objects recursion depth (default 10), returns old value</td>
142: 
143: </tr>
144: <tr class="a" valign=top><td class="a"><tt><b>set_print_length</b>(len<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">for printing / string conversion: sets max string length (default 100000), returns old value</td>
145: 
146: </tr>
147: <tr class="a" valign=top><td class="a"><tt><b>set_print_quoted</b>(quoted<font color="#666666">: bool</font>) -> <font color="#666666">int</font></tt></td><td class="a">for printing / string conversion: if the top level value is a string, whether to convert it with escape codes and quotes (default false), returns old value</td>
148: 
149: </tr>
150: <tr class="a" valign=top><td class="a"><tt><b>set_print_decimals</b>(decimals<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">for printing / string conversion: number of decimals for any floating point output (default -1, meaning all), returns old value</td>
151: 
152: </tr>
153: <tr class="a" valign=top><td class="a"><tt><b>set_print_indent</b>(spaces<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">for printing / string conversion: number of spaces to indent with. default is 0: no indent / no multi-line, returns old value</td>
154: 
155: </tr>
156: <tr class="a" valign=top><td class="a"><tt><b>get_line</b>(prefix<font color="#666666">: string</font>) -> <font color="#666666">string</font></tt></td><td class="a">reads a string from the console if possible (followed by enter). Prefix will be printed before the input</td>
157: 
158: </tr>
159: <tr class="a" valign=top><td class="a"><tt><b>append</b>(xs<font color="#666666">: [any]</font>, ys<font color="#666666">: [any]</font>) -> <font color="#666666">[any]</font></tt></td><td class="a">creates a new vector by appending all elements of 2 input vectors</td>
160: 
161: </tr>
162: <tr class="a" valign=top><td class="a"><tt><b>append_into</b>(dest<font color="#666666">: [any]</font>, src<font color="#666666">: [any]</font>) -> <font color="#666666">[any]</font></tt></td><td class="a">appends all elements of the second vector into the first</td>
163: 
164: </tr>
165: <tr class="a" valign=top><td class="a"><tt><b>vector_capacity</b>(xs<font color="#666666">: [any]</font>, len<font color="#666666">: int</font>) -> <font color="#666666">[any]</font></tt></td><td class="a">ensures the vector capacity (number of elements it can contain before re-allocating) is at least "len". Does not actually add (or remove) elements. This function is just for efficiency in the case the amount of "push" operations is known. returns original vector.</td>
166: 
167: </tr>
168: <tr class="a" valign=top><td class="a"><tt><b>length</b>(x<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">length of int (identity function, useful in combination with string/vector version)</td>
169: 
170: </tr>
171: <tr class="a" valign=top><td class="a"><tt><b>length</b>(s<font color="#666666">: string</font>) -> <font color="#666666">int</font></tt></td><td class="a">length of string</td>
172: 
173: </tr>
174: <tr class="a" valign=top><td class="a"><tt><b>length</b>(xs<font color="#666666">: [any]</font>) -> <font color="#666666">int</font></tt></td><td class="a">length of vector</td>
175: 
176: </tr>
177: <tr class="a" valign=top><td class="a"><tt><b>equal</b>(a<font color="#666666">: any</font>, b<font color="#666666">: any</font>) -> <font color="#666666">int</font></tt></td><td class="a">structural equality between any two values (recurses into vectors/objects, unlike == which is only true for vectors/objects if they are the same object)</td>
178: 
179: </tr>
180: <tr class="a" valign=top><td class="a"><tt><b>push</b>(xs<font color="#666666">: [any]</font>, x<font color="#666666">: any</font>) -> <font color="#666666">[any]</font></tt></td><td class="a">appends one element to a vector, returns existing vector</td>
181: 
182: </tr>
183: <tr class="a" valign=top><td class="a"><tt><b>pop</b>(xs<font color="#666666">: [any]</font>) -> <font color="#666666">any</font></tt></td><td class="a">removes last element from vector and returns it</td>
184: 
185: </tr>
186: <tr class="a" valign=top><td class="a"><tt><b>top</b>(xs<font color="#666666">: [any]</font>) -> <font color="#666666">any</font></tt></td><td class="a">returns last element from vector</td>
187: 
188: </tr>
189: <tr class="a" valign=top><td class="a"><tt><b>insert</b>(xs<font color="#666666">: [any]</font>, i<font color="#666666">: int</font>, x<font color="#666666">: any</font>) -> <font color="#666666">[any]</font></tt></td><td class="a">inserts a value into a vector at index i, existing elements shift upward, returns original vector</td>
190: 
191: </tr>
192: <tr class="a" valign=top><td class="a"><tt><b>remove</b>(xs<font color="#666666">: [any]</font>, i<font color="#666666">: int</font>) -> <font color="#666666">any</font></tt></td><td class="a">remove element at index i, following elements shift down. returns the element removed.</td>
193: 
194: </tr>
195: <tr class="a" valign=top><td class="a"><tt><b>remove_range</b>(xs<font color="#666666">: [any]</font>, i<font color="#666666">: int</font>, n<font color="#666666">: int</font>)</tt></td><td class="a">remove n elements at index i, following elements shift down.</td>
196: 
197: </tr>
198: <tr class="a" valign=top><td class="a"><tt><b>remove_obj</b>(xs<font color="#666666">: [any]</font>, obj<font color="#666666">: any</font>) -> <font color="#666666">any</font></tt></td><td class="a">remove all elements equal to obj (==), returns obj.</td>
199: 
200: </tr>
201: <tr class="a" valign=top><td class="a"><tt><b>truncate</b>(xs<font color="#666666">: [any]</font>, i<font color="#666666">: int</font>)</tt></td><td class="a">removes all elements starting from index i, does nothing if i &gt;= len</td>
202: 
203: </tr>
204: <tr class="a" valign=top><td class="a"><tt><b>binary_search</b>(xs<font color="#666666">: [int]</font>, key<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">does a binary search for key in a sorted vector, returns as first return value how many matches were found, and as second the index in the array where the matches start (so you can read them, overwrite them, or remove them), or if none found, where the key could be inserted such that the vector stays sorted. This overload is for int vectors and keys.</td>
205: 
206: </tr>
207: <tr class="a" valign=top><td class="a"><tt><b>binary_search</b>(xs<font color="#666666">: [float]</font>, key<font color="#666666">: float</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">float version.</td>
208: 
209: </tr>
210: <tr class="a" valign=top><td class="a"><tt><b>binary_search</b>(xs<font color="#666666">: [string]</font>, key<font color="#666666">: string</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">string version.</td>
211: 
212: </tr>
213: <tr class="a" valign=top><td class="a"><tt><b>binary_search_object</b>(xs<font color="#666666">: [any]</font>, key<font color="#666666">: any</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">object version. compares by reference rather than contents.</td>
214: 
215: </tr>
216: <tr class="a" valign=top><td class="a"><tt><b>binary_search_first_field_string</b>(xs<font color="#666666">: [any]</font>, key<font color="#666666">: string</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">object version where key is the first field (must be string, runtime error if it is not)</td>
217: 
218: </tr>
219: <tr class="a" valign=top><td class="a"><tt><b>binary_search_first_field_object</b>(xs<font color="#666666">: [any]</font>, key<font color="#666666">: any</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">object version where key is the first field (must be object, runtime error if it is not)</td>
220: 
221: </tr>
222: <tr class="a" valign=top><td class="a"><tt><b>copy</b>(x<font color="#666666">: any</font>) -> <font color="#666666">any</font></tt></td><td class="a">makes a shallow copy of any object/vector/string.</td>
223: 
224: </tr>
225: <tr class="a" valign=top><td class="a"><tt><b>deepcopy</b>(x<font color="#666666">: any</font>, depth<font color="#666666">: int</font>) -> <font color="#666666">any</font></tt></td><td class="a">makes a deep copy of any object/vector/string. DAGs become trees, and cycles will clone until it reach the given depth. depth == 1 would do the same as copy.</td>
226: 
227: </tr>
228: <tr class="a" valign=top><td class="a"><tt><b>slice</b>(xs<font color="#666666">: [any]</font>, start<font color="#666666">: int</font>, size<font color="#666666">: int</font>) -> <font color="#666666">[any]</font></tt></td><td class="a">returns a sub-vector of size elements from index start. size can be negative to indicate the rest of the vector.</td>
229: 
230: </tr>
231: <tr class="a" valign=top><td class="a"><tt><b>any</b>(xs<font color="#666666">: [any]</font>) -> <font color="#666666">int</font></tt></td><td class="a">returns whether any elements of the vector are true values</td>
232: 
233: </tr>
234: <tr class="a" valign=top><td class="a"><tt><b>any</b>(xs<font color="#666666">: intN</font>) -> <font color="#666666">int</font></tt></td><td class="a">returns whether any elements of the numeric struct are true values</td>
235: 
236: </tr>
237: <tr class="a" valign=top><td class="a"><tt><b>all</b>(xs<font color="#666666">: [any]</font>) -> <font color="#666666">int</font></tt></td><td class="a">returns whether all elements of the vector are true values</td>
238: 
239: </tr>
240: <tr class="a" valign=top><td class="a"><tt><b>all</b>(xs<font color="#666666">: intN</font>) -> <font color="#666666">int</font></tt></td><td class="a">returns whether all elements of the numeric struct are true values</td>
241: 
242: </tr>
243: <tr class="a" valign=top><td class="a"><tt><b>substring</b>(s<font color="#666666">: string</font>, start<font color="#666666">: int</font>, size<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">returns a substring of size characters from index start. size can be negative to indicate the rest of the string.</td>
244: 
245: </tr>
246: <tr class="a" valign=top><td class="a"><tt><b>find_string</b>(s<font color="#666666">: string</font>, substr<font color="#666666">: string</font>, offset<font color="#666666">: int</font> = 0) -> <font color="#666666">int</font></tt></td><td class="a">finds the index at which substr first appears, or -1 if none. optionally start at a position other than 0</td>
247: 
248: </tr>
249: <tr class="a" valign=top><td class="a"><tt><b>find_string_reverse</b>(s<font color="#666666">: string</font>, substr<font color="#666666">: string</font>, offset<font color="#666666">: int</font> = 0) -> <font color="#666666">int</font></tt></td><td class="a">finds the index at which substr first appears when searching from the end, or -1 if none. optionally start at a position other than the end of the string</td>
250: 
251: </tr>
252: <tr class="a" valign=top><td class="a"><tt><b>split_string</b>(s<font color="#666666">: string</font>, delimiter<font color="#666666">: string</font>) -> <font color="#666666">string</font>, <font color="#666666">string</font></tt></td><td class="a">returns two strings, the parts of the input before and after the first delimiter. if not found, returns the input and an empty string</td>
253: 
254: </tr>
255: <tr class="a" valign=top><td class="a"><tt><b>split_string_reverse</b>(s<font color="#666666">: string</font>, delimiter<font color="#666666">: string</font>) -> <font color="#666666">string</font>, <font color="#666666">string</font></tt></td><td class="a">returns two strings, the parts of the input before and after the last delimiter. if not found, returns an empty string and the input</td>
256: 
257: </tr>
258: <tr class="a" valign=top><td class="a"><tt><b>replace_string</b>(s<font color="#666666">: string</font>, a<font color="#666666">: string</font>, b<font color="#666666">: string</font>, count<font color="#666666">: int</font> = 0) -> <font color="#666666">string</font></tt></td><td class="a">returns a copy of s where all occurrences of a have been replaced with b. if a is empty, no replacements are made. if count is specified, makes at most that many replacements</td>
259: 
260: </tr>
261: <tr class="a" valign=top><td class="a"><tt><b>string_to_int</b>(s<font color="#666666">: string</font>, base<font color="#666666">: int</font> = 0) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">converts a string to an int given the base (2..36, e.g. 16 for hex, default is 10).returns 0 if no numeric data could be parsed; second return value is true if allcharacters of the string were parsed.</td>
262: 
263: </tr>
264: <tr class="a" valign=top><td class="a"><tt><b>string_to_float</b>(s<font color="#666666">: string</font>) -> <font color="#666666">float</font>, <font color="#666666">int</font></tt></td><td class="a">converts a string to a float. returns 0.0 if no numeric data could be parsed;second return value is true if all characters of the string were parsed.</td>
265: 
266: </tr>
267: <tr class="a" valign=top><td class="a"><tt><b>tokenize</b>(s<font color="#666666">: string</font>, delimiters<font color="#666666">: string</font>, whitespace<font color="#666666">: string</font>, dividing<font color="#666666">: int</font> = 0) -> <font color="#666666">[string]</font></tt></td><td class="a">splits a string into a vector of strings, by splitting into segments upon each dividing or terminating delimiter. Segments are stripped of leading and trailing whitespace. Example: "; A ; B C;; " becomes [ "", "A", "B C", "" ] with ";" as delimiter and " " as whitespace. If dividing was true, there would be a 5th empty string element.</td>
268: 
269: </tr>
270: <tr class="a" valign=top><td class="a"><tt><b>unicode_to_string</b>(us<font color="#666666">: [int]</font>) -> <font color="#666666">string</font></tt></td><td class="a">converts a vector of ints representing unicode values to a UTF-8 string.</td>
271: 
272: </tr>
273: <tr class="a" valign=top><td class="a"><tt><b>string_to_unicode</b>(s<font color="#666666">: string</font>) -> <font color="#666666">[int]</font>, <font color="#666666">int</font></tt></td><td class="a">converts a UTF-8 string into a vector of unicode values. second return value is false if there was a decoding error, and the vector will only contain the characters up to the error</td>
274: 
275: </tr>
276: <tr class="a" valign=top><td class="a"><tt><b>number_to_string</b>(number<font color="#666666">: int</font>, base<font color="#666666">: int</font>, minchars<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">converts the (unsigned version) of the input integer number to a string given the base (2..36, e.g. 16 for hex) and outputting a minimum of characters (padding with 0).</td>
277: 
278: </tr>
279: <tr class="a" valign=top><td class="a"><tt><b>lowercase</b>(s<font color="#666666">: string</font>) -> <font color="#666666">string</font></tt></td><td class="a">converts a UTF-8 string from any case to lower case, affecting only A-Z</td>
280: 
281: </tr>
282: <tr class="a" valign=top><td class="a"><tt><b>uppercase</b>(s<font color="#666666">: string</font>) -> <font color="#666666">string</font></tt></td><td class="a">converts a UTF-8 string from any case to upper case, affecting only a-z</td>
283: 
284: </tr>
285: <tr class="a" valign=top><td class="a"><tt><b>escape_string</b>(s<font color="#666666">: string</font>, set<font color="#666666">: string</font>, prefix<font color="#666666">: string</font>, postfix<font color="#666666">: string</font>) -> <font color="#666666">string</font></tt></td><td class="a">prefixes &amp; postfixes any occurrences or characters in set in string s</td>
286: 
287: </tr>
288: <tr class="a" valign=top><td class="a"><tt><b>concat_string</b>(v<font color="#666666">: [string]</font>, sep<font color="#666666">: string</font>) -> <font color="#666666">string</font></tt></td><td class="a">concatenates all elements of the string vector, separated with sep.</td>
289: 
290: </tr>
291: <tr class="a" valign=top><td class="a"><tt><b>repeat_string</b>(s<font color="#666666">: string</font>, n<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">returns a string consisting of n copies of the input string.</td>
292: 
293: </tr>
294: <tr class="a" valign=top><td class="a"><tt><b>pow</b>(a<font color="#666666">: int</font>, b<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">a raised to the power of b, for integers, using exponentiation by squaring</td>
295: 
296: </tr>
297: <tr class="a" valign=top><td class="a"><tt><b>pow</b>(a<font color="#666666">: float</font>, b<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">a raised to the power of b</td>
298: 
299: </tr>
300: <tr class="a" valign=top><td class="a"><tt><b>pow</b>(a<font color="#666666">: floatN</font>, b<font color="#666666">: float</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">vector elements raised to the power of b</td>
301: 
302: </tr>
303: <tr class="a" valign=top><td class="a"><tt><b>log</b>(a<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">natural logaritm of a</td>
304: 
305: </tr>
306: <tr class="a" valign=top><td class="a"><tt><b>log2</b>(a<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">base 2 logaritm of a</td>
307: 
308: </tr>
309: <tr class="a" valign=top><td class="a"><tt><b>sqrt</b>(f<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">square root</td>
310: 
311: </tr>
312: <tr class="a" valign=top><td class="a"><tt><b>ceiling</b>(f<font color="#666666">: float</font>) -> <font color="#666666">int</font></tt></td><td class="a">the nearest int &gt;= f</td>
313: 
314: </tr>
315: <tr class="a" valign=top><td class="a"><tt><b>ceiling</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">the nearest ints &gt;= each component of v</td>
316: 
317: </tr>
318: <tr class="a" valign=top><td class="a"><tt><b>floor</b>(f<font color="#666666">: float</font>) -> <font color="#666666">int</font></tt></td><td class="a">the nearest int &lt;= f</td>
319: 
320: </tr>
321: <tr class="a" valign=top><td class="a"><tt><b>floor</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">the nearest ints &lt;= each component of v</td>
322: 
323: </tr>
324: <tr class="a" valign=top><td class="a"><tt><b>int</b>(f<font color="#666666">: float</font>) -> <font color="#666666">int</font></tt></td><td class="a">converts a float to an int by dropping the fraction</td>
325: 
326: </tr>
327: <tr class="a" valign=top><td class="a"><tt><b>int</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">converts a vector of floats to ints by dropping the fraction</td>
328: 
329: </tr>
330: <tr class="a" valign=top><td class="a"><tt><b>round</b>(f<font color="#666666">: float</font>) -> <font color="#666666">int</font></tt></td><td class="a">converts a float to the closest int</td>
331: 
332: </tr>
333: <tr class="a" valign=top><td class="a"><tt><b>round</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">converts a vector of floats to the closest ints</td>
334: 
335: </tr>
336: <tr class="a" valign=top><td class="a"><tt><b>fraction</b>(f<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">returns the fractional part of a float: short for f - floor(f)</td>
337: 
338: </tr>
339: <tr class="a" valign=top><td class="a"><tt><b>fraction</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">returns the fractional part of a vector of floats</td>
340: 
341: </tr>
342: <tr class="a" valign=top><td class="a"><tt><b>float</b>(i<font color="#666666">: int</font>) -> <font color="#666666">float</font></tt></td><td class="a">converts an int to float</td>
343: 
344: </tr>
345: <tr class="a" valign=top><td class="a"><tt><b>float</b>(v<font color="#666666">: intN</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">converts a vector of ints to floats</td>
346: 
347: </tr>
348: <tr class="a" valign=top><td class="a"><tt><b>sin</b>(angle<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">the y coordinate of the normalized vector indicated by angle (in degrees)</td>
349: 
350: </tr>
351: <tr class="a" valign=top><td class="a"><tt><b>sin</b>(angle<font color="#666666">: floatN</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">the y coordinates of the normalized vector indicated by the angles (in degrees)</td>
352: 
353: </tr>
354: <tr class="a" valign=top><td class="a"><tt><b>cos</b>(angle<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">the x coordinate of the normalized vector indicated by angle (in degrees)</td>
355: 
356: </tr>
357: <tr class="a" valign=top><td class="a"><tt><b>cos</b>(angle<font color="#666666">: floatN</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">the x coordinates of the normalized vector indicated by the angles (in degrees)</td>
358: 
359: </tr>
360: <tr class="a" valign=top><td class="a"><tt><b>tan</b>(angle<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">the tangent of an angle (in degrees)</td>
361: 
362: </tr>
363: <tr class="a" valign=top><td class="a"><tt><b>tan</b>(angle<font color="#666666">: floatN</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">the tangents of the angles (in degrees)</td>
364: 
365: </tr>
366: <tr class="a" valign=top><td class="a"><tt><b>sincos</b>(angle<font color="#666666">: float</font>) -> <font color="#666666">float2</font></tt></td><td class="a">the normalized vector indicated by angle (in degrees), same as float2 { cos(angle), sin(angle) }</td>
367: 
368: </tr>
369: <tr class="a" valign=top><td class="a"><tt><b>asin</b>(y<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">the angle (in degrees) indicated by the y coordinate projected to the unit circle</td>
370: 
371: </tr>
372: <tr class="a" valign=top><td class="a"><tt><b>acos</b>(x<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">the angle (in degrees) indicated by the x coordinate projected to the unit circle</td>
373: 
374: </tr>
375: <tr class="a" valign=top><td class="a"><tt><b>atan</b>(x<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">the angle (in degrees) indicated by the y coordinate of the tangent projected to the unit circle</td>
376: 
377: </tr>
378: <tr class="a" valign=top><td class="a"><tt><b>radians</b>(angle<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">converts an angle in degrees to radians</td>
379: 
380: </tr>
381: <tr class="a" valign=top><td class="a"><tt><b>degrees</b>(angle<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">converts an angle in radians to degrees</td>
382: 
383: </tr>
384: <tr class="a" valign=top><td class="a"><tt><b>atan2</b>(vec<font color="#666666">: float2</font>) -> <font color="#666666">float</font></tt></td><td class="a">the angle (in degrees) corresponding to a normalized 2D vector</td>
385: 
386: </tr>
387: <tr class="a" valign=top><td class="a"><tt><b>radians</b>(angle<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">converts an angle in degrees to radians</td>
388: 
389: </tr>
390: <tr class="a" valign=top><td class="a"><tt><b>degrees</b>(angle<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">converts an angle in radians to degrees</td>
391: 
392: </tr>
393: <tr class="a" valign=top><td class="a"><tt><b>normalize</b>(vec<font color="#666666">: floatN</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">returns a vector of unit length</td>
394: 
395: </tr>
396: <tr class="a" valign=top><td class="a"><tt><b>dot</b>(a<font color="#666666">: floatN</font>, b<font color="#666666">: floatN</font>) -> <font color="#666666">float</font></tt></td><td class="a">the length of vector a when projected onto b (or vice versa)</td>
397: 
398: </tr>
399: <tr class="a" valign=top><td class="a"><tt><b>magnitude</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">float</font></tt></td><td class="a">the geometric length of a vector</td>
400: 
401: </tr>
402: <tr class="a" valign=top><td class="a"><tt><b>magnitude_squared</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">float</font></tt></td><td class="a">the geometric length of a vector squared</td>
403: 
404: </tr>
405: <tr class="a" valign=top><td class="a"><tt><b>magnitude_squared</b>(v<font color="#666666">: intN</font>) -> <font color="#666666">int</font></tt></td><td class="a">the geometric length of a vector squared</td>
406: 
407: </tr>
408: <tr class="a" valign=top><td class="a"><tt><b>manhattan</b>(v<font color="#666666">: intN</font>) -> <font color="#666666">int</font></tt></td><td class="a">the manhattan distance of a vector</td>
409: 
410: </tr>
411: <tr class="a" valign=top><td class="a"><tt><b>cross</b>(a<font color="#666666">: float3</font>, b<font color="#666666">: float3</font>) -> <font color="#666666">float3</font></tt></td><td class="a">a perpendicular vector to the 2D plane defined by a and b (swap a and b for its inverse)</td>
412: 
413: </tr>
414: <tr class="a" valign=top><td class="a"><tt><b>volume</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">float</font></tt></td><td class="a">the volume of the area spanned by the vector</td>
415: 
416: </tr>
417: <tr class="a" valign=top><td class="a"><tt><b>volume</b>(v<font color="#666666">: intN</font>) -> <font color="#666666">int</font></tt></td><td class="a">the volume of the area spanned by the vector</td>
418: 
419: </tr>
420: <tr class="a" valign=top><td class="a"><tt><b>rnd</b>(max<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">a random value [0..max).</td>
421: 
422: </tr>
423: <tr class="a" valign=top><td class="a"><tt><b>rnd</b>(max<font color="#666666">: intN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">a random vector within the range of an input vector.</td>
424: 
425: </tr>
426: <tr class="a" valign=top><td class="a"><tt><b>rnd_float</b>() -> <font color="#666666">float</font></tt></td><td class="a">a random float [0..1)</td>
427: 
428: </tr>
429: <tr class="a" valign=top><td class="a"><tt><b>rnd_gaussian</b>() -> <font color="#666666">float</font></tt></td><td class="a">a random float in a gaussian distribution with mean 0 and stddev 1</td>
430: 
431: </tr>
432: <tr class="a" valign=top><td class="a"><tt><b>rnd_seed</b>(seed<font color="#666666">: int</font>)</tt></td><td class="a">explicitly set a random seed for reproducable randomness</td>
433: 
434: </tr>
435: <tr class="a" valign=top><td class="a"><tt><b>rnd_select</b>(index<font color="#666666">: int</font>)</tt></td><td class="a">select a different random number generator to be active. default is 0, max is 1000000</td>
436: 
437: </tr>
438: <tr class="a" valign=top><td class="a"><tt><b>rndm</b>(max<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">deprecated: old mersenne twister version of the above for backwards compat.</td>
439: 
440: </tr>
441: <tr class="a" valign=top><td class="a"><tt><b>rndm_seed</b>(seed<font color="#666666">: int</font>)</tt></td><td class="a">deprecated: old mersenne twister version of the above for backwards compat.</td>
442: 
443: </tr>
444: <tr class="a" valign=top><td class="a"><tt><b>div</b>(a<font color="#666666">: int</font>, b<font color="#666666">: int</font>) -> <font color="#666666">float</font></tt></td><td class="a">forces two ints to be divided as floats</td>
445: 
446: </tr>
447: <tr class="a" valign=top><td class="a"><tt><b>clamp</b>(x<font color="#666666">: int</font>, min<font color="#666666">: int</font>, max<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">forces an integer to be in the range between min and max (inclusive)</td>
448: 
449: </tr>
450: <tr class="a" valign=top><td class="a"><tt><b>clamp</b>(x<font color="#666666">: float</font>, min<font color="#666666">: float</font>, max<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">forces a float to be in the range between min and max (inclusive)</td>
451: 
452: </tr>
453: <tr class="a" valign=top><td class="a"><tt><b>clamp</b>(x<font color="#666666">: intN</font>, min<font color="#666666">: intN</font>, max<font color="#666666">: intN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">forces an integer vector to be in the range between min and max (inclusive)</td>
454: 
455: </tr>
456: <tr class="a" valign=top><td class="a"><tt><b>clamp</b>(x<font color="#666666">: floatN</font>, min<font color="#666666">: floatN</font>, max<font color="#666666">: floatN</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">forces a float vector to be in the range between min and max (inclusive)</td>
457: 
458: </tr>
459: <tr class="a" valign=top><td class="a"><tt><b>in_range</b>(x<font color="#666666">: int</font>, range<font color="#666666">: int</font>, bias<font color="#666666">: int</font> = 0) -> <font color="#666666">int</font></tt></td><td class="a">checks if an integer is &gt;= bias and &lt; bias + range. Bias defaults to 0.</td>
460: 
461: </tr>
462: <tr class="a" valign=top><td class="a"><tt><b>in_range</b>(x<font color="#666666">: float</font>, range<font color="#666666">: float</font>, bias<font color="#666666">: float</font> = 0.000000) -> <font color="#666666">int</font></tt></td><td class="a">checks if a float is &gt;= bias and &lt; bias + range. Bias defaults to 0.</td>
463: 
464: </tr>
465: <tr class="a" valign=top><td class="a"><tt><b>in_range</b>(x<font color="#666666">: int2</font>, range<font color="#666666">: int2</font>, bias<font color="#666666">: int2</font> = nil) -> <font color="#666666">int</font></tt></td><td class="a">checks if a 2d integer vector is &gt;= bias and &lt; bias + range. Bias defaults to 0.</td>
466: 
467: </tr>
468: <tr class="a" valign=top><td class="a"><tt><b>in_range</b>(x<font color="#666666">: int3</font>, range<font color="#666666">: int3</font>, bias<font color="#666666">: int3</font> = nil) -> <font color="#666666">int</font></tt></td><td class="a">checks if a 3d integer vector is &gt;= bias and &lt; bias + range. Bias defaults to 0.</td>
469: 
470: </tr>
471: <tr class="a" valign=top><td class="a"><tt><b>in_range</b>(x<font color="#666666">: float2</font>, range<font color="#666666">: float2</font>, bias<font color="#666666">: float2</font> = nil) -> <font color="#666666">int</font></tt></td><td class="a">checks if a 2d float vector is &gt;= bias and &lt; bias + range. Bias defaults to 0.</td>
472: 
473: </tr>
474: <tr class="a" valign=top><td class="a"><tt><b>in_range</b>(x<font color="#666666">: float3</font>, range<font color="#666666">: float3</font>, bias<font color="#666666">: float3</font> = nil) -> <font color="#666666">int</font></tt></td><td class="a">checks if a 2d float vector is &gt;= bias and &lt; bias + range. Bias defaults to 0.</td>
475: 
476: </tr>
477: <tr class="a" valign=top><td class="a"><tt><b>abs</b>(x<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">absolute value of an integer</td>
478: 
479: </tr>
480: <tr class="a" valign=top><td class="a"><tt><b>abs</b>(x<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">absolute value of a float</td>
481: 
482: </tr>
483: <tr class="a" valign=top><td class="a"><tt><b>abs</b>(x<font color="#666666">: intN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">absolute value of an int vector</td>
484: 
485: </tr>
486: <tr class="a" valign=top><td class="a"><tt><b>abs</b>(x<font color="#666666">: floatN</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">absolute value of a float vector</td>
487: 
488: </tr>
489: <tr class="a" valign=top><td class="a"><tt><b>sign</b>(x<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">sign (-1, 0, 1) of an integer</td>
490: 
491: </tr>
492: <tr class="a" valign=top><td class="a"><tt><b>sign</b>(x<font color="#666666">: float</font>) -> <font color="#666666">int</font></tt></td><td class="a">sign (-1, 0, 1) of a float</td>
493: 
494: </tr>
495: <tr class="a" valign=top><td class="a"><tt><b>sign</b>(x<font color="#666666">: intN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">signs of an int vector</td>
496: 
497: </tr>
498: <tr class="a" valign=top><td class="a"><tt><b>sign</b>(x<font color="#666666">: floatN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">signs of a float vector</td>
499: 
500: </tr>
501: <tr class="a" valign=top><td class="a"><tt><b>min</b>(x<font color="#666666">: int</font>, y<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">smallest of 2 integers.</td>
502: 
503: </tr>
504: <tr class="a" valign=top><td class="a"><tt><b>min</b>(x<font color="#666666">: float</font>, y<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">smallest of 2 floats.</td>
505: 
506: </tr>
507: <tr class="a" valign=top><td class="a"><tt><b>min</b>(x<font color="#666666">: intN</font>, y<font color="#666666">: intN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">smallest components of 2 int vectors</td>
508: 
509: </tr>
510: <tr class="a" valign=top><td class="a"><tt><b>min</b>(x<font color="#666666">: floatN</font>, y<font color="#666666">: floatN</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">smallest components of 2 float vectors</td>
511: 
512: </tr>
513: <tr class="a" valign=top><td class="a"><tt><b>min</b>(v<font color="#666666">: intN</font>) -> <font color="#666666">int</font></tt></td><td class="a">smallest component of a int vector.</td>
514: 
515: </tr>
516: <tr class="a" valign=top><td class="a"><tt><b>min</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">float</font></tt></td><td class="a">smallest component of a float vector.</td>
517: 
518: </tr>
519: <tr class="a" valign=top><td class="a"><tt><b>min</b>(v<font color="#666666">: [int]</font>) -> <font color="#666666">int</font></tt></td><td class="a">smallest component of a int vector, or INT_MAX if length 0.</td>
520: 
521: </tr>
522: <tr class="a" valign=top><td class="a"><tt><b>min</b>(v<font color="#666666">: [float]</font>) -> <font color="#666666">float</font></tt></td><td class="a">smallest component of a float vector, or FLT_MAX if length 0.</td>
523: 
524: </tr>
525: <tr class="a" valign=top><td class="a"><tt><b>max</b>(x<font color="#666666">: int</font>, y<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">largest of 2 integers.</td>
526: 
527: </tr>
528: <tr class="a" valign=top><td class="a"><tt><b>max</b>(x<font color="#666666">: float</font>, y<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">largest of 2 floats.</td>
529: 
530: </tr>
531: <tr class="a" valign=top><td class="a"><tt><b>max</b>(x<font color="#666666">: intN</font>, y<font color="#666666">: intN</font>) -> <font color="#666666">intN</font></tt></td><td class="a">largest components of 2 int vectors</td>
532: 
533: </tr>
534: <tr class="a" valign=top><td class="a"><tt><b>max</b>(x<font color="#666666">: floatN</font>, y<font color="#666666">: floatN</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">largest components of 2 float vectors</td>
535: 
536: </tr>
537: <tr class="a" valign=top><td class="a"><tt><b>max</b>(v<font color="#666666">: intN</font>) -> <font color="#666666">int</font></tt></td><td class="a">largest component of a int vector.</td>
538: 
539: </tr>
540: <tr class="a" valign=top><td class="a"><tt><b>max</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">float</font></tt></td><td class="a">largest component of a float vector.</td>
541: 
542: </tr>
543: <tr class="a" valign=top><td class="a"><tt><b>max</b>(v<font color="#666666">: [int]</font>) -> <font color="#666666">int</font></tt></td><td class="a">largest component of a int vector, or INT_MIN if length 0.</td>
544: 
545: </tr>
546: <tr class="a" valign=top><td class="a"><tt><b>max</b>(v<font color="#666666">: [float]</font>) -> <font color="#666666">float</font></tt></td><td class="a">largest component of a float vector, or FLT_MIN if length 0.</td>
547: 
548: </tr>
549: <tr class="a" valign=top><td class="a"><tt><b>popcount</b>(x<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">number of bits set in an integer</td>
550: 
551: </tr>
552: <tr class="a" valign=top><td class="a"><tt><b>lerp</b>(x<font color="#666666">: float</font>, y<font color="#666666">: float</font>, f<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">linearly interpolates between x and y with factor f [0..1]</td>
553: 
554: </tr>
555: <tr class="a" valign=top><td class="a"><tt><b>lerp</b>(a<font color="#666666">: floatN</font>, b<font color="#666666">: floatN</font>, f<font color="#666666">: float</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">linearly interpolates between a and b vectors with factor f [0..1]</td>
556: 
557: </tr>
558: <tr class="a" valign=top><td class="a"><tt><b>spherical_lerp</b>(a<font color="#666666">: float4</font>, b<font color="#666666">: float4</font>, f<font color="#666666">: float</font>) -> <font color="#666666">float4</font></tt></td><td class="a">spherically interpolates between a and b quaternions with factor f [0..1]</td>
559: 
560: </tr>
561: <tr class="a" valign=top><td class="a"><tt><b>smoothmin</b>(x<font color="#666666">: float</font>, y<font color="#666666">: float</font>, k<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">k is the influence range</td>
562: 
563: </tr>
564: <tr class="a" valign=top><td class="a"><tt><b>smoothstep</b>(x<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">input must be in range 0..1, https://en.wikipedia.org/wiki/Smoothstep</td>
565: 
566: </tr>
567: <tr class="a" valign=top><td class="a"><tt><b>smoothstep</b>(a<font color="#666666">: float</font>, b<font color="#666666">: float</font>, f<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">hermite interpolation between a and b by f [0..1], https://registry.khronos.org/OpenGL-Refpages/gl4/html/smoothstep.xhtml</td>
568: 
569: </tr>
570: <tr class="a" valign=top><td class="a"><tt><b>smootherstep</b>(x<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">input must be in range 0..1, https://en.wikipedia.org/wiki/Smoothstep</td>
571: 
572: </tr>
573: <tr class="a" valign=top><td class="a"><tt><b>cardinal_spline</b>(z<font color="#666666">: floatN</font>, a<font color="#666666">: floatN</font>, b<font color="#666666">: floatN</font>, c<font color="#666666">: floatN</font>, f<font color="#666666">: float</font>, tension<font color="#666666">: float</font>) -> <font color="#666666">floatN</font></tt></td><td class="a">computes the position between a and b with factor f [0..1], using z (before a) and c (after b) to form a cardinal spline (tension at 0.5 is a good default)</td>
574: 
575: </tr>
576: <tr class="a" valign=top><td class="a"><tt><b>line_intersect</b>(line1a<font color="#666666">: float2</font>, line1b<font color="#666666">: float2</font>, line2a<font color="#666666">: float2</font>, line2b<font color="#666666">: float2</font>) -> <font color="#666666">int</font>, <font color="#666666">float2</font></tt></td><td class="a">computes if there is an intersection point between 2 line segments, with the point as second return value</td>
577: 
578: </tr>
579: <tr class="a" valign=top><td class="a"><tt><b>circles_within_range</b>(dist<font color="#666666">: float</font>, positions<font color="#666666">: [float2]</font>, radiuses<font color="#666666">: [float]</font>, positions2<font color="#666666">: [float2]</font>, radiuses2<font color="#666666">: [float]</font>, gridsize<font color="#666666">: int2</font>) -> <font color="#666666">[[int]]</font></tt></td><td class="a">Given a vector of 2D positions (and same size vectors of radiuses), returns a vector of vectors of indices (to the second set of positions and radiuses) of the circles that are within dist of eachothers radius. If the second set are [], the first set is used for both (and the self element is excluded). gridsize optionally specifies the size of the grid to use for accellerated lookup of nearby points. This is essential for the algorithm to be fast, too big or too small can cause slowdown. Omit it, and a heuristic will be chosen for you, which is currently sqrt(num_circles) * 2 along each dimension, e.g. 100 elements would use a 20x20 grid. Efficiency wise this algorithm is fastest if there is not too much variance in the radiuses of the second set and/or the second set has smaller radiuses than the first.</td>
580: 
581: </tr>
582: <tr class="a" valign=top><td class="a"><tt><b>wave_function_collapse</b>(tilemap<font color="#666666">: [string]</font>, size<font color="#666666">: int2</font>) -> <font color="#666666">[string]</font>, <font color="#666666">int</font></tt></td><td class="a">returns a tilemap of given size modelled after the possible shapes in the input tilemap. Tilemap should consist of chars in the 0..127 range. Second return value the number of failed neighbor matches, this should ideally be 0, but can be non-0 for larger maps. Simply call this function repeatedly until it is 0</td>
583: 
584: </tr>
585: <tr class="a" valign=top><td class="a"><tt><b>hash</b>(x<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">hashes an int value into a positive int; may be the identity function</td>
586: 
587: </tr>
588: <tr class="a" valign=top><td class="a"><tt><b>hash</b>(x<font color="#666666">: any</font>) -> <font color="#666666">int</font></tt></td><td class="a">hashes any ref value into a positive int</td>
589: 
590: </tr>
591: <tr class="a" valign=top><td class="a"><tt><b>hash</b>(x<font color="#666666">: function</font>) -> <font color="#666666">int</font></tt></td><td class="a">hashes a function value into a positive int</td>
592: 
593: </tr>
594: <tr class="a" valign=top><td class="a"><tt><b>hash</b>(x<font color="#666666">: float</font>) -> <font color="#666666">int</font></tt></td><td class="a">hashes a float value into a positive int</td>
595: 
596: </tr>
597: <tr class="a" valign=top><td class="a"><tt><b>hash</b>(v<font color="#666666">: intN</font>) -> <font color="#666666">int</font></tt></td><td class="a">hashes a int vector into a positive int</td>
598: 
599: </tr>
600: <tr class="a" valign=top><td class="a"><tt><b>hash</b>(v<font color="#666666">: floatN</font>) -> <font color="#666666">int</font></tt></td><td class="a">hashes a float vector into a positive int</td>
601: 
602: </tr>
603: <tr class="a" valign=top><td class="a"><tt><b>call_function_value</b>(x<font color="#666666">: function</font>)</tt></td><td class="a">calls a void / no args function value.. you shouldn't need to use this, it is a demonstration of how native code can call back into Lobster</td>
604: 
605: </tr>
606: <tr class="a" valign=top><td class="a"><tt><b>type_id</b>(ref<font color="#666666">: any</font>) -> <font color="#666666">int</font></tt></td><td class="a">int uniquely representing the type of the given reference (object/vector/string/resource). this is the same as typeof, except dynamic (accounts for subtypes of the static type). useful to compare the types of objects quickly. specializations of a generic type will result in different ids.</td>
607: 
608: </tr>
609: <tr class="a" valign=top><td class="a"><tt><b>type_string</b>(ref<font color="#666666">: any</font>) -> <font color="#666666">string</font></tt></td><td class="a">string representing the type of the given reference (object/vector/string/resource)</td>
610: 
611: </tr>
612: <tr class="a" valign=top><td class="a"><tt><b>type_element_string</b>(v<font color="#666666">: [any]</font>) -> <font color="#666666">string</font></tt></td><td class="a">string representing the type of the elements of a vector</td>
613: 
614: </tr>
615: <tr class="a" valign=top><td class="a"><tt><b>type_field_count</b>(obj<font color="#666666">: any</font>) -> <font color="#666666">int</font></tt></td><td class="a">number of fields in an object, or 0 for other reference types</td>
616: 
617: </tr>
618: <tr class="a" valign=top><td class="a"><tt><b>type_field_string</b>(obj<font color="#666666">: any</font>, idx<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">string representing the type of a field in an object, or empty for other reference types</td>
619: 
620: </tr>
621: <tr class="a" valign=top><td class="a"><tt><b>type_field_name</b>(obj<font color="#666666">: any</font>, idx<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">name of a field in an object, or empty for other reference types</td>
622: 
623: </tr>
624: <tr class="a" valign=top><td class="a"><tt><b>type_field_value</b>(obj<font color="#666666">: any</font>, idx<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">string representing the value of a field in an object, or empty for other reference types</td>
625: 
626: </tr>
627: <tr class="a" valign=top><td class="a"><tt><b>type_enum_value_name</b>(enum_type_id<font color="#666666">: typeid(any)</font>, idx<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">string representing the name of an enum value, belonging to the enum (use typeof)</td>
628: 
629: </tr>
630: <tr class="a" valign=top><td class="a"><tt><b>program_name</b>() -> <font color="#666666">string</font></tt></td><td class="a">returns the name of the main program (e.g. "foo.lobster"), "" if running from lpak.</td>
631: 
632: </tr>
633: <tr class="a" valign=top><td class="a"><tt><b>vm_compiled_mode</b>() -> <font color="#666666">int</font></tt></td><td class="a">returns if the VM is running in compiled mode (Lobster -&gt; C++), or false for JIT.</td>
634: 
635: </tr>
636: <tr class="a" valign=top><td class="a"><tt><b>seconds_elapsed</b>() -> <font color="#666666">float</font></tt></td><td class="a">seconds since program start as a float, unlike gl.time() it is calculated every time it is called</td>
637: 
638: </tr>
639: <tr class="a" valign=top><td class="a"><tt><b>date_time</b>(utc<font color="#666666">: bool</font> = false) -> <font color="#666666">[int]</font></tt></td><td class="a">a vector of integers representing date &amp; time information (index with date_time.lobster). By default returns local time, pass true for UTC instead.</td>
640: 
641: </tr>
642: <tr class="a" valign=top><td class="a"><tt><b>date_time_string</b>(utc<font color="#666666">: bool</font> = false) -> <font color="#666666">string</font></tt></td><td class="a">a string representing date &amp; time information in the format: 'Www Mmm dd hh:mm:ss yyyy'. By default returns local time, pass true for UTC instead.</td>
643: 
644: </tr>
645: <tr class="a" valign=top><td class="a"><tt><b>date_time_string_format</b>(format<font color="#666666">: string</font>, utc<font color="#666666">: bool</font> = false) -> <font color="#666666">string</font></tt></td><td class="a">a string representing date &amp; time information using a formatting string according to https://en.cppreference.com/w/cpp/chrono/c/strftime, for example "%Y_%m_%d_%H_%M_%S". By default returns local time, pass true for UTC instead.</td>
646: 
647: </tr>
648: <tr class="a" valign=top><td class="a"><tt><b>date_time_build_info</b>() -> <font color="#666666">string</font></tt></td><td class="a">a string representing information from when this program was compiled.</td>
649: 
650: </tr>
651: <tr class="a" valign=top><td class="a"><tt><b>get_stack_trace</b>() -> <font color="#666666">string</font></tt></td><td class="a">gets a stack trace of the current location of the program (needs --runtime-stack-trace) without actually stopping the program.</td>
652: 
653: </tr>
654: <tr class="a" valign=top><td class="a"><tt><b>get_memory_usage</b>(n<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">gets a text showing the top n object types that are using the most memory.</td>
655: 
656: </tr>
657: <tr class="a" valign=top><td class="a"><tt><b>pass</b>()</tt></td><td class="a">does nothing. useful for empty bodies of control structures.</td>
658: 
659: </tr>
660: <tr class="a" valign=top><td class="a"><tt><b>reference_count</b>(val<font color="#666666">: any</font>) -> <font color="#666666">int</font></tt></td><td class="a">get the reference count of any value. for compiler debugging, mostly</td>
661: 
662: </tr>
663: <tr class="a" valign=top><td class="a"><tt><b>set_console</b>(on<font color="#666666">: bool</font>)</tt></td><td class="a">lets you turn on/off the console window (on Windows)</td>
664: 
665: </tr>
666: <tr class="a" valign=top><td class="a"><tt><b>set_output_level</b>(level<font color="#666666">: int</font>)</tt></td><td class="a">0 = debug, 1 = verbose, 2 = warn (default), 3 = error, 4 = program</td>
667: 
668: </tr>
669: <tr class="a" valign=top><td class="a"><tt><b>set_exit_code</b>(code<font color="#666666">: int</font>)</tt></td><td class="a">this will be returned when run as a console application</td>
670: 
671: </tr>
672: <tr class="a" valign=top><td class="a"><tt><b>command_line_arguments</b>() -> <font color="#666666">[string]</font></tt></td><td class="a"></td>
673: 
674: </tr>
675: <tr class="a" valign=top><td class="a"><tt><b>thread_information</b>() -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">returns the number of hardware threads, and the number of cores</td>
676: 
677: </tr>
678: <tr class="a" valign=top><td class="a"><tt><b>is_worker_thread</b>() -> <font color="#666666">int</font></tt></td><td class="a">whether the current thread is a worker thread</td>
679: 
680: </tr>
681: <tr class="a" valign=top><td class="a"><tt><b>start_worker_threads</b>(numthreads<font color="#666666">: int</font>)</tt></td><td class="a">launch worker threads</td>
682: 
683: </tr>
684: <tr class="a" valign=top><td class="a"><tt><b>stop_worker_threads</b>()</tt></td><td class="a">only needs to be called if you want to stop the worker threads before the end of the program, or if you want to call start_worker_threads again. workers_alive will become false inside the workers, which should then exit.</td>
685: 
686: </tr>
687: <tr class="a" valign=top><td class="a"><tt><b>workers_alive</b>() -> <font color="#666666">int</font></tt></td><td class="a">whether workers should continue doing work. returns false after stop_worker_threads() has been called.</td>
688: 
689: </tr>
690: <tr class="a" valign=top><td class="a"><tt><b>thread_write</b>(object<font color="#666666">: any</font>)</tt></td><td class="a">put this object in the thread queue</td>
691: 
692: </tr>
693: <tr class="a" valign=top><td class="a"><tt><b>thread_read</b>(type<font color="#666666">: typeid(any)</font>) -> <font color="#666666">any?</font></tt></td><td class="a">get an object from the thread queue. pass the typeof object. blocks if no such objects available. returns object, or nil if this was the result of thread_wake() or stop_worker_threads() was called</td>
694: 
695: </tr>
696: <tr class="a" valign=top><td class="a"><tt><b>thread_check</b>(type<font color="#666666">: typeid(any)</font>) -> <font color="#666666">any?</font></tt></td><td class="a">tests if an object is available on the thread queue. pass the typeof object. returns object, or nil if none available, or if stop_worker_threads() was called</td>
697: 
698: </tr>
699: <tr class="a" valign=top><td class="a"><tt><b>thread_wake</b>(type<font color="#666666">: typeid(any)</font>)</tt></td><td class="a">wakes up one thread that are currently blocked on a thread_read for this type. this will cause them to return nil since no object is sent. it is similar to thread_write(nil)</td>
700: 
701: </tr>
702: <tr class="a" valign=top><td class="a"><tt><b>crash_test_cpp_nullptr_exception</b>()</tt></td><td class="a">only for testing crash dump functionality, don't use! :)</td>
703: 
704: </tr>
705: </table></td></tr>
706: <tr class="a" valign=top><td><h3>compiler</h3></td></tr>
707: <tr><td><table class="a" border=1 cellspacing=0 cellpadding=4><tr class="a" valign=top><td class="a"><tt><b>compile_run_code</b>(code<font color="#666666">: string</font>, args<font color="#666666">: [string]</font>) -> <font color="#666666">string</font>, <font color="#666666">string?</font></tt></td><td class="a">compiles and runs lobster source, sandboxed from the current program (in its own VM). the argument is a string of code. returns the return value of the program as a string, with an error string as second return value, or nil if none. using parse_data(), two program can communicate more complex data structures even if they don't have the same version of struct definitions.</td>
708: 
709: </tr>
710: <tr class="a" valign=top><td class="a"><tt><b>compile_run_file</b>(filename<font color="#666666">: string</font>, args<font color="#666666">: [string]</font>) -> <font color="#666666">string</font>, <font color="#666666">string?</font></tt></td><td class="a">same as compile_run_code(), only now you pass a filename.</td>
711: 
712: </tr>
713: </table></td></tr>
714: <tr class="a" valign=top><td><h3>file</h3></td></tr>
715: <tr><td><table class="a" border=1 cellspacing=0 cellpadding=4><tr class="a" valign=top><td class="a"><tt><b>format_time</b>(format<font color="#666666">: string</font>, time<font color="#666666">: int</font>, localtime<font color="#666666">: bool</font>) -> <font color="#666666">string</font></tt></td><td class="a">convert a time in seconds since 00:00:00 UTC, Thursday, 1 January 1970 into a string, using the same format string syntax as POSIX strftime. If localtime is true, then the time will be displayed using the local timezone, otherwise it will use UTC. Returns an empty string on error.</td>
716: 
717: </tr>
718: <tr class="a" valign=top><td class="a"><tt><b>scan_folder</b>(folder<font color="#666666">: string</font>, rel<font color="#666666">: bool</font> = false) -> <font color="#666666">[string]?</font>, <font color="#666666">[int]?</font>, <font color="#666666">[int]?</font></tt></td><td class="a">returns three vectors representing all elements in a folder, the first vector containing all names, the second vector containing sizes in bytes (or -1 if a directory), and the third as the number of seconds since 00:00:00 UTC, Thursday, 1 January 1970, not including leap seconds. set rel use a relative path, default is absolute. Returns nil if folder couldn't be scanned.</td>
719: 
720: </tr>
721: <tr class="a" valign=top><td class="a"><tt><b>read_file</b>(file<font color="#666666">: string</font>, textmode<font color="#666666">: int</font> = 0) -> <font color="#666666">string?</font></tt></td><td class="a">returns the contents of a file as a string, or nil if the file can't be found. you may use either \ or / as path separators</td>
722: 
723: </tr>
724: <tr class="a" valign=top><td class="a"><tt><b>write_file</b>(file<font color="#666666">: string</font>, contents<font color="#666666">: string</font>, textmode<font color="#666666">: int</font> = 0, absolute_path<font color="#666666">: int</font> = 0) -> <font color="#666666">int</font></tt></td><td class="a">creates a file with the contents of a string, returns false if writing wasn't possible</td>
725: 
726: </tr>
727: <tr class="a" valign=top><td class="a"><tt><b>rename_file</b>(old_file<font color="#666666">: string</font>, new_file<font color="#666666">: string</font>) -> <font color="#666666">int</font></tt></td><td class="a">renames a file, returns false if it wasn't possible</td>
728: 
729: </tr>
730: <tr class="a" valign=top><td class="a"><tt><b>delete_file</b>(file<font color="#666666">: string</font>) -> <font color="#666666">int</font></tt></td><td class="a">deletes a file, returns false if it wasn't possible. Will search in all import dirs.</td>
731: 
732: </tr>
733: <tr class="a" valign=top><td class="a"><tt><b>exists_file</b>(file<font color="#666666">: string</font>) -> <font color="#666666">int</font></tt></td><td class="a">checks whether a file exists.</td>
734: 
735: </tr>
736: <tr class="a" valign=top><td class="a"><tt><b>launch_subprocess</b>(commandline<font color="#666666">: [string]</font>, stdin<font color="#666666">: string</font> = nil) -> <font color="#666666">int</font>, <font color="#666666">string</font></tt></td><td class="a">launches a sub process, with optionally a stdin for the process, and returns its return code (or -1 if it couldn't launch at all), and any output</td>
737: 
738: </tr>
739: <tr class="a" valign=top><td class="a"><tt><b>vector_to_buffer</b>(vec<font color="#666666">: [any]</font>, width<font color="#666666">: int</font> = 4) -> <font color="#666666">string</font></tt></td><td class="a">converts a vector of ints/floats (or structs of them) to a buffer, where each scalar is written with "width" bytes (1/2/4/8, default 4). Returns nil if the type couldn't be converted. Uses native endianness.</td>
740: 
741: </tr>
742: <tr class="a" valign=top><td class="a"><tt><b>ensure_size</b>(string<font color="#666666">: string</font>, size<font color="#666666">: int</font>, char<font color="#666666">: int</font>, extra<font color="#666666">: int</font> = 0) -> <font color="#666666">string</font></tt></td><td class="a">ensures a string is at least size characters. if it is, just returns the existing string, otherwise returns a new string of that size (with optionally extra bytes added), with any new characters set to char. You can specify a negative size to mean relative to the end, i.e. new characters will be added at the start. </td>
743: 
744: </tr>
745: <tr class="a" valign=top><td class="a"><tt><b>write_int64_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: int</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">writes a value as little endian to a string at location i. Uses ensure_size to make the string twice as long (with extra 0 bytes) if no space. Returns new string if resized, and the index of the location right after where the value was written. The _back version writes relative to the end (and writes before the index)</td>
746: 
747: </tr>
748: <tr class="a" valign=top><td class="a"><tt><b>write_int32_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: int</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
749: 
750: </tr>
751: <tr class="a" valign=top><td class="a"><tt><b>write_int16_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: int</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
752: 
753: </tr>
754: <tr class="a" valign=top><td class="a"><tt><b>write_int8_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: int</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
755: 
756: </tr>
757: <tr class="a" valign=top><td class="a"><tt><b>write_float64_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: float</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
758: 
759: </tr>
760: <tr class="a" valign=top><td class="a"><tt><b>write_float32_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: float</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
761: 
762: </tr>
763: <tr class="a" valign=top><td class="a"><tt><b>write_int64_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: int</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
764: 
765: </tr>
766: <tr class="a" valign=top><td class="a"><tt><b>write_int32_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: int</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
767: 
768: </tr>
769: <tr class="a" valign=top><td class="a"><tt><b>write_int16_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: int</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
770: 
771: </tr>
772: <tr class="a" valign=top><td class="a"><tt><b>write_int8_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: int</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
773: 
774: </tr>
775: <tr class="a" valign=top><td class="a"><tt><b>write_float64_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: float</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
776: 
777: </tr>
778: <tr class="a" valign=top><td class="a"><tt><b>write_float32_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, val<font color="#666666">: float</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">(see write_int64_le)</td>
779: 
780: </tr>
781: <tr class="a" valign=top><td class="a"><tt><b>write_substring</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, substr<font color="#666666">: string</font>, nullterm<font color="#666666">: int</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a">writes a substring into another string at i (see also write_int64_le)</td>
782: 
783: </tr>
784: <tr class="a" valign=top><td class="a"><tt><b>write_substring_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>, substr<font color="#666666">: string</font>, nullterm<font color="#666666">: int</font>) -> <font color="#666666">string</font>, <font color="#666666">int</font></tt></td><td class="a"></td>
785: 
786: </tr>
787: <tr class="a" valign=top><td class="a"><tt><b>compare_substring</b>(string_a<font color="#666666">: string</font>, i_a<font color="#666666">: int</font>, string_b<font color="#666666">: string</font>, i_b<font color="#666666">: int</font>, len<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">returns if the two substrings are equal (0), or a &lt; b (-1) or a &gt; b (1).</td>
788: 
789: </tr>
790: <tr class="a" valign=top><td class="a"><tt><b>read_int64_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">reads a value as little endian from a string at location i. The value must be within bounds of the string. Returns the value, and the index of the location right after where the value was read. The _back version reads relative to the end (and reads before the index)</td>
791: 
792: </tr>
793: <tr class="a" valign=top><td class="a"><tt><b>read_int32_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
794: 
795: </tr>
796: <tr class="a" valign=top><td class="a"><tt><b>read_int16_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
797: 
798: </tr>
799: <tr class="a" valign=top><td class="a"><tt><b>read_int8_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
800: 
801: </tr>
802: <tr class="a" valign=top><td class="a"><tt><b>read_uint64_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">reads a value as little endian from a string at location i. The value must be within bounds of the string. Returns the value, and the index of the location right after where the value was read. The _back version reads relative to the end (and reads before the index)</td>
803: 
804: </tr>
805: <tr class="a" valign=top><td class="a"><tt><b>read_uint32_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
806: 
807: </tr>
808: <tr class="a" valign=top><td class="a"><tt><b>read_uint16_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
809: 
810: </tr>
811: <tr class="a" valign=top><td class="a"><tt><b>read_uint8_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
812: 
813: </tr>
814: <tr class="a" valign=top><td class="a"><tt><b>read_float64_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">float</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
815: 
816: </tr>
817: <tr class="a" valign=top><td class="a"><tt><b>read_float32_le</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">float</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
818: 
819: </tr>
820: <tr class="a" valign=top><td class="a"><tt><b>read_int64_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
821: 
822: </tr>
823: <tr class="a" valign=top><td class="a"><tt><b>read_int32_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
824: 
825: </tr>
826: <tr class="a" valign=top><td class="a"><tt><b>read_int16_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
827: 
828: </tr>
829: <tr class="a" valign=top><td class="a"><tt><b>read_int8_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
830: 
831: </tr>
832: <tr class="a" valign=top><td class="a"><tt><b>read_uint64_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
833: 
834: </tr>
835: <tr class="a" valign=top><td class="a"><tt><b>read_uint32_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
836: 
837: </tr>
838: <tr class="a" valign=top><td class="a"><tt><b>read_uint16_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
839: 
840: </tr>
841: <tr class="a" valign=top><td class="a"><tt><b>read_uint8_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">int</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
842: 
843: </tr>
844: <tr class="a" valign=top><td class="a"><tt><b>read_float64_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">float</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
845: 
846: </tr>
847: <tr class="a" valign=top><td class="a"><tt><b>read_float32_le_back</b>(string<font color="#666666">: string</font>, i<font color="#666666">: int</font>) -> <font color="#666666">float</font>, <font color="#666666">int</font></tt></td><td class="a">(see read_int64_le)</td>
848: 
849: </tr>
850: </table></td></tr>
851: <tr class="a" valign=top><td><h3>flatbuffers</h3></td></tr>
852: <tr><td><table class="a" border=1 cellspacing=0 cellpadding=4><tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_int64</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>, def<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">reads a flatbuffers field from a string at table location tablei, field vtable offset vo, and default value def. The value must be within bounds of the string. Returns the value (or default if the field was not present)</td>
853: 
854: </tr>
855: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_int32</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>, def<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">(see flatbuffers.field_int64)</td>
856: 
857: </tr>
858: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_int16</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>, def<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">(see flatbuffers.field_int64)</td>
859: 
860: </tr>
861: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_int8</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>, def<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">(see flatbuffers.field_int64)</td>
862: 
863: </tr>
864: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_uint64</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>, def<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">reads a flatbuffers field from a string at table location tablei, field vtable offset vo, and default value def. The value must be within bounds of the string. Returns the value (or default if the field was not present)</td>
865: 
866: </tr>
867: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_uint32</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>, def<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">(see flatbuffers.field_int64)</td>
868: 
869: </tr>
870: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_uint16</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>, def<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">(see flatbuffers.field_int64)</td>
871: 
872: </tr>
873: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_uint8</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>, def<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">(see flatbuffers.field_int64)</td>
874: 
875: </tr>
876: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_float64</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>, def<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">(see flatbuffers.field_int64)</td>
877: 
878: </tr>
879: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_float32</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>, def<font color="#666666">: float</font>) -> <font color="#666666">float</font></tt></td><td class="a">(see flatbuffers.field_int64)</td>
880: 
881: </tr>
882: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_string</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">reads a flatbuffer string field, returns "" if not present</td>
883: 
884: </tr>
885: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_vector_len</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">reads a flatbuffer vector field length, or 0 if not present</td>
886: 
887: </tr>
888: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_vector</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">returns a flatbuffer vector field element start, or 0 if not present</td>
889: 
890: </tr>
891: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_table</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">returns a flatbuffer table field start, or 0 if not present</td>
892: 
893: </tr>
894: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_struct</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">returns a flatbuffer struct field start, or 0 if not present</td>
895: 
896: </tr>
897: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.field_present</b>(string<font color="#666666">: string</font>, tablei<font color="#666666">: int</font>, vo<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">returns if a flatbuffer field is present (unequal to default)</td>
898: 
899: </tr>
900: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.indirect</b>(string<font color="#666666">: string</font>, index<font color="#666666">: int</font>) -> <font color="#666666">int</font></tt></td><td class="a">returns a flatbuffer offset at index relative to itself</td>
901: 
902: </tr>
903: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.string</b>(string<font color="#666666">: string</font>, index<font color="#666666">: int</font>) -> <font color="#666666">string</font></tt></td><td class="a">returns a flatbuffer string whose offset is at given index</td>
904: 
905: </tr>
906: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.binary_to_json</b>(schemas<font color="#666666">: string</font>, binary<font color="#666666">: string</font>, includedirs<font color="#666666">: [string]</font>) -> <font color="#666666">string</font>, <font color="#666666">string?</font></tt></td><td class="a">returns a JSON string generated from the given binary and corresponding schema.if there was an error parsing the schema, the error will be in the second returnvalue, or nil for no error</td>
907: 
908: </tr>
909: <tr class="a" valign=top><td class="a"><tt><b>flatbuffers.json_to_binary</b>(schema<font color="#666666">: string</font>, json<font color="#666666">: string</font>, includedirs<font color="#666666">: [string]</font>) -> <font color="#666666">string</font>, <font color="#666666">string?</font></tt></td><td class="a">returns a binary flatbuffer generated from the given json and corresponding schema.if there was an error parsing the schema, the error will be in the second returnvalue, or nil for no error</td>
910: 
911: </tr>
912: </table></td></tr>
913: <tr class="a" valign=top><td><h3>parsedata</h3></td></tr>
914: <tr><td><table class="a" border=1 cellspacing=0 cellpadding=4><tr class="a" valign=top><td class="a"><tt><b>parse_data</b>(typeid<font color="#666666">: typeid(any)</font>, stringdata<font color="#666666">: string</font>) -> <font color="#666666">any?</font>, <font color="#666666">string?</font></tt></td><td class="a">parses a string containing a data structure in lobster syntax (what you get if you convert an arbitrary data structure to a string) back into a data structure. supports int/float/string/vector and classes. classes will be forced to be compatible with their  current definitions, i.e. too many elements will be truncated, missing elements will be set to 0/nil if possible. useful for simple file formats. returns the value and an error string as second return value (or nil if no error)</td>
915: 
916: </tr>
917: <tr class="a" valign=top><td class="a"><tt><b>flexbuffers_value_to_binary</b>(val<font color="#666666">: any</font>, max_nesting<font color="#666666">: int</font> = 0, cycle_detection<font color="#666666">: bool</font> = false) -> <font color="#666666">string</font></tt></td><td class="a">turns any reference value into a flexbuffer. max_nesting defaults to 100. cycle_detection is by default off (expensive)</td>
918: 
919: </tr>
920: <tr class="a" valign=top><td class="a"><tt><b>flexbuffers_binary_to_value</b>(typeid<font color="#666666">: typeid(any)</font>, flex<font color="#666666">: string</font>) -> <font color="#666666">any?</font>, <font color="#666666">string?</font></tt></td><td class="a">turns a flexbuffer into a value</td>
921: 
922: </tr>
923: <tr class="a" valign=top><td class="a"><tt><b>flexbuffers_binary_to_json</b>(flex<font color="#666666">: string</font>, field_quotes<font color="#666666">: bool</font>, indent_string<font color="#666666">: string</font>) -> <font color="#666666">string?</font>, <font color="#666666">string?</font></tt></td><td class="a">turns a flexbuffer into a JSON string. If indent_string is empty, will be a single line string</td>
924: 
925: </tr>
926: <tr class="a" valign=top><td class="a"><tt><b>flexbuffers_json_to_binary</b>(json<font color="#666666">: string</font>, filename_for_errors<font color="#666666">: string</font> = nil) -> <font color="#666666">string</font>, <font color="#666666">string?</font></tt></td><td class="a">turns a JSON string into a flexbuffer, second value is error, if any</td>
927: 
928: </tr>
929: <tr class="a" valign=top><td class="a"><tt><b>lobster_value_to_binary</b>(val<font color="#666666">: any</font>) -> <font color="#666666">string</font></tt></td><td class="a">turns any reference value into a binary using a fast &amp; compact Lobster native serialization format. this is intended for threads/networking, not for storage (since it is not readable by other languages). data structures participating must have been marked by attribute serializable. does not provide protection against cycles, use flexbuffers if that is a concern. </td>
930: 
931: </tr>
932: <tr class="a" valign=top><td class="a"><tt><b>lobster_binary_to_value</b>(typeid<font color="#666666">: typeid(any)</font>, bin<font color="#666666">: string</font>) -> <font color="#666666">any?</font>, <font color="#666666">string?</font></tt></td><td class="a">turns binary created by lobster_value_to_binary back into a value</td>
933: 
934: </tr>
935: </table></td></tr>
936: <tr class="a" valign=top><td><h3>matrix</h3></td></tr>
937: <tr><td><table class="a" border=1 cellspacing=0 cellpadding=4><tr class="a" valign=top><td class="a"><tt><b>matrix.multiply</b>(a<font color="#666666">: [float]</font>, b<font color="#666666">: [float]</font>) -> <font color="#666666">[float]</font></tt></td><td class="a">input matrices must be 4x4 elements</td>
938: 
939: </tr>
940: <tr class="a" valign=top><td class="a"><tt><b>matrix.rotate_x</b>(angle<font color="#666666">: float2</font>) -> <font color="#666666">[float]</font></tt></td><td class="a"></td>
941: 
942: </tr>
943: <tr class="a" valign=top><td class="a"><tt><b>matrix.rotate_y</b>(angle<font color="#666666">: float2</font>) -> <font color="#666666">[float]</font></tt></td><td class="a"></td>
944: 
945: </tr>
946: <tr class="a" valign=top><td class="a"><tt><b>matrix.rotate_z</b>(angle<font color="#666666">: float2</font>) -> <font color="#666666">[float]</font></tt></td><td class="a"></td>
947: 
948: </tr>
949: <tr class="a" valign=top><td class="a"><tt><b>matrix.translation</b>(trans<font color="#666666">: float3</font>) -> <font color="#666666">[float]</font></tt></td><td class="a"></td>
950: 
951: </tr>
952: </table></td></tr>
953: 
954: </table></center></body>
955: </html>

================
File: .clang-format
================
 1: ---
 2: BasedOnStyle: Google
 3: ---
 4: Language: Cpp
 5: IndentWidth: 4
 6: ColumnLimit: 100
 7: UseTab: Never
 8: AccessModifierOffset: 0
 9: AlignTrailingComments: true
10: AllowShortBlocksOnASingleLine: true
11: AllowShortCaseLabelsOnASingleLine: true
12: AllowShortFunctionsOnASingleLine : All
13: AllowShortLoopsOnASingleLine: true
14: BinPackParameters: true
15: ConstructorInitializerAllOnOneLineOrOnePerLine: true
16: IndentCaseLabels: true
17: NamespaceIndentation: None
18: PointerAlignment: Right
19: SpaceBeforeParens: ControlStatements
20: SpaceAfterTemplateKeyword: false
21: Standard: Cpp11
22: Cpp11BracedListStyle: true
23: SpaceBeforeCpp11BracedList: true
24: IndentPPDirectives: BeforeHash
25: AlwaysBreakTemplateDeclarations: false

================
File: .gitattributes
================
1: * text=auto

================
File: CMakeLists.txt
================
  1: cmake_minimum_required(VERSION 3.25)
  2: 
  3: ### Project
  4: 
  5: set(TREESHEETS_VERSION "" CACHE STRING "Version string (e.g., 1.2.3). Leave empty for auto-timestamp.")
  6: 
  7: if("${TREESHEETS_VERSION}" STREQUAL "")
  8:     string(TIMESTAMP TREESHEETS_VERSION "%y%m%d.%H%M" UTC)
  9:     message(STATUS "Auto-generated version: ${TREESHEETS_VERSION}")
 10: else()
 11:     message(STATUS "User-defined version: ${TREESHEETS_VERSION}")
 12: endif()
 13: 
 14: project(TreeSheets
 15:     DESCRIPTION "A free-form hierarchical data organizer"
 16:     HOMEPAGE_URL "https://github.com/aardappel/treesheets"
 17:     VERSION "${TREESHEETS_VERSION}")
 18: 
 19: ### Settings
 20: 
 21: set(CMAKE_CXX_STANDARD 20)
 22: set(CMAKE_EXPORT_COMPILE_COMMANDS 1)
 23: OPTION(ENABLE_LOBSTER "Enable Lobster scripting" ON)
 24: option(ENABLE_IPO "Enable interprocedural/link time optimization" ON)
 25: if(ENABLE_IPO)
 26:     include(CheckIPOSupported)
 27:     check_ipo_supported(RESULT iporesult OUTPUT output)
 28:     if(iporesult)
 29:         set(CMAKE_INTERPROCEDURAL_OPTIMIZATION TRUE)
 30:     else()
 31:         message(WARNING "IPO is not supported: ${output}")
 32:     endif()
 33: endif()
 34: 
 35: ## Compiler-specific
 36: 
 37: # Use statically-linked libraries with MSVC
 38: if(MSVC)
 39:     set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
 40:     set(wxBUILD_USE_STATIC_RUNTIME ON CACHE BOOL "" FORCE)
 41:     set(wxBUILD_SHARED OFF CACHE BOOL "" FORCE)
 42: endif(MSVC)
 43: 
 44: # Silence warnings in GCC that contain lots of false positives
 45: if(CMAKE_COMPILER_IS_GNUCXX)
 46:     set(CMAKE_CXX_FLAGS
 47:     "${CMAKE_CXX_FLAGS} -Wno-array-bounds -Wno-stringop-overflow -Wno-maybe-uninitialized")
 48: endif()
 49: 
 50: ### Thirdparty dependencies
 51: 
 52: # Set wxWidgets options to exclude unused libraries
 53: 
 54: set(wxUSE_WEBVIEW FALSE)
 55: set(wxUSE_STC FALSE)
 56: set(wxUSE_RIBBON FALSE)
 57: set(wxUSE_PROPGRID FALSE)
 58: set(wxUSE_RICHTEXT FALSE)
 59: set(wxUSE_MEDIACTRL FALSE)
 60: if (NOT wxBUILD_SHARED)
 61:     set(wxUSE_HTML FALSE)
 62:     set(wxUSE_WXHTML_HELP FALSE)
 63: endif()
 64: 
 65: # Include dependencies
 66: 
 67: include(FetchContent)
 68: 
 69: FetchContent_Declare(
 70:     wxwidgets
 71:     URL https://github.com/wxWidgets/wxWidgets/releases/download/v3.3.2/wxWidgets-3.3.2.tar.bz2
 72:     URL_HASH SHA256=50a28cb668de47b0e006cd6ebed8cf4f76c1cac6116fb3c978c44478219103f2
 73:     FIND_PACKAGE_ARGS 3.3.2 NAMES wxWidgets
 74: )
 75: FetchContent_MakeAvailable(wxwidgets)
 76: 
 77: if(ENABLE_LOBSTER)
 78:     FetchContent_Declare(
 79:         lobster
 80:         URL https://github.com/aardappel/lobster/archive/refs/tags/v2026.1.tar.gz
 81:         URL_HASH SHA256=fecd443c8bf03052f0eca3308bb8428e1f40292d61fb370e060e55094f853d67
 82:     )
 83:     FetchContent_MakeAvailable(lobster)
 84: endif()
 85: 
 86: ### Options
 87: 
 88: ## Run clang-tidy linter
 89: 
 90: OPTION(WITH_CLANG_TIDY "Run clang-tidy linter" OFF)
 91: if (WITH_CLANG_TIDY)
 92:     set(CMAKE_CXX_CLANG_TIDY clang-tidy -checks=cppcoreguidelines-*,clang-analyzer-*,readability-*,performance-*,portability-*,concurrency-*,modernize-*)
 93: endif()
 94: 
 95: ### Libraries (lobster, lobster-impl)
 96: 
 97: ## lobster (script interpreter)
 98: if(ENABLE_LOBSTER)
 99:     add_library(lobster STATIC
100:         ${lobster_SOURCE_DIR}/dev/external/flatbuffers/src/idl_gen_text.cpp
101:         ${lobster_SOURCE_DIR}/dev/external/flatbuffers/src/idl_parser.cpp
102:         ${lobster_SOURCE_DIR}/dev/external/flatbuffers/src/util.cpp
103:         ${lobster_SOURCE_DIR}/dev/src/builtins.cpp
104:         ${lobster_SOURCE_DIR}/dev/src/compiler.cpp
105:         ${lobster_SOURCE_DIR}/dev/src/file.cpp
106:         ${lobster_SOURCE_DIR}/dev/src/lobsterreader.cpp
107:         ${lobster_SOURCE_DIR}/dev/src/platform.cpp
108:         ${lobster_SOURCE_DIR}/dev/src/vm.cpp
109:         ${lobster_SOURCE_DIR}/dev/src/vmdata.cpp
110:         ${lobster_SOURCE_DIR}/dev/src/tccbind.cpp
111:         ${lobster_SOURCE_DIR}/dev/external/libtcc/libtcc.c)
112:     if(WIN32)
113:         target_sources(lobster PRIVATE
114:             ${lobster_SOURCE_DIR}/dev/include/StackWalker/StackWalker.cpp
115:             ${lobster_SOURCE_DIR}/dev/include/StackWalker/StackWalkerHelpers.cpp)
116:     endif(WIN32)
117:     target_include_directories(lobster PUBLIC
118:         ${lobster_SOURCE_DIR}/dev/src
119:         ${lobster_SOURCE_DIR}/dev/include
120:         ${lobster_SOURCE_DIR}/dev/external
121:         ${lobster_SOURCE_DIR}/dev/external/libtcc)
122: 
123:     ## lobster-impl (provider of TreeSheets functions in lobster)
124: 
125:     add_library(lobster-impl STATIC src/lobster_impl.cpp)
126:     target_link_libraries(lobster-impl PRIVATE lobster)
127: endif(ENABLE_LOBSTER)
128: 
129: ### TreeSheets executable
130: 
131: add_executable(TreeSheets
132:     src/main.cpp
133:     src/stdafx.cpp)
134: 
135: target_compile_definitions(TreeSheets PRIVATE "PACKAGE_VERSION=\"${TREESHEETS_VERSION}\"")
136: if(ENABLE_LOBSTER)
137:     target_compile_definitions(TreeSheets PRIVATE "ENABLE_LOBSTER=1")
138: endif(ENABLE_LOBSTER)
139: 
140: if(APPLE)
141:     set_target_properties(TreeSheets PROPERTIES
142:         MACOSX_BUNDLE TRUE
143:         MACOSX_BUNDLE_BUNDLE_NAME "${CMAKE_PROJECT_NAME}"
144:         MACOSX_BUNDLE_BUNDLE_VERSION "${CMAKE_PROJECT_VERSION}"
145:         MACOSX_BUNDLE_COPYRIGHT "Copyright © 2025 Wouter van Oortmerssen and Tobias Predel. All rights reserved."
146:         MACOSX_BUNDLE_GUI_IDENTIFIER "com.strlen.TreeSheets"
147:         MACOSX_BUNDLE_ICON_FILE "App.icns"
148:         MACOSX_BUNDLE_INFO_PLIST "${CMAKE_SOURCE_DIR}/platform/osx/Info.plist"
149:     )
150: elseif(WIN32)
151:     target_sources(TreeSheets PRIVATE
152:         platform/win/treesheets.rc)
153:     set_target_properties(TreeSheets PROPERTIES
154:         WIN32_EXECUTABLE TRUE
155:     )
156: endif()
157: 
158: target_precompile_headers(TreeSheets PUBLIC src/stdafx.h)
159: 
160: ## Link wxWidgets and lobster-impl into TreeSheets
161: if(ENABLE_LOBSTER)
162:     target_link_libraries(TreeSheets PRIVATE lobster-impl)
163: endif(ENABLE_LOBSTER)
164: target_link_libraries(TreeSheets PRIVATE wx::aui wx::adv wx::core wx::xml wx::net)
165: 
166: ### Installation
167: 
168: ## Platform specific installation paths
169: 
170: if(LINUX OR BSD)
171:     OPTION(TREESHEETS_RELOCATABLE_INSTALLATION "Install data relative to the TreeSheets binary, instead of respecting the Filesystem Hierarchy Standard" OFF)
172: endif()
173: 
174: if((LINUX OR BSD) AND NOT TREESHEETS_RELOCATABLE_INSTALLATION)
175:     include(GNUInstallDirs)
176: 
177:     set(TREESHEETS_BINDIR ${CMAKE_INSTALL_BINDIR})
178:     set(TREESHEETS_DOCDIR ${CMAKE_INSTALL_DOCDIR})
179:     set(TREESHEETS_FULL_DOCDIR ${CMAKE_INSTALL_FULL_DOCDIR})
180:     set(TREESHEETS_PKGDATADIR ${CMAKE_INSTALL_DATADIR}/${CMAKE_PROJECT_NAME})
181:     set(TREESHEETS_FULL_PKGDATADIR ${CMAKE_INSTALL_FULL_DATADIR}/${CMAKE_PROJECT_NAME})
182: 
183:     # Convert relative to absolute paths because only absolute paths are looked up on Linux (and BSD)
184:     target_compile_definitions(TreeSheets PRIVATE
185:         "LOCALEDIR=L\"${CMAKE_INSTALL_FULL_LOCALEDIR}\""
186:         "TREESHEETS_DOCDIR=\"${TREESHEETS_FULL_DOCDIR}\""
187:         "TREESHEETS_DATADIR=\"${TREESHEETS_FULL_PKGDATADIR}\""
188:     )
189: 
190:     # Adapt the AppStream metainfo to release version and date
191:     string(TIMESTAMP TREESHEETS_RELEASE_DATE "%Y-%m-%d")
192:     configure_file(
193:         "${CMAKE_CURRENT_SOURCE_DIR}/platform/linux/com.strlen.TreeSheets.metainfo.xml.in"
194:         "${CMAKE_CURRENT_BINARY_DIR}/com.strlen.TreeSheets.metainfo.xml"
195:         @ONLY
196:     )
197: 
198:     install(FILES platform/linux/com.strlen.TreeSheets.svg DESTINATION ${CMAKE_INSTALL_DATADIR}/icons/hicolor/scalable/apps)
199:     install(FILES platform/linux/com.strlen.TreeSheets.desktop DESTINATION ${CMAKE_INSTALL_DATADIR}/applications)
200:     install(FILES platform/linux/com.strlen.TreeSheets.xml DESTINATION ${CMAKE_INSTALL_DATADIR}/mime/packages)
201:     install(FILES "${CMAKE_CURRENT_BINARY_DIR}/com.strlen.TreeSheets.metainfo.xml" DESTINATION ${CMAKE_INSTALL_DATADIR}/metainfo)
202: elseif(APPLE)
203:     # Paths must be relative to use with CPack
204:     set(TREESHEETS_BINDIR .)
205:     set(TREESHEETS_DOCDIR TreeSheets.app/Contents/Resources)
206:     set(TREESHEETS_PKGDATADIR TreeSheets.app/Contents/Resources)
207: else()
208:     set(TREESHEETS_BINDIR .)
209:     set(TREESHEETS_DOCDIR .)
210:     set(TREESHEETS_PKGDATADIR .)
211: endif()
212: 
213: ## Installation
214: 
215: install(TARGETS TreeSheets DESTINATION ${TREESHEETS_BINDIR})
216: install(DIRECTORY TS/docs DESTINATION ${TREESHEETS_DOCDIR})
217: file(GLOB treesheets_readme_files "TS/readme*.html")
218: install(FILES ${treesheets_readme_files} DESTINATION ${TREESHEETS_DOCDIR})
219: install(DIRECTORY TS/examples DESTINATION ${TREESHEETS_DOCDIR})
220: 
221: install(DIRECTORY TS/images DESTINATION ${TREESHEETS_PKGDATADIR})
222: install(DIRECTORY TS/scripts DESTINATION ${TREESHEETS_PKGDATADIR})
223: if(ENABLE_LOBSTER)
224:     set(lobster_modules
225:         ${lobster_SOURCE_DIR}/modules/std.lobster
226:         ${lobster_SOURCE_DIR}/modules/stdtype.lobster
227:         ${lobster_SOURCE_DIR}/modules/vec.lobster
228:         ${lobster_SOURCE_DIR}/modules/color.lobster
229:     )
230:     install(FILES ${lobster_modules} DESTINATION ${TREESHEETS_PKGDATADIR}/scripts/modules)
231: endif(ENABLE_LOBSTER)
232: 
233: ## Apple icon set
234: if(APPLE)
235:     install(
236:         FILES "platform/osx/App.icns"
237:         DESTINATION "TreeSheets.app/Contents/Resources"
238:     )
239: endif()
240: 
241: ## Localization
242: 
243: # Install translations to correct platform-specific path.
244: # See: https://docs.wxwidgets.org/trunk/overview_i18n.html#overview_i18n_mofiles
245: file(GLOB mo_files "${CMAKE_CURRENT_SOURCE_DIR}/TS/translations/*/ts.mo")
246: foreach(mo_file IN LISTS mo_files)
247:     cmake_path(GET mo_file PARENT_PATH locale_dir)
248:     cmake_path(GET locale_dir FILENAME locale)
249: 
250:     if(WIN32 OR TREESHEETS_RELOCATABLE_INSTALLATION)
251:         # Paths must be relative to use with CPack
252:         install(FILES "${mo_file}" DESTINATION "translations/${locale}")
253:     elseif(APPLE)
254:         # Paths must be relative to use with CPack
255:         install(FILES "${mo_file}" DESTINATION "TreeSheets.app/Contents/Resources/translations/${locale}.lproj")
256:     else()
257:         # Falling back to GNU scheme
258:         install(FILES "${mo_file}" DESTINATION "${CMAKE_INSTALL_LOCALEDIR}/${locale}/LC_MESSAGES")
259:     endif()
260: endforeach()
261: 
262: ### Packaging with CPack
263: 
264: set(CPACK_PACKAGE_VENDOR "Wouter van Oortmerssen")
265: if(APPLE)
266:     set(CPACK_GENERATOR "DragNDrop")
267: elseif(WIN32)
268:     set(CPACK_GENERATOR "INNOSETUP" "ZIP")
269:     set(CPACK_PACKAGE_INSTALL_DIRECTORY ${CMAKE_PROJECT_NAME})
270:     set(CPACK_PACKAGE_EXECUTABLES "TreeSheets" "TreeSheets")
271:     if(CMAKE_VS_PLATFORM_NAME STREQUAL "ARM64" OR CMAKE_SYSTEM_PROCESSOR MATCHES "ARM64|arm64")
272:         set(CPACK_INNOSETUP_SETUP_ArchitecturesAllowed "arm64")
273:         set(ARCH_NAME "winarm64")
274:     else()
275:         set(CPACK_INNOSETUP_SETUP_ArchitecturesAllowed "x64compatible")
276:         set(ARCH_NAME "winx64")
277:     endif()
278:     set(CPACK_PACKAGE_FILE_NAME "${CMAKE_PROJECT_NAME}-${PROJECT_VERSION}-${ARCH_NAME}")
279:     set(CPACK_INNOSETUP_RUN_EXECUTABLES "TreeSheets")
280:     set(CPACK_INNOSETUP_PACKAGE_NAME ${CMAKE_PROJECT_NAME})
281:     set(CPACK_INNOSETUP_LANGUAGES
282:         "brazilianPortuguese" "english" "french" "german" 
283:         "italian" "korean" "russian"
284:     )
285:     set(CPACK_INNOSETUP_SETUP_PrivilegesRequired "lowest")
286:     set(CPACK_INNOSETUP_IGNORE_README_PAGE ON)
287:     set(CPACK_INNOSETUP_IGNORE_LICENSE_PAGE ON)
288:     set(CPACK_INNOSETUP_USE_MODERN_WIZARD ON)
289: else()
290:     set(CPACK_GENERATOR "DEB")
291:     set(CPACK_DEBIAN_PACKAGE_SECTION "contrib/text")
292:     set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Wouter van Oortmerssen")
293:     set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
294:     set(CPACK_DEBIAN_FILE_NAME DEB-DEFAULT)
295:     set(CPACK_DEBIAN_PACKAGE_EPOCH 2)
296: endif()
297: include(CPack)





================================================================
End of Codebase
================================================================
