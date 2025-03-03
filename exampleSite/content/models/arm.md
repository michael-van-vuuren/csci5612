---
title: Association Rule Mining (ARM)
type: docs
prev: models/clustering
next: models/
weight: 3
---

## Definition

text

## Data Preparation

>[!NOTE]
>Source code can be found here:\
>[github.com/michael-van-vuuren/csci5612-workspace/models/unsupervised/ARM.ipynb](https://github.com/michael-van-vuuren/csci5612-workspace/blob/main/models/unsupervised/ARM.ipynb)

**Starting Data:**
![PreARMReady](/images/arm/PreARMReady.png)

**ARM-Ready Data:**
![ARMReady](/images/arm/ARMReady.png)

## Results

{{< tabs items="By Support,By Confidence,By Lift" >}}
  {{< tab >}}

  **Rules:**

  ![Top15Support](/images/arm/Top15Support.png)

  **Visualization:**

  ![Top15SupportGraph](/images/arm/Top15SupportGraph.png)
  
  {{< /tab >}}
  {{< tab >}}

  **Rules:**

  ![Top15Confidence](/images/arm/Top15Confidence.png)

  **Visualization:**

  ![Top15ConfidenceGraph](/images/arm/Top15ConfidenceGraph.png)
  
  {{< /tab >}}
  {{< tab >}}

  **Rules:**

  ![Top15Lift](/images/arm/Top15Lift.png)

  **Visualization:**

  ![Top15LiftGraph](/images/arm/Top15LiftGraph.png)
  
  {{< /tab >}}
{{< /tabs >}}

## Conclusions