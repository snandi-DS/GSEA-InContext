
GSEA-InContext: Gene Set Enrichment Analysis In Context
========

Update: Our paper was published in *Bioinformatics* as a Proceedings paper for the 2018 ISCB Conference on Intelligent Systems for Molecular Biology. `Read it here <https://academic.oup.com/bioinformatics/article/34/13/i555/5045793>`_ for details on GSEA-InContext and some example applications of the method. We are also honored to announce that our paper was selected for the `Ian Lawson Van Toch Memorial Award for Outstanding Student Paper <https://www.iscb.org/ismb2018general/ismb2018-awardwinners>`_! 

Gene Set Enrichment Analysis (GSEA) is routinely used to analyze and interpret coordinate changes in transcriptomics experiments. For an experiment where less than seven samples per condition are compared, GSEA employs a competitive null hypothesis to test significance. A gene set enrichment score is tested against a null distribution of enrichment scores generated from permuted gene sets, where genes are randomly selected from the input experiment. Looking across a variety of biological conditions, however, genes are not randomly distributed with many showing consistent patterns of up- or down-regulation. As a result, common patterns of positively and negatively enriched gene sets are observed across experiments. Placing a single experiment into the context of a relevant set of background experiments allows us to identify both the common and experiment-specific patterns of gene set enrichment. We developed the GSEA-InContext method to allow a user to account for gene expression patterns within a defined background set of experiments to identify statistically significantly enriched gene sets in their own experiment.

See below for examples on running the GSEA-InContext algorithm.

This repo is a fork of `GSEApy <https://github.com/BioNinja/GSEApy>`_ (`original documentation here <http://gseapy.rtfd.io/>`_). We have added a new tool ``GSEA_InContext`` which runs the GSEAPreranked algorithm but uses a background set of ranked lists to calculate an empirical null distribution for informing the permutation procedure. For examples using the original GSEApy library, `visit this page <http://gseapy.readthedocs.io/en/master/gseapy_example.html>`_.


About GSEA-InContext
--------------------------------------------------------------------------------------------

Currently, there are no methods available for a user to easily compare their GSEA results to GSEA results obtained in other experiments to discern similar and/or distinct patterns affected across experiments. GSEA-InContext accounts for gene-specific variation estimated from an experimental background. Whereas GSEA identifies all signiificantly enriched gene sets in an experiment, this method allows the user to ask a complementary question; namely, which gene sets are uniquely enriched in a single experiment compared to many other, independent experiments.

Our method applies the same approach as GSEA to calculate the nominal p-value. However, in contrast to GSEAPreranked, GSEA-InContext employs an alternative significance testing procedure to generate the null distribution, in which permuted gene sets are generated using the density of gene ranks estimated from a set of user-defined background experiments. We estimate a gene's probability density using a Gaussian kernel over the experiments in the background set.

The GSEA-InContext algorithm can be run using the ``incontext`` subcommand. Additional subcommands can be run as in the original GSEApy, including: ``gsea``, ``prerank``, ``ssgsea``, ``replot`` ``enrichr``. See the original `GSEApy <https://github.com/BioNinja/GSEApy>`_ repository.

The full ``GSEA`` is described in:
`GSEA  <http://www.broadinstitute.org/cancer/software/gsea/wiki/index.php/Main_Page>`_ documentation. All files' formats for GSEApy are identical to ``GSEA`` desktop version.


Data & Availability
---------------------

The data, results and analysis described in `our preprint <https://www.biorxiv.org/content/early/2018/02/04/259440>`_ are hosted in a Synapse project available `here <https://www.synapse.org/GSEA_InContext>`_ (doi:10.7303/syn11804693).

Create the Conda Environment
-----------------------------
conda env create -f environment.yaml

Adding a Python Environment Kernel in Jupyter Notebook
----------------------------------------------------------

To use a specific Python environment in Jupyter Notebook, you need to install the `ipykernel` package and add the environment as a Jupyter kernel. Follow these steps:

 Run the following command in bash: pip install ipykernel

Add the Environment to Jupyter
---------------------------------
Once ipykernel is installed, you can add the environment to Jupyter as a kernel. Run the following command:
python -m ipykernel install --user --name=<Python environment_name>



Dependencies & Requirements
--------------
* Python 3.9+
* Numpy
* Pandas
* Matplotlib
* Beautifulsoup4
* Requests (for enrichr API)

You may also need to install lxml and html5lib to parse xml files.


Running GSEApy and GSEA-InContext
-----------------

Before you start:
~~~~~~~~~~~~~~~~~~~~~~

Convert all gene symbol names to uppercase. The ranked lists input to ``prerank`` or ``incontext`` can be supplied as file paths (.rnk) or a two-column Pandas DataFrame (columns ``gene_name`` and ``fold_change``). The background ranked lists input to ``incontext`` is supplied as a text file containing the list of .rnk files to use in permutation, or as a .csv file containing pre-permuted gene lists created with the ``make_background_dist()`` function.


Run GSEAPY inside Python console:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

| Running GSEAPreranked and GSEA-InContext in Python using file paths as input

.. code:: python

    import gseapy

    # Run GSEA Prerank
    gseapy.prerank(rnk='gsea_data.rnk', gene_sets='gene_sets.gmt', outdir='out')

    # Run GSEA-InContext
    gseapy.incontext(rnk='gsea_data.rnk', gene_sets='gene_sets.gmt', backround_rnks = 'permuted_background.csv', outdir='out')

A full example can be seen in ``run_example.ipynb``. The full analysis of Kegg and Hallmarks gene sets was run with ``run_all_442.py``.


Bug Reports
~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you would like to report any bugs when you running the ``incontext`` module, please create an issue on GitHub here. For issues relating to other modules, you may wish to visit the `original GSEAPY repo <https://github.com/BioNinja/GSEApy>`_.
