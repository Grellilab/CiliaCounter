# CiliaCounter

Automated Fiji/ImageJ macro to measure cilia length and incidence. The script is adapted from 10.7554/eLife.33067 to include the ability to calculate cilia incidence. 


run("Z Project...", "projection=[Max Intensity]");
//run("Channels Tool...");
title = getTitle();
run("Split Channels");
selectWindow("C4-" + title);
close();
selectImage("C3-" + title);
run("Subtract Background...", "rolling=5"); 
run("Median...", "radius=3"); 
//run("Gamma...", "value=0.70");
setAutoThreshold("Yen dark");
setOption("BlackBackground", false);
run("Convert to Mask"); 
run("Dilate");
run("Dilate"); 
run("Skeletonize");
run("Set Measurements...", "area perimeter redirect=None decimal=3");
run("Analyze Particles...", "size=0.1-100 show=[Overlay Masks] display exclude clear include summarize add in_situ");
        for(n=0; n<nResults;n++) {
	        setResult("Cilia Length", n, ((getResult("Perim.", n))/2));
        } 
selectWindow("C1-" + title);
run("8-bit");
run("Subtract Background...", "rolling=50");
run("Auto Threshold", "method=Huang");
run("Make Binary");
run("Watershed");
run("Set Scale...", "distance=0 known=0 pixel=1 unit=pixel");
run("Analyze Particles...", "size=L-H pixel circularity=0.70-1.00 show=Nothing display exclude clear summarize");
selectWindow("Summary");
selectWindow("Results");
selectWindow("C3-" + title);
close();
