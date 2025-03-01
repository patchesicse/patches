# Understanding Obsolete Knowledge Derived from software Patches

## Project summary

In software development, developers use issue trackers and code repositories to record and manage issue reports and their patches. With the evolution of software, such systems accumulate a large number of patches when issue reports are resolved. From accumulated patches, researchers have mined rich knowledge that is useful in assisting various programming tasks, e.g., locating bugs and vulnerabilities. In recent decades, mining software repositories have been a hot research topic, and the mined knowledge assists various programming tasks. Despite the successful stories, the knowledge derived from patches can be unreliable or become obsolete with the evolution of software. Such knowledge is misleading and harmful. However, to the best of our knowledge, no study has explored this problem, and many research questions are still open. For instance, what knowledge can be learned? To what degree can such knowledge become obsolete or unreliable?

In this paper, we conducted the first empirical study on the obsolete knowledge to answer the above questions. In particular, we analyzed 396 issue reports and their patches from 9 Apache projects. We manually built a taxonomy of the knowledge learned from the patches of fixed issue reports and analyzed whether the knowledge became obsolete or unreliable in the context of the latest source files. We found that more project-specific knowledge was learned than domain knowledge from issue reports. In some issue reports, the added code lines of their patches are overwritten or rolled back. In this paper, we refer to such patches as modified patches and define their modification ratio as the number of remaining lines divided by the total number of added lines. We found that modification ratios were correlated with obsolete knowledge, especially for project-specific knowledge. Besides deriving findings, we analyzed how obsolete knowledge affects an issue-recommendation approach. According to our results, 36% of its recommendations were obsolete and we improved all such recommendations.

## Our findings

**RQ1. What kinds of knowledge can be learned?**
Based on whether the knowledge is specific to a project, we classify the knowledge into domain knowledge and project-specific knowledge. In particular, projectspecific knowledge is learned for 75% of issue reports. The most frequent project-specific knowledge is about writing documents and algorithms, and the most frequent domain knowledge is about API calls (Finding 1).

<table border="13" >
	<tr >
		<td rowspan="3">K1. Domain Knowledge(99/396, 25%)</td>
		<td>K1.1. API call (49/396, 12.37%)</td>
	</tr>
	<tr >
		<td>K1.2. Compile configuration (41/396, 10.35%)</td>
	</tr>
  <tr >
		<td>K1.3. Others (9/396, 2.27%)</td>
	</tr>
  	<tr >
		<td rowspan="10">K2. Project-specific Knowledge (297/396, 75%)</td>
		<td>K2.1. Document (message) (55/396, 13.89%)</td>
	</tr>
	<tr >
		<td>K2.2. Algorithm (54/396, 13.64%)</td>
	</tr>
  <tr >
		<td>K2.3. Configuration (45/396, 11.36%)</td>
	</tr>
  <tr >
		<td>K2.4. Test (44/396, 11.11%)</td>
	</tr>
  <tr >
		<td>K2.5. Initialization (28/396, 7.07%)</td>
	</tr>
  <tr >
		<td>K2.6. Debug (19/396, 4.80%)</td>
	</tr>
  <tr >
		<td>K2.7. Refactoring (13/396, 3.28%)</td>
	</tr>
  <tr >
		<td>K2.8. Script (11/396, 2.78%)</td>
	</tr>
  <tr >
		<td>K2.9. Annotations (10/396, 2.53%)</td>
	</tr>
  <tr >
		<td>K2.10. Others (18/396, 4.55%)</td>
	</tr>
</table>

Table 1. The full taxonomy of knowledge. $$ratio_{c} = \frac{N_{c}}{N_{all}}$$ where $N_{c}$ is the number of the knowledge in category $C$, and $N_{all}$ is the number of all the knowledge learned from resolving issue reports, i.e., 396.


We manually classified the knowledge learned from resolving the issue report. The details are in：
[RQ1_modification_ratio_1.md](https://github.com/patchesicse/patches/blob/main/RQ1_modification_ratio_1.md), [RQ1_modification_ratio_0.md](https://github.com/patchesicse/patches/blob/main/RQ1_modification_ratio_0.md)

**RQ2. To what degree are patches modified?**
If a project has a long history, about 40% of its patches are modified. (Finding 2). Recent patches have lower modification ratios than old patches. (Finding 3).

We identified the modification ratios of the patchs from nine projects. Their modification ratios are as follows: 
[aries](https://github.com/patchesicse/patches/blob/main/aries.txt), [calcite](https://github.com/patchesicse/patches/blob/main/calcite.txt), [cassandra](https://github.com/patchesicse/patches/blob/main/cassandra.txt), [derby](https://github.com/patchesicse/patches/blob/main/derby.txt), [flink](https://github.com/patchesicse/patches/blob/main/flink.txt), [geode](https://github.com/patchesicse/patches/blob/main/geode.txt),  [hbase](https://github.com/patchesicse/patches/blob/main/hbase.txt), [hive](https://github.com/patchesicse/patches/blob/main/hive.txt), and [nutch](https://github.com/patchesicse/patches/blob/main/nutch.txt).

**RQ3. Can modifications to patches affect the reliability of their derived knowledge?**
Modifications to patches significantly affect their embedded knowledge (Finding 4). Compared to domain knowledge, project-specific knowledge is more likely to be affected by modifications to patches. Among them, the initializations are the most affected knowledge, and its reduction of effectiveness ratio is 0.7128 (Finding 5). The modification ratio of a patch is an indicator of the obsolete
knowledge (Finding 6).

|K2.5|K2.1|K2.10|K2.2|K1.3|K2.9|K2.4|K2.3|K2.8|K1.2|K2.6|K1.1 |K2.7|
| :------------- | :------------- | :-------------|:------------- | :------------- | :------------- |:------------- | :------------- | :------------- |:------------- | :------------- | :------------- |:-------------|
|0.7128|0.7082|0.6932|0.675|0.6000|0.5556|0.5427|0.5000|0.4545|0.4187|0.3462|0.1923|0.1667|

Table 2. The effectiveness ratio reduction of knowledge. $$reduction_{e_{c}} = \frac{N_{e_{c_{0}}}}{N_{c_{0}}} - \frac{N_{e_{c_{1}}}}{N_{c_{1}}}$$, where $N_{e_{c_{0}}}$ is the number of unmodified (modification ratio = 0) patches whose knowledge is effective, $N_{c_{0}}$ is the number of unmodified patches, $N_{e_{c_{1}}}$ is the number of completely modified (modification ratio = 1) patches whose knowledge is effective, and $N_{c_{1}}$ is the number of completely modified patches.

**RQ4. How can obsolete knowledge affect an approach?**
We also analyze how obsolete knowledge affect an issue-recommendation approach, called CrossFix [1]. Modifications to patches affect the results of this approach [1]. In particular, 48.18% of its recommended issues’s patches are modified, and the modification ratio median is 0.4683 (Finding 7).

The details are in： [Impact_on_issue_recommendation.md](https://github.com/patchesicse/patches/blob/main/Impact_on_issue_recommendation.md)



[1] Shin Hwei Tan, Ziqiang Li, and Lu Yan. 2023. CrossFix: Resolution of GitHub issues via similar bugs recommendation. Journal of Software: Evolution and Process (2023), e2554.
