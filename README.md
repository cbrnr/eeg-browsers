## EEG browsers

### Introduction

This document provides an overview of [free and open-source software](https://en.wikipedia.org/wiki/Free_and_open-source_software) EEG browsers. I only include tools written with freely available programming languages here, which excludes everything written in MATLAB.


### Definition

An EEG browser is an application which visualizes certain aspects of [electroencephalographic](https://en.wikipedia.org/wiki/Electroencephalography) (EEG) or similar modalities. Such signals are basically a collection of time series recorded simultaneously from different locations. At the very least, interactively visualizing the time course of these signals is one of the core features of any EEG browser.


### Core features

Of course, the effectiveness of an EEG browser depends critically on how and which visualization features are implemented. If users can smoothly scroll and zoom through the data in both time and channels, assessing data quality is much easier than when only whole pages of displayed data are updated. Displaying a large quantity of data points smoothly is not trivial.

For example, a typical segment of interest consists of dozens of channels and several seconds of data with a sampling frequency of 512 Hz or higher. This amounts to roughly 300,000 data points that need to be rendered simultaneously. Scrolling forward or backward in time means that new data needs to be visualized almost instantaneously for a smooth user experience. In most cases, this requires some sort of downsampling to process such large amounts of data. In fact, there are only a limited number of pixels available to display the data, so for example drawing 10,000 data points in a window consisting of 1,000 pixels horizontally is both inefficient and not necessary.

In addition to displaying the raw EEG signals, it is also important that EEG browsers support creating and editing annotations. An annotation is a time segment in the data associated with a specific start and stop time, a specific channel (or all channels), and a label (such as "artifact"). An EEG recording can have multiple, possibly overlapping, annotations. An EEG browser needs to support creating annotations (for example using mouse actions such as dragging over a specific data segment), editing and deleting annotations, as well as showing and hiding all or specific types of annotations.

Online data streaming is a bonus feature that some users might find useful to monitor incoming EEG data (such as when recording with [LSL](https://labstreaminglayer.readthedocs.io/index.html)). However, it might be preferrable to have dedicated data streaming viewers for such applications instead of extending EEG browsers with these capabilities.


### Data formats

There are dozens of file formats for EEG data. [MNE](https://mne.tools/stable/index.html) (a widely used Python package for EEG/MEG analysis) supports most widely used formats – which means that Python-based EEG browsers can easily import these formats. Once imported, EEG data can be represented as an array of floating point numbers (double precision in most cases).

The most commonly used formats are:

- [EDF](https://www.edfplus.info/) (European Data Format, the probably the most widely used format)
- [BrainVision](https://www.brainproducts.com/support-resources/brainvision-core-data-format-1-0/) (mainly used by Brain Products amplifiers)
- [EEGLAB](https://eeglab.org/tutorials/ConceptsGuide/Data_Structures.html) (basically a MATLAB file with specific data structures)
- [BDF](https://www.biosemi.com/faq/file_format.htm) (mainly used by Biosemi amplifiers, based on and similar to EDF)

These formats are also supported by the [BIDS EEG standard](https://bids-specification.readthedocs.io/en/stable/04-modality-specific-files/03-electroencephalography.html), and they specifically recommend to use EDF or BrainVision whenever possible.


###  Test data

The [MNE testing data repository](https://github.com/mne-tools/mne-testing-data) contains test files for many formats. Specifically, here are some files for the four formats discussed previously:

- The EDF format uses only one file ([test_edf_stim_resamp.edf](https://github.com/mne-tools/mne-testing-data/raw/master/EDF/test_edf_stim_resamp.edf))
- The BrainVision format consists of three files ([test_NO.vhdr](https://raw.githubusercontent.com/mne-tools/mne-testing-data/master/Brainvision/test_NO.vhdr), [test_NO.vmrk](https://raw.githubusercontent.com/mne-tools/mne-testing-data/master/Brainvision/test_NO.vmrk), [test_NO.eeg](https://github.com/mne-tools/mne-testing-data/raw/master/Brainvision/test_NO.eeg))
- The EEGLAB format can consist of one ([test_raw_onefile.set](https://github.com/mne-tools/mne-testing-data/raw/master/EEGLAB/test_raw_onefile.set)) or two files ([test_raw.set](https://github.com/mne-tools/mne-testing-data/raw/master/EEGLAB/test_raw.set), [test_raw.fdt](https://github.com/mne-tools/mne-testing-data/raw/master/EEGLAB/test_raw.fdt))
- The BDF format is almost identical to EDF ([multiple_annotation_chans.bdf](https://github.com/mne-tools/mne-testing-data/blob/master/BDF/multiple_annotation_chans.bdf))


### Available apps

- [SigViewer](https://github.com/cbrnr/sigviewer) (Windows/macOS/Linux): Written in C++/Qt. Supports multiple file formats. Smooth scrolling. Currently requires that all of the data is loaded into memory.
- [MNE](https://mne.tools/stable/index.html) (Windows/macOS/Linux): Written in Python. Based on Matplotlib. Supports multiple file formats. Chunky scrolling (page-based). Should also work when data has not been loaded completely into memory.
- [MNE-Qt-Browser](https://github.com/mne-tools/mne-qt-browser) (Windows/macOS/Linux): Written in Python, based on [PyQtGraph](https://www.pyqtgraph.org/). Works as an alternative browser backend in MNE, which means it supports the same file formats. Relatively smooth scrolling and zooming (but this depends on the platform and settings such as OpenGL). Requires all data to be available in memory.
- [EDFbrowser](https://www.teuniz.net/edfbrowser/) (Windows/macOS/Linux): Written in C++/Qt. Supports only EDF files. Relatively smooth scrolling. Not officially supported on macOS.

All four browsers use Qt in the background, sometimes directly (SigViewer, EDFbrowser), and sometimes through other packages (PyQtGraph and Matplotlib). Qt does a pretty good job in abstracting away platform idiosyncrasies, but sometimes differences in looks and/or behavior still crop up. This means that if these tools want to support all three major platforms (Windows/macOS/Linux), developing and testing on all three platforms cannot be avoided completely.


### Web-based EEG browsers

This is where web-based tools (running in a modern browser) might come in handy.
