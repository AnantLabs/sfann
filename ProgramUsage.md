

# Data format #

Sfann accepts two data format: the native Fann data format and the [Icsiboost](http://code.google.com/p/icsiboost) data format.

## Native Fann data format ##

```
num_train_data num_input num_output
inputdata seperated by space
outputdata seperated by space
[...etc...]
inputdata seperated by space
outputdata seperated by space
```

Head of a file containing 240 examples for a MLP with 5 inputs and 3 outputs:

```
240 5 3
-1 -2 10 13 -7
-1 1 -1
19 -6 -13 22 5
1 -1 -1
[...etc...]
```

## Icsiboost data format ##

Please read the excellent documentation in the [Icsiboost's documentation](http://code.google.com/p/icsiboost/wiki/FileFormats).

Currently, Sfann supports the fallowing Icsiboost classifiers:
  * Continuous: direct use of the value on a MLP input neuron
  * Labels: one input neuron is created for each label; for a given example, the values are -1 except for the activated labels.
<a href='Hidden comment: 
* Text (comming soon): one input neuron is created for each different word; for a given example, the values are the frequency of the words in the supplied text.
'></a>

# Usage examples #

## Training a MLP ##

For training a MLP with 50 artificial neurons in the hidden layer on the data in mytrain.fanndata, and saving in maxdev.ann the MLP that performs the best on mydev.fanndata :

```
  ./sfann --do-training --train mytrain.fanndata --dev mydev.fanndata --num-hidden 50 --save-max-dev maxdev.ann
```

## Running a MLP ##

For running the MLP saved in the file maxdev.ann on the data mytest.fanndata and saving the results in mytest.result.fanndata :

```
  ./sfann --do-running --load-ann maxdev.ann --test mytest.fanndata --save-loaded-run mytest.result.fanndata
```

## Cross-validating a MLP ##

For performing a 50-fold cross-validation with the data mytrain.fanndata and using 2 folds as dev corpus used for selecting the MLP at each run, with 50 artificial neurons in the hidden layer of the MLP :

```
  ./sfann --do-cross-validation --train mytrain.fanndata --num-hidden 50 --cv-num-folds 50 --cv-num-dev 2 
```

The output of Sfann should look like this:

```
 ->  Reading toto.names ... Ok !
 ->  Reading toto.data ... Ok ! (63 examples)
 ->  Reading toto.dev ... Ok ! (32 examples)
 ->  Creating cross-validation folds... 63 into 50 folds = 2 / fold... Ok !
 ->  Validation number 1 ...
     - creating corpus couple ...  test:2 dev:4 train:57... Ok !
     - training ... Ok !
     - classif. rate for this iteration :
-> Classif. rates of the best train MLP: train-mse=0.00  dev-ccr=100.00 %  test-ccr=100.00 %  
-> Classif. rates of the best dev MLP  : train-mse=0.00  dev-ccr=100.00 %  test-ccr=100.00 %  
-> Classif. rates of the best test MLP : train-mse=0.00  dev-ccr=100.00 %  test-ccr=100.00 %  
 ->  Validation number 2 ...
[...etc...]
 ->  Validation number 50 ...
     - creating corpus couple ...  test:1 dev:4 train:58... Ok !
     - training ... Ok !
     - classif. rate for this iteration :
-> Classif. rates of the best train MLP: train-mse=0.00  dev-ccr=100.00 %  test-ccr=100.00 %  
-> Classif. rates of the best dev MLP  : train-mse=0.25  dev-ccr=100.00 %  test-ccr=100.00 %  
-> Classif. rates of the best test MLP : train-mse=0.25  dev-ccr=100.00 %  test-ccr=100.00 %  
 => Overall classif. rate :
-> Classif. rates of the best train MLP: train-mse=0.00  dev-ccr=100.00 %  test-ccr=100.00 %  
-> Classif. rates of the best dev MLP  : train-mse=0.17  dev-ccr=100.00 %  test-ccr=100.00 %  
-> Classif. rates of the best test MLP : train-mse=0.20  dev-ccr=94.44 %  test-ccr=100.00 %   
```

The lines starting by "-> Classif. rates of the best `<corpus>` MLP:" reports the performances on all the corpora of the MLP that performs the best on the corpus `<corpus>`. For a definition of CCR and MSE, please read the [FAQ](FAQ#What_does_CCR_and_MSE_stand_for_?.md).

The overall performances of the MLP are given after the line " => Overall classif. rate :".

# Program options #

Here are the options available in the [revision 16](https://code.google.com/p/sfann/source/detail?r=16):

```
Usage : sfann <action> [options]

Generic options:
  -h [ --help ]         prints this help message
  -v [ --verbose ]      verbose outputs

Action to be performed:
  --do-cross-validation  Perform a cross-validation on the train corpus
  --do-training          train an ANN using the specified train/dev/test 
                         corpora
  --do-running           run the specified ANN on the specified test corpus
  --do-nothing           does not train ANN, only load data and generate 
                         corpora

Data-related options:
  -S [ --stem ] arg     Icsiboost-style data location: <stem>{.names,.data,.tes
                        t,.dev} (Icsiboost data format)
  -t [ --train ] arg    data file containing the training documents (fann data 
                        format)
  -d [ --dev ] arg      data file containing the development documents (fann 
                        data format)
  -s [ --test ] arg     data file containing the test documents (fann data 
                        format)
  -a [ --auto-dev ] arg automatically construct a dev corpus with <arg>% of the
                        train
  --save-dev arg        save the automatically build development corpus

ANN Specifications:
  -l [ --num-hidden ] arg Size of the hidden layer

General training options:
  -r [ --randomize-data ]      Randomize the order of the elements in the data 
                               vectors before training
  -i [ --clever-init ]         Use the Widrow and Nguyen's algorithm for 
                               initialize weights
  --reports arg (=100)         Number of epoch between reports
  --max-epoch arg (=5000)      Max epoch
  --desired-error arg (=0.001) Desired error
  -n [ --num-runs ] arg (=1)   Number of training/testing cycles for finding 
                               the ANN that perform the best on the Dev

Do-cross-validation specific options:
  --cv-num-folds arg (=10) specify the number of folds (k-folds) for cross 
                           validating on the train corpus
  --cv-num-dev arg (=1)    specify the number of folds used for the dev corpus
  --cv-shuffle             shuffle data before creating folds
  -o [ --leave-one-out ]   use leave-one-out validation on the train corpus

Do-training specific options:
  --save-max-dev arg       save the ANN that obtains the best score on the dev
  --save-max-train arg     save the ANN that obtains the best score on the 
                           train
  --save-max-test arg      save the ANN that obtains the best score on the test
  --save-max-test-run arg  save the results on the test corpus of the best test
                           ANN
  --save-max-dev-run arg   save the results on the test corpus of the best dev 
                           ANN
  --save-max-train-run arg save the results on the test corpus of the best 
                           train ANN

Do-running specific options:
  --load-ann arg        load the specified ANN for classifying the test data
  --save-loaded-run arg save the results on the test corpus of the ANN loaded 
                        with --load-ann



```