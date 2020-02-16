# Structure

The files in the root directory are a sample webpage I would write. The style displayed on these files is at the core of this proposal.

The files in the actualoutput directory contain the outputs of some tidy validation commands ran against the sample webpage.
The filenames are the command that was ran, and the contents correspond to the output (both stderr and stdout)

The expectedoutput directory contain the outputs that I'd expect if the proposal were to be implemented.
The expectedoutput contains some outputs that aren't present in the actualoutput folder, these are new features.

# Why custom-tags

Consider the following html fragments:
`<corp-employee> Tomás Zubiri </corp-employee>`
and
`<div class="corp-employee"> Tomás Zubiri </div>`

There are merits to both styles, one is cleaner, easier to write, read, and can enjoy better static checking due to the closing tag. The other is more traditional and has been standarized and implemented in browsers for longer, however both are completely legal.

# Why custom-tags raise an error.

Despite the WHATWG standard defining unknown custom tags as functionally identical to divs, an error is raised upon encounter, the user is asked to define these tags through the command line or config file.
The error is de facto downgraded to a warning since it can be silenced without modifying the code. Tidy is an opinionated tool, so a warning is interpreted as a disincentive to use the feature.
The warning is raised not due to an interaction with the browser, but due to an inability of tidy to disambiguate between a spelling mistake or a custom tag. Unlike a lack of namespace, this warning is an artifact of the validator and brings no value. I propose tidy has plenty of options to disambiguate, possible features like, reading definitions from a css file, allowing all custom tags through an option, allowing tags from a specified namespace, spell checking an unknown tag, giving less warnings to hyphenated tags (adding a hyphen to a custom tag can be an organic way to silence a warning)
Spell checking could even be applied to classes (along with css file definitions) to improve the general quality of validity checks.

# Why unhyphenated custom tags issue a warning
Currently tidy shows the following error if the developer defines a custom tag without a hyphen.

> Warning: <customtag> is not approved by W3C

The condition for this error message can be found in the function elementisAutonomousCustomFormat in the [tags.c file](https://github.com/htacg/tidy-html5/blob/a71031f9e529f0aa74a819576411594e21767be4/src/tags.c#L1056).

> const char *ptr = strchr(element, '-');
> /* Tag must contain hyphen not in first character. */

 this implements the [6th and 7th conditions of section 3.2.2 of the WHATWG living HTML standard]( https://html.spec.whatwg.org/multipage/dom.html#htmlunknownelement) 

> 6. If name is a valid custom element name, then return HTMLElement.
> 7. [ Otherwise, if none of the first 6 conditions are met] return HTMLUnknownElement.

The definition of valid custom name can be found in section [4.13.3](https://html.spec.whatwg.org/multipage/custom-elements.html#valid-custom-element-name)

> [valid custom names] contain a hyphen, used for namespacing and to ensure forward compatibility (since no elements will be added to HTML, SVG, or MathML with hyphen-containing local names in the future).

Github issue [#2](https://github.com/TZubiri/tidy-custom-tags-mcve/issues/2) aims to warn the user of potential issues of not using a hyphen. And [#3](https://github.com/TZubiri/tidy-custom-tags-mcve/issues/2) aims to give users the possiblity of ignoring this warning, in case they deem the risk small enough or nil.
