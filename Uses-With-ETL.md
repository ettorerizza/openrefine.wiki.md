_Is OpenRefine an ETL tool ? or can it be used in batch operations or job control ?_

OpenRefine is a discovery and cleanup tool mostly, as it stands now at release 2.5. It also has the benefit of aligning, reconciling, and exporting data, to Freebase or somewhere else. Some folks in the community have expressed the idea or need for batching transform operations or evolving OpenRefine into an automated pipeline process.

One feature of OpenRefine that folks find intriguing is it's ability to display inconsistencies in your data and clean it up easily.

Usage with Enterprise data or large databases,

- For instance, you might leverage OpenRefine to analyze and find faults in the output of some of your stored procedures within an existing database and then fix them outright.
- Or, you might make slight job changes inside your program or ETL tool after you discover (using OpenRefine) that your output isn't always correct, and the job needs to be modified.

Also, if your interest is transforming large amounts of data, you might also want to look at existing database ETL tools (Extract, Transform, Load) which some we have listed here: [Related Software](Related+Software)

( Currently, using OpenRefine with extremely large amounts of data and millions of rows does require plenty of RAM and Java 64 bit, so you might find a hard time absorbing and utilizing, for example, a 1 billion row Enterprise database table. )

We're interested in your ideas and comments ! and patches are always welcome !

