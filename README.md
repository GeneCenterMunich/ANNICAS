Usage of ANNICAS:

	1) running the executable
	2) compiling the executable from a Matlab .m file
	3) training the neuronal network yourself

-----------------------------------------------------------------------------------------------------
 ANNICAS: 	ANN based Image Categorization and Analysis Software
		Version 1.0, 2022
		send requests/comments to hohle@genzentrum.lmu.de
-----------------------------------------------------------------------------------------------------


1) running the executable

We compiled an executable (.exe) in order to provide a stand-alone software. You can 
directly use the ANNICAS.exe without having Matlab installed on your computer. 
Running just the executable is what one would do in the day-to-day lab routine.
ANNICAS.exe is compiled for Windows 10.

What does ANNICAS.exe do? 	Checking a source folder whether a new image has been 
				generated (i.e. by a microscope). If there is a new image
				in the source folder, ANNICAS.exe analyzes the image
				and saves the analyzed image in a target folder.

Requirements			Matlab runtime version 9.11. It is freely available
				here: 
				https://www.mathworks.com/products/compiler/matlab-runtime.html

				> 2GB RAM

				moderate GPU(s) 

Some info			ANNICAS.exe checks automatically for avaiable GPUs and 
				parallelizes the computation process (Matlab is inherently
				optimized for operations involving matrices and vectors).
			 	After launching the .exe, it might take a few seconds before
				it is available since the network has to be loaded into the
				memory. 
 
Installation:			a) Download and install Matlab runtime version 9.11.

				(https://www.mathworks.com/products/compiler/matlab-runtime.html)
				
				Follow the instructions provided by the user interface.
				This proces should take only a few minutes. You need to do that
				only once. Note: you don't need Matlab on your
				computer for the runtime.

				b) Download and unpack ANNICAS.zip in desired folder 
				(let's call it the "ANNICAS folder"). The executable is now
				ready to go!

Usage:				a) Create a source folder and a tagret folder

				b) Open your Windows command prompt

				c) Navigate to your "ANNICAS folder", i.e.
				cd C:\Users\my_stuff\ANNICAS\

				d) launch ANNICAS.exe in the Windows command prompt
				by providing the source folder and the target folder
				as input arguments

				ANNICAS.exe "C:\Users\my_stuff\Source" "C:\Users\my_stuff\Target"
				
				As soon as the ANNICAS icon shows up, the software is ready.
				Image analysis should take only a few seconds if your computer
				has at least 2GB RAM and moderade GPUs.

				Note 1: Without GPUs the process might take up to 20sec!

				Note 2: ANNICAS.exe will always run in the background in order
				to be ready for the next image (therefore, the networks have
				to be read into the memory only once). During this stand-by mode,
				ANNICAS.exe requires only little computational resources and 
				does not affect your computer's performance. 
				You can turn off ANNICAS.exe by closing the command prompt

				Note 3: Ideally, you put ANNICAS.exe in the auto start of your
				computer so that it is always available and the average user
				doesn't need to worry about these details.

				Note 4: ANNICAS.exe reads all kinds of image formats (.jpeg, .tif
				etc) but does not check for non-image formats. Therefore, don't 
				move files of any non-image format to the source folder while
				ANNICAS.exe is being executed. 




--------------------------------------------------------------------------------------------------

2) compiling the executable from a Matlab .m file

You can modify the executable or compile your own ANNICAS executable. You just need to modify
and run our CompileMyFile.m and/or the main ANNICAS.m. A Matlab license is however required.
Note: compiling the executable from a Matlab .m file is not neccessary, if you just want to
use the software. However, we want you to be able to modify it, if you wish. This section
requires some moderate Matlab programming skills.  

Requirements:			Matlab toolboxes (version 2021b or later)

				- Matlab
				- API Compiler
				- Parallel Processing
				- Support Package for inceptionresnetv2

				Our Matlab subroutines

				- PatternDetect7.m
				- ANNICAS.m
				- CategorizingPixelVals9.m

				Our pre-trained networks

				- inceptionresnetv2_square_detect_637_MBS16_doubleAug_RandomCut.mat
				(for segmentation --> finding the squares)

				- input_Categorizing_PixelVals9.mat
				(for categorization --> categorizing squares)

				
				Optional: our icon

				-logo1.jpg
	  			
				A compiling routine; either your own or use our

				CompileMyFile.m 

				as a start.


Usage:				Open Matlab and run
				
				CompileMyFile.m

				This .m file compiles ANNICAS.m to an executable (see comments
				in the source code).
				When running CompileMyFile.m, the subroutines PatternDetect7.m,
				CategorizingPixelVals9.m and ANNICAS.m need to be in the same 
				folder, as well as the networks 
				inceptionresnetv2_square_detect_637_MBS16_doubleAug_RandomCut.mat
				and input_Categorizing_PixelVals9.mat and optionally the logo file
				logo1.jpg.


CompileMyFile.m just compiles an existing main program, here ANNICAS.m. However, ANNICAS.m itself
determines the properties of the executable. You can create your own executable with user defined 
properties by modifying ANNICAS.m (see comments in the source code). Note, that ANNICAS.m is very 
basic in order to keep it simple and make it easy to understand. 

ANNICAS.m calls CategorizingPixelVals9.m which categorizes the squares detected by PatternDetect7.m

The segmentation itself takes place in PatternDetect7.m (see comments in the source code).




--------------------------------------------------------------------------------------------------

3) training the neuronal network yourself

PatternDetect7.m and CategorizingPixelVals9.m read pre-trained networks that have been
trained with our data set (see paper for details). Due to the limited amount of data, the accuracy
of ANNICAS is currently at 90%. Also, although we have trained the network with all different
grids and resolutions, your experimental setup might differ and therefore you might need to
train our pre-trained networks with your own data in order to improve performance.  
This section requires only basic Matlab programming skills.


a) Segmentation

The function learningSquaresFind2.m trains the segmentation network. You can use it as 
a starting point for your own training (see comments in the source code).
learningSquaresFind2.m generates the input network for PatternDetect7.m.

Requirements:			Matlab toolboxes (version 2021b or later)

				- Matlab
				- Image Processing
				- Computer Vision

				Our Matlab subroutines

				- cutOutRandomly.m
			

				Our pre-trained network

				- inceptionresnetv2_square_detect_637_MBS16_doubleAug_RandomCut.mat
				(for segmentation --> finding the squares)
 
				or 
                             
 				optionally load the fresh inceptionresnetv2 network from Matlab (not
				pre-trained with our data) with

				lgraph = deeplabv3plusLayers(ANNsize, numClasses, 'inceptionresnetv2'); 


We publish our codes in order to make the training process more transparent for the user. Please
send bug reports to hohle@genzentrum.lmu.de.
See comments in the source code for more details. Note, that we ran learningSquaresFind2.m on
a linux machine. Therefore, some commands like "ls" vs "dir" might differ. 


b) Categorization

The function learningPixelVals5.m trains the network (CNN) for categorizing the squares. As training set, 
it requires an Excel file containing a list of these squares (we saved every square as .tif file) in
one column and the corresponding labeling in a second column (see comments in the source code).
Note that these specifications have been adapted to our internal workflow. You might need to modify
the code significantly in order to adapted to your workflow (see section 1) and 3) in the source
code). We optained the best performance using darknet19. 
learningPixelVals5.m generates the input network for CategorizingPixelVals9.m.
Please send bug reports to markus.hohle@berkeley.edu

Requirements:			Matlab toolboxes (version 2021b or later)

				- Matlab
				- Image Processing
				- Deep Learning

				Matlab subroutines (see paper for references)

				- createLgraphUsingConnections.m  
				- findLayersToReplace.m           	
				- freezeWeights.m   

				optionally our pre-trained network 

				- darknet19_pretrained_3800





























