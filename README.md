# paf_graphs_formatter_plotter
a paf formatter and plotter for the graph analysis and see the alignment of the miniasm and it uses faster approach for the analysis of the paf alignments and also outputs and extracts the corresponding paf alignments for the graphs. use the miniasm for the alignment and generation of the paf formats and then plot the paf alignments with this graph utility. I have released the first version and adding more functions plus a web application for the direct visualization of the alignments, meanwhile use this. 

```
def pafplotter(paffile = None, \
                    querychr = None, \
                        mapping = None, \
                                lengthplot = "FALSE", \
                                        catplot = "FALSE", \
                                                    mapping_quality_plot = "FALSE", \
                                                                scatterplot = "FALSE"):
    # Universitat Potsdam
    # Author Gaurav Sablok
    # date: 2024-2-28
    """"
    a pafplotter function which will allow you to plot many analysis
    and summary stats from the paf alignment files for the graphs. It 
    analyses the paf alignment files faster and you can chunck the indiviual
    block of the code as the separate functions. 
    """
    import pandas as pd 
    import seaborn as sns
    sns.set_theme()                                        
    if paffile is not None and querychr is None:
        pafdataframe = pd.DataFrame([line.strip().split("\t")[0:12] for line in open(paffile)], 
        columns=["query", "query_length", "query_start", "query_end", "strand", "target", \
                                 "target_length", "target_start", "target_end", "residue_matches", 
                                                                "alignment_length", "mapping_quality"])
        return pafdataframe
    if paffile and querychr is not None:
        pafdataframe = pd.DataFrame([line.strip().split("\t")[0:12] for line in open(paffile)], 
        columns=["query", "query_length", "query_start", "query_end", "strand", "target", \
                                 "target_length", "target_start", "target_end", "residue_matches", 
                                                                "alignment_length", "mapping_quality"])
        selectedpafdataframe = pafdataframe.where(pafdataframe["query"] == querychr).dropna()
        return selectedpafdataframe
    if paffile and mapping is not None:
        pafdataframe = pd.DataFrame([line.strip().split("\t")[0:12] for line in open(paffile)], 
        columns=["query", "query_length", "query_start", "query_end", "strand", "target", \
                                 "target_length", "target_start", "target_end", "residue_matches", 
                                                                "alignment_length", "mapping_quality"])
        selectedpafdataframe = pafdataframe.where(pafdataframe["mapping_quality"] == mapping).dropna()
        return selectedpafdataframe
    if paffile is not None and querychr is not None and lengthplot == "TRUE":
        pafdataframe = pd.DataFrame([line.strip().split("\t")[0:12] for line in open(paffile)], 
        columns=["query", "query_length", "query_start", "query_end", "strand", "target", \
                                 "target_length", "target_start", "target_end", "residue_matches", 
                                                                "alignment_length", "mapping_quality"])
        selectedpafdataframe = pafdataframe.where(pafdataframe["query"] == querychr).dropna()
        pd.DataFrame(pafdataframe.where(pafdataframe["query"] == querychr).dropna()["query_end"].apply(lambda num: int(num)) - \
               pafdataframe.where(pafdataframe["query"] == querychr).dropna()["query_start"].apply(lambda num: int(num)), \
                                                                                    columns = ["plot"]).plot(kind = "bar")
        return selectedpafdataframe
    if paffile is not None and querychr is not None and catplot == "TRUE":
        pafdataframe = pd.DataFrame([line.strip().split("\t")[0:12] for line in open(paffile)], 
        columns=["query", "query_length", "query_start", "query_end", "strand", "target", \
                                 "target_length", "target_start", "target_end", "residue_matches", 
                                                                "alignment_length", "mapping_quality"])
        selectedpafdataframe = pafdataframe.where(pafdataframe["query"] == querychr).dropna()
        catcorrdinates = pd.DataFrame(pafdataframe.where(pafdataframe["query"] == querychr).dropna()["query_end"].apply(lambda num: int(num)) - \
               pafdataframe.where(pafdataframe["query"] == querychr).dropna()["query_start"].apply(lambda num: int(num)), \
                                                                                    columns = ["plot"])
        return sns.catplot(catcorrdinates)
    if paffile is not None and mapping_quality_plot == "TRUE":
        pafdataframe = pd.DataFrame([line.strip().split("\t")[0:12] for line in open(paffile)], 
        columns=["query", "query_length", "query_start", "query_end", "strand", "target", \
                                 "target_length", "target_start", "target_end", "residue_matches", 
                                                                "alignment_length", "mapping_quality"])
        return sns.catplot(pafdataframe, kind = "box", x="residue_matches", y="alignment_length", height = 10)
    if paffile is not None and scatterplot == "TRUE":
        pafdataframe = pd.DataFrame([line.strip().split("\t")[0:12] for line in open(paffile)], 
        columns=["query", "query_length", "query_start", "query_end", "strand", "target", \
                                 "target_length", "target_start", "target_end", "residue_matches", 
                                                                "alignment_length", "mapping_quality"])
        return sns.scatterplot(pafdataframe, x="query", y="mapping_quality")
```

Gaurav Sablok \
Academic Staff Member \
Bioinformatics \
Institute for Biochemistry and Biology \
University of Potsdam \
Potsdam,Germany 
