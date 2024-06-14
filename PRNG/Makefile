BUILD           := build
PDF             := $(BUILD)/PRNG.pdf
DATA_GENERATOR  := $(BUILD)/data_generator
DATA            := $(BUILD)/data
TESTDATA        := $(DATA)/testing.dat

.PHONY: dieharder ent read clean

$(PDF): $(DATA) prng.latex references.bib
	tectonic -o $(BUILD) --synctex prng.latex
	# latexmk -outdir=$(BUILD) -pdfxe -interaction=nonstopmode prng.latex


$(DATA_GENERATOR): BMP.h data_generator.cpp
	mkdir -p $(BUILD)
	$(CXX) -O2 -std=c++11 data_generator.cpp -o $(DATA_GENERATOR)

BMP.h:
	mkdir -p $(BUILD)
	cd $(BUILD) && curl -OL https://github.com/sol-prog/cpp-bmp-images/raw/master/BMP.h


$(DATA): $(DATA_GENERATOR)
	mkdir -p $(DATA) && cd $(BUILD) && ./data_generator
	magick convert $(DATA)/test.bmp $(DATA)/test.png

$(TESTDATA): $(BUILD) $(DATA_GENERATOR)
	mkdir -p $(DATA)
	cd $(BUILD) && ./data_generator --testing


ent: $(TESTDATA)
	ent $(TESTDATA)

dieharder: $(TESTDATA)
	dieharder -a -g 202 -f $(TESTDATA)


read:
	zathura build/*.pdf


clean:
	$(RM) -r $(BUILD)
