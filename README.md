# PlayaCV, Plug and play LaTeX Application class

v1.0 (3 Nov 2023), by Martin Knöfel (martin.knoefel@gmail.com)

* Added cv setting to enable quick content selection to include elements
  *  `\newcommand{\cvsetting}[3][]{\ifnum #2=1 #3 \else #1\fi}`
  e.g.
  *`\cvsetting[\myOtherStuff]{1}{\myStuff}`
  If ]{1}{ the output is `\myStuff` 
  else `\myOtherStuff` is the output if e.g. ]{9}{.
  * With just `\cvsetting{9}{\myStuff}` the output is empty.
* Many commands now support selective highlighting thanks to cvsetting.
  * e.g.`\cvsetting[\cvtag{Tag}]{\UserTwo}{\cvtag[1]{Tag}}`
    renders a highlighted Tag if `\UserTwo` = 1,
    or a regular Tag if `\UserTwo` = 9 (or another number)
  * used with
    \cvitems[3]{}{}{}{}{},
	\cventry[9]{}{}{}{},
	\cvevent[1]{}{}{}{},
	\cvachievement[]{}{}{},
	\cvskill[]{}{},
	\cvtag[]{},
    \cvref[]{}{}{}{}{}{}{}{}`
    *If [9] the option is deactivated
    *If [1..5] (only \cvitems) the 1..5th item is highlighted
    *If [1] Text is highlighted (format depends on user settings)
  * you can just use the commands without []
* Added application letter option with own header, footer and geometry
* Added watermark to headers and footers using
  `\RequirePackage{transparent}`
* Added warning suppression for item indentation (see [!indent] in playacv.cls) using
  `\RequirePackage{silence}`
* preferential two colors sample with RuleColor for header/heading rules and SymbolColor for accentuated elements
* improved file structure with options to hide or highlight any element keeping everything else intact

# AltaCV, yet another LaTeX CV/Résumé class

v1.6.5 (3 Nov 2022), by LianTze Lim (liantze@gmail.com)

* Added \mynames{...} to specify names to be highlighted in the publication list on 3 Nov 2022
* Starred `\NewInfoField*` command to handle Mastodon; Icons, `\cvskills`, `\wheelchart` have "copyable" text values; `\cvskill` supports numerical values {0.5, 1, ..., 4.5, 5} on 21 May 2021
* Moved `biblatex`-related code to `*.cfg` files for easier edit on 8 May 2021
* Removed dependency on `academicons` on 12 Apr 2021
* Clickable hyperlinked info fields added on 10 May 2020
* Sample file with new `paracol` layout added on 2 February 2020

(Thanks to [Nur](https://github.com/nurh) for the name.)

It all started with this:

[<img src="tweet-that-started-this.png" width="500px">](https://twitter.com/Leonduck/status/764281546408923136)

Leonardo was talking about a [résumé of Marissa Mayer that Business Insider put together](http://www.businessinsider.my/a-sample-resume-for-marissa-mayer-2016-7/) using [enhancv.com](https://enhancv.com).
I _knew_ I had to do something about it. And so AltaCV was born.

## Samples
In sample.tex you can see the full display of options, template.tex provides a standard cv template as you know them.

## Requirements and Compilation

* pdflatex + biber + pdflatex
* PlayaCV uses [`fontawesome5`](http://www.ctan.org/pkg/fontawesome5).
* Use the `normalphoto` option to get normal (i.e. non-circular) photos.
* Separate your image filenames with commas _without_ spaces if given the option.
* Use the `ragged2e` option to activate hyphenations while keeping text left-justified; line endings will thus be less jagged and more aesthetically pleasing.
* As of altacv v1.3 the `withhyper` document class option will make the "personal info" fields into clickable hyperlinks (where it makes sense). See below for more details.
* Can now be compiled with pdflatex, XeLaTeX and LuaLaTeX!
  * Note that to compile with XeLaTeX, you should use a command line as follows, per [the `pdfx` documentation](http://mirrors.ctan.org/macros/latex/contrib/pdfx/pdfx.pdf): `xelatex -shell-escape -output-driver="xdvipdfmx -z 0" sample.tex`
* The samples here use the [Lato](http://www.latofonts.com/lato-free-fonts/) and [Roboto Slab fonts](https://github.com/googlefonts/robotoslab). Feel free to use a different typeface package instead—often a different typeface will change the entire CV's feel.

## `sample.tex` [WAS `sample-alt.tex` 2 FEBRUARY 2020, DEFAULT SINCE 10 MAY 2020] ##
Many users have overlooked the optional argument of `\cvsection` to insert the right sidebar contents, and often confused that the right sidebar doesn't automatically break across pages. This new layout uses the `paracol` package for typesetting the left and right columns that _can_ break across pages. It also makes changing the column widths easier:

```latex
%% Set the left/right column width ratio to 6:4.
\columnratio{0.6}

% Start a 2-column paracol. Both the left and right columns will automatically
% break across pages if things get too long.
\begin{paracol}{2}
\cvsection{Experience}
...
... END OF LEFT COLUMN CONTENTS ...

% Now switch to the right column.
\switchcolumn
\cvsection{Education}
...
...END OF RIGHT COLUMN CONTENTS ...
\end{paracol}
```
You can also use `\swithcolumn*` for "synchronising" the columns, as well as other commands from the `paracol` package. See the [`paracol` package documentation](http://texdoc.net/pkg/paracol) for further details.

**You do not need use the `fullwidth` environment nor use optional arguments with `\cvsection` with this new template.**

## Clickable Info fields

As of v1.3, the `withhyper` document class option will load the `hyperref` package, and make fields in the personal detail fields into clickable hyperlinks (where it makes sense anyway).

*BIG CAVEAT:* Remember that not all readers may want to click on hyperlinks in PDFs. You may therefore sometimes want to _remove_ `withhyper`, and spell out the field URL details a bit more completely, e.g. `\github{github.com/your-id}`.

Anyway assuming that you _do_ keep `withhyper` enabled: For each field e.g. `\homepage{foobar.com}`, a `\homepagesymbol` has been defined, and the clickable hyperlink is generated by prepending the `\homepagehyperprefix` to `foobar.com`. The `\homepgehyperprefix` is defined to be `\https://`, so this generates the hyperlink `https://foobar.com`.

If your homepage doesn't use HTTPS yet, or if you want to use a different symbol, you can re-define them with
```latex
\renewcommand{\homepagehyperprefix}{http://}
\renewcommand{\homepagesymbol}{\faLink}
```


## New Information Fields ####

I've decided against adding definitions for too many fields and symbols in the `.cls` itself; otherwise we'll have all possible platforms in the world (and more services are born everyday!) within `altacv.cls` before we know it.

You can actually just typeset your own arbitrary information fields using the `\printinfo{symbol}{detail}[optional hyperlink prefix]` command within `\personalinfo`:

````latex
\printinfo{\faPaw}{Hey ho!}
\printinfo{\faGitLab}{your-handle}[https://gitlab.com/]
````

Or if you really prefer, you can define a new field yourself with `\NewInfoFiled{fieldname}{symbol}[optional hyperlink prefix]` before  using it:

````latex
\NewInfoField{gitlab}{\faGitlab}[https://gitlab.com/]
\gitlab{your_id}
````

For services and platforms like Mastodon where there isn't a straightforward relation between the more popular user ID or nickname and the hyperlink, you can use `\printinfo` directly e.g.

```latex
\printinfo{\faMastodon}{@username@instace}[https://instance.url/@username]
```

But if you absolutely want to create new dedicated info fields for such platforms, then use `\NewInfoField*` with a star:

```latex
\NewInfoField*{mastodon}{\faMastodon}
```

then you can use `\mastodon` with TWO arguments where the 2nd argument is the full hyperlink.

```latex
\mastodon{@username@instance}{https://instance.url/@username}
```


## Configurable colours

Use `\colorlet` or `\definecolor` to change these.
* `accent`
* `emphasis`
* `heading`
* `headingrule`
* `subheading`
* `body`
* `name`
* `tagline`

## Configurable fonts

Use `\renewcommand` to change these.
* `\namefont`
* `\taglinefont`
* `\personalinfofont`
* `\cvsectionfont`
* `\cvsubsectionfont`

---

## `legacy/sample-old.tex`

This is the original sample template file until 5 May 2020. The right sidebar is actually a _`marginpar`_, so it doesn't support footnote and cannot automatically break across pages if it's too long. You would need to split your right sidebar contents into separate files e.g. `p1sidebar.tex` and `p2sidebar.tex`, and insert them as the optional argument of the `\cvsection{...}` that you want to align them with:

```latex
\cvsection[p1sidebar]{Experience}
...
... END OF FIRST PAGE OF YOUR CV ...
\cvsection[page2sidebar]{Publications}
...
```

This assumes that the next page's main column would start immediately with a `\cvsection`, so that the top of your right sidebar contents also appear at the top of the page. Now if the _next_ page doesn't start with a `\cvsection` but you'd still like to add a sidebar, then use this command on the _current_ page to add it. The optional argument lets you pull up the sidebar a bit so that it looks aligned with the top of the main column:

```latex
\addnextpagesidebar[-1ex]{page3sidebar}
```

If you want to change the left and right columns' widths, you'll need to tinker with the `right` (distance from paper's right edge until the main column's right edge) and `marginparwidth` (width of the right sidebar) options in the `\geometry` line. For example, to make the right sidebar wider by 2cm, you could use

```latex
%% original was right=9cm, marginparwidth=6.8cm
\geometry{left=1cm,right=11cm,marginparwidth=8.8cm,marginparsep=1.2cm,top=1cm,bottom=1cm}
```
as well as doing a bit of arithmetic when you're making the header to get it full-width, i.e. reducing the sidebar by 2cm and extending the main column by 2cm.

```latex
\begin{adjustwidth}{}{-10cm}  %% original was -8cm
\makecvheader
\end{adjustwidth}
```
