---
title: "Topic Modelling using LDA with MALLET"
date: 2017-06-04
layout: single
classes: wide
author_profile: false
read_time: true
categories:
  - machine-learning
  - nlp
  - topic-modeling
tags:
  - lda
  - mallet
  - topic-modeling
  - nlp
  - machine-learning
---
Machine Learning for Language Toolkit, in short MALLET, is a tool written in Java for applications of machine learning such as natural language processing, document classification, clustering, topic modeling, and information extraction to texts. To learn what MALLET has to offer in detail, [visit this page](http://mallet.cs.umass.edu/index.php).

In this post, we see how we can create topic models from a large collection of unlabeled text documents and use the model to infer topics in new documents.

Topic models use different algorithms to extract *topics* from a *corpus of texts*. MALLET uses Gibbs sampling based implementations of Latent Dirichlet Allocation (LDA), Pachinko Allocation and Hierarchical LDA. Check [this page](https://tedunderwood.com/2012/04/07/topic-modeling-made-just-simple-enough/) if you want to know about topic modeling in detail.

## Setting up MALLET

Go to the MALLET [download page](http://mallet.cs.umass.edu/download.php) and download the latest version of MALLET. At the time of writing this post, the latest version is 2.0.8.

### Installation on Windows

Ideally, unzip MALLET into your *C:*. Your path to MALLET will then be something similar to *C:\\mallet-2.0.8*. **This directory is referred to as the MALLET directory here onwards.** Now you will be able to access MALLET from anywhere on the command prompt using *C:\\mallet-2.0.8\\bin\\mallet*. To avoid typing the full path every time, we can set up an environment variable. To do so, go to *Start Menu > Control Panel > System > Advanced System Settings > Environment Variables*. Under *User variables* section, select *PATH* and click *Edit*. Go to the end of the text, type *;* followed by *C:\\mallet-2.0.8\\bin\\* and click *Save*. Now you will be able to access MALLET with just the *mallet* command. To verify it is working, type the following on the command prompt.

```
> mallet --help
```

You should see a list of MALLET commands.

***Note**: Windows uses backslash (`\\`) as a directory separator while *nix systems use forward slash (`/`). Examples in this post were run on a *nix system (macOS). Hence, forward slash has been used as a directory separator. You should remember to change them to a backslash while running them on Windows Command Prompt.*

### Installation on *nix (Linux, FreeBSD, Mac OS X)

Unzip MALLET. Typically, you would unzip to paths like */usr/local/bin* or */opt*. For this post, I have unzipped to */usr/local/opt/mallet-2.0.8*. **This path is referred to as the MALLET directory here onwards.** To avoid typing the full path every time, we can set up a path variable. To do so, open *~/.bashrc* or *~/.bash_profile* (for *bash* shell) depending upon your distribution and add the following line.

```
export PATH=$PATH:/usr/local/opt/mallet-2.0.8/bin
```

To put the changes into effect, type the following in your shell:

```
$ . ~/[.bashrc | .bash_profile]
```

You can now access MALLET from anywhere. To verify that it works type:

```
$ mallet --help
```

It should list all the MALLET commands.

## Working with MALLET

Topic modeling with MALLET is all about three simple steps:

1. Import data (documents) into MALLET format
2. Train your model using the imported data
3. Use the trained model to infer the topic composition of a new document

In this tutorial, we will use the sample data that comes pre-packaged with MALLET. The sample data is found in *sample-data* directory inside MALLET directory. Before proceeding further, change your current directory to MALLET directory by typing:

```
$ cd [Your MALLET directory]
```

*Note: **tree** command may not be available by default in your system and you might have to install it manually.*

## Importing Data

There are two methods of importing data into MALLET format.

### Importing directories

You would import a directory if the source data consists of many separate files. In this case, each file is considered as one instance. The following command imports all files from a directory *sample-data/web/en* and converts to single MALLET file named *train.mallet* in your current directory.

```
$ mallet import-dir \
--input sample-data/web/en/ \
--output train.mallet \
--remove-stopwords TRUE \
--keep-sequence TRUE
```

Here, options except *input* and *output* are optional. You can also pass more than one directory; directory names should be separated by spaces.

*remove-stopwords TRUE* removes words such as *a*, *an*, *the*, *if* and so on. By default, MALLET's default English dictionary of [stop words](https://en.wikipedia.org/wiki/Stop_words) is used. If you wish to supply your own list of stopwords, which you would customize for your application, you can do so by passing the file name to the *stoplist-file* option. The stoplist contains stop words separated by space, a tab character, or a line break.

The MALLET toolkit requires *keep-sequence* option set to *TRUE* for topic modeling.

To see more options type

```
$ mallet import-dir --help
```

In this tutorial, we are using this method.

### Importing a file

You'd use this method if all of your data is in a single file, one instance per line, in the following format:

```
[instance_name] [label] [text without line breaks]
```

*instance_name* uniquely identifies each instance. For topic modeling, *instance_name* and *label* can be the same.

You'd type the following command.

```
$ mallet import-file \
--input [file_name] \
--output train.mallet \
```

All the options that apply to *import-dir* also apply to *import-file*.

***Note**: If you are importing extremely large file or file collections, you might get 'Exception in thread "main" java.lang.OutOfMemoryError: Java heap space' error. If you encounter this error, you have run into your memory limit which is 1 GB by default. To update the limit, open the file named mallet (or mallet.bat in case of Windows) in 'bin' directory inside mallet directory with a text editor, find the line 'MEMORY=1g' and update the value '1g' to higher values like '2g', '4g' or higher depending on your system's RAM.*

## Training the model

After you have imported documents into MALLET format, you need to build a topic model. The following command takes the file *train.mallet* which we created in the previous section, creates 5 topics *(topics.txt)* and calculates the topic proportion for each instance *(topic-composition.txt)*.

```
$ mallet train-topics \
--input train.mallet \
--inferencer-filename inferencer.mallet \
--num-topics 5 \
--output-topic-keys topics.txt \
--output-doc-topics topic-composition.txt
```

If you open *topics.txt*, you will see 5 lines. In each line, the first number is the topic number, the second number is the indication of the *weight* of that topic and the words following them are the most frequently occurring words that fall into that topic.

*topic-composition.txt* file lists the composition of each instance or document under the topics listed in *topic.txt*. In each line, the first value is the instance number, the second value is an instance or document name and the numbers following are the weight of corresponding topics in *topics.txt*.

To see more options, type

```
$ mallet train-topics --help
```

### Deciding the number of topics

There is no *natural* number of topics. To find a suitable number of topics, we have to run *train-topics* with a varying number of topics and see how the topic composition breaks down. If the majority of the words group to a very narrow number of topics, we need to increase the number of topics. On the other hand, if related words fall under different topics, the setting is too broad and we need to narrow it down by reducing the number of topics.

## Inferring topic composition of new documents

To infer the topic composition of new documents, you first need to import the new documents into MALLET format similar to what we did in the first section.

```
$ mallet [import-dir | import-file] \
--input [directory_name | file_name] \
--output new.mallet \
--remove-stopwords TRUE \
--keep-sequence TRUE \
--use-pipe-from train.mallet
```

Notice the *use-pipe-from* option though. **It is very important that you include this option at this stage.** This option is used to make sure that the new data is compatible with our training data, i.e. new data and training data have the same alphabet mappings.

Finally, the following command infers the topic composition of the new documents and stores it in *new-topic-composition.txt*.

```
$ mallet infer-topics \
--input new.mallet \
--inferencer inferencer.mallet \
--output-doc-topics new-topic-composition.txt
```

To see more options type

```
$ mallet infer-topics --help
```

This will infer the topic composition of new documents and save it to *new-topic-composition.txt*.

Please leave your comments or any query you have in the comment section below. I will be happy to help.
