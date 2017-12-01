Most of the idea list are from [https:_groups.google.com/forum/#!searchin/openrefine/%22feature$20requests%22/openrefine/Qs7vcbdz2zs/SlebN0AX7KMJ_](David%27s+feature+requests+thread)

Considering the length of the GSoC, We will focus on following tasks out of above tasks:

#### pipelining mode using Spark data processing

The idea is to integrate the Apache Spark into the OpenRefine to allow the in-memory parallelism data processing. It will be a light-weight solution for OpenRefine adding the ability to processing the big data. There already an existing project and need well tested and put into the master: [https://github.com/andreybratus/RefineOnSpark](https://github.com/andreybratus/RefineOnSpark)

#### pipelining mode using the Hadoop

This approach will establish a full-fledged Hadoop cluster including the HDFS as a storage and the MapReduce as the processing engigne. See this repository for some idea: [https://github.com/rmalla1/OpenRefine-HD](https://github.com/rmalla1/OpenRefine-HD)

#### plugin manager

OpenRefine plugin provides the capability to extend the functionality and interact with third party service provider. But installation and management(enable/disable) only can can be archived manually. We want to set up an package management server / repository to manage the plugin distribution and installation like apt-get does. Also the UI will provide a way to interact with the package manager server. Here is another open sourced project we can leverage: [https://f-droid.org/contribute/](https://f-droid.org/contribute/) It’s for android. But the data management can be easily migrate to server a general purpose.

#### Better interface with databases or system

Currently Refine accept only file based data to start a project. This force use to often export data from their system to a csv file before loading them into Refine. Having Refine to support credentialing process and bein able to connect to remote data or system will enable smoother workflow for user when importing data. For example Refine should be able to load data directly from database tables, write back to database tables, exporting SQL commands to update. [https:_groups.google.com/forum/#!topic/openrefine-dev/lRU-tleLYfM_](see+discussion+on+the+list)

