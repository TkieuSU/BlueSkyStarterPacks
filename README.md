# BlueSkyStarterPacks

File Descriptions

1. Data Cleaning (Data_Cleaning.py): This script collects starter pack data from the Bluesky API.

It first reads the starter pack URIs from starterpacks.jsonl, then queries the Bluesky API to retrieve the starter pack metadata, including the accounts and feeds contained in each pack. The script processes the data one line at a time and writes results incrementally to avoid data loss if the script stops mid-run.

Outputs:
- Trang_account_all.jsonl: Contains (starter pack, account) pairs for all accounts included in each starter pack.

- Trang_feeds_all.jsonl: Contains (starter pack, feed) pairs for feeds referenced in each starter pack.

- Trang_processed.txt: Stores the list of starter pack URIs that have already been processed.

2. Collecting Account Follower Data (CollectingAccountFollower.py): This script retrieves metadata for each account appearing in the starter packs.

It reads the accounts from Trang_account_all.jsonl, then calls the Bluesky getProfile API using each account's DID. The script collects follower counts and post counts for each account.

Input:
-Trang_account_all.jsonl

Output:
- Trang_accounts_info.jsonl

Contains account-level information including:
- account DID
- follower count
- post count

3. Description Clustering (Description_Clustering.py): This script performs community detection on starter packs based on their descriptions.

We apply clustering methods (including Leiden community detection and HDBSCAN) to identify groups of similar starter packs. After clusters are generated, we manually examine the cluster descriptions and assign semantic labels to each community.

Outputs:
- Community_Detection.jsonl: Contains the community assignment for each starter pack.

- Clusters_labels.jsonl: Contains manually assigned labels for each detected community.

4. Compute Median (Compute_Median.py): This script performs the main analysis and generates the statistics used in the paper.

It combines community assignments, community labels, and account follower information to compute popularity metrics for each starter pack and community. Median follower counts are used to measure the typical popularity of accounts within each community.

Inputs:
- Clusters_labels.jsonl
- Community_Detection.jsonl
- Trang_accounts_info.jsonl

Outputs:
- Aggregated statistics used for visualization
- Figures used in the analysis report

5. Jaccard Similarity Analysis (Jaccard_Similarity.py): This script analyzes the overlap of accounts across starter packs.

Using the (starter pack, account) dataset, it computes Jaccard similarity between starter packs to measure how many accounts they share. These similarities are then used to detect community structures and visualize clusters of overlapping starter packs.

Input:
- Trang_account_all.jsonl

Outputs:
- Jaccard similarity scores between starter packs
- Community clustering visualization based on account overlap
