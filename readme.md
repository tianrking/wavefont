# wavefont

A typeface for rendering data: waveforms, spectrums, diagrams, bars etc.

* [Demo](https://dy.github.io/wavefont)
* [v-fonts/wavefont](https://v-fonts.com/fonts/wavefont).

<a href="https://a-vis.github.io/wavefont"><img src="./preview.png" width="240px"/></a>

## Usage

Place [_wavefont.woff2_](./wavefont.woff2) into your project directory and use this code:

```html
<style>
	@font-face {
		font-family: wavefont;
    font-display: block;
		src: url(./wavefont.woff2) format('woff2');
	}
	.wavefont {
		--wght: 10;
		font-family: wavefont;
		font-variation-settings: 'wght' var(--wght), 'ROND' 30;
	}
</style>

<textarea id="waveform" class="wavefont" cols="100"></textarea>

<script>
	// either enter values manually (less precise)
	waveform.textContent = 'abcdefghijklmnopqrstuvwwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'

	// or set programmatically (more precise)
	let values = [...Array(100).keys()] // create your input values from 0..100 range
	waveform.textContent = values.map(v => String.fromCharCode(0x100 + v)).join('')
</script>
```

## Characters

Wavefont provides bars corresponding to values from 0 to 100, assigned to different characters:

* <kbd>0-9</kbd> chars are for simplified manual input with step 10.
* <kbd>a-zA-Z</kbd> for extended manual input with step 2, softned at edges <kbd>a</kbd> and <kbd>Z</kbd> to step 1.
* <kbd>U+0100-016F</kbd> for 0..100 bottom aligned values (convert as `char = String.fromCharCode(0x100 + value)`).
* <kbd>U+0400-046F</kbd> for 0..100 center aligned values (convert as `char = String.fromCharCode(0x400 + value)`).

## Variable axes

Tag | Range | Meaning
---|---|---
`wght` | _1_-_400_ | Bar weight, or boldness (in upm).
`ROND` | _0_-_50_ | Border radius (roundness), percentage of glyph width.

## Features

* Letter-spacing can be adjusted via `letter-spacing` CSS property. `ch` units are useful in that regard, since 1 ch is 1 bar width.
* Charcodes fall under _marking characters_ unicode category, ie. recognized as word by regexp and can be selected with <kbd>Ctrl</kbd> + <kbd>→</kbd> or double click. Eg. waveform chunks are selectable, if separated by space.
* Shifting up can be done via combining accent acute <kbd>&nbsp;&#x0301;</kbd> (U+0301) for 1-step up, or circumflex accent <kbd>&nbsp;&#x0302;</kbd> (U+0302) for 10-steps up. Eg. `\u0101\u0302\u0302\u0301\u0301\u0301` shifts 1 value 23 steps up.
* Shifting down can be done via combining accent grave <kbd>&nbsp;&#x0300;</kbd> (U+0300) for 1-step down, eg. `\u0101\u0300\u0300\u0300` shifts bar 3 values down.
* Values below chars range are limited to 0, eg. _0x0ff_ is mapped to 0.
* Values above chars range are supported to some extent and then clipped, eg. _0x164_ (dec 101) is supported and value above 108 is clipped.
* Space, tab and other non-marking chas map to _0_ value.
* `-–._*` map to _1_ value, `|` maps to max value, `▁▂▃▄▅▆▇█` map to corresponding bars.

## Anti-FOUT

To avoid flash of system font (aka [FOUT](https://css-tricks.com/fout-foit-foft/)), use [Adobe Blank](https://github.com/adobe-fonts/adobe-blank-vf) font-face fallback. The best practice is to have it included synchronously as data-url.

```css
@font-face {
	font-family: blank;
	font-display: block;
	/* AdobeBlank2VF.ttf.woff2 */
	src: url(data:application/x-font-woff;charset=utf-8;base64,d09GMgABAAAAAARMABMAAAAAC1AAAAPjAAEAxQAAAAAAAAAAAAAAAAAAAAAAAAAAGUYcID9IVkFSVAZgP1NUQVQkP1ZWQVJjAIIAL0YKWFwwLgE2AiQDCAsGAAQgBYsyBzEXJBgIGwEKEcXkJfmqgCdD4yWAULaeeBovVzstIc6riunXpnmtXivg+BLWsFA1x8PTXP+eO5PNAn8AUCwJsYCgqxxZ1JUELrLsXkOo3TUpDjbRB8Tvo61HSFe6NVBVO1SvlxDEvYxsxE4DfNN8ImIOJ0gqtv/9fq0+Wat/NsTds46FRuqU8lf0gse1iqY3dLfkGRqhqiTVConpREoxCY1FPHelp1Ny+uwFWpZYoBA7dh04oaUEhFLUFDri7o2bj2BaFYGOOeZ1vLhvdQ0IMAUABHoC6yWZLbYAgJ9NX06XhIBcaa5iDdL76Qz2k7njkrV+ci1cx5uLNbeOtx22hbrOrMaqxrO/9QL9NwVgAUJoSZgClkjP3HjyTmDP7j0nIoQAM3AGAJfCk0F+cI8EIeugpw/+AUwGvq0R/IaJbxBM/EE9gRAIQEJCRkaBAg00UKJEE0200EIbbXTQQRdd9NBDH30MMMAQQ4wwEuEmSaYkBnnYDCBXEDnpwW9EJK5WZJBAgQZGmA0oQIMiXw91sjku790e3LzZfHZ461Z5+/ozFVTN2diLFz9/ql67cl8k+nvMWO4OBBKYtXsY6LwaespXbYFXt+TqtTcIhK83bvTx5eGGf1s5V/D9qt/p9najlt/Mj6KUQFCxsYrB4sZY4VoloVbYkpHGKGGx3RT/yaJo4wSfCEucQDLwLLLV3u5asKHhhk9QmpVZaOnEerTNyqaunQW61ucM+umj+BAD6xtRwIJe7SdfPJ0j6BTPIkzxelO3WVhf//c2jpwWnwkGXVHJ7/FoDWDAQVkQ+OBTFKZJu8lXljDZnk7YqITmExH/y0MxUyoCDOBqChwdIU8mZhKKCJ2upGVgbGinpSoKOgThuVdIkAJUCkD1UYqNlj3J5kSRrqElYYogo7sLvSE3eCVefdEJa/SRA1H0BWVMIMAKSoG1TMqBSbtr5Q+DNGe+gASmqJQCTiiX8mISEDDtHgvYUEpCDjCuCZD0oQJ1YQ3sjHRPlXdfK+E/rSY28Bg4cI6cV00RDZaMcjGklsyXggUDJ2BRVwCGvwLAfxa/WTib27k4LwoOHB0NbJMLE/HNdo7AyM7W2AINfIZwPg2I0KNAtGALqE+jIEEkfiTfWAAkgSiiBZJkLDgSCXf6EaZ2NnDRspAUq7MFXOYSaLDGnozJK4dkPCmTRSmRKSxxWiDAMFMxKVLnoZ7iPA0oLgj+UBlRkrmlg3DbW9ieSDMnJbu4KqNIxL1kjISZ9GjJ5+t2TALq0U3AcYLmZeFeX9Ww6+F30lD/+kXetcg6ZJg3EalzO/5jvn7dbo3cSO6f/4dc8dJzzBkJMwsAAAA=) format('woff2');
}
.wavefont {
	font-style: wavefont, blank;
	--wdth: 20;
	--wght: 10;
	font-variation-settings: 'wdth' var(--wdth), 'wght' var(--wght), 'ROND' 50;
}
```

## JS package

To facilitate calculation, wavefont exposes a JS package with bar calculators for bottom-aligned or center-aligned faces.

```js
import * as wavefont from 'wavefont'

// get bottom aligned characters for values from 0..100 range
wavefont.low(0, 1, 50, 99, 100, 101, ...)

// get center aligned characters for values from 0..100 range
wavefont.center(0, 1, 50, 99, 100, 101, ...)
```

## Building

Wavefont is generated in 2 steps.

1. First, UFOs are generated from `_source` template into `source/wavefont100` folder by `npm run build-ufo` command. It uses [plopfile](./plopfile.js) to evaluate the template. The step can be skipped since the `source` folder is stored in the repository.

2. TODO: gftools do the rest of the job, compiling UFOs to ...

## See also

* [linefont](https://github.com/a-vis/linefont) − font-face for rendering linear data.

## References

* [unified font object spec](https://unifiedfontobject.org/versions/ufo3) − unified human-readable format for storing font data.
* [feature file spec](https://adobe-type-tools.github.io/afdko/OpenTypeFeatureFileSpecification.html#6.h) − defining opentype font features.
* [unicode-table](https://unicode-table.com/) − convenient unicode table.
* [adobe-variable-font-prototype](https://github.com/adobe-fonts/adobe-variable-font-prototype) − example variable font.
* [designspace xml spec](https://github.com/fonttools/fonttools/tree/main/Doc/source/designspaceLib#document-xml-structure) − human-readable format for describing variable fonts.
* [Adobe Blank](https://github.com/adobe-fonts/adobe-blank-vf) − blank characters variable font.

<p align="center"><a href="https://github.com/krsnzd/license/">🕉</a><p>
