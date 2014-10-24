
I am now coming to the end of my internship working on Zoön. I will quickly cover what I have done in three months, then give some recent updates.

## Work achieved

As github gives us some nice summary statistics, I thought I'd put them here as a nice way to underline my time working on the project.

### Main package

~120 commits containing ~3.5 thousand lines of code. The big blob in the middle is the lead up to the workshop. 56 issues opened, 39 closed.


### Modules

~100 commits with ~2 thousand lines of code.


### Features

And just to try and summarise my contributions, here is a list of features

#### Core package

- Github repo set up
- Github repo for modules set up
- Framework for calling packages from the repository
- Include multiple modules either within one analysis or to split analyses for comparison
- Crossvalidation and external validation
- Save current progress of analysis on crash
- Rerun a workflow (or run from break point on crashed workflow)
- Take a workflow, change a few modules and rerun from where stuff is changed
- Automatic module documentation building (not CI), and ability to get module help from R.

#### Select Modules

- Collect data from GBIF and other sources
- Collect environmental data from NCEP and Bioclim
- Wrapper for biomod giving Random Forest, bioclim, Maxent (untested), GAMs and other models
- Basic map plotting
- Validation statistics (AUC, Kappa, sensitivity)
- Upload whole analysis to figshare



## Recent updates

I realise I haven't actually written a blog since the post soon after the workshop. However, a lot of the work since then has been cleaning up code, writing better documentation and code comments, and writing unit tests (now nearly all functions are tested).

However, what has been added is functions to make zoon easier to use interactively for example during development, or for trying out different analyses.

Firstly, workflow now saves the progress so far if it crashes. 

    w <- workflow(SpOcc(species = 'Anopheles plumbeus', extent = c(-10, 10, 30, 50)),
                  Bioclim(extent = c(-20, -10, 0, 10)),
                  OneHundredBackground,
                  LogisticRegression,
                  SameTimePlaceMap)

    tmpZoonWorkflow

As our occurrence datapoints are outside the extent of the covariate data, the workflow crashes. However, the object tmpZoonWorkflow now contains our progress so far. As this includes some data downloaded from online repositories, this might be quite a time saver.

I guess this won't work if R fully hangs (which is entirely possible). Or even if it does work, having tmpZoonWorkflow in the namespace is not very useful if your R session has crashed. So perhaps this should be saving a tempory .RData file after each module. 

There is also functions to rerun or change a workflow. However, they still are underdevelopment (I really wanted to finish these before the end.) They work only for simple cases at the moment.

    # Make a new function that breaks
    breaks = function() stop('B-b-b-breaaak')

    # Uhoh, it broke
    b <- workflow(UKAnophelesPlumbeus, UKAir, breaks, LogisticRegression, PrintMap)  

    # Hooray, a lurid green 'map'!
    b <- ChangeWorkflow(tmpZoonWorkflow, process = OneHundredBackground)            

Similarly, you might want to rerun a workflow. If a workflow breaks because of an internet connection drop or something similar, this might be easier to use than ChangeWorkflow. Otherwise, if a paper publishes an analysis, with a Zoön worklow uploaded to Figshare or Datadryad, the first thing you will want to do is rerun it.

  # Download a workflow. We've got one called 'b' from above so pretend we downloaded that.
  r <- RerunWorkflow(b)

Then we can compare our results with that in the paper. We also now have the data, and model objects so we can fully examine their work if we wish.

## Thanks

So I think that's about all for now. Thanks Greg, Nick and Emiel. I've really enjoyed it and look forward to following the progress of Zoön (I'm sure I'll contribute some modules here and there.)

Tim
[@timcdlucas](www.twitter.com/timcdlucas)



