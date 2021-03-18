The slideshow can be built if you have [Pandoc](https://pandoc.org/) installed:

```bash
sudo apt install pandoc
```

Build the slides:

```bash
cd slides
mkdir build && cd build
cmake ..
make revealjs
```
