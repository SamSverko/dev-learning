[SO - Troubleshooting JSON formatting](https://stackoverflow.com/questions/55010596/troubleshooting-json-formatting/).

## IDE

I've been using the [Atom IDE](https://atom.io/) because it's free, developed by GitHub, has great package add-ons, and supports open source. I used to work with [Sublime](https://www.sublimetext.com/), but didn't want to pay for it and the `upgrade now` prompt got really annoying after a while. I tried playing around with [Visual Code Studio](https://code.visualstudio.com/), but it just seems so bulky for what I need. I'm sure it's great ([considering most developers apparently use it](https://2018.stateofjs.com/other-tools/)), but it's not for me at the moment.

### [Atom packages](https://atom.io/packages)

To install a package on Atom, go to `Settings / Install / then search for the package`.

To manage installed packages, go to `Settings / Packages`

If I can offer any advice when it comes to IDE packages, is that I found adding packages only after you've encountered a particular problem you need fixed, or you realize you are spending way too much time doing something, is the way to go. Basically only add packages that you will use. If you throw on a dozen packages right when you first download Atom, you will be overwhelmed with that each of them can do. Slowly introduce a package and familiarize yourself with each one before overloading the IDE.

A few packages that really help my JavaScript workflow:

#### [atom-beautify](https://atom.io/packages/atom-beautify)
* **Description:** Beautify HTML, CSS, JavaScript, PHP, Python, Ruby, Java, C, C++, C#, Objective-C, CoffeeScript, TypeScript, Coldfusion, SQL, and more in Atom.
* **Usage:** Just right click your editor (no code highlighting needed), and select `Beautify editor contents`. It will then properly format/ auto-indent your json code.


#### [custom-folds](https://atom.io/packages/custom-folds)
* **Description:** Define custom tags for defining foldable blocks of code.
* **Usage:** Insert `<editor-fold>` where you want the fold to begin, and close it with `</editor-fold>`. Works well with super long code where you might need the middle part, and can fold in up the upper and lower sections, just an example.

#### [emmet](https://atom.io/packages/emmet)
* **Description** Emmet – the essential tool for web developers.
* **Usage** It's just an awesome auto-complete tool for so many languages. Here are some great examples for `html` by creating several elements using shorthand code.
* **Example 1:** Let's say you're starting a blank html page, just type in `html` and hit enter. This will expand that to this:
```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>

	</body>
</html>
```
* **Example 2:** Let's say you want to create 3 `divs`, each with the class `game-card`. Simply type in `div.game-card*3` and hit tab. It will generate this:
```html
<div class="game-card"></div>
<div class="game-card"></div>
<div class="game-card"></div>
```
Let's break down what happened, `div.game-card*3`:
 * `div` - create a div (can use any element, `<li>, <p>...`).
 * `.game-card` - add in a class. Think `css`, where `.`'s are classes and `#`'s are id's. Meaning, if you wanted to create a `div` with an `id` of `main`, just type `div#main` and hit tab.
 * `*3` - create three of these (you can use any number).

#### [open-in-browser](https://atom.io/packages/open-in-browser)
* **Description** Simple Package to open file in default application.
* **Usage** Just right click anywhere on your code and select `Open in Browser`. This will open the file in your default browser.

#### [script](https://atom.io/packages/script)
* **Description** Run code in Atom!
* **Usage** When you want to test a script (let's say a console log in a `.js`), hit the shorcut `cmd + i` and it will display the console at the bottom of the Atom editor. This one is great if you're playing around with some JavaScript and you don't want to use an online code editor like [JSFiddle](https://jsfiddle.net/) or [CodePen](https://codepen.io/).

#### markdown-preview
* **Description** This package comes pre-installed with Atom (also known as a `core package`). It allows you to preview markdown files as you compose them.
* **Usage** While editing your `file.md`, select `control + shift + m` and the markdown preview window will appear beside your code. Keep that open to preview the markdown file while you're editing!

## Useful Links

* [Can I use](https://caniuse.com/) - Provides up-to-date browser support tables for support of front-end web technologies on desktop and mobile web browsers.
* [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) - Compare foreground and background colours (hex values) and check their level of WCAG compliance.
* [CSS Clip-Path Maker](http://bennettfeely.com/clippy/) - The `clip-path` property allows you to make complex shapes in CSS by clipping an element to a basic shape (circle, ellipse, polygon, or inset), or to an SVG source.
* [CSS-Tricks - A Complete Guide to FlexBox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/).
* [First Timers Only](https://www.firsttimersonly.com/) - Friendly Open Source projects should reserve specific issues for newbies.
* [Lighthouse - Google Chrome Extension](https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk?hl=en) - Lighthouse is an open-source, automated tool for improving the performance, quality, and correctness of your web apps. When auditing a page, Lighthouse runs a barrage of tests against the page, and then generates a report on how well the page did. From here you can use the failing tests as indicators on what you can do to improve your app.
* [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).
* [State of JS](https://2018.stateofjs.com/) - They surveyed over 20,000 JavaScript developers to figure out what they're using, what they're happy with, and what they want to learn. And the result is a unique collection of stats and insights that will hopefully help you make your own way through the JavaScript ecosystem.

### Since You've Made It This Far

I hope this little guide helps you with your journey as a developer. Feel free to [reach out to me for anything](https://github.com/SamLegros/ama/issues/new) :)

Lastly, I still consider myself a `newish` developer. Still making my way and learning a ton. I am currently learning towards full stack [MERN development](http://mern.io/). While I complete tutorials I am also working towards building a web app, [Flatbread](https://github.com/SamLegros/flatbread). This app will curate recipes based on what's on sale. I'm trying to combine my love of code with cooking (and saving money). As you can see by the repo, it's *juuust* starting out. I would like it to be an open-source project for people to learn and collaborate. I'm not trying to push anything here, but if you are looking for a project to work towards, this one is open to contributors of any skill-level :)
