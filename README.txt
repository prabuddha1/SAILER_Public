**********************************************
Version 1.0

Disclaimer: All software, documents, and HDL files, etc., are provided as is.

You running this script/function means you will not blame the author(s) for any damage to your system or data. Do not attempt to violate the law with anything contained here. We reserve the right to modify the Disclaimer at any time without notice.


      ___           ___                                     ___           ___     
     /  /\         /  /\        ___                        /  /\         /  /\    
    /  /:/_       /  /::\      /  /\                      /  /:/_       /  /::\   
   /  /:/ /\     /  /:/\:\    /  /:/      ___     ___    /  /:/ /\     /  /:/\:\  
  /  /:/ /::\   /  /:/~/::\  /__/::\     /__/\   /  /\  /  /:/ /:/_   /  /:/~/:/  
 /__/:/ /:/\:\ /__/:/ /:/\:\ \__\/\:\__  \  \:\ /  /:/ /__/:/ /:/ /\ /__/:/ /:/___
 \  \:\/:/~/:/ \  \:\/:/__\/    \  \:\/\  \  \:\  /:/  \  \:\/:/ /:/ \  \:\/:::::/
  \  \::/ /:/   \  \::/          \__\::/   \  \:\/:/    \  \::/ /:/   \  \::/~~~~ 
   \__\/ /:/     \  \:\          /__/:/     \  \::/      \  \:\/:/     \  \:\     
     /__/:/       \  \:\         \__\/       \__\/        \  \::/       \  \:\    
     \__\/         \__\/                                   \__\/         \__\/


#System Requirements:
----------------------------------------------
SAILER 1.0 has been tested on the following setup:
*Ubuntu 22.04.2 LTS x86

**********************************************
#Data Generation:
----------------------------------------------

SAILER 1.0 currently supports flattened Verilog designs and Cadence GSC 180nm standard cell library with the following gates only:
AND2X1, AOI21X1, AOI22X1, INVX1, MX2X1, NAND2X1, NAND3X1, NAND4X1, NOR2X1, NOR3X1, NOR4X1, OAI21X1, OAI22X1,
OAI33X1, OR2X1, OR4X1, XOR2X1, DFFSRX1

Assign statements are only supported after input/output/wire declarations and before gate instance declarations.

<netlist, label> pairs should have the naming convention <design_name>.v and <design_name>.label.

Examples are provided in the folder "data".

**********************************************
#Training a SAILER Model From Scratch:
--------------------------------------------

Executable to use: SAILER_TrainModel 
--------------------------------------------

Sample Command:

./SAILER_TrainModel -XNodeCut 3_4_5 -XInDepthCutoff 2 -XOutDepthCutoff 2 -unityDSPath ./data/ -verbose 0 -modelOutPath ./model.npy

-XNodeCut: A list of locality size cutoffs when constructing the design locality representations. Multiple localities separated by "_".

-XInDepthCutoff: Maximum posterior depth of Breadth-First-Search when constructing the locality representations

-XOutDepthCutoff: Maximum anterior depth of Breadth-First-Search when constructing the locality representations

-unityDSPath: Path to the Training Dataset. For a design X, name the verilog design "X.v" (place inside ./design/netlists/) and name the labels file as "X.label" (place inside ./design/labels/). Please put a "/" at the end of the path. The script will create, delete, modify folders/files in this path so make sure you have a backup of all the data in this location. 

-verbose: Keep it at 0 to avoid seeing debug messages.

-modelOutPath: The path and name of the trained model file that SAILER will be generating. Please use ".npy" as the extension. The script will overwrite any exisiting file at this path. 

--------------------------------------------


**********************************************
#Evaluating a <netlist, Ground Truth Label> pair using a pre-trained SAILER model:
--------------------------------------------

Executable to use: SAILER_Evaluate (Run it from inside ./binary/)
--------------------------------------------

Sample Commands:

./SAILER_Evaluate -XNodeCut 3_4_5 -XInDepthCutoff 2 -XOutDepthCutoff 2 -modelPath model.npy -tempFolderPath ./data/tmp/ -verbose 0 -datasetpath_netlist ./data/netlists/0_c1355.v -datasetpath_label ./data/labels/0_c1355.label


-XNodeCut: A list of locality size cutoffs when constructing the post-synthesis locality representations. Multiple localities separated by "_".

-XInDepthCutoff: Maximum posterior depth of Breadth-First-Search when constructing the post-synthesis locality representations

-XOutDepthCutoff: Maximum anterior depth of Breadth-First-Search when constructing the post-synthesis locality representations

-modelPath: The path to the trained SAILER model.

-tempFolderPath: Path to a working space for the script. The script will create, delete, modify folders/files in this path so make sure you have a backup of all the data in this location.

-verbose: Keep it at 0 to avoid seeing debug messages.

-datasetpath_netlist: Path to the design netlist.

-datasetpath_label: Path to the labels file.


--------------------------------------------

The predictions are stored in <tempFolderPath>/dataset/SAILER_results

**********************************************

Tips:

1) Keeping XInDepthCutoff, XOutDepthCutoff and YDepthCutoff set to 2 should be sufficient for dealing with localities upto size 6 gates. For evaluating a very large locality size above 6, you may try setting them to 3 or higher.

2) Inside the labels file, each line indicates <Gate Name, Reverse Engineering Correctness>


**********************************************



