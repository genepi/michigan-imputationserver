---
hide:
  - navigation
  - toc 
---

![](images/logo.png){.right}


# Michigan Imputation Server 2<br><small>Free Next-Generation Genotype Imputation Platform</small>


[Michigan Imputation Server 2](https://imputationserver.sph.umich.edu) provides a free genotype imputation service (chromosomes 1-22, chromosome X and HLA region) using [Minimac4](http://genome.sph.umich.edu/wiki/Minimac4). You can upload phased or unphased GWAS genotypes and receive phased and imputed genomes in return. Our server supports imputation from [numerous reference panels](reference-panels.md). For all uploaded datasets a comprehensive QC is performed. The complete source code is hosted on [GitHub](https://github.com/genepi/imputationserver2/).

Please cite this paper if you use Michigan Imputation Server 2:

> Das S, Forer L, Schönherr S, Sidore C, Locke AE, Kwong A, Vrieze S, Chew EY, Levy S, McGue M, Schlessinger D, Stambolian D, Loh PR, Iacono WG, Swaroop A, Scott LJ, Cucca F, Kronenberg F, Boehnke M, Abecasis GR, Fuchsberger C. [Next-generation genotype imputation service and methods](https://www.ncbi.nlm.nih.gov/pubmed/27571263). Nature Genetics 48, 1284–1287 (2016).

---


## Latest News

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 19 September 2024</small><br>
    We have migrated to a new architecture and released Michigan Imputation Server 2. Please note the change regarding allele swaps in Minimac4, which may affect your QC.
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 21 May 2021</small><br>
    We have increased the max sample size to 110k.
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 15 April 2021</small><br>
    Update to new framework completed! Currently, max sample size will be limited to 25k, but we expect to lift this limitation in the next few weeks.
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 18 March 2020</small><br>
    Due to coronavirus-related impacts support may be slower than usual. If you haven't heard back from us after a week or so, feel free to e-mail again to check on the status of things. Take care!
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 07 November 2019</small><br>
    Updated MIS to v1.2.4! Major improvements: Minimac4 for imputation, improved chrX support, QC check right after upload, better documentation. Checkout out our <a href="https://github.com/genepi/imputationserver" target="_blank">GitHub repository</a> for further information.
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 17 October 2019</small><br>
    Michigan Imputation Server at ASHG19. All information is available <a href="https://imputationserver.sph.umich.edu/ashg19/" target="_blank">here</a>.
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 27 November 2018</small><br>
    Redesigned user interface to improve user experience.
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 27 June 2017</small><br>
    Updated pipeline to v1.0.2. Release notes can be found <a href="https://github.com/genepi/imputationserver/releases/tag/1.0.2" target="_blank">here</a>.
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 29 Aug 2016</small><br>
    Imputation server paper is out now: <a href="http://www.nature.com/ng/journal/v48/n10/full/ng.3656.html" target="_blank">Das et al., Nature Genetics 2016</a>
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 19 April 2016</small><br>
    Updated <a target="_blank" href="http://www.haplotype-reference-consortium.org/">HRC Panel (r1.1)</a> available.
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 12 January 2016</small><br>
    New Reference Panel (<a target="_blank" href="./#!pages/caapa">CAAPA</a>) available.
</p>

<p>
    <small class="text-muted"><i class="far fa-calendar-alt"></i> 24 April 2015</small><br>
    <a target="_blank" href="http://www.haplotype-reference-consortium.org/">HRC release 1</a> (64,976 haplotypes at 39,235,157 SNPs) is now ready for use for HRC consortium members.
</p>