## Note for API and functinality of font system
### Glyph Definition
* 字形是指为了表达这个意义的具体表达。同一字可以有不同的字形，而不影响其表达的意思，例如拉丁字母第一个字母可以写作a或ɑ，汉字中的“強／强”、“戶／户／戸”。
* In typography, *kerning* is the process of adjusting the spacing between characters in a proportional font, usually to achieve a visually pleasing result.
* A font is a collection of various *character images* that can be used to display or print text.
* The character images are called **glyphs**.
* A font file contains a set of glyphs; each one can be stored as a bitmap, a vector representation, or any other scheme (most scalable formats use a combination of mathematical representation and control data/programs). These glyphs can be stored in any order in the font file, and are typically *accessed through a simple glyph index*.
* The font file contains one or more tables, called character maps (also called ‘charmaps’ or ‘cmaps’ for short), which are used to convert character codes for a given encoding (e.g. ASCII, Unicode, DBCS, Big5, etc.) into glyph indices relative to the font file.
  * FT_Get_Char_Index: unicode char to glyph index
* **Outlines**: scalable representations of glyph images
* pixel_size = point_size * resolution / 72
* proper glyph rendering needs the scaled points to be aligned along the target device pixel grid, through an operation called *grid-fitting* (often called *hinting*).
### Face
* For example, ‘Palatino Regular’ and ‘Palatino Italic’ are two distinct faces from the same family, called ‘Palatino’ itself.
### Data formats
* *Bitmap* fonts consist of a matrix of dots or pixels representing the image of each glyph in each face and size.
* *Outline* fonts (also called vector fonts) use Bézier curves, drawing instructions and mathematical formulae to describe each glyph, which make the character outlines scalable to any size.
* *Stroke* fonts use a series of specified lines and additional information to define the profile, or size and shape of the line in a specific face, which together describe the appearance of the glyph.
### Example
```cpp
class Text
{
  class Line
  {

  };
  class LineProperty
  {
    postion;
    color;
    capitalization;
    vertical_justify;
    horiziontal_justify;
    scale;
    wrap;
  };
  class Font;
  void DrawText(text, position, justication ...)  
  {
    localize(text);
    hanlder(text format);
    do_adjust();
    real_text_draw();
  };
  void handler()
  {
    //for each speacial parameter
      //format them
  };
  void do_adjust()
  {
      gettextwidth();
      gettextheight();
      getsacles();
      justication();
      icons();
  };
  void real_text_draw()
  {
    //for each character
      //draw it with font and scales ....
  };
};
```
