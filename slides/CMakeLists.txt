cmake_minimum_required(VERSION 3.17)

project(NTDGoodSoftwareSlides)

# Target to compile the slideshow, which requires Pandoc and LaTeX
find_program(PANDOC pandoc)
find_path(SLIDES_DIR slides.md PATHS ${CMAKE_CURRENT_SOURCE_DIR} NO_DEFAULT_PATH)
find_path(REVEALJS_DIR reveal.js PATHS ${CMAKE_CURRENT_SOURCE_DIR} NO_DEFAULT_PATH)
message(${REVEALJS_DIR})
set(SLIDES_MD ${SLIDES_DIR}/slides.md)
set(SLIDES_PDF ${SLIDES_DIR}/slides.pdf)
set(SLIDES_HTML ${SLIDES_DIR}/slides.html)

add_custom_target(
        revealjs COMMAND
	${PANDOC} -t revealjs -s ${SLIDES_MD} -V revealjs-url=${REVEALJS_DIR}/reveal.js
	-V theme=oxrse -o ${SLIDES_HTML}
)
add_custom_target(
        beamer COMMAND
	${PANDOC} -t markdown -t beamer --highlight-style ${SLIDES_DIR}/pygments_custom.theme
        ${SLIDES_MD} -V theme:metropolis -V aspectratio:169 -o ${SLIDES_PDF}
)
      
