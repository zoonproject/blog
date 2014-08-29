# Progress

Progress has been a little slow this week what with the Bank holiday. Also, most of work done has been under the hood stuff that is time consuming and uninteresting in the sense that it doesn't add new features.

## Environments

I spent quite a while working out how environments work in R and then fixing the package so it uses them more correctly. Before, a number of functions were saved into the global environment. Modules, which are each just a module, were put into the global environment. Further more, the [BiomodModels](https://github.com/zoonproject/modules/blob/master/R/BiomodModel.R) module creates a predict method for the biomod model class. This was also saved to the global environment.

All these objects are now saved into the environment of the workflow function call. This keeps the global environment clean and tidy.

## Tests

I have finally gotten around to setting up and writting unit tests for the package. This are written with the [testthat](http://r-pkgs.had.co.nz/tests.html) package. I have now written tests for most of the functions in zoon as well as some whole workflow tests. I am still not very clear on what a good strategy for testing functions with complex outputs is (e.g. workflow()). At the moment I am:
- testing all the internal functions that create the complex output
- testing each bit of the complex output object

In one case, I am testing the function by running it in two ways which should give the same output and then testing the equality of the entire output object. This seems messy to me. But anything less complete feels at tisk of missing a bug.

I am also struggling to write tests for the GetModuleList() function as it requires browser authentification.

I will try looking at other packages to continue getting a better idea of how other people test their code.

## Workflow with lists

I am still working on the workflow function so that it accepts lists of modules of each type. This is quite involved so it is not finished yet. After discussion we decided that giving a list of covariate modules should create parallel analyses in the same way lists of occurrence or model modules would. I will write a function to combine covariate datasets from different modules although apparently this could be a bit tricky, getting the extents the same etc.

Just thinking about this now, I wonder whether this should be written to accomodate multi-threaded processing from the beginning. But probably not. As it is being written with *apply functions, it should be fairly simple to swap in the multicore versions later.

# Discussions and decisions.

## Cross validation

We decided that the best way to implement k fold cross validation is not to treat the k folds as k seperate analyses, and then join them later, but to have an extra column in the dataframe that gets passed from process modules to model modules with an integer indicating the fold. This column could also usefully indicate data that is external validation data. 

## Documentation

For now documentation will be a simple search to online documentatiaon. This should be fairly simple. I can make the BuildModule() function so that it requires some information on the module when it is built as well.

# Future work

As usual we have a lot of stuff we would love to add. Currently the priorities are making the package accept lists of modules, good documentation and cross validation. We have then discussed adding [INLA](http://www.r-inla.org/) (although apparently this isn't straight forward), [worldclim](http://www.r-inla.org/) data, and quite a range of outputs as there is currently almost no output.










