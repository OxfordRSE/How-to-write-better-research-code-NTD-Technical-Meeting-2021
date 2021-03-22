cmake_minimum_required(VERSION 3.17)

project(NTDGoodSoftwareSlides)

# Target to compile the slideshow, which requires Pandoc and LaTeX
find_program(PANDOC pandoc)
find_path(SLIDES_DIR slides.md PATHS ${CMAKE_CURRENT_SOURCE_DIR} NO_DEFAULT_PATH)
find_path(REVEALJS_DIR reveal.js PATHS ${CMAKE_CURRENT_SOURCE_DIR} NO_DEFAULT_PATH)
set(OUTPUT_DIR ../output)

add_custom_target(
    slides ALL
    )

add_dependencies(slides
    revealjs
    example1
    longparam
    nested
    longfunction)

add_custom_target(
    revealjs COMMAND
    ${PANDOC} -s ${SLIDES_DIR}/slides.md -o ${OUTPUT_DIR}/slides.html
    -t revealjs
    --template ../pandoc-template.html
    -V revealjs-url=${REVEALJS_DIR}/reveal.js
    -V theme=oxrse)

set(EXAMPLES example1 longparam nested longfunction)
set(REVEALJS_CODEBLOCK_FILTER ${CMAKE_CURRENT_SOURCE_DIR}/lua-filters/revealjs-codeblock/revealjs-codeblock.lua)

foreach(example IN LISTS EXAMPLES)
    set(MD_FILE ${SLIDES_DIR}/${example}.md)
    set(HTML_FILE ${OUTPUT_DIR}/${example}.html)

    add_custom_target(
        ${example} COMMAND
	${PANDOC} ${MD_FILE} -o ${HTML_FILE}
	-t revealjs
	-L ${REVEALJS_CODEBLOCK_FILTER}
	--template ../pandoc-template.html
	-V revealjs-url=${REVEALJS_DIR}/reveal.js
	-V theme=oxrse
	-V highlightjs
	-V highlightjs-theme:stackoverflow-light
	-V width='\"100%\"' -V height='\"100%\"')
endforeach()
