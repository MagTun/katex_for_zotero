# katex_for_zotero
Add latex formula to Zotero notes, based on Katex



## How to get latex formula in Zotero note:
1) download the folder katex_for_zotero from here (you should have a folder and inside these 4 files : katex.min.js, katex.min.css, plugin.css, plugin.js)

2) Close zotero, go to your Zotero installation folder (ex: C:\Program Files (x86)\Zotero), make a backup of zotero.jar (ex: copy it and rename it into zotero-backup.jar), and move zotero.jar (not the backup) to your desktop

3) Open the zotero.jar with a zip manager (ex: e 7-zip). Alternatively, you can also unzip the .jar (this file is just a zip with another extension), but this can create some bugs.

4) In the zip manager, go to resource\tinymce\plugins and drag/drop the katex_for_zotero folder

5) Still in the zip manager, go to resource\tinymce\,  right-click on note.html and select "Edit". This will open the file in a text editor :

5.1) replace `plugins: "autolink`  by `plugins: "katex_for_zotero, autolink`   
5.2) just below the line `<script type="text/javascript" src="locale.js"></script>`, add these two lines:
	`<link type="text/css" rel="stylesheet" href="plugins/katex_for_zotero/katex.min.css" />
	<script type="text/javascript" src="plugins/katex_for_zotero/katex.min.js"></script>`  

6) Close the text editor (if your text editor can open several files at once, you need to close the text editor not just the file), go back to the zip manager and click Ok to update note.html

7) Move the edited zotero.jar back in Zotero installation folder


## How to use it:
1) Open Zotero, open a note, and write a katex formula ex: 
`\tag{A}\fcolorbox{red}{aqua}{$c = \displaystyle\sum_{n=1}^{\infty} \pm \sqrt[3]{(a^2 + b^3)^n} + \prod_{i=a}^{b} f(i)$}`

2) Select the katex formula and press `ALT + K`

3) Now press `F5` to render the katex formula 

3) Each time the note is loaded (ex: after opening zotero, when you open the source code of the note and click ok , when you switch to another note and come back, when you switch from viewing the note in a separate window/directly in zotero...), you need to press `F5`



## More details 
* TinyMCE is the html editor Zotero uses in the note: https://www.tiny.cloud/docs/advanced/creating-a-plugin/
* Katex is a JavaScript library for TeX math rendering license under MIT: https://github.com/KaTeX/KaTeX


### How it works:
Alt+K calls a function that adds a class "katexCode" to the text that is selected.
F5 calls a function that :
- parse the content of the note, 
- send all the elements with the class "katexCode" to the katexkatex.renderToString function, which returns the katex formulas. Each katex formula is added to a new element with the class katexRenderer just after the parent element of its associated katexCode (katexCode is a span and the parent element is a p)
- see also improvements needed #1



### Improvements needed (in random order):
This plugin works but many improvements are needed.
1) To use this plugin, as it is now, one has to  modify the zotero.jar folder which isn't great. 
It's possible to apply the change automatically, for example with a python script like https://forums.zotero.org/discussion/comment/318554/#Comment_318554. But this is still tricky. The best would be to have a Zotero add-on (but I don't know how to do it) or even better, to integrate this feature in Zotero (there are many requests for this so it could be a great improvement, and it doesn't require big changes).
2) Each time the note is loaded, the katex formula style is lost by Zotero because it filters out the html tags katex use:
This is the code katex generates : `<span class="katex"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><semantics><mtable width="100%"><mtr><mtd width="50%"></mtd><mtd><mrow><mi>f</mi><mo stretchy="false">(</mo><mstyle mathcolor="blue"><mi>x</mi><mstyle mathcolor="black"><mo stretchy="false">)</mo><mo>=</mo><mn>3</mn><mo>∗</mo><msup><mi>x</mi><mn>2</mn></msup><mo>∗</mo><mroot><mrow><mn>1</mn><msup><mn>5</mn><mrow><mo stretchy="false">(</mo><mn>5</mn><mstyle mathcolor="red"><mi>s</mi><mo stretchy="false">)</mo></mstyle></mrow></msup><mstyle mathcolor="black"></mstyle></mrow><mn>3</mn></mroot></mstyle></mstyle></mrow></mtd><mtd width="50%"></mtd><mtd><mtext>(A)</mtext></mtd></mtr></mtable><annotation encoding="application/x-tex">\tag{A} f(\color{blue}x\color{black})= 3*x^2*\sqrt[3]{15^ {(5\color{red}s)}\color{black}}</annotation></semantics></math></span>`

This is the code after zotero has filtered out the html tags: `<span class="katex">f(x)=3∗x2∗15(5s)3f(\color{blue}x\color{black})= 3*x^2*\sqrt[3]{15^ {(5\color{red}s)}\color{black}}</span>`

I didn't find a way to fix this. The work around is to use the F5 shortcut to reapply all the styling (it first remove all the katexRenderer elements, then parse the katexCode elements, then recreate the katexRenderer elements) 

3) A better way to display the katex code: the katex formulas have to be kept for several reasons (to recreate the katexRenderer, to be able to correct a typo...) but the display could be improved : maybe the katexCodecould be hidden when the katexRenderer are displayed 

4) The katex js and css should be loaded by the plugin and not by note.html but I didn't find a way to do it.

5) I wasn't able to use Katex auto-render module because the latex formula are removed by the module once it has rendered them and thus Zotero loose the ability to display them after on F5

