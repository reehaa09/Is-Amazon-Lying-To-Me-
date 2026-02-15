# Is-Amazon-Lying-To-Me-
This project examines Amazon's displayed ratings and whether they accurately reflect the opinions of verified purchasers across different price points. It also explores how this 'trust gap' evolves as categories accumulate non-verified reviews.

View visualization here: https://public.tableau.com/shared/DR9WKCS8Q?:display_count=n&:origin=viz_share_link
## Overview

Amazon displays a single average rating on every product page but does that number tell the whole story? This project examines the gap between Amazon's displayed ratings and the average ratings from verified purchasers, exploring how this "trust gap" varies across product categories and price points.

We analyzed **571 million+ reviews** (275 GB) and **35 million product listings** (101 GB) from the [Amazon Reviews 2023 dataset](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023) to find out.

## Tech Stack

| Tool | Purpose |
|------|---------|
| **PySpark** | Distributed processing of 571M+ reviews |
| **Tableau** | Interactive animated scatter plot visualization |
| **Python** | Data cleaning and transformation |

## Dataset

Two datasets from [McAuley-Lab/Amazon-Reviews-2023](https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023) on Hugging Face:

- **User Reviews**: ~571.54 million reviews (May 1996 – September 2023), 275 GB
- **Item Metadata**: ~35 million product listings, 101 GB

The datasets were joined on `parent_asin` (since products of different colors/sizes share the same parent ID) and filtered to ensure each category had at least 5 verified and 5 non-verified reviews.

## Methodology

1. **Data Cleaning**: Removed 151 invalid zero-rated reviews, dropped rows with null prices/ratings, and created a `category_from_file` column to handle missing category data.
2. **Trust Gap Calculation**: Computed the percentage difference between Amazon's displayed average rating and the verified purchaser average: `((Overall Avg Rating - Verified Avg Rating) / Verified Avg Rating) × 100`
3. **Visualization Design**: Iterated through 3 design sketches before arriving at the final animated scatter plot.

## Visualization

We built an **animated scatter plot** in Tableau that reveals how rating inflation evolves as non-verified reviews accumulate across categories.

### Design Highlights

- **Animated ghost trails**: Categories appear sequentially as non-verified review counts increase, leaving faded traces of previous positions
- **Custom color gradient**: Light shades for categories with fewer non-verified reviews, dark shades for more — ensuring ghost trails remain visible while new categories draw attention
- **Reference line at Y=0**: Clearly separates categories where Amazon oversells (above) vs undersells (below)
- **Grid lines at 40% opacity**: Reduces clutter while maintaining spatial reference

### Design Iteration

We went through three sketch iterations before arriving at the final design:

| Sketch | Approach | Why We Moved On |
|--------|----------|-----------------|
| 1 | Bar chart (trust gap) + line chart (price) <br> <img width="381" height="505" alt="Screenshot 2026-02-15 at 6 51 14 PM" src="https://github.com/user-attachments/assets/60416ab8-21b3-47bd-af4f-7a4ecccbfa7d" /> | Messy with mixed positive/negative values |
| 2 | Bar chart + circles for price <br> <img width="286" height="389" alt="Screenshot 2026-02-15 at 6 52 14 PM" src="https://github.com/user-attachments/assets/694b873c-f3d5-445f-abcb-38d04f609251" /> | Cleaner, but didn't convey the relationship effectively |
| 3 | Scatter plot with bubble sizes <br> <img width="329" height="381" alt="Screenshot 2026-02-15 at 6 52 40 PM" src="https://github.com/user-attachments/assets/d075d9f2-7438-446c-ba9f-d430bc6a0476" /> <br> <img width="531" height="523" alt="Screenshot 2026-02-15 at 6 53 22 PM" src="https://github.com/user-attachments/assets/e6469ae0-e2b4-4e00-9500-865f9fc67281" /> | Perfect on paper, but small bubbles disappeared in Tableau |
| **Final** | **Animated scatter plot with ghost trails** | **Clear, engaging, and informative** |
