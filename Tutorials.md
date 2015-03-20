Comming soon.

<a href='Hidden comment: 

<wiki:toc max_depth="2" />

= Getting and Compiling Sfann =

For compiling the latest version of Sfann, you have to install on your system the latest version of the FANN library ([http://leenissen.dk/fann/ available here]), and a recent version of Boost ([http://www.boost.org/ available here]). Now you have to get the sources from the Subversion repository, compile them and copy the Sfann binary somewhere in your PATH:

```
# Getting the sources
svn checkout http://sfann.googlecode.com/svn/trunk/ sfann
# Moving into the source directory
cd sfann
# Configuring the build process
./configure
# Compiling Sfann
make
# Moving the Sfann binary in your PATH
cp src/sfann <somewhere/in/your/path>
```

= Getting the data =

We will use the adult database, which consists in a classification problem where you have to determine the income of a person knowning various facts about him/her.

The Icsiboost-formatted version of the database is available on the UCI repository:

```
http://archive.ics.uci.edu/ml/machine-learning-databases/adult/
```

Getting it with wget:

```
wget http://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.names
wget http://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data
wget http://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.test
```

This Icsiboost-formatted version of the database contains a file describing the different classes and features (adult.names), a file containing training examples (adult.data) and a file containing test examples (adult.test).

==The names file: _adult.names_==

```
>50K, <=50K.
age: continuous.
workclass: Private, Self-emp-not-inc, Self-emp-inc, Federal-gov, Local-gov, State-gov, Without-pay, Never-worked.
fnlwgt: continuous.
...
sex: Female, Male.
capital-gain: continuous.
```

The .names file contains a first line defining a comma separated list of classes ended by a period (here: >50K and <=50K being the classes of income that we try to predict).

Then one feature column is described on every line. The line consists of a feature name (age, workclass, ...) followed by a column and information about values allowed for that feature. Features can be real valued (_continuous_, as for age), space separated words (_text_, not yet supported by Sfann) or a set of nominal values (the values themselves, as for sex).

==The training examples: _adult.data_==

```
39, State-gov, 77516, Bachelors, 13, Never-married, Adm-clerical, Not-in-family, White, Male, 2174, 0, 40, United-States, <=50K.
50, Self-emp-not-inc, 83311, Bachelors, 13, Married-civ-spouse, Exec-managerial, Husband, White, Male, 0, 0, 13, United-States, <=50K.
38, Private, 215646, HS-grad, 9, Divorced, Handlers-cleaners, Not-in-family, White, Male, 0, 0, 40, United-States, <=50K.
```

The training examples are input in a single file containing one instance per line. The different feature columns are comma separated and must appear in the same order and following the same constraints described in the .names file. The last column contains the real class of the example, followed by a period.

Some features can be unknown because of privacy concerns or other restrictions on training data collection. These features use a value of "?" (question mark) and will get a special processing in the training/testing stages.

'></a>