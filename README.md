# Data Quality Assessment Tool

The scripts included in this repository can be used as a way to audit the quality of a dataset through 5 dimensions. Currently the focus is on data generated by IoT sensors, specifically used in the context of assessing various aspects of smart cities, such as AQM, ITMS, and Bangalore Ambulance Data.

For each of the dimensions that are used to quantify the quality of a dataset, the tool aims to provide a metric score between 0 and 1, where 1 is the highest possible score, indicating a 100% score.
Currently, the tool is able to assess five parameters of these dimensions, namely:

- Format Adherence
- Field Completeness
- Additional Attribute Presence
- Inter-Arrival Timeliness
- Duplicate Presence

A note to remember is that each dataset has a linked JSON Schema that defines the attributes that can be present in the dataset, whether there are any required attributes, and what units and datatypes these attributes need to have.

### Format Adherence

This metric serves to compare the dataset with the JSON Schema that it is linked to, and checks each attribute to see if the format present in the dataset matches the format specified in the Schema. The output is a score between 0 and 1, where 1 is the highest possible score, indicating that the dataset is 100% adherent to the format.

### Field Completeness

This metric serves to compare the dataset with the JSON Schema that it is linked to and checks whether all the attributes marked as 'required' in the schema are present in the dataset. The output is a score between 0 and 1, where 1 is the highest possible score, indicating that the dataset is 100% complete.

### Additional Attribute Presence

This metric serves to compare the dataset with the JSON Schema that it is linked to and checks whether the Schema limits the presence of additional attributes and whether there are any additional attributes not mentioned in the Schema present in the dataset. The output is a score between 0 and 1, where 1 is the highest possible score, indicating that there are no additional attributes present.

### Duplicate Presence

This metric serves to check two columns that are input by the user for any duplicate values in the dataset. A value is considered to be a duplicate if both columns contain the exact same values for any data packet. Here, it is important to choose the appropriate combination of columns to correctly find values that are duplicates. The output is a score between 0 and 1, where 1 is the highest possible score, computed by calculating (1 - r), where r is the ratio of duplicate data packets that were found in the dataset when compared to the total number of data packets.

### Inter-Arrival Timeliness

This metric calculates the difference between the data packet timestamps and plots a histogram of these time deltas for all sensors in a particular time period. The output report shows the metric score, which is a score from 0 to 1, 1 being the highest possible score. This indicates that there are no data packets that are sending data with a time delta of lesser than or greater than the (*mode +/- alpha x mode*). Here, alpha is a user-defined constant - appropriate alpha selection will enable the user to understand the spread of the inter-arrival times from the mode. Along with the metric score, the script also outputs a plot of the time deltas in a '.pdf' format, as well as the the mean, standard deviation, and mode of the time deltas of any dataset.

## How to Run the Tool
Prior to running the tool, ensure that the IUDX SDK is installed on your computer using the following command.

```console
pip install git+https://github.com/datakaveri/iudx-python-sdk
```
Once inside the directory where the repo was cloned, run:
```console
pip install .
```
### Running the tool
Clone the repo from:

``` console
git clone https://github.com/novoneel-iudx/data-quality-assessment.git
```

### Required libraries and packages
Once in the scripts folder, run the following command to install the package and library dependencies:

```console
pip install -r requirements.txt
```

Present in the *config* folder is a config file in *JSON* format with the name of the dataset prepended to it. This file requires one to input the name of the datafile as well as select the attributes that one would like to check for duplicates. In this case, the name of the datafile and the appropriate attributes for selection are already included in the file as below: 
- *observationDateTime*
- *id* for AQM data & *trip_id* for ITMS data

Present in the *data* folder in the repository is a sample dataset of ITMS data from Surat, as well as a sample dataset of AQM data from Pune. Inside the *schemas* folder are the corresponding schemas for these datasets. In order to assess the quality of these datasets, the scripts can be run in the order below with included system arguments:

```console
python3 DuplicationDetection.py <config file name>
```
```console
python3 InterArrivalTime.py <config file name>
```
```console
python3 FormatValidation.py <config file name>
```

Ensure that the datasets are in *.csv* and *JSON* format and are located in the *data* folder.

The output report file will be generated in a *JSON* format and and a *.pdf* format and will be saved in the outputReports folder. All the scripts will append their results to the same output file.
