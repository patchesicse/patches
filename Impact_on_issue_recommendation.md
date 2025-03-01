## Summary

When Tan et al. designed CrossFix, they did not consider modified patches. Given an open bug report, CrossFix can retrieve a list of relevant fixed bug reports with similar stack traces, triggering conditions, or titles. However, if the recommended
bug report’s patch has been modified, the knowledge derived from it may be unreliable. This research question explores how such patches affect the results. According to our results, Modifications to patches affect the results of this approach. On average,
we remove 36% of issue reports whose patches were modified.

## Result
|aries|calcite|cassandra|derby|flink|geode|hbase|hive|nutch|
| :----------: | :----------: | :----------:|:----------: | :----------: | :----------: |:----------: | :----------: | :----------: |
|71|44|36|56|43|76|52|51|53|

Table 2. The reduced modified patches. Each project has 30 open issues, and CrossFix recommends 5 candidates for each open issue. In total, CrossFix recommends 150 candidates for each project. From the 150 candidates from each project, we reduce 71 (aries), 
44(calcite), 36 (cassandra), 56 (derby), 43 (flink), 76 (geode), 52 (hbase), 51 (hive), and 53 (nutch) obsolete issue reports. On average, we remove 36% of modified patches.

## Reference
[1] Shin Hwei Tan, Ziqiang Li, and Lu Yan. 2023. CrossFix: Resolution of GitHub issues via similar bugs recommendation. Journal of Software: Evolution and Process (2023), e2554.
