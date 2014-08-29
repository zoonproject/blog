# Progress

## Extra modules

I have written a number of new, more flexible modules. 

### NPEC climate data
Access NPEC data. Request as many variables as you wish.

### Species Occurrence
Use rOpenSci's [spocc](https://github.com/ropensci/spocc) package to collect occurrence data from a number of databases (GBIF, bison, iNaturalist etc.) However there does seem to be some problems with the package so there are often errors when using more than one database at the same time.

### BIOMOD2 wrapper

A module that wraps the main modelling function of biomod2 allowing access to many models (GLM, GAM, maxent etc.). This presented an interesting issue in that so far I have been working under the assumption that a model module will return an object whose class has a predict method. This way, output modules can all use the output from any model module. However, the BIOMOD2 model class has its own function for predicting from the model. I chose to write the module so that it saves a predict method for the biomod2 class as well as running the models. I'm not totally sure this is ideal but seems best for now. It makes the writing of modules much less easy if they are wrapping models without predict methods already written.

## Find modules

I have written a function GetModuleList() which uses the github API to get a list of modules that are in the github repo. If we end up keeping modules somewhere else then this function will be obsolete, but it's useful for now and I imagine it will be useful for the workshops etc.



# Current work

## Taking multiple modules
I am currently working on rewriting the main workflow function to allow for multiple modules of each type to be handled. For example, I might want to pass a list of species names and then get a SDM for each species. Or pass a list of model modules and have a species be modelled by each. There are different ways that people might want to use multiple modules however. The above examples are what I call parallel. They split the analysis and then each analysis continues on separately. I imagine people may want to use process modules either in parallel or in series i.e. clean the data, then create pseudo absence points, then split into training and validation sets. This will be done with a separate DaisyChain function.

So I these are currently how I think this will work. Giving a list of modules will behave differently depending on the module type.

+ List of occurrence => parallel
+ List of covariate => add all layers into one stack
+ List of processes => parallel
+ DaisyChain function => processes in series
+ List in daisyChain => parallel
+ List of models => parallel
+ List of output => parallel

I haven't thought all of these through fully. I think we might have to restrict is to only allowing one one list. It is not clear what multiple lists would mean. 

# Questions

## Documentation

I am starting to think about how best to document the modules. The way the package currently works, the modules are held in an online repository and only downloaded when needed. This means there isn't documentation before a module is run in a workflow and currently there isn't documentation even then. I'm not sure how best to fix this.

## Cross validation

One of the major things that needs to be implemented is cross validation. In term of the workflows, I think this is best represented as a process module that takes the occurrence data and splits it. This makes cross validation more analogous to validation with a separate dataset. It also allows flexibility in how cross validation is used; there are different ways of splitting the data for example such as spatially, a simple random sampling or others.

Cross validation typically splits the data into k parts and trains the model on k-1 parts of the data and then tests on the witheld data. This then would be thought of as splitting the data and running k parralel analyses. However, the data should then be regrouped so that output modules can run on the output of all folds together. However it is not clear how this will work as the process module containing the cross validation only affects the data before the models. But the output modules shouldn't need to all be able to recombine the cross validated data.


# Conclusions

So that is the progress so far. I have largely been fleshing out the package and dealing with or thinking about problems as they arrise.




