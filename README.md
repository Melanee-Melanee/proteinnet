# ProteinNet
ProteinNet is a standardized data set for machine learning of protein structure. It provides protein sequences, structures ([secondary](https://en.wikipedia.org/wiki/Protein_secondary_structure) and [tertiary](https://en.wikipedia.org/wiki/Protein_tertiary_structure)), multiple sequence alignments ([MSAs](https://en.wikipedia.org/wiki/Multiple_sequence_alignment)), position-specific scoring matrices ([PSSMs](https://en.wikipedia.org/wiki/Position_weight_matrix)), and standardized [training / validation / test](https://en.wikipedia.org/wiki/Training,_test,_and_validation_sets) splits. ProteinNet builds on the biennial [CASP](http://predictioncenter.org/) assessments, which carry out blind predictions of recently solved but publicly unavailable protein structures, to provide test sets that push the frontiers of computational methodology. It is organized as a series of data sets, spanning CASP 7 through 12 (covering a ten-year period), to provide a range of data set sizes that enable assessment of new methods in relatively data poor and data rich regimes.

**Note that this is a preliminary release.** The raw data used for construction of the data sets, as well as the MSAs, are not yet generally available. However, the raw MSA data (4TB) for ProteinNet 12 is available upon request. Transfer requires downloading of a Globus client. See the [raw data](https://github.com/aqlaboratory/proteinnet/blob/master/docs/raw_data.md) section for more information.

### Motivation
Protein structure prediction is one of the central problems of biochemistry. While the problem is well-studied within the biological and chemical sciences, it is less well represented within the machine learning community. We suspect this is due to two reasons: 1) a high barrier to entry for non-domain experts, and 2) lack of standardization in terms of training / validation / test splits that make fair and consistent comparisons across methods possible. If these two issues are addressed, protein structure prediction can become a major source of innovation in ML research, alongside the canonical tasks of computer vision, NLP, and speech recognition. Much like [ImageNet](http://www.image-net.org) helped [spur the development](https://qz.com/1034972/the-data-that-changed-the-direction-of-ai-research-and-possibly-the-world/) of new computer vision techniques, ProteinNet aims to facilitate ML research on protein structure by providing a standardized data set, and standardized training / validation / test splits, that any group can use with minimal effort to get started.

### Approach
Once every two years the [CASP](http://predictioncenter.org/) assessment is held. During this competition structure predictors from across the globe are presented with protein sequences whose structures have been recently solved but which have not yet been made publicly available. The predictors make blind predictions of these structures, which are then assessed for their accuracy. The CASP structures thus provide a standardized benchmark for how well prediction methods perform at a _given moment in time_. The basic idea behind ProteinNet is to piggyback on CASP, by using CASP structures as test sets. ProteinNet augments these test sets with training / validation sets that _reset the historical record_ to the conditions preceding each CASP experiment. In particular, ProteinNet restricts the set of sequences (used for building PSSMs and MSAs) and structures to those available prior to the commencement of each CASP. This is critical as standard databases such as [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) do not maintain historical versions. We use time-reset versions of the [UniParc](http://www.uniprot.org/uniparc/) dataset as well as metagenomic sequences from the [JGI](https://img.jgi.doe.gov/) to build sequence databases for deriving MSAs. ProteinNet further provides carefully split validation sets that range in difficulty from easy (>90% seq. id.), useful for assessing a model's ability to predict minor changes in protein structure such as mutations, to extremely difficult (<10 seq. id.), useful for assessing a model's abiliy to predict entirely new protein folds, as in the CASP Free Modeling (FM) category. In a sense, our validation sets provide a series of transferability challenges to test how well a model can withstand distributional shifts in the data set. We have found that our most difficult validation subsets exceed the difficulty of CASP FM targets.

### Download
ProteinNet records are provided in two forms: human- and machine-readable text files that can be used programmatically by any tool, and TensorFlow-specific `TFRecord` files. More information on the file format can be found in the documentation [here](https://github.com/aqlaboratory/proteinnet/blob/master/docs/proteinnet_records.md#file-formats).

| CASP7 | CASP8 | CASP9 | CASP10 | CASP11 | CASP12* |
| --- | --- | --- | --- | --- | --- |
| [Text-based](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/human_readable/casp7.tar.gz) | [Text-based](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/human_readable/casp8.tar.gz) | [Text-based](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/human_readable/casp9.tar.gz) | [Text-based](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/human_readable/casp10.tar.gz) | [Text-based](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/human_readable/casp11.tar.gz) | [Text-based](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/human_readable/casp12.tar.gz) |
| [TF Records](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/tfrecords/casp7.tar.gz) | [TF Records](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/tfrecords/casp8.tar.gz) | [TF Records](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/tfrecords/casp9.tar.gz) | [TF Records](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/tfrecords/casp10.tar.gz) | [TF Records](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/tfrecords/casp11.tar.gz) | [TF Records](https://sharehost.hms.harvard.edu/sysbio/alquraishi/proteinnet/tfrecords/casp12.tar.gz) |

| Secondary Structure Data |
| --- |
|[ASTRAL entries](https://www.dropbox.com/s/59y3nud4rixombf/single_domain_dssp_annotations.json.gz?dl=0)|
|[PDB entries](https://www.dropbox.com/s/sne2ak1woy1lrqr/full_protein_dssp_annotations.json.gz?dl=0)|

\* CASP12 test set is incomplete due to embargoed structures. Once the embargo is lifted we will release all structures.

### Documentation
* [ProteinNet Records](docs/proteinnet_records.md)
* [Splitting Methodology](docs/splitting_methodology.md)
* [Raw Data](docs/raw_data.md)
* [FAQ](docs/FAQ.md)

### PyTorch Parser
ProteinNet includes an official TensorFlow-based parser. [Jeppe Hallgren](https://github.com/JeppeHallgren) has kindly created a PyTorch-based parser that is available [here](https://github.com/OpenProtein/openprotein/blob/master/preprocessing.py).

### Extensions
[SideChainNet](https://github.com/jonathanking/sidechainnet) extends ProteinNet by adding angle and atomic coordinate information for side chain atoms.

### Citation
Please cite the [ProteinNet paper](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-019-2932-0) in BMC Bioinformatics.

### Acknowledgements
Construction of this data set consumed millions of compute hours and was possible thanks to the generous support of the [HMS Laboratory of Systems Pharmacology](http://hits.harvard.edu/the-program/laboratory-of-systems-pharmacology/about/), the [Harvard Program in Therapeutic Science](http://hits.harvard.edu/the-program/program-in-regulatory-science/about/), and the [Research Computing](https://rc.hms.harvard.edu) group at [Harvard Medical School](https://hms.harvard.edu). We also thank [Martin Steinegger](https://github.com/martin-steinegger) and [Milot Mirdita](https://github.com/milot-mirdita) for their extensive help with the MMseqs2 and HHblits software packages, [Sergey Ovchinnikov](http://site.solab.org/) for providing metagenomic sequences, [Andriy Kryshtafovych](http://predictioncenter.org/people/kryshtafovych/index.cgi) for his assistance with CASP data, and [Sean Eddy](https://github.com/cryptogenomicon) for his help with the HMMer software package. This data set is hosted by the [HMS Research Information Technology Solutions](https://rits.hms.harvard.edu) group at Harvard University.

### Funding
This work was supported by NIGMS grant P50GM107618 and NCI grant U54-CA225088.


### Reference: 

![Screenshot from 2022-03-06 14-01-06](https://user-images.githubusercontent.com/74653444/156936454-32669ac4-b316-47b9-9b57-6c462df248de.png)

