Bleualign
=========
An MT-based sentence alignment tool

Copyright ⓒ 2010
Rico Sennrich <sennrich@cl.uzh.ch>

A project of the Computational Linguistics Group at the University of Zurich (http://www.cl.uzh.ch).

Project Homepage: http://github.com/rsennrich/bleualign

This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation

GENERAL INFO
------------

Bleualign is a tool to align parallel texts (i.e. a text and its translation) on a sentence level.
Additionally to the source and target text, Bleualign requires an automatic translation of at least one of the texts.
The alignment is then performed on the basis of the similarity (modified BLEU score) between the source text sentences (translated into the target language) and the target text sentences.
See section PUBLICATIONS for more details.

Obtaining an automatic translation is up to the user. The only requirement is that the translation must correspond line-by-line to the source text (no line breaks inserted or removed).

REQUIREMENTS
------------

The software was developed on Linux using Python 2.6, but should also support newer versions of Python (including 3.X) and other platforms.
Please report any issues you encounter to sennrich@cl.uzh.ch


USAGE INSTRUCTIONS
------------------

The input and output formats of bleualign are one sentence per line.
A line which only contains .EOA is considered a hard delimiter (end of article).
Sentence alignment does not cross these delimiters: reliable delimiters improve speed and performance, wrong ones will seriously degrade performance.

Given the files sourcetext.txt, targettext.txt and sourcetranslation.txt (the latter being sentence-aligned with sourcetext.txt), a sample call is

    ./bleualign.py -s sourcetext.txt -t targettext.txt --srctotarget sourcetranslation.txt -o outputfile

It is also possible to provide several translations and/or translations in the other translation direction.
bleualign will run once per translation provided, the final output being the intersection of the individual runs (i.e. sentence pairs produced in each individual run).

    ./bleualign.py -s sourcetext.txt -t targettext.txt --srctotarget sourcetranslation1.txt --srctotarget sourcetranslation2.txt --targettosrc targettranslation1.txt -o outputfile

    ./bleualign.py -h will show more usage options

To facilitate batch processing multiple files, `batch_align.py` can be used.

    python batch_align directory source_suffix target_suffix translation_suffix

example: given the directory `raw_files` with the files `0.de`, `0.fr` and `0.trans` and so on, (`0.trans` being the translation of `0.de` into the target language), then this command will align all files: 

    python batch_align.py raw_files de fr trans

This will produce the files `0.de.aligned` and `0.fr.aligned`


SIMPLE IMPORT INSTRUCTIONS
------------------
	from bleualign.align import Aligner
	options={}
	#source and target files needed by Aligner
	#they can be filenames, arrays of strings, io objects.
	options['srcfile'] = source_language_data
	options['targetfile'] = target_language_data
	#they can be filenames, arrays of strings, io objects, too.
	options['srctotarget'] = [data1, data2]
	a = Aligner(options)
	a.mainloop()
	output_src, output_target = a.results()
	#output_src is StringIO because options['output-src'] is unset
	output_src.getvalue() #StringIO member function
	
See codes in example/ directory and get more.
If you want to know all options, you can see Aligner.default_options variable in bleualign/aligner.py.

PUBLICATIONS
------------

The algorithm is described in

Rico Sennrich, Martin Volk (2010):
   MT-based Sentence Alignment for OCR-generated Parallel Texts. In: Proceedings of AMTA 2010, Denver, Colorado.

Rico Sennrich; Martin Volk (2011): 
    Iterative, MT-based sentence alignment of parallel texts. In: NODALIDA 2011, Nordic Conference of Computational Linguistics, Riga.


CONTACT
-------

For questions and feeback, please contact sennrich@cl.uzh.ch or use the GitHub repository.
