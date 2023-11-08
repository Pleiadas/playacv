# PlayaCV, Plug and play LaTeX Application class
v1.1 (5 Nov 2023), by Martin Knöfel (martin.knoefel@gmail.com)
* changed `\cvref` to be aligned to the left
* lacking spacing compensation for missing fields added

v1.0 (3 Nov 2023)
* Added cv setting to enable quick content selection to include elements
  * `\newcommand{\cvsetting}[3][]{\ifnum #2=1 #3 \else #1\fi}`
  e.g.
  * `\cvsetting[\myOtherStuff]{1}{\myStuff}`
  * If ]{1}{ the output is `\myStuff` 
  else `\myOtherStuff` is the output if e.g. ]{9}{.
  * With just `\cvsetting{9}{\myStuff}` the output is empty.
* Many commands now support selective highlighting thanks to cvsetting.
  * e.g.`\cvsetting[\cvtag{Tag}]{\UserTwo}{\cvtag[1]{Tag}}`
    renders a highlighted Tag if `\UserTwo` = 1,
    or a regular Tag if `\UserTwo` = 9 (or another number)
  * used with
    `\cvitems[3]{}{}{}{}{},
	\cventry[9]{}{}{}{},
	\cvevent[1]{}{}{}{},
	\cvachievement[]{}{}{},
	\cvskill[]{}{},
	\cvtag[]{},
    \cvref[]{}{}{}{}{}{}{}{}`
    * If [9] the option is deactivated
    * If [1..5] (only \cvitems) the 1..5th item is highlighted
    * If [1] Text is highlighted (format depends on user settings)
  * you can just use the commands without []
* Added application letter option with own header, footer and geometry
* Added watermark to headers and footers using
  `\RequirePackage{transparent}`
* Added warning suppression for item indentation (see [!indent] in playacv.cls) using
  `\RequirePackage{silence}`
* Added support for including pdf files e.g. as appendix using
  `\RequirePackage{pdfpages}`
* Preferential two colors sample with RuleColor for header/heading rules and SymbolColor for accentuated elements
* Improved file structure with options to hide or highlight any element keeping everything else intact
  
## How to use
1) Go to cv.tex and setup all your default cv information. Be careful about special characters. 
2) (recommended to) copy the template.tex contect into a file with a new name e.g. name.tex.
3) Compile and enjoy!
4) Go through your options in cv.tex to setup the chapters and default display options
5) From now on, you're set: just copy name.tex into name2.tex, change/highlight something and there is your new unique cv

## Requirements and Compilation

* pdflatex + biber + pdflatex
* PlayaCV uses [`fontawesome5`](http://www.ctan.org/pkg/fontawesome5).
* Use the `normalphoto` option to get normal (i.e. non-circular) photos.
* Separate your image filenames with commas _without_ spaces if given the option.
* Use the `ragged2e` option to activate hyphenations while keeping text left-justified; line endings will thus be less jagged and more aesthetically pleasing.
* As of altacv v1.3.1 the `withhyper` document class option will make the "personal info" fields into clickable hyperlinks (where it makes sense). See below for more details.
* Can now be compiled with pdflatex, XeLaTeX and LuaLaTeX!
  * Note that to compile with XeLaTeX, you should use a command line as follows, per [the `pdfx` documentation](http://mirrors.ctan.org/macros/latex/contrib/pdfx/pdfx.pdf): `xelatex -shell-escape -output-driver="xdvipdfmx -z 0" sample.tex`
* The samples here use the [Lato](http://www.latofonts.com/lato-free-fonts/) and [Roboto Slab fonts](https://github.com/googlefonts/robotoslab). Feel free to use a different typeface package instead—often a different typeface will change the entire CV's feel.

## Sample
In sample.pdf you can see the full display of options available in fictionary résumé, template.tex provides a standard cv template to start your own.
* sample.tex source:
```latex
% sample.tex v1.0 playacv full standardised options cv file
% sample.pdf output is in appendix folder
% Remove the "normalphoto" option if you want an image cropped to a circle instead of a square in the header
% \documentclass[10pt,a4paper,withhyper,normalphoto]{playacv} % or add document options, e.g. withhyper for hidden urls

\documentclass[11pt, a4paper, withhyper, ragged2e]{playacv}
%
% packages for sample dummy content and documentation
\usepackage{lipsum}
\usepackage{verbatim}

% Application settings begin here %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% application specific cv document(s), it is recommended to copy this file to compile a new application

%%% your/recipient corporate identity touch                                                                    #0
\definecolor{RuleColor}{HTML}{16CC91} % changes lines under chapter titles, e.g. CI colour of recipient e1e3e7
\definecolor{SymbolColor}{HTML}{e93333} % changes symbols and rated skills colour
\definecolor{HighlightColor}{HTML}{174591} % changes highlights text colour

\renewcommand{\itemmarker}{{\footnotesize\faStar}}
\renewcommand{\ratingmarker}{\faHeart}

% default cv data
\input{cv}

% change these in your cv.tex file, used here to showcase function in sample pdf
\renewcommand{\itemmarker}{{\tiny\faStar}}
\renewcommand{\ratingmarker}{\faHeart}
%
\begin{document}
	
	% letter and pages settings
	\newcommand{\OnePager}{9}
	
	% change the letter page layout if you need to
	\newgeometry{left=1.5cm,right=2cm,top=1.5cm,bottom=1.5cm,columnsep=0.3cm}
	
	% optional pages settings (999 = only CV)                                                                    #(4)
	\renewcommand{\OnlyCoverLetter}{9} % set to 1 to print only letter to PDF
	\renewcommand{\WithCoverLetter}{1} % set to 1 to print letter + CV to PDF
	\renewcommand{\WithAppendix}{1} % set to 1 to print appendix pages to PDF
	
	\renewcommand{\myName}{Corinna Vitalium}
	\renewcommand{\myMail}{cv@pla.ya}
	\renewcommand{\myPhone}{+987 654 3210-12}
	\renewcommand{\myStreet}{Unicorn street 1}
	\renewcommand{\myZIP}{40132}
	\renewcommand{\myCity}{City}
	\renewcommand{\myState}{Earth}
	\renewcommand{\myAddress}{\myStreet, \myZIP{} \myCity, \myState}
	\renewcommand{\myLinkedin}{my-linkedin}
	% you can add an address link here and or other urls in the cv, but don't forget to escape or encode the right characters!
	\renewcommand{\myAddressURL}{https://www.openstreetmap.org/\#map=5/69/42}
	
	% recipient information
	\renewcommand{\RecipientName}{Sabrina Hiring} % name (e.g. "Sabrina Hiring")
	\renewcommand{\Company}{Microsoft Corporation} % company (e.g. "Microsoft Corporation")
	\renewcommand{\CompanyStreet}{1 Microsoft Way} % company's street address (e.g. "1 Microsoft Way")
	\renewcommand{\CompanyCity}{Redmond} % company's city (e.g. "Redmond")
	\renewcommand{\CompanyState}{WA} % company's state prefix (e.g. "WA")
	\renewcommand{\CompanyZip}{98052} % company's zip code (e.g. "98052")
	
	% application letter settings
	\renewcommand{\Subject}{Application for dream job 12345} % company (e.g. "Microsoft Corporation")
	\renewcommand{\HeaderWidthR}{0.25}
	\renewcommand{\bHeaderWatermark}{1.5}
	\renewcommand{\cHeaderWatermark}{1}
	\renewcommand{\dHeaderWatermark}{15.5}
	\renewcommand{\eHeaderWatermark}{-.69}
	\renewcommand{\FooterPhotoL}{1}
	\renewcommand{\dFooterPhotoL}{1}
	\renewcommand{\FooterWatermark}{1}
	\renewcommand{\cFooterWatermark}{.26}
	
	\renewcommand{\Body}{
		{\small
			\begin{minipage}[!t]{0.725\linewidth}
				{\lipsum[1]}
			\end{minipage}
			\hfill
			\begin{minipage}[!t]{0.25\linewidth}
				\cvbox[flush left] % cvbox text alignment
				{\textbf{Matching skills:}\\
					\medskip
					\cvskill{I can do this\\}{5}
					\cvskill[1]{My passion\\}{4}
					\bigskip
					\cvtag{This soft skill {\color{green}\faCheck}}\\
					\cvtag[1]{Perfect soft skill {\color{HighlightColor}\faGift}}\\
					\cvtag{Teamwork {\color{RuleColor}\faUsers}}\\
				} % cvbox text
				{black} % cvbox text colour
				{LightGrey!5} % cvbox fill colour
				{.98} % cvbox width
			\end{minipage}
			\medskip
		}
		\begin{itemize}
			\item you can change the layout and looks of your letter and cv easily with \textsl{PlayaCV}
			\item set up your own design for headers, footers, page styles and margins, fonts, symbols and colours intuitively
			\item add any cv elements in the cover letter too, if you like: on the right you can see an example with a combination of a \textsl{\bfseries cvbox} containing text, some \textsl{\bfseries cvskill} and some \textsl{\bfseries cvtag}, with some \textcolor{HighlightColor}{\bfseries highlights}
			\item all common résumé sections and contents are included, combined with a simplistic data input form in \textsl{\bfseries cv.tex}
			\item once set up, you only need to change a few lines to compile a taylored application for your desired position
			\item \textsl{\bfseries template.tex} is provided as a starting point for setting up your own applications. Just copy, paste and enjoy!
			\item this \textsl{\bfseries sample.pdf} contains at least one instance of all available cv elements. Its code can be found in \textsl{\bfseries README.md}
		\end{itemize}
		\medskip
		\lipsum[2-3]
		\bigskip
	}
	
	%
	\input{chap/_letter}
	%
	
	%%% CV layout and content settings
	%%% #1: layout, header, hooter, chapter selection and order
	
	% change the cv page layout if you need to
	\newgeometry{left=1cm,right=1.3cm,top=1.5cm,bottom=1.5cm,columnsep=0.3cm}    
	
	%% Change text under name for this application
	\renewcommand{\aHeaderQuote}{A sample Résumé with \LaTeX{} PlayaCV}
	
	% header and footer options
	\renewcommand{\bHeaderWatermark}{3.17}
	\renewcommand{\cHeaderWatermark}{.137}
	\renewcommand{\dHeaderWatermark}{0}
	\renewcommand{\FooterWatermark}{1}
	\renewcommand{\cFooterWatermark}{.69}
	
	
	%
	\input{chap/_cvHeader}
	%
	
	%%% choose what to include and in which order
	% chapters to be displayed on the left side, from top to bottom
	\renewcommand{\LeftColumn}{
		\input{chap/_Summary}
		\input{chap/_PersonalInfo}
		\input{chap/_Trainings}
		\input{chap/_Certifications}
		\input{chap/_Skills}
		\input{chap/_Languages}
		
		% you can force a page break within a column with \newpage
		\newpage
		\input{chap/_Awards}
		\input{chap/_SoftSkills}
		\input{chap/_Projects}
		\input{chap/_References}
		
	}
	
	% chapters to be displayed on the right side, from top to bottom
	\renewcommand{\RightColumn}{
		\input{chap/_Education}
		\input{chap/_Publications}
		\input{chap/_Experience}
		\input{chap/_Volunteering}
		\input{chap/_Hobbies}
		\input{chap/mychapter}
	}
	
	%%% #2 chapters content settings: create your own and include content as you wish for this application.    #2	
	%%% You might want to add paramters you need to change often here. 
	%%%
	%%% Do that by copying the settings defined with \newcommand{cvSetting}... in cv.tex
	%%% and pasting them here. Using \renewcommand they will be overwritten with your new values.
	%%% Also, you can combine it with a \MySetting = 1 to print custom content to the cv only if needed, using 
	%%% \newcommand{\MySetting}{1} % before e.g. a shorter new text for your summary in the Summary chapter, with
	%%% \cvsetting{\MySetting}{
		%%% \renewcommand{\aSummary}{My shorter summary text}
		%%% }
	
	%% Choose chapters for this application
	\renewcommand{\WithAwards}{1}
	\renewcommand{\WithCertifications}{1}
	\renewcommand{\WithHobbies}{1}
	\renewcommand{\WithPersonalInfo}{1}
	\renewcommand{\WithProjects}{1}
	\renewcommand{\WithPublications}{1}
	\renewcommand{\WithReferences}{1}
	\renewcommand{\WithSkills}{1}
	\renewcommand{\WithSoftSkills}{1}
	\renewcommand{\WithSummary}{1}
	\renewcommand{\WithTrainings}{1}
	\renewcommand{\WithSocial}{1}
	\renewcommand{\WithUser}{1}
	
	% change summary
	\renewcommand{\aSummary}{\textsl{ \bfseries This is a short summary on why you are perfect for this position.\\
			\smallskip
			Useful for applications where no cover letter is required}}
	\renewcommand{\cSummary}{RuleColor!10}
	
	%% Change languages for this position
	\renewcommand{\aLanguageOne}{English}
	\renewcommand{\aLanguageTwo}{Chinese}
	\renewcommand{\aLanguageThree}{Russian}
	\renewcommand{\LanguageFour}{1}
	\renewcommand{\LanguageFive}{1}
	\renewcommand{\LanguageSix}{9}
	
	%% Change hard skills for this position
	\renewcommand{\aSkillOne}{Relevant skill}
	\renewcommand{\bSkillOne}{5}
	\renewcommand{\aSkillTwo}{The perfect skill}
	\renewcommand{\bSkillTwo}{4}
	\renewcommand{\aSkillThree}{IT skill}
	\renewcommand{\bSkillThree}{3.5}
	\renewcommand{\aSkillFour}{Another skill}
	%\renewcommand{\bSkillFour}{3.5}
	\renewcommand{\aSkillFive}{I got this}
	\renewcommand{\bSkillFive}{5}
	
	\renewcommand{\SkillSix}{9}
	\renewcommand{\aSkillSix}{6}
	\renewcommand{\bSkillSix}{2.5}
	\renewcommand{\SkillSeven}{1}
	\renewcommand{\aSkillSeven}{\LaTeX}
	\renewcommand{\bSkillSeven}{3}
	
	%% Change soft skills for this position, with a 4:6 column ratio max total of ca. 30 char per line 
	\renewcommand{\SoftSkillOne}{1}
	\renewcommand{\aSoftSkillOne}{33 characters, }
	\renewcommand{\SoftSkillTwo}{1}
	\renewcommand{\aSoftSkillTwo}{27 numbers}
	\renewcommand{\SoftSkillThree}{1}
	\renewcommand{\aSoftSkillThree}{max here}
	\renewcommand{\SoftSkillFour}{1}
	\renewcommand{\aSoftSkillFour}{showcase}
	\renewcommand{\fSoftSkillFive}{1}
	\renewcommand{\aSoftSkillFive}{your soft skills}
	\renewcommand{\SoftSkillSix}{1}
	\renewcommand{\aSoftSkillSix}{for hirers}
	
	\renewcommand{\SocialOne}{1}
	\renewcommand{\SocialTwo}{9}
	\renewcommand{\SocialThree}{1}
	\renewcommand{\SocialFour}{9}
	\renewcommand{\SocialFive}{1}
	
	\renewcommand{\ProjectTwo}{9}
	
	\renewcommand{\TrainingsItems}{\cvlist[\TrainingsIndent]
		{2024}{First aid \faHeartbeat} % item[{x}] {y} #1
		{2020}{Learned to do ..} % item #2
		{}{}
		{}{}
	}
	
	%% Change user specific content settings e.g.
	\newcommand{\PhD}{9}
	
	%% Change experiences content
	
	% exp 1
	\renewcommand{\eExperienceItemsOne}{\cvitems[2] % <- highlight options with \fExp.. = 1..5 or 9
		{Doing this and that} % items {1}..{5}
		{Highlighted task}
		{Also relevant for the application}{}{}}
	
	% exp 2
	%{Minor reviewing of papers for conferences on photonic technologies}
	%{3D modeling for radar simulations for automotive systems research}
	%{Tutoring of bachelor student groups in electronics lab experiments}
	\renewcommand{\eExperienceItemsTwo}{\cvitems[\fExperienceTwo]
		{Masterfully achieved this}
		{}
		{}{}{}}
	
	
	\renewcommand{\EducationOne}{1} % set to 1 to include education entry
	%\renewcommand{\aEducationOne}{} % plain title on the left
	%\renewcommand{\bEducationOne}{} % smaller, bold text on the right
	%\renewcommand{\cEducationOne}{\faMapMarker{} } % location/institution
	%\renewcommand{\dEducationOne}{\faCalendar Oct 2017 -- Nov 2023} % date
	\renewcommand{\eEducationItemOne}{1} % set to 1 to show Items
	%\renewcommand{\eEducationOne}{} % text (above Items)
	\renewcommand{\fEducationOne}{9} % set to 1 to highlight education entry (item)
	\renewcommand{\eEducationItemsOne}{
		\cvitems
		[\fEducationOne] % highlight 1..5 or 9
		{Major 1} % items {1}..{5}
		{Major 2}
		{}
		{}
		{}
	}
	
	\renewcommand{\EducationTwo}{1} % education #2
	
	\renewcommand{\EducationThree}{1} % education #3
	\renewcommand{\aEducationThree}{School of life}
	\renewcommand{\bEducationThree}{Lifetime learner}
	\renewcommand{\cEducationThree}{Somewhere}
	\renewcommand{\dEducationThree}{always -- \today}
	\renewcommand{\eEducationItemThree}{1}
	\renewcommand{\eEducationThree}{}
	\renewcommand{\fEducationThree}{9}
	\renewcommand{\eEducationItemsThree}{\cvitems[\fEducationThree]{Everyone can teach you something}{}{}{}{}}
	
	\renewcommand{\ReferenceOne}{1}
	\renewcommand{\fReferenceTwo}{1}
	
	% highlights or other changes for this position, copy paste to bottom to create semantics short selections %%%
	%%% Every last \renewcommand{\CvSetting}{SettingValue} will overwrite the previous setting                     #3
	
	% highlights
	\renewcommand{\fExperienceOne}{9} % highlight
	\renewcommand{\fSkillTwo}{1}
	\renewcommand{\fSoftSkillThree}{1}
	\renewcommand{\fSocialThree}{9}
	\renewcommand{\fAwardTwo}{1}
	\renewcommand{\fReferenceOne}{9}
	\renewcommand{\fEducationTwo}{9}
	%\renewcommand{\eExperienceItemsTwo}{\cvitems{}{}{}{}}
	
	% appendix settings
	\renewcommand{\aAppendixOne}{A sister (one page pdf/image)} % appendix TITLE
	\renewcommand{\bAppendixOne}{appendix/snow} % appendix file (image, pdf)
	\renewcommand{\cAppendixOne}{0.96} % scaling of pdf pages
	\renewcommand{\dAppendixOne}{1} % change chap/_appendix for multiple pages pdf for appendix #1, uses includegraphics here as it fits one page pdfs under the title
	
	\renewcommand{\WithAppendix}{9}
	\renewcommand{\AppendixTwo}{1} % appendix #2
	\renewcommand{\aAppendixTwo}{Appendix 2 (more pages pdf)}
	\renewcommand{\bAppendixTwo}{appendix/lea}
	\renewcommand{\cAppendixTwo}{0.95}
	\renewcommand{\dAppendixTwo}{2}
	
	% your own cv settings
	
	% setting example
	\cvsetting{\PhD}{
		\renewcommand{\WithPublications}{1}
		\renewcommand{\fPublicationTwo}{1}
	}
	
	% setting to shrink cv to one page
	\cvsetting{\OnePager}{
		\renewcommand{\LeftColumn}{
			%\input{chap/_Summary}
			\input{chap/_Trainings}
			\input{chap/_Awards}
			\input{chap/_Skills}
			\input{chap/_SoftSkills}
			\input{chap/_Languages}
		}
		
		% Choose chapters to be displayed on the right side, from top to bottom
		\renewcommand{\RightColumn}{
			\input{chap/_Education}
			\input{chap/_Experience}
			\input{chap/_Volunteering}
		}
		
		% shorten chapters for one pager cv
		\renewcommand{\dExperienceFour}{Apr 2012 -- Apr 2017}
		\renewcommand{\eExperienceItemTwo}{9}
		\renewcommand{\eExperienceItemThree}{9}
		\renewcommand{\eExperienceItemFour}{1}
		\renewcommand{\eExperienceItemsFour}{\cvitems{something}{}{}{}{}}
		\renewcommand{\eExperienceItemFive}{9}
		\renewcommand{\aExperienceFive}{This job \hfill {\small \bfseries Company name}}
		\renewcommand{\eSocialItemOne}{9}
		\renewcommand{\eSocialItemTwo}{9}
		\renewcommand{\eSocialTwo}{}
		\renewcommand{\eSocialItemThree}{9}
		\renewcommand{\eSocialItemFour}{9}
		
		\renewcommand{\LanguageFour}{1}
		\renewcommand{\LanguageFive}{1}
		\renewcommand{\LanguageSix}{1}
		\renewcommand{\WithFooter}{9}
	}
	
	% End of application settings here %%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
	%
	%% cv body start
	%
	% left/right column width ratio, 0.5 = centred
	\columnratio{\MyColumnRatio}
	%
	%
	% start a 2-column paracol. Both the left and right columns will automatically
	% break across pages if things get too long.
	\begin{paracol}{2}
		
		%% you might want to adjust the vertical alignment of the top of the left column here 
		\vspace{0.0mm}
		%
		\LeftColumn
		%
		% switch to the right column
		\switchcolumn
		
		%% you might want to adjust the vertical alignment of the top of the right column here
		\vspace{0.0mm} 
		%
		\RightColumn
		%
		%% body end 
	\end{paracol}
	% 
	\input{chap/_cvFooter}
	%
	
	% change the appendix page layout if you need to
	\newgeometry{left=1cm,right=1.3cm,top=1.5cm,bottom=1.5cm,columnsep=0.3cm}
	%
	% input appendix
	\input{chap/_appendix}
	%
\end{document}
```

based on:

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

As of v1.3.1, the `withhyper` document class option will load the `hyperref` package, and make fields in the personal detail fields into clickable hyperlinks (where it makes sense anyway).

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
