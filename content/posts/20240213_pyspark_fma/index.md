---
title: "Does a song with a long title have a longer duration? A PySpark lesson"
url: pyspark_analytics_tutorial
date: 2024-02-13
categories:
- Tutorials
tags:
- python
- spark
- data
- analytics
excerpt: This is an example of how to use PySpark to load, describe, and explore a dataset to check a hypothesis
showReadingTime: false
---

Following the previous post on basic ML with PySpark, we continue with a tutorial on how to run day-to-day data analytics. In this case, we explore the [FMA dataset](https://github.com/mdeff/fma) trying to answer if there is a correlation between the title of a song and its duration. 

I think this could be a nice example of how to use some data transformations with PySpark to run a data analysis. 

The corresponding Jupyter Notebook is available [here](https://github.com/juanmanuel-tirado/pyspark-tutorial/blob/main/pyspark_fma.ipynb).  If you want to fork the repo:

```sh
git clone https://github.com/juanmanuel-tirado/pyspark-tutorial
```

Additionally, you can see the rendered version below.

Thanks for reading,

{{< notebook "jupyter/pyspark_mllib/pyspark-fma" 3000 >}}


