---
layout: post
title:  "Added New Exercise: Reading & Writing data files"
date:   2021-06-22 14:44:00 -0400
categories: physics jupyter Julia Python data
---


It seemed to take forever, but I have added an exercise in writing and reading data files. This will be used in our junior/senior laboratory course called iLab (Intermediate Physics Lab). On github, you will see a python version:

	- using np.savetxt and np.loadtxt from numPy
	- using matplotlib to plot the data
	
and a julia version:

	- using writedlm and readdlm from the DelimitedFiles.jl and CSV.jl packages
	- using Plots.jl with the GR() backend. 
	
Again, only the questions are publicly posted; solutions are available for instructors. 

Here are nbviewer links to the questions:

 [ReadWriteJulia](https://nbviewer.jupyter.org/github/paulnakroshis/ComputationalPhysicsQuestions/blob/main/Experimental_Physics/ImportingData/ReadWriteJulia/ReadingWritingData-julia.ipynb)

[ReadWritePython](https://nbviewer.jupyter.org/github/paulnakroshis/ComputationalPhysicsQuestions/blob/main/Experimental_Physics/ImportingData/ReadWritePython/ReadingWritingData-py.ipynb)

Further exercises will be forthcoming; next up will be fitting data with uncertainties.