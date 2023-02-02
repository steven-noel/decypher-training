# decypher-training
This project is for sharing training data for DeCypher, a system for transforming natural language questions to analytic queries for cyber situational understanding in operational environments.

The CyGraph tool has demonstrated progressive capabilities for graph-based cyber situational understanding, e.g., in tactical military operations [(Noel et al, 2018)](https://ieeexplore.ieee.org/document/8405029). Experience from operational exercises such as the U.S. Army's Cyber Quest [(MITRE, 2019)](https://www.mitre.org/news-insights/impact-story/quest-field-better-cyber-awareness-tools-warfighters) highlight the need for making cyber analytic workflows accessible to those who are not data science experts or computer programmers. The DeCypher system helps address these challenges in effective human-computer interaction for cyber situational understanding. DeCypher is integrated with CyGraph, constructing analytic graph query patterns from natural language user dialog. CyGraph and DeCypher are components of a reference implementation for a tool to be fielded to operational units [(Pomerleau, 2021)](https://www.c4isrnet.com/cyber/2021/10/11/the-us-army-will-soon-be-able-to-see-itself-in-cyberspace-on-the-battlefield/).

For experimental evaluation of user query intent classification in DeCypher, a set of 101 initial user questions were created. The questions are derived from an evaluation exercise, as described in [(Noel et al, 2021)](https://journals.sagepub.com/doi/abs/10.1177/15485129211051385). These questions span the following 11 user query intent classes:

1. `edgeProperty`
2. `listProperties`
3. `loadAll`
4. `multiDirection`
5. `multiLoadAll`
6. `noDirection`
7. `orQuery`
8. `returnCount`
9. `returnNames`
10. `setLimit`
11. `toDirection`

In DeCypher, domain-specific data masking and augmentation is applied to the training data to create the actual data set for training query intent classification. This removes training bias specific to the application domain, so that intent classification is domain agnostic. Domain-specific words in the original training instances are masked (excluded from the intent classification training data) and replaced with randomly generated words as training data augmentation. For example, the training instance "flows between ip and non-ldif" (for the `noDirection` intent) is masked as "\$ between \$ and \$," since network flows, IP addresses, and non-LDIF hosts are specific edge and node types in the application domain. The result is that the `noDirection` intent is trained with multiple instances of input questions, with a variety of arbitrary words replacing the domain-specific ones.

After data tagging and domain-specific masking, for each question instance with a dollar sign (domain-specific mask), 50 augmentation instances are generated, with a random word of 3-15 characters replacing each masked word. This results in 5,050 instances for training and tuning the intent classification algorithms. These data instances are in `intent_training_masked.csv` and `intent_training_generated.csv`, respectively.
