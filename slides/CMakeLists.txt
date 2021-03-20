cmake_minimum_required(VERSION 3.17)

project(NTDGoodSoftwareSlides)

# Target to compile the slideshow, which requires Pandoc and LaTeX
find_program(PANDOC pandoc)
find_path(SLIDES_DIR slides.md PATHS ${CMAKE_CURRENT_SOURCE_DIR} NO_DEFAULT_PATH)
find_path(REVEALJS_DIR reveal.js PATHS ${CMAKE_CURRENT_SOURCE_DIR} NO_DEFAULT_PATH)
message(${REVEALJS_DIR})
set(SLIDES_MD ${SLIDES_DIR}/slides.md)
set(EXAMPLE1_MD ${SLIDES_DIR}/example1.md)
set(LONGPARAM_MD ${SLIDES_DIR}/longparam.md)
set(NESTED_MD ${SLIDES_DIR}/nested.md)
set(LONGFUNCTION_MD ${SLIDES_DIR}/longfunction.md)
set(SLIDES_PDF ${SLIDES_DIR}/slides.pdf)
set(SLIDES_HTML ${SLIDES_DIR}/slides.html)
set(EXAMPLE1_PDF ${SLIDES_DIR}/example1.pdf)
set(EXAMPLE1_HTML ${SLIDES_DIR}/example1.html)
set(LONGPARAM_PDF ${SLIDES_DIR}/longparam.pdf)
set(LONGPARAM_HTML ${SLIDES_DIR}/longparam.html)
set(NESTED_PDF ${SLIDES_DIR}/nested.pdf)
set(NESTED_HTML ${SLIDES_DIR}/nested.html)
set(LONGFUNCTION_PDF ${SLIDES_DIR}/longfunction.pdf)
set(LONGFUNCTION_HTML ${SLIDES_DIR}/longfunction.html)

add_custom_target(
        revealjs COMMAND
	${PANDOC} -t revealjs -s ${SLIDES_MD} -V revealjs-url=${REVEALJS_DIR}/reveal.js
	-V theme=oxrse -o ${SLIDES_HTML}
	)

add_custom_target(
        example1 COMMAND
	${PANDOC} ${EXAMPLE1_MD} -o ${EXAMPLE1_HTML}
	-t revealjs 
	-L ${CMAKE_CURRENT_SOURCE_DIR}/lua-filters/revealjs-codeblock/revealjs-codeblock.lua
	--template ../lua-filters/revealjs-codeblock/template.html
	-V revealjs-url=${REVEALJS_DIR}/reveal.js
	-V theme=oxrse
	-V highlightjs
	-V highlightjs-theme:stackoverflow-light
	-V width='\"80%\"' -V height='\"100%\"'
	)

add_custom_target(
        longparam COMMAND
	${PANDOC} ${LONGPARAM_MD} -o ${LONGPARAM_HTML}
	-t revealjs 
	-L ${CMAKE_CURRENT_SOURCE_DIR}/lua-filters/revealjs-codeblock/revealjs-codeblock.lua
	--template ../lua-filters/revealjs-codeblock/template.html
	-V revealjs-url=${REVEALJS_DIR}/reveal.js
	-V theme=oxrse
	-V highlightjs
	-V highlightjs-theme:stackoverflow-light
	-V width='\"100%\"' -V height='\"100%\"'
	)

add_custom_target(
        nested COMMAND
	${PANDOC} ${NESTED_MD} -o ${NESTED_HTML}
	-t revealjs 
	-L ${CMAKE_CURRENT_SOURCE_DIR}/lua-filters/revealjs-codeblock/revealjs-codeblock.lua
	--template ../lua-filters/revealjs-codeblock/template.html
	-V revealjs-url=${REVEALJS_DIR}/reveal.js
	-V theme=oxrse
	-V highlightjs
	-V highlightjs-theme:stackoverflow-light
	-V width='\"100%\"' -V height='\"100%\"'
	)

add_custom_target(
        longfunction COMMAND
	${PANDOC} ${LONGFUNCTION_MD} -o ${LONGFUNCTION_HTML}
	-t revealjs 
	-L ${CMAKE_CURRENT_SOURCE_DIR}/lua-filters/revealjs-codeblock/revealjs-codeblock.lua
	--template ../lua-filters/revealjs-codeblock/template.html
	-V revealjs-url=${REVEALJS_DIR}/reveal.js
	-V theme=oxrse
	-V highlightjs
	-V highlightjs-theme:stackoverflow-light
	-V width='\"100%\"' -V height='\"100%\"'
	)
      
add_custom_target(
        beamer COMMAND
	${PANDOC} -t markdown -t beamer --highlight-style ${SLIDES_DIR}/pygments_custom.theme
        ${SLIDES_MD} -V theme:metropolis -V aspectratio:169 -o ${SLIDES_PDF}
)
      
