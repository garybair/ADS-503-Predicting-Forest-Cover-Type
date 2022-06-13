
<!-- rnb-text-begin -->

---
title: "R Notebook"
output: html_notebook
---


<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxubGlicmFyeShnZ3Bsb3QyKVxubGlicmFyeShtbGJlbmNoKVxubGlicmFyeShHR2FsbHkpXG5gYGAifQ== -->

```r
library(ggplot2)
library(mlbench)
library(GGally)
```

<!-- rnb-source-end -->

<!-- rnb-output-begin eyJkYXRhIjoiV2FybmluZzogcGFja2FnZSDigJhHR2FsbHnigJkgd2FzIGJ1aWx0IHVuZGVyIFIgdmVyc2lvbiA0LjEuM1xuUmVnaXN0ZXJlZCBTMyBtZXRob2Qgb3ZlcndyaXR0ZW4gYnkgJ0dHYWxseSc6XG4gIG1ldGhvZCBmcm9tICAgXG4gICsuZ2cgICBnZ3Bsb3QyXG4ifQ== -->

```
Warning: package ‘GGally’ was built under R version 4.1.3
Registered S3 method overwritten by 'GGally':
  method from   
  +.gg   ggplot2
```



<!-- rnb-output-end -->

<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxubGlicmFyeShjYXJldClcbmxpYnJhcnkoZTEwNzEpXG5gYGAifQ== -->

```r
library(caret)
library(e1071)
```

<!-- rnb-source-end -->

<!-- rnb-output-begin eyJkYXRhIjoiV2FybmluZzogcGFja2FnZSDigJhlMTA3MeKAmSB3YXMgYnVpbHQgdW5kZXIgUiB2ZXJzaW9uIDQuMS4zXG4ifQ== -->

```
Warning: package ‘e1071’ was built under R version 4.1.3
```



<!-- rnb-output-end -->

<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxubGlicmFyeShNQVNTKVxuYGBgIn0= -->

```r
library(MASS)
```

<!-- rnb-source-end -->

<!-- rnb-output-begin eyJkYXRhIjoiXG5BdHRhY2hpbmcgcGFja2FnZTog4oCYTUFTU+KAmVxuXG5UaGUgZm9sbG93aW5nIG9iamVjdCBpcyBtYXNrZWQgZnJvbSDigJhwYWNrYWdlOmRwbHly4oCZOlxuXG4gICAgc2VsZWN0XG4ifQ== -->

```

Attaching package: ‘MASS’

The following object is masked from ‘package:dplyr’:

    select
```



<!-- rnb-output-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZGYgPSByZWFkLmNzdihcIi4uL2RhdGEvY292ZXJ0eXBlLmNzdlwiLFxuICAgICAgICAgICAgICAgIHNlcCA9IFwiLFwiKVxuYGBgIn0= -->

```r
df = read.csv("../data/covertype.csv",
                sep = ",")
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZGYkV2lsZGVybmVzc19BcmVhMSA9IGFzLmZhY3RvcihkZiRXaWxkZXJuZXNzX0FyZWExKVxuZGYkV2lsZGVybmVzc19BcmVhMiA9IGFzLmZhY3RvcihkZiRXaWxkZXJuZXNzX0FyZWEyKVxuZGYkV2lsZGVybmVzc19BcmVhMyA9IGFzLmZhY3RvcihkZiRXaWxkZXJuZXNzX0FyZWEzKVxuZGYkV2lsZGVybmVzc19BcmVhNCA9IGFzLmZhY3RvcihkZiRXaWxkZXJuZXNzX0FyZWE0KVxuZGYkQ292ZXJfVHlwZSA9IGFzLmZhY3RvcihkZiRDb3Zlcl9UeXBlKVxuYGBgIn0= -->

```r
df$Wilderness_Area1 = as.factor(df$Wilderness_Area1)
df$Wilderness_Area2 = as.factor(df$Wilderness_Area2)
df$Wilderness_Area3 = as.factor(df$Wilderness_Area3)
df$Wilderness_Area4 = as.factor(df$Wilderness_Area4)
df$Cover_Type = as.factor(df$Cover_Type)
```

<!-- rnb-source-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->


<!-- rnb-source-begin eyJkYXRhIjoiYGBgclxuZ2dwYWlycyhkZlssMToxMF0pXG5gYGAifQ== -->

```r
ggpairs(df[,1:10])
```

<!-- rnb-source-end -->

<!-- rnb-output-begin eyJkYXRhIjoiXG4gcGxvdDogWzEsMV0gWz4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dICAxJSBlc3Q6IDBzIFxuIHBsb3Q6IFsxLDJdIFs9PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAgMiUgZXN0OjEwcyBcbiBwbG90OiBbMSwzXSBbPT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gIDMlIGVzdDogOHMgXG4gcGxvdDogWzEsNF0gWz09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dICA0JSBlc3Q6IDhzIFxuIHBsb3Q6IFsxLDVdIFs9PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAgNSUgZXN0OiA3cyBcbiBwbG90OiBbMSw2XSBbPT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gIDYlIGVzdDogN3MgXG4gcGxvdDogWzEsN10gWz09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dICA3JSBlc3Q6IDdzIFxuIHBsb3Q6IFsxLDhdIFs9PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAgOCUgZXN0OiA3cyBcbiBwbG90OiBbMSw5XSBbPT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gIDklIGVzdDogNnMgXG4gcGxvdDogWzEsMTBdIFs9PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDEwJSBlc3Q6IDZzIFxuIHBsb3Q6IFsyLDFdIFs9PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAxMSUgZXN0OiA2cyBcbiBwbG90OiBbMiwyXSBbPT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gMTIlIGVzdDogOXMgXG4gcGxvdDogWzIsM10gWz09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDEzJSBlc3Q6MTBzIFxuIHBsb3Q6IFsyLDRdIFs9PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAxNCUgZXN0OiA5cyBcbiBwbG90OiBbMiw1XSBbPT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gMTUlIGVzdDogOXMgXG4gcGxvdDogWzIsNl0gWz09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDE2JSBlc3Q6IDlzIFxuIHBsb3Q6IFsyLDddIFs9PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAxNyUgZXN0OiA4cyBcbiBwbG90OiBbMiw4XSBbPT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gMTglIGVzdDogOHMgXG4gcGxvdDogWzIsOV0gWz09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDE5JSBlc3Q6IDhzIFxuIHBsb3Q6IFsyLDEwXSBbPT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAyMCUgZXN0OiA4cyBcbiBwbG90OiBbMywxXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gMjElIGVzdDogN3MgXG4gcGxvdDogWzMsMl0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDIyJSBlc3Q6IDlzIFxuIHBsb3Q6IFszLDNdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAyMyUgZXN0OjEwcyBcbiBwbG90OiBbMyw0XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gMjQlIGVzdDoxMHMgXG4gcGxvdDogWzMsNV0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDI1JSBlc3Q6IDlzIFxuIHBsb3Q6IFszLDZdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAyNiUgZXN0OiA5cyBcbiBwbG90OiBbMyw3XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gMjclIGVzdDogOXMgXG4gcGxvdDogWzMsOF0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDI4JSBlc3Q6IDlzIFxuIHBsb3Q6IFszLDldIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAyOSUgZXN0OiA4cyBcbiBwbG90OiBbMywxMF0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gMzAlIGVzdDogOHMgXG4gcGxvdDogWzQsMV0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDMxJSBlc3Q6IDhzIFxuIHBsb3Q6IFs0LDJdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAzMiUgZXN0OiA5cyBcbiBwbG90OiBbNCwzXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gMzMlIGVzdDogOXMgXG4gcGxvdDogWzQsNF0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDM0JSBlc3Q6MTBzIFxuIHBsb3Q6IFs0LDVdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAzNSUgZXN0OjEwcyBcbiBwbG90OiBbNCw2XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gMzYlIGVzdDogOXMgXG4gcGxvdDogWzQsN10gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDM3JSBlc3Q6IDlzIFxuIHBsb3Q6IFs0LDhdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSAzOCUgZXN0OiA5cyBcbiBwbG90OiBbNCw5XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gMzklIGVzdDogOHMgXG4gcGxvdDogWzQsMTBdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDQwJSBlc3Q6IDhzIFxuIHBsb3Q6IFs1LDFdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA0MSUgZXN0OiA4cyBcbiBwbG90OiBbNSwyXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNDIlIGVzdDogOHMgXG4gcGxvdDogWzUsM10gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDQzJSBlc3Q6IDlzIFxuIHBsb3Q6IFs1LDRdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA0NCUgZXN0OiA5cyBcbiBwbG90OiBbNSw1XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNDUlIGVzdDogOXMgXG4gcGxvdDogWzUsNl0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDQ2JSBlc3Q6IDlzIFxuIHBsb3Q6IFs1LDddIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA0NyUgZXN0OiA5cyBcbiBwbG90OiBbNSw4XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNDglIGVzdDogOXMgXG4gcGxvdDogWzUsOV0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDQ5JSBlc3Q6IDhzIFxuIHBsb3Q6IFs1LDEwXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA1MCUgZXN0OiA4cyBcbiBwbG90OiBbNiwxXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNTElIGVzdDogOHMgXG4gcGxvdDogWzYsMl0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDUyJSBlc3Q6IDhzIFxuIHBsb3Q6IFs2LDNdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA1MyUgZXN0OiA4cyBcbiBwbG90OiBbNiw0XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNTQlIGVzdDogOHMgXG4gcGxvdDogWzYsNV0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDU1JSBlc3Q6IDhzIFxuIHBsb3Q6IFs2LDZdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA1NiUgZXN0OiA4cyBcbiBwbG90OiBbNiw3XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNTclIGVzdDogOHMgXG4gcGxvdDogWzYsOF0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDU4JSBlc3Q6IDhzIFxuIHBsb3Q6IFs2LDldIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA1OSUgZXN0OiA4cyBcbiBwbG90OiBbNiwxMF0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNjAlIGVzdDogN3MgXG4gcGxvdDogWzcsMV0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDYxJSBlc3Q6IDdzIFxuIHBsb3Q6IFs3LDJdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA2MiUgZXN0OiA3cyBcbiBwbG90OiBbNywzXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNjMlIGVzdDogN3MgXG4gcGxvdDogWzcsNF0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDY0JSBlc3Q6IDdzIFxuIHBsb3Q6IFs3LDVdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA2NSUgZXN0OiA3cyBcbiBwbG90OiBbNyw2XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNjYlIGVzdDogN3MgXG4gcGxvdDogWzcsN10gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDY3JSBlc3Q6IDdzIFxuIHBsb3Q6IFs3LDhdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA2OCUgZXN0OiA3cyBcbiBwbG90OiBbNyw5XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNjklIGVzdDogNnMgXG4gcGxvdDogWzcsMTBdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDcwJSBlc3Q6IDZzIFxuIHBsb3Q6IFs4LDFdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA3MSUgZXN0OiA2cyBcbiBwbG90OiBbOCwyXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNzIlIGVzdDogNnMgXG4gcGxvdDogWzgsM10gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDczJSBlc3Q6IDZzIFxuIHBsb3Q6IFs4LDRdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA3NCUgZXN0OiA2cyBcbiBwbG90OiBbOCw1XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNzUlIGVzdDogNXMgXG4gcGxvdDogWzgsNl0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDc2JSBlc3Q6IDVzIFxuIHBsb3Q6IFs4LDddIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA3NyUgZXN0OiA1cyBcbiBwbG90OiBbOCw4XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gNzglIGVzdDogNXMgXG4gcGxvdDogWzgsOV0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDc5JSBlc3Q6IDVzIFxuIHBsb3Q6IFs4LDEwXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA4MCUgZXN0OiA0cyBcbiBwbG90OiBbOSwxXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS0tLS0tLV0gODElIGVzdDogNHMgXG4gcGxvdDogWzksMl0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tLS0tLS1dIDgyJSBlc3Q6IDRzIFxuIHBsb3Q6IFs5LDNdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLS0tXSA4MyUgZXN0OiA0cyBcbiBwbG90OiBbOSw0XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLS0tLV0gODQlIGVzdDogNHMgXG4gcGxvdDogWzksNV0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tLS0tLS1dIDg1JSBlc3Q6IDRzIFxuIHBsb3Q6IFs5LDZdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLS0tLS0tXSA4NiUgZXN0OiAzcyBcbiBwbG90OiBbOSw3XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS0tLV0gODclIGVzdDogM3MgXG4gcGxvdDogWzksOF0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tLS0tLS1dIDg4JSBlc3Q6IDNzIFxuIHBsb3Q6IFs5LDldIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLS0tLS0tXSA4OSUgZXN0OiAzcyBcbiBwbG90OiBbOSwxMF0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS0tLV0gOTAlIGVzdDogMnMgXG4gcGxvdDogWzEwLDFdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS0tLS1dIDkxJSBlc3Q6IDJzIFxuIHBsb3Q6IFsxMCwyXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLS0tLS0tXSA5MiUgZXN0OiAycyBcbiBwbG90OiBbMTAsM10gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS0tLS0tLV0gOTMlIGVzdDogMnMgXG4gcGxvdDogWzEwLDRdIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tLS1dIDk0JSBlc3Q6IDJzIFxuIHBsb3Q6IFsxMCw1XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tLS0tXSA5NSUgZXN0OiAxcyBcbiBwbG90OiBbMTAsNl0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT4tLS0tLV0gOTYlIGVzdDogMXMgXG4gcGxvdDogWzEwLDddIFs9PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09Pi0tLS1dIDk3JSBlc3Q6IDFzIFxuIHBsb3Q6IFsxMCw4XSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LS0tXSA5OCUgZXN0OiAxcyBcbiBwbG90OiBbMTAsOV0gWz09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT0+LV0gOTklIGVzdDogMHMgXG4gcGxvdDogWzEwLDEwXSBbPT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT09PT1dMTAwJSBlc3Q6IDBzIFxuICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBcbiJ9 -->

```

 plot: [1,1] [>------------------------------------------------------------------------------------------------------------------------------]  1% est: 0s 
 plot: [1,2] [==>----------------------------------------------------------------------------------------------------------------------------]  2% est:10s 
 plot: [1,3] [===>---------------------------------------------------------------------------------------------------------------------------]  3% est: 8s 
 plot: [1,4] [====>--------------------------------------------------------------------------------------------------------------------------]  4% est: 8s 
 plot: [1,5] [=====>-------------------------------------------------------------------------------------------------------------------------]  5% est: 7s 
 plot: [1,6] [=======>-----------------------------------------------------------------------------------------------------------------------]  6% est: 7s 
 plot: [1,7] [========>----------------------------------------------------------------------------------------------------------------------]  7% est: 7s 
 plot: [1,8] [=========>---------------------------------------------------------------------------------------------------------------------]  8% est: 7s 
 plot: [1,9] [==========>--------------------------------------------------------------------------------------------------------------------]  9% est: 6s 
 plot: [1,10] [============>-----------------------------------------------------------------------------------------------------------------] 10% est: 6s 
 plot: [2,1] [=============>-----------------------------------------------------------------------------------------------------------------] 11% est: 6s 
 plot: [2,2] [==============>----------------------------------------------------------------------------------------------------------------] 12% est: 9s 
 plot: [2,3] [================>--------------------------------------------------------------------------------------------------------------] 13% est:10s 
 plot: [2,4] [=================>-------------------------------------------------------------------------------------------------------------] 14% est: 9s 
 plot: [2,5] [==================>------------------------------------------------------------------------------------------------------------] 15% est: 9s 
 plot: [2,6] [===================>-----------------------------------------------------------------------------------------------------------] 16% est: 9s 
 plot: [2,7] [=====================>---------------------------------------------------------------------------------------------------------] 17% est: 8s 
 plot: [2,8] [======================>--------------------------------------------------------------------------------------------------------] 18% est: 8s 
 plot: [2,9] [=======================>-------------------------------------------------------------------------------------------------------] 19% est: 8s 
 plot: [2,10] [========================>-----------------------------------------------------------------------------------------------------] 20% est: 8s 
 plot: [3,1] [==========================>----------------------------------------------------------------------------------------------------] 21% est: 7s 
 plot: [3,2] [===========================>---------------------------------------------------------------------------------------------------] 22% est: 9s 
 plot: [3,3] [============================>--------------------------------------------------------------------------------------------------] 23% est:10s 
 plot: [3,4] [=============================>-------------------------------------------------------------------------------------------------] 24% est:10s 
 plot: [3,5] [===============================>-----------------------------------------------------------------------------------------------] 25% est: 9s 
 plot: [3,6] [================================>----------------------------------------------------------------------------------------------] 26% est: 9s 
 plot: [3,7] [=================================>---------------------------------------------------------------------------------------------] 27% est: 9s 
 plot: [3,8] [===================================>-------------------------------------------------------------------------------------------] 28% est: 9s 
 plot: [3,9] [====================================>------------------------------------------------------------------------------------------] 29% est: 8s 
 plot: [3,10] [=====================================>----------------------------------------------------------------------------------------] 30% est: 8s 
 plot: [4,1] [======================================>----------------------------------------------------------------------------------------] 31% est: 8s 
 plot: [4,2] [========================================>--------------------------------------------------------------------------------------] 32% est: 9s 
 plot: [4,3] [=========================================>-------------------------------------------------------------------------------------] 33% est: 9s 
 plot: [4,4] [==========================================>------------------------------------------------------------------------------------] 34% est:10s 
 plot: [4,5] [===========================================>-----------------------------------------------------------------------------------] 35% est:10s 
 plot: [4,6] [=============================================>---------------------------------------------------------------------------------] 36% est: 9s 
 plot: [4,7] [==============================================>--------------------------------------------------------------------------------] 37% est: 9s 
 plot: [4,8] [===============================================>-------------------------------------------------------------------------------] 38% est: 9s 
 plot: [4,9] [=================================================>-----------------------------------------------------------------------------] 39% est: 8s 
 plot: [4,10] [=================================================>----------------------------------------------------------------------------] 40% est: 8s 
 plot: [5,1] [===================================================>---------------------------------------------------------------------------] 41% est: 8s 
 plot: [5,2] [====================================================>--------------------------------------------------------------------------] 42% est: 8s 
 plot: [5,3] [======================================================>------------------------------------------------------------------------] 43% est: 9s 
 plot: [5,4] [=======================================================>-----------------------------------------------------------------------] 44% est: 9s 
 plot: [5,5] [========================================================>----------------------------------------------------------------------] 45% est: 9s 
 plot: [5,6] [=========================================================>---------------------------------------------------------------------] 46% est: 9s 
 plot: [5,7] [===========================================================>-------------------------------------------------------------------] 47% est: 9s 
 plot: [5,8] [============================================================>------------------------------------------------------------------] 48% est: 9s 
 plot: [5,9] [=============================================================>-----------------------------------------------------------------] 49% est: 8s 
 plot: [5,10] [==============================================================>---------------------------------------------------------------] 50% est: 8s 
 plot: [6,1] [================================================================>--------------------------------------------------------------] 51% est: 8s 
 plot: [6,2] [=================================================================>-------------------------------------------------------------] 52% est: 8s 
 plot: [6,3] [==================================================================>------------------------------------------------------------] 53% est: 8s 
 plot: [6,4] [====================================================================>----------------------------------------------------------] 54% est: 8s 
 plot: [6,5] [=====================================================================>---------------------------------------------------------] 55% est: 8s 
 plot: [6,6] [======================================================================>--------------------------------------------------------] 56% est: 8s 
 plot: [6,7] [=======================================================================>-------------------------------------------------------] 57% est: 8s 
 plot: [6,8] [=========================================================================>-----------------------------------------------------] 58% est: 8s 
 plot: [6,9] [==========================================================================>----------------------------------------------------] 59% est: 8s 
 plot: [6,10] [===========================================================================>--------------------------------------------------] 60% est: 7s 
 plot: [7,1] [============================================================================>--------------------------------------------------] 61% est: 7s 
 plot: [7,2] [==============================================================================>------------------------------------------------] 62% est: 7s 
 plot: [7,3] [===============================================================================>-----------------------------------------------] 63% est: 7s 
 plot: [7,4] [================================================================================>----------------------------------------------] 64% est: 7s 
 plot: [7,5] [==================================================================================>--------------------------------------------] 65% est: 7s 
 plot: [7,6] [===================================================================================>-------------------------------------------] 66% est: 7s 
 plot: [7,7] [====================================================================================>------------------------------------------] 67% est: 7s 
 plot: [7,8] [=====================================================================================>-----------------------------------------] 68% est: 7s 
 plot: [7,9] [=======================================================================================>---------------------------------------] 69% est: 6s 
 plot: [7,10] [=======================================================================================>--------------------------------------] 70% est: 6s 
 plot: [8,1] [=========================================================================================>-------------------------------------] 71% est: 6s 
 plot: [8,2] [==========================================================================================>------------------------------------] 72% est: 6s 
 plot: [8,3] [============================================================================================>----------------------------------] 73% est: 6s 
 plot: [8,4] [=============================================================================================>---------------------------------] 74% est: 6s 
 plot: [8,5] [==============================================================================================>--------------------------------] 75% est: 5s 
 plot: [8,6] [================================================================================================>------------------------------] 76% est: 5s 
 plot: [8,7] [=================================================================================================>-----------------------------] 77% est: 5s 
 plot: [8,8] [==================================================================================================>----------------------------] 78% est: 5s 
 plot: [8,9] [===================================================================================================>---------------------------] 79% est: 5s 
 plot: [8,10] [====================================================================================================>-------------------------] 80% est: 4s 
 plot: [9,1] [======================================================================================================>------------------------] 81% est: 4s 
 plot: [9,2] [=======================================================================================================>-----------------------] 82% est: 4s 
 plot: [9,3] [========================================================================================================>----------------------] 83% est: 4s 
 plot: [9,4] [==========================================================================================================>--------------------] 84% est: 4s 
 plot: [9,5] [===========================================================================================================>-------------------] 85% est: 4s 
 plot: [9,6] [============================================================================================================>------------------] 86% est: 3s 
 plot: [9,7] [=============================================================================================================>-----------------] 87% est: 3s 
 plot: [9,8] [===============================================================================================================>---------------] 88% est: 3s 
 plot: [9,9] [================================================================================================================>--------------] 89% est: 3s 
 plot: [9,10] [================================================================================================================>-------------] 90% est: 2s 
 plot: [10,1] [==================================================================================================================>-----------] 91% est: 2s 
 plot: [10,2] [===================================================================================================================>----------] 92% est: 2s 
 plot: [10,3] [====================================================================================================================>---------] 93% est: 2s 
 plot: [10,4] [=====================================================================================================================>--------] 94% est: 2s 
 plot: [10,5] [=======================================================================================================================>------] 95% est: 1s 
 plot: [10,6] [========================================================================================================================>-----] 96% est: 1s 
 plot: [10,7] [=========================================================================================================================>----] 97% est: 1s 
 plot: [10,8] [==========================================================================================================================>---] 98% est: 1s 
 plot: [10,9] [============================================================================================================================>-] 99% est: 0s 
 plot: [10,10] [=============================================================================================================================]100% est: 0s 
                                                                                                                                                           
```



<!-- rnb-output-end -->

<!-- rnb-chunk-end -->


<!-- rnb-text-begin -->



<!-- rnb-text-end -->


<!-- rnb-chunk-begin -->



<!-- rnb-chunk-end -->

