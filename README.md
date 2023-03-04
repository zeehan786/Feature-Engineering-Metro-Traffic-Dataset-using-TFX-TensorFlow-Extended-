# Feature-Engineering-Metro-Traffic-Dataset-using-TFX-TensorFlow-Extended-

TensorFlow Extended TFX is a platform for building end to end machine learning pipelines. 

Advantages of TFX: With TFX, a developer can ingest and store large amount of datasets in an efficient format (TF Record), efficiently analyze the dataset, check for any underlying incorrectness such as null values, derive useful statistics from the data, perform efficient feature engineering and finally monitor and launch machine learning system into production. In the project, I have used TFX components to perform the necessary feature engineering.

Moreover, several TFX libraries can be integrated with Apache Beam for processing data in large amounts. With Apache Beam, TFX can provide a high degree of scalibility and parallelism across distributed systems.

Following are list of TFX components that deals with feature engineering (i.e. a critical step before feeding the data into a model):

ExampleGen: The first component of TFX. It is used to ingest large amounts of datasets and convert the data into tf.train.Example format. As tensorflow works with data best when they are in TFRecord file format, ExampleGen stores the data in the ideal format.

(What is TFRecord: TFRecord is a binary file format that takes significantly less disk space, highly optimized for Tensorflow, and therefore extremely efficient when it comes to preprocessing, loading and performing training on the data)

StatistisGen: StatisticsGen is used to derive useful statistics from the dataset. The statistics are used by other components of the TFX e.g. to detect any anomalies.

SchemaGen: This component is used to derive definition of the dataset (schema table). It contains information about the data types, ranges, and other useful properties. The schema table is used by other components such as TFX transform component when performing preprocessing of the data.

ExampleValidator: Used the output results of statistics gen and schema gen to detect any anomalies. One of the major purposes of the component is to compare the incoming (serving data) with the schema table to verify the correctness of the incoming data. Some of the anomalies ExampleValidator can detect are data drift, training-serving skew, and any missing features.

Transform: TFX Transorm performs feature engineering on the tf. Examples produced by the ExampleGen component, using the schema table. It uses a user-implemented "preprocessing_fn" to perform the necessary preprocessing of the data. For example using "tft.scale_to_0_1" to scale the values between 0 and 1 or "compute_and_apply_vocabulary" to generate vocabulary for the string feature by converting the words to unique integer indices. The output of the component includes a transformation graph that is used to preprocess future datasets. Using a transformation graph ensures the serving dataset goes through the exact same order of preprocessing steps as the training set. This prevents any unexpected model performance and training/serving skew.
