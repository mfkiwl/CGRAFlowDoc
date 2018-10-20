# Usage

## Installation
See [Installation](installation.md) section for how to install
dependencies and how to compile Halide.

## Make targets
For the Halide_CoreIR repo, generation has three basic targets: `make clean`, 
`make design_top.json`, and `make out.png`. This will first remove old generated
files, then create the CoreIR file, and last test the CoreIR.
```
make clean             # remove generated files
     design_top.json   # create CoreIR
     graph.png         # use graphviz to create visualization of circuit 
     out.png           # create output file
     test              # create CoreIR and check if circuit matches cached result
     update_golden     # update reference CoreIR json file
```

For the Halide-to-Hardware repo, some of the targets were updated. These
updates were meant to separate targets to a series of smaller steps. For
example, generating the reference CPU output no longer also tests CoreIR.
the basic steps remain: `make clean`, `make design_top.json`, and `make out.png`.
In addition, these same targets are duplicated with new target names.
```
make clean             # remove generated files (bin directory)
     generator         # create Halide generator
     design_top.json   # create CoreIR design
     design-coreir     # create CoreIR design
     design-cpu        # create CPU design
     graph.png         # use graphviz to create visualization of circuit 
     output.png        # create output file using CPU implementation
     run-cpu           # create output file using CPU implementation
     run-coreir        # create output file using CoreIR implementation
     compare-coreir    # compare CoreIR output file to golden image
     golden            # update reference CoreIR json file and output image
```



## Application Folder
The following is a list of the files that are important for generating
images and are used further in the flow. The CoreIR json files are created
for mapping to the CGRA. Also, an output image, out.png, is generated as a 
the reference image to determine if execution is correct. `out.png` is 
created using a CPU implementation of Halide, and its output should be
used for validation during CoreIR interpretation and CGRA simulation.

### Directory for Halide_CoreIR repository
```
Halide_CoreIR
└── apps
    ├── coreir_tests                       // contains simpler test cases
    └── coreir_examples                    // contains all apps compiled to coreir
        └── conv_bw                        // one of the apps: does 3x3 convolution
            ├── Makefile                   // specifies commands for 'make'
            ├── pipeline.cpp               // contains Halide algorithm and schedule
            ├── design_top_golden.json     // reference coreir output expected by compiler
            ├── input.png                  // input image for testing
            ├── run.cpp                    // runs input image with design to create ouput image
            │
            │                              //// Running 'make all' generates:
            ├── design_top.json            // generated CoreIR design
            ├── design_top.txt             // generated graphiz representation of CoreIR
            ├── graph.png                  // generated graphiz image of circuit (using 'make graph.png')
            └── out.png                    // output image created during testing
```

### Directory for Halide-to-Hardware repository
```
Halide_CoreIR
└── apps
    └── hardware_benchmarks                    // contains simpler test cases
        ├── apps                               // contains all apps compiled to coreir
        └── tests                              // contains all simpler test cases
            └── conv_3_3                       // one of the apps: does 3x3 convolution
                ├── Makefile                   // specifies commands for 'make'
                ├── conv_3_3_generator.cpp     // contains Halide algorithm and schedule
                ├── input.png                  // input image for testing
                ├── process.cpp                // runs input image with design to create ouput image
                ├── golden                     // this holds all expected output files
                │   └── golden_output.png      // output image expected
                │
                └── bin                        //// Running 'make all' generates in this folder:
                    ├── design_top.json        // generated CoreIR design
                    ├── design_top.txt         // generated graphiz representation of CoreIR
                    ├── graph.png              // generated graphiz image of circuit (using 'make graph.png')
                    └── output.png             // output image created during testing
```