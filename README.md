# Programming project

## Group elements

Identify all group elements (numbers and names).

- up202201391 Manoela Blanke Américo
- up202301897 Casemiro Melo Jorge de Medeiros
- up202304412 Pedro Alexandre Henriques Ferreira

---  

# SVG Parser & Shape Interpreter (C++)

This project implements a lightweight **SVG reader/parser in C++** using **tinyxml2**.  
It loads an `.svg` file, reads its canvas dimensions, parses common SVG primitives, applies basic geometric transforms, and converts everything into an internal representation based on custom `SVGElement` classes.

The parsed elements are stored in a `std::vector<SVGElement*>`, enabling downstream operations such as rendering, exporting, analysis, or further geometric processing (depending on the rest of the codebase).

---

## ✅ What It Does

Given an SVG file, the parser:

- Reads the SVG **width** and **height** from the root element
- Iterates through child elements and converts them into C++ objects:
  - `circle` → `Circle`
  - `ellipse` → `Ellipse`
  - `line` → `Line`
  - `polyline` → `Polyline`
  - `polygon` → `Polygon`
  - `rect` → `Rect`
- Extracts key attributes such as:
  - positions (`x`, `y`, `cx`, `cy`, `x1`, `y1`, `x2`, `y2`)
  - sizes (`r`, `rx`, `ry`, `width`, `height`)
  - colors (`fill`, `stroke`) using a `parse_color(...)` helper
- Applies supported transforms when present:
  - `translate(tx ty)` *(and for rectangles also `translate(tx,ty)`)*  
  - `rotate(angle)` with optional `transform-origin`
  - `scale(s)` with optional `transform-origin`  
- Outputs:
  - `dimensions` (canvas size)
  - `svg_elements` (a list of dynamically allocated shape objects)

---

## 🔧 Supported SVG Elements

| SVG Tag     | Internal Class |
|------------|----------------|
| `circle`   | `Circle`       |
| `ellipse`  | `Ellipse`      |
| `line`     | `Line`         |
| `polyline` | `Polyline`     |
| `polygon`  | `Polygon`      |
| `rect`     | `Rect`         |

> Note: Group elements (`<g>...</g>`) appear partially planned (commented out) and are not fully enabled in the current implementation.

---

## 🔁 Supported Transforms

The parser supports basic transforms through the `transform` attribute:

- `translate(tx ty)`  
- `rotate(angle)` *(optionally around `transform-origin`)*  
- `scale(s)` *(optionally around `transform-origin`)*  

Transforms are applied to geometry (points) before creating the corresponding `SVGElement` objects.

---

## 📦 Dependencies

- [tinyxml2](https://github.com/leethomason/tinyxml2) — XML parsing library used to read SVG files  
  (included under `external/tinyxml2/` in this repository)

---

## 🧱 How It Works (High-Level)

The core function is:

- `readSVG(const std::string& svg_file, Point& dimensions, std::vector<SVGElement*>& svg_elements)`

It loads the SVG file with tinyxml2, reads the root attributes (width/height), then iterates through each child element, identifies its type, extracts attributes, applies transforms, and pushes the corresponding shape object into `svg_elements`.

---
