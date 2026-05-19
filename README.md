# BlueSky Starter Packs

This project investigates whether Bluesky Starter Packs promote social diversity or reinforce echo chamber-like communities. Using Bluesky API data, we analyze account overlap, starter pack descriptions, thematic communities, and follower influence across starter packs.

This repository supports our research report, **“Echo Chambers in BlueSky Starter Packs.”** The project analyzes Starter Pack URIs, extracts accounts and feeds, clusters starter packs by description and account overlap, and computes popularity metrics based on follower counts.

## Research Questions

1. **User Overlap:** Do different starter packs share overlapping accounts that indicate potential community structures?
2. **Thematic Categorization:** Do starter pack descriptions reveal distinct themes such as journalism, sports, politics, art, or tech?
3. **Pack Popularity:** Do different communities contain accounts with different levels of follower influence?

## Data Source

The project uses a provided dataset of approximately **300,000 Bluesky Starter Pack URIs**. The Starter Pack data was collected using Bluesky API endpoints such as `get_starter_pack`, `get_list`, and `get_profile`.

## Repository Structure

### `Data_Cleaning.py`

Collects starter pack data from the Bluesky API.

The script reads starter pack URIs from `starterpacks.jsonl`, queries the Bluesky API, and retrieves starter pack metadata, accounts, and feeds. It processes data line by line and writes results incrementally to reduce data loss if the script stops during collection.

**Outputs:**

- `Trang_account_all.jsonl`  
  Contains `(starter pack, account)` pairs for all accounts included in each starter pack.

- `Trang_feeds_all.jsonl`  
  Contains `(starter pack, feed)` pairs for feeds referenced in each starter pack.

- `Trang_processed.txt`  
  Stores starter pack URIs that have already been processed.

---

### `CollectingAccountFollower.py`

Retrieves metadata for each account appearing in the starter packs.

The script reads accounts from `Trang_account_all.jsonl`, calls the Bluesky `getProfile` API using each account DID, and collects account-level metadata such as follower counts and post counts.

**Input:**

- `Trang_account_all.jsonl`

**Output:**

- `Trang_accounts_info.jsonl`

This file contains:

- account DID
- account handle
- follower count
- post count

---

### `Description_Clustering.py`

Performs thematic clustering on starter packs based on their descriptions.

Starter pack descriptions are converted into embeddings and clustered using community detection methods, including Leiden community detection and HDBSCAN. After clusters are generated, descriptions are manually reviewed and assigned semantic labels.

**Outputs:**

- `Community_Detection.jsonl`  
  Contains the community assignment for each starter pack.

- `Clusters_labels.jsonl`  
  Contains manually assigned labels for each detected community.

---

### `Compute_Median.py`

Performs the main analysis and generates statistics used in the research report.

This script combines community assignments, community labels, and account follower information to compute popularity metrics for starter packs and communities. Median follower counts are used to measure the typical popularity of accounts within each community.

**Inputs:**

- `Clusters_labels.jsonl`
- `Community_Detection.jsonl`
- `Trang_accounts_info.jsonl`

**Outputs:**

- Aggregated statistics for visualization
- Figures used in the analysis report

---

### `Jaccard_Similarity.py`

Analyzes account overlap across starter packs.

Using the `(starter pack, account)` dataset, this script computes Jaccard similarity between starter packs to measure shared accounts. These similarity scores are used to detect community structure and visualize clusters of overlapping starter packs.

**Input:**

- `Trang_account_all.jsonl`

**Outputs:**

- Jaccard similarity scores between starter packs
- Community clustering visualization based on account overlap

## Main Methods

### Starter Pack Data Collection

We collected starter pack metadata, account lists, and feed references from Bluesky Starter Pack URIs using the Bluesky API.

### Account Metadata Collection

For each account found in a starter pack, we collected profile-level information such as follower count and post count.

### User Overlap Analysis

We measured overlap between starter packs using **Jaccard similarity**:

```text
Jaccard Similarity = |A ∩ B| / |A ∪ B|
