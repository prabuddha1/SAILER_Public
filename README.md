git clone https://github.com/prabuddha1/SAILER_Public
cd SAILER_Public/

Training:
./SAILER_TrainModel -XNodeCut 3_4_5 -XInDepthCutoff 2 -XOutDepthCutoff 2 -unityDSPath ./data/ -verbose 0 -modelOutPath ./model.npy

Evaluating:
./SAILER_Evaluate -XNodeCut 3_4_5 -XInDepthCutoff 2 -XOutDepthCutoff 2 -modelPath model.npy -tempFolderPath ./data/tmp/ -verbose 0 -datasetpath_netlist ./data/netlists/0_c1355.v -datasetpath_label ./data/labels/0_c1355.label


