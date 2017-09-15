

##Requirements:
	> 2 GB of memory
	Caffe and OpenCV installed
	Webcam
		Note: Webcam currently does not work on virtual machines.
		Try using a native Mac or Linux system. Don't try on a virtual machine running on Windows.
	
	If you want to run on GPU:
		2GB or more VRAM
		CUDA and CuDNN libraries installed
		Caffe must be compiled with these CUDA and CuDNN selected



##Description of files:

#Main scripts: 
	gather_training_data.py - Use this to generate a custom training set
	process_dataset.py - Read in an entire training set and calculate accuracy over the set
	process_image.py   - Read in a single image, add the correct emoji, and write to file
	video_generate.py  - Run HappyNet in real-time and save output to video
	video_test.py      - Run HappyNet in real-time; does not save to video

#Scripts for retraining the network on new data:

	These 5 scripts are to be run in numerical order
	Note you'll need to modify them with your own paths

	Generate Caffe-compatible database of input images:
		execute_0_create_file_list
		exceute_1_create_lmdb_databse

	Generate a mean image (mean.binaryproto file) from input dataset:
		execute_2_create_mean_image

	Retrain an existing caffe model with the new inputs:
		execute_3_train_custom_model

		* Note, this needs to be modified if you are running on GPU. Our network was
		VGG_S net, which requires 2 GB of GPU memory, so we ran on CPU.
		The modification is just an extra flag, something like "-gpu 0"

	Delete all unnecessary files
		execute_4_cleanup_training_data

		This deletes the output files from scripts 0, 1, and 2. 
		Run this when you are getting ready to start over from file 0.
		Don't run it until then though - you might want to reuse the info in those files!

#Utility functions:
	caffe_functions.py  - anything dealing primarily with caffe
	opencv_functions.py - anything dealing primarily with opencv
	utility_functions.py - General functions mostly related to file I/O

#Datasets:
	Only contains the emojis we used.
	Cohn-Kanade Plus (CK+) and Japanese Female Facial Expressions (JAFFE) can be downloaded online.

#Models:
	deploy.prototxt - Architecture of our model (this file should not need to be changed)
	solver.prototxt - This configures the retraining process.
	train.prototxt - This configures the architecture during training. 
					 Mainly used to add layer-specific learning rates.
	loss_history.txt - Log file from our last retraining on our dataset

  NOT INCLUDED:
  EmotiW_VGG_S.caffemodel - 
      This is the file with all the weights. It is 500 MB and cannot be archived. 
      However, you can download the Emotios in the Wild model from Caffe Model Zoo. 
      Retrain it on new data for a day or two, and you can get similar numbers to 
      our model.
      We trained on:
         Homebrewed dataset of 2000 images of 5 people making all 6 emotions
         All data was generated with the script generate_training_data.py
      We trained for:
         About 24 hours
      A bigger dataset could be collected in a couple hours and would
                            likely greatly improve performance.


