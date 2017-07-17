subedit
=======

Command-line subtitle editor written in BASH.

The script generally works with srt (SubRip) but a few parameters work with sub (MicroDVD) subtitle files too.
- - - - -

### PARAMETERS  
`-i "Input file"`  
>Subtitle file to process. Accepts srt or sub files, depending on what other parameter is used.  
Does not work with -D parameter (synchronize with directory) and -r parameter (recurse).

`-d "Input directory"`
>Directory containing subtitle files to process. Can contain srt, sub or both subtitle types.  
Works with all parameters except with -y (synchronize with file) and -j (join two srt subtitles).  
Instead of Synchronize with file you can use Synchronize with directory (-D parameter).

`-r [Switch]`
>Searches recursively for subtitle files in directory given with -d parameter.

`-b [Switch]`
>Backup input file. Makes a copy of the input file in the same directory with the .bak sufix.  
If the backup already exists, it does nothing. If -b is not set and the backup file exists, it is removed.  
Can be used with any other parameter.

`-s (+/-/a/z)hh:mm:ss,fff`
>Shift time. Plus sign (or no sign at all) adds time, where the minus sign subtracts time.  
"a" and "z" indicate absolute time value for the beginning and end time of the subtitle file respectively.  
In this case the shifting time will be calculated.

`-p [Switch]`
>Change the fps from NTSC (23.976/29.970) to PAL (25).

`-n [Switch]`
>Change the fps from PAL (25) to NTSC (23.976/29.970).

`-a (+/-)hh:mm:ss,fff (+/-)hh:mm:ss,fff`
>Adjust time. Beginning and end time will be set to these values and the rest will be adjusted proportionally.  
The time value is absolute if you don't use the +/- and will be shifted if you do use either sign.

`-1 (+/-)hh:mm:ss,fff`
>Adjust time. Beginning time will be set to this value and the rest will be adjusted proportionally.  
The time value is absolute if you don't use the +/- and will be shifted if you do use either sign.

`-2 (+/-)hh:mm:ss,fff`
>Adjust time. End time will be set to this value and the rest will be adjusted proportionally.  
The time value is absolute if you don't use the +/- and will be shifted if you do use either sign.

`-y "Input file"`
>Synchronize with file. The subtitle times of the srt given with -i parameter will be adjusted with  
beginning and end time of this srt.

`-D "Input directory"`
>Synchronize with directory. To be used only with -d parameter.  
This directory contains srt subtitle files with correct times.  
The two directories (given with -d and -D parameters) must contain srt subtitle files with the same names.  
Synchronizes each srt subtitle file in the input directory (-d) with the srt subtitle file with the same name  
that's in the directory (-D).

`-Y [Switch]`
>Synchronize subtitle times one by one. Optional switch to be used with -y and -D.  
Replaces times of file to be synchronized with times of synchronization file, respectively.

`-f "Find text"`
>Search, case insensitive. The double quotes are necessary. Works with srt and sub.

`-F "Find text"`
>Search, case sensitive. The double quotes are necessary. Works with srt and sub.

`-e "Text to be replaced" "New text"`
>Replace, case insensitive. The double quotes are necessary. Works with srt and sub.

`-E "Text to be replaced" "New text"`
>Replace, case sensitive. The double quotes are necessary. Works with srt and sub.

`-t film/ntsc/pal/custom fps`
>Convert sub to srt. Pal = 25, film = 23.976, ntsc = 29.970. Custom fps can be integer or float number.  
Works only with sub subtitle files.

`-u film/ntsc/pal/custom fps`
>Convert srt to sub. Pal = 25, film = 23.976, ntsc = 29.970. Custom fps can be integer or float number.  
Any srt tags that cannot be converted to sub tags will pass to the .sub file. For example, a tag that  
includes one word in a sentence.

`-U film/ntsc/pal/custom fps`
>Convert srt to sub. Pal = 25, film = 23.976, ntsc = 29.970. Custom fps can be integer or float number.  
Same as the -u parameter above but also ignores any srt tags that were not converted to respective sub tags.  
This is the behavior that you'd expect from other subtitle edit programs.

`-j "Input file"`
>Join two srt subtitles. This srt will be appended to the srt given with -i parameter.

`-J hh:mm:ss,fff`
>Join time. Optional parameter, to be used with -j.  
Shifts the time after beginning of the second srt in the output file.

`-x (+/-)hh:mm:ss,fff` *`or`* `(+/-)SUB_INTEGER` *`or`* `(+/-)INTEGER:INTEGERt` *`or`* `(+/-)INTEGER:INTEGERn`
>Split srt subtitle. Can take 4 different syntaxes:  
>1. `(+/-)hh:mm:ss,fff:` splitting time.
>2. `(+/-)SUB_INTEGER:` splitting subtitle integer.
>3. `(+/-)INTEGER:INTEGERt:` fraction where we split the subtitle file according to total time.
>4. `(+/-)INTEGER:INTEGERn:` fraction where we split the subtitle file according to number of subtitles.  

>The minus sign means that counting begins from the end of the subtitle file.  
The plus sign means that counting begins from the beginning. (Equivalent of using no sign at all).

`-X [Switch]`
>Split time. Optional switch, to be used with -x. Makes the second generated srt begin with time 00:00:00,000.  
Can be used with any -x syntax.

`-c [Switch]`
>Clean trash.  
>- Replace multiple whitespaces with one  
>- clean beginning and trailing spaces
>- replace two single quotes with a double quote
>- replace 2 or more "." with "..."
>- capitalize the first letter after '.', '!' and '?'
>- remove subtitles with zero duration
>- and fix many more common errors usually found in subtitles  
>- Also removes subtitles that contain the key words or key phrases (case insensitive) in `/etc/subeditrc`.  
/etc/subeditrc is a text file that you can edit and add key words or key phrases. If ~/.subeditrc exists  
(you have to create it manually), then it will be used instead of /etc/subeditrc. Works with srt and sub.

`-k '([{?mM'`
> Delete text for hearing impaired.  
The `([{?` characters are the symbols that the text for the hearing impaired is enclosed by. "`m`" and "`M`" are for music symbols.  
With "m" only the music symbols are deleted. With "M" the text that is enclosed by the music symbols is deleted too.  
Use only the symbol(s) that are needed for each subtitle file. The single quotes are necessary. Works with srt and sub.

`-m [Switch]`
>Fix common errors. Similar to 'Clean trash' but fixes different errors.
>- Fix common OCR mistakes like zero and capital 'o' ripping mistakes
>- replace "I" with "l" in the middle of a lowercase word
>- replace a single "l" with "I"
>- replace "l'm" with "I'm" etc.  

>Also fixes:
>- Invalid times (duration < 0)
>- too long durations (max of 7 seconds or 0.15 seconds per character)
>- overlapping display times
>- too short durations (0.06 seconds per character)
>- subtitles over two lines
>- too long lines

>Works with srt and sub.

`-h [Switch]`
>Display help.

`-H [Switch]`
>Display help with more information and examples.
