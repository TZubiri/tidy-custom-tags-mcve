=Structure=

The files in the root directory are a sample webpage I would write. The style displayed on these files is at the core of this proposal.

The files in the actualoutput directory contain the outputs of some tidy validation commands ran against the sample webpage.
The filenames are the command that was ran, and the contents correspond to the output (both stderr and stdout)

The expectedoutput directory contain the outputs that I'd expect if the proposal were to be implemented.
The expectedoutput contains some outputs that aren't present in the actualoutput folder, these are new features.


=Why do custom tags without hyphens issue a warning?=
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
