# How to fix imaxima in Ubuntu 20.04

2020-10-22

imaxima is an emacs interface for maxima that prints the results as
latex-processed images.  This looks much-much better than the default
ASCII-art.

Unfortunately, the default imaxima installation in ubuntu 20 (Maxima
5.43.2) is broken.  The problem boils down to the way
imaxima creates latex format.  In particular, the function
imaxima-dump-tex in `/usr/share/emacs/site-lisp/maxima/imaxima.el`
contains lines:

```
...
     "\\usepackage{color}\n"
     "\\usepackage{exscale}\n"
     "\\usepackage[cmbase]{flexisym}\n"
     "\\usepackage{breqn}\n"
...
```

Apparently, in the current texlive one has to load _amsmath_ **before**
_flexisym_ package, so these lines should look like

```
...
     "\\usepackage{color}\n"
     "\\usepackage{exscale}\n"
     "\\usepackage{amsmath}\n"
     "\\usepackage[cmbase]{flexisym}\n"
     "\\usepackage{breqn}\n"
...
```

Unfortunately, this is hard-coded into the function and not configurable.

I could have fixed these lines in the file, or in a copy of the file, 
but instead, I copied the most recent
(Maxima 5.44) imaxima.el and imaxima.list into my local
`~/share/emacs` and put it in front of the load path by

```lisp
(setq load-path 
      (append (list (concat (getenv "HOME") "/share/emacs"))
			  load-path))
```

5.44 has seen quite a bit changes related to how emacs interacts with
the latex process, and this problem has gone away too :-)

Hope I did not break anything else.
