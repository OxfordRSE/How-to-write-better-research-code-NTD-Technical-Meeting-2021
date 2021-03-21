To build the slides:

Clone the repository with submodules:
```bash
git clone git@github.com:OxfordRSE/How-to-write-better-research-code-NTD-Technical-Meeting-2021.git --recurse-submodules
```

Make sure you have pandoc installed

```bash
sudo apt install pandoc
```

Build the slides:

```bash
cd slides
mkdir build && cd build
cmake ..
make
```
