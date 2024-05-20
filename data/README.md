Update 05/20/24

## **Full merged & clean data files (tabular/dataframes) linked below:**
* [760MB] [amazon_fashion_clean_051624.parquet](https://drive.google.com/file/d/1DePC-rTUNBzQgIqq-GSLA228hNCTxEJL/view?usp=share_link)
  * Shape: (776976, 18)

**FEATURES**

* `title` [STR]
  * title scraped from website
* `avg_rating` [FLOAT64]
  * avg rating scraped from website
* `rating_number` [INT64] 
* `features` [STR]
  * product "about" section text, scraped from website
* `description` [STR]
  * only ~59,289 products out of 776,000+ have descriptions
* `price` [FLOAT64]
  * missing values are also of type FLOAT64
* `images` [LST of DICT]
  * all sizes
* `store` [STR]
  * company name
* `details` [DICT]
  * more product details 
  * see NOTES below for full list/more info
* `parent_asin` [STR]
  * unique product identifier
* `title_review_agg` [STR]
  * ALL titles of user reviews, concatenated into one str
* `user_id` [LST of STR]
  * unique reviewer id
* `timestamp` [LST of DATETIME]
  * time of review in MM-DD-YYYY format
* `avg_rating_reviewers` [FLOAT64]
  * avg rating amongst reviewers who left a text review
* `coefvar_rating_reviewers` [FLOAT64]
  * coefficient of variation in rating; captures dispersion of ratings
* `text_agg` [STR]
  * ALL text reviews, concatenated into one str
* `text_weighted_agg` [STR]
  * some reviews had upvotes, majority didn't have any
  * in order to express the relative importance of some reviews compared to others, there was a "helpful_vote" feature in the raw data
  * I use "helpful_vote" as a **weight** to express review importance
  * How this would work is we would multiply the original `text` content of this review by the number of "helpful_vote" times
  * Now, I didn't think we needed a 1:1 ratio. Also, the most upvotes for a review in this dataset is 371, and we don't want to be copying a chunk of text 371 times
  * So I decide to transform "helpful_vote" by taking the square root of each vote
  * I think this still works because this (weighing of the "helpful_vote" weight) process REWARDS upvotes when they're sparse. In other words, the MARGINAL VALUE of each upvote decreases over time (arguably, it's more important that someone finds a review relatable and thus upvotes it for the 1st, 2nd time, compared to when the 200th person is upvoting a post that already has 199 votes...in which case sheer visibility might play a role...such a popular review likely will be found at the top of the review section)
  * Application: probably best review text feature for embedding, in my opinion. Unless embedding models handle repeated text badly.
  * Example calculation: `text_weighted_agg` contains all text reviews with weights but for the one review with 371 upvotes in particular, the text for this review has been repeated 19 times. (19 is the upper limit for text copies/weight).
  * Limitation: increase text size 
* `images_review_cln` [LST of DICT]
  * images taken by reviewers
  * all sizes

**NOTES**
* Fewer rows - some products were dropped because the only review for some products were unverified, and so the associated product was dropped
* We can use `details` feature to check for additional info like `Date First Available`, `Department`.
  * Full list of possible items in `details` feature here: [product_DETAILS_keys](https://drive.google.com/file/d/1iO2ci4Tz6jp4CL4uNtfgv-IpM2lMCt0D/view?usp=share_link) [PICKLE]
  * These could be used in in a graph. 
  * `Date First Available` - do reviews change over time
  * `Department` - could serve as nodes or edges, helps determine traffic to particular departments
* There ARE users who left reviews for multiple products. Enough to instances to conduct analysis on 

## **Individual datasets, clean**
* [537MB] [reviews_modified_051624.parquet](https://drive.google.com/file/d/1eFBR7PrBlnVgE6gKwx4J4-dnckiEh8XK/view?usp=share_link)
  * Each row contains one USER and their review. Maybe can be used for graph analysis because of this data format.
* [278MB] [meta_modified_051624.parquet](https://drive.google.com/file/d/131iDXr_TI25X53dG97DGlmz5mvm_eG8k/view?usp=share_link)

## Other data
* Full list of possible items in `details` feature here: [product_DETAILS_keys](https://drive.google.com/file/d/1iO2ci4Tz6jp4CL4uNtfgv-IpM2lMCt0D/view?usp=share_link) [PICKLE]
* Json with link to every products' large image: [product_image_links.json](https://drive.google.com/file/d/1daLqAtINSUweE3qY8OwFlbxap4ceF5Ar/view?usp=sharing)
  * A dictionary of dictionaries, with `parent_asin` as outer key, inside the nested dicts are `image_link` and `title`

## **Raw data**
* Reviews: [Amazon_Fashion.jsonl](https://drive.google.com/file/d/1A_HSH_-vocuNcY4D0AhgTcl-VgspwjCj/view?usp=share_link)
* Product info: [meta_Amazon_Fashion.jsonl](https://drive.google.com/file/d/1fzm243T5JylfFvaqAf5EpBKPmCH5WdX6/view?usp=share_link)

