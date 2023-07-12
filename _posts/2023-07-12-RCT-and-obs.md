# Causal Inference: Combining Experimental and Observational Data
In modern medical research, randomized controlled trials (RCTs) are widely applied for assessing the causal effect of an intervention or a treatment on an outcome of interest. Experimental data collected through RCTs are regarded to be reliable since confounding is eliminated via meticulous design and randomization. However, due to the cost of RCTs and some ethical factors, the size of experimental data are often limited. In contrast, observational data are easily accessible, but their internal validity is not guaranteed due to the existence of confounding bias.

In this blog we will discuss combining experimental and observational data. Colnet et al. (2023) has summarized the advantage of combining experimental and observational data as follows.
+ accounting for the lack of representativeness of RCT, as observational data can constitute an external representative sample of a target population of interest;
+ making observational evidence more credible using RCT to ground observational analysis, such as detecting a confounding bias;
+ improving statistical efficiency, for example to better estimate heterogeneous treatment effects as RCTs are often under-powered in such settings.

## Set-up

