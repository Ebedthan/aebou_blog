---
title: R is overtaking Python in microbes analysis
author: Anicet Ebou
date: '2019-10-26'
slug: r-is-overtaking-python-in-microbes-analysis
categories:
  - R
tags:
  - microbes
  - rstats
toc: no
images: ~
---

L'analyse microbienne est un bon domaine d'analyse des données. Le fait que les microbes sont omniprésents dans nos environnements et que le séquençage à haut débit provoque un déluge de données en est la cause principale.

Les étapes cruciales de l'analyse microbienne sont l'élimination des amorces, le démultiplexage, le filtrage et l'ajustement des séquences, lqa construction de modèles d'erreur, le regroupement des séquences et la classification taxonomique.

Parmi les étapes ci-dessus, le regroupement des séquences et les affectations taxonomiques semblent être les plus difficiles. La clusterisation des séquences, vise la réduction des erreurs inhérentes à la PCR et au séquençage de l'ADN. 
Traditionnellement, les séquences étaient regroupées en unités taxonomiques opérationnelles (OTU) suivant un seuil de 97%. Mais comme le soulignent des études récentes ([Callahan et al., 2016](https://www.ncbi.nlm.nih.gov/pubmed/27214047),[Amir et al., 2016](https://msystems.asm.org/content/2/2/e00191-16),[Edgar RC, 2018](https://www.ncbi.nlm.nih.gov/pubmed/29506021)), l'olygotypage est maintenant le bon moyen de regrouper les séquences d'amplicons en utilisant un seuil de mise à jour des variantes des séquences d'amplicon (ASV).
Et pour ce faire, les deux algorithmes majeurs ont été écrits en R (dada2) et Python (deblur). Mais Dada2 semble être le plus utilisé.

Pour la classification taxonomique, le[classificateur RDP](https://rdp.cme.msu.edu/classifier/classifier.jsp) qui est toujours largement utilisé est la référence pour la classification et il a sa propre implémentation dans R (fonction assignTaxonomy dans le package dada2). [IdTaxa](https://microbiomejournal.biomedcentral.com/articles/10.1186/s40168-018-0521-5) qui rapporte une meilleure précision que tous les classificateurs taxonomiques précédents, est écrit en R et fait partie du paquet R DECIPHER.

C'est comme si quand vous voulez faire l'analyse de données biologiques, vous devez obligatoirement connaître le langage R.

Le leadership de R dans l'analyse microbienne révèle la capacité de R à s'adapter à la situation en place et c'est un langage étonnnament adéquart pour les données biologiques.
