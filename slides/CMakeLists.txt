cmake_minimum_required(VERSION 3.17)

project(NTDGoodSoftwareSlides)

# Target to compile the slideshow, which requires Pandoc and LaTeX
find_program(PANDOC pandoc)
find_path(SLIDES_DIR slides.md PATHS ${CMAKE_CURRENT_SOURCE_DIR} NO_DEFAULT_PATH)
set(SLIDES_MD ${SLIDES_DIR}/slides.md)
set(SLIDES_PDF ${SLIDES_DIR}/slides.pdf)

add_custom_target(
        slides COMMAND
        ${PANDOC} -f markdown -t beamer --highlight-style ${SLIDES_DIR}/pygments_custom.theme
        ${SLIDES_MD} -V theme:metropolis -V aspectratio:169 -o ${SLIDES_PDF}
)
