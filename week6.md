# Progress

## Cross and external validation

I have now implemented cross validation and external validation. For example use 


    workflow( occurMod = 'UKAnophelesPlumbeus', 
              covarMod = 'UKAir',
              procMod = 'BackgroundAndCrossvalid',
              modelMod = 'LogisticRegression',
              outMod = 'PerformanceMeasures'
            )
                            
This uses the (obligatory) A. plumbeus data that is bundled with the package and some NCEP air data from NCEP (that is also bundled). The data is split into the default of 5 sets for crossvalidation. 

Logistic regression is performed on each of the test sets. The model of each is then used to predict the value of the held out data.

Logistic regression is then ALSO performed on the full dataset. The model module then passes on the model trained on the full dataset and a dataframe with lat, lon, true value, number indicator for crossvalidation fold, predicted value and the environmental data.

Finally, a PerformanceMeasures is a module that calculates AUC, kappy, sensitivity, specificity etc. 

The crossvalidation is written so that it will automatically run crossvalidation if the dataframe from the process module suggests there are more than 1 group of data. The number 1, 2, 3... indicate groups for cross validation. 0 indicates external validation data that is not ever used to train the data.

## Output

We now have a couple of output options.

    workflow( occurMod = 'UKAnophelesPlumbeus', 
              covarMod = 'UKAir',
              procMod = 'BackgroundAndCrossvalid',
              modelMod = 'LogisticRegression',
              outMod = list('PerformanceMeasures', 'PrintMap')
            )

This workflow prints a map of the probability surface and calculates the performance measures. I haven't put much thought into printing maps straight to the graphics device when there are multiple maps to be printed (e.g. when two models were run). Currently one map is printed, and then the second replaces the first.

A module 'SurfaceMap' saves maps to a specified directory and this DOES work with multiple models.

## Documentation

Modules are now documented with roxygen2 as well as the main zoon repo. However, using BuildModule you just specificy a couple of arguments with a description of the module and a description of the parameters and it builds the roxygen2 comments for you. Then we just need to run roxygen2 on the modules repo every now and then.

ModuleHelp() is a new function which reads the documentation .Rd file from github and prints it to the console. This is entirely hacked from '?' (i.e. help()) in base R. I will be able to make this read your R options and print to the browser or print to the console depending. But I haven't done that yet.


# Workshop

I now just have a lot to do for the workshop. I'm writing some of the little modules that make zoon much more flexible as I think of them. I also need to think how best to demonstrate Zoon.



# Questions and to dos

## Linking modules
Unfortunately I haven't made the functions to link modules together. This would have been really nice to have done for the workshop but it's a bit trickier than I realised. So there are some glaring workflows that are currently awkward. A common workflow would be to download  occurrence data from GBIF, and load in some external validation data, then run a typical analysis to see how well models trained on the GBIF do at predicting the external data. This isn't currently possible because you can't daisy chain the 'SpOcc' module and the 'LocalOccurrenceData' module.

Similarly I can't write one module that splits the data for crossvalidation and another module that samples background pseudoabsences. So if (as is common) you want to do both of these process you are currently limited to the 'BackgroundAndCrossvalid' module which does both of these on it's own.

## Structured module repo

I wasted a lot of time splitting the module repo into different folders for each type of module (occurrence, covaraiate etc.) I realised that this irreperably broke the module documentation so I removed it. 





