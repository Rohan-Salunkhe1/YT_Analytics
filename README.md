### Project: In-Depth Data Analysis of YouTube Trending Videos in India

#### **Project Overview:**
This project focuses on analyzing the YouTube trending videos in India using a dataset that includes 251,199 rows of data. The aim is to explore various aspects of video performance, such as trends in viewership, likes, dislikes, comments, category performance, and other relevant metrics. The analysis will also provide actionable insights for content creators, businesses, and marketing teams.

---

### **Step 1: Data Exploration & Cleaning**

#### **Initial Dataset Overview:**
The dataset includes the following fields:

- `video_id`: Unique identifier for each video.
- `title`: Video title.
- `publishedAt`: Date and time the video was published.
- `channelId`: Unique identifier for the channel.
- `channelTitle`: The name of the channel.
- `categoryId`: Category to which the video belongs.
- `trending_date`: Date the video appeared in the trending list.
- `tags`: Tags associated with the video.
- `view_count`: Number of views.
- `likes`: Number of likes.
- `dislikes`: Number of dislikes.
- `comment_count`: Number of comments.

**Tasks:**
1. **Load the data** and check for missing values, outliers, and duplicates.
   - Identify any missing or incorrect values (e.g., missing tags).
   - Handle missing values in a manner appropriate for the analysis (drop or impute based on context).
   - Check for duplicated `video_id` entries.
2. **Data type conversion**: Ensure the appropriate data types (e.g., date fields as datetime objects).

```sql
-- Example: Checking for null values and duplicates
SELECT COUNT(*) AS total_rows, 
       SUM(CASE WHEN tags IS NULL THEN 1 ELSE 0 END) AS missing_tags, 
       COUNT(DISTINCT video_id) AS unique_videos 
FROM youtube_trending_videos;
```

---

### **Step 2: Analyze Video Popularity Metrics**

#### **Query 1: View Count Distribution**
Analyze the distribution of view counts to understand the range of video popularity.

```sql
-- View count distribution for trending videos
SELECT view_count, COUNT(*) AS video_count
FROM youtube_trending_videos
GROUP BY view_count
ORDER BY video_count DESC;
```
- **Objective:** Understand the general trends in viewership. What is the average view count? Are there outliers (videos with exceptionally high or low views)?

#### **Query 2: Correlation Between Likes, Dislikes, and Views**
Examine how `likes` and `dislikes` correlate with the number of views to determine if higher engagement leads to higher views.

```sql
-- Correlation analysis between likes, dislikes, and view counts
SELECT
  CORR(likes, view_count) AS likes_view_corr,
  CORR(dislikes, view_count) AS dislikes_view_corr
FROM youtube_trending_videos;
```
- **Objective:** Understand whether more liked (or disliked) videos tend to have higher views.

---

### **Step 3: Category-Level Analysis**

#### **Query 3: Most Popular Categories**
Which video categories (e.g., music, entertainment, sports) have the highest viewership? You can rank the categories based on their total view counts or likes.

```sql
-- Popular video categories based on view count
SELECT categoryId, SUM(view_count) AS total_views, COUNT(video_id) AS total_videos
FROM youtube_trending_videos
GROUP BY categoryId
ORDER BY total_views DESC;
```
- **Objective:** Determine which content categories are most popular among viewers in India.

#### **Query 4: Engagement (Likes, Dislikes, Comments) by Category**
Analyze user engagement metrics (likes, dislikes, comments) across different video categories.

```sql
-- Engagement metrics by video category
SELECT categoryId, 
       AVG(likes) AS avg_likes, 
       AVG(dislikes) AS avg_dislikes, 
       AVG(comment_count) AS avg_comments
FROM youtube_trending_videos
GROUP BY categoryId;
```
- **Objective:** Compare engagement levels across categories and identify which types of videos get the most interaction.

---

### **Step 4: Time-Based Analysis**

#### **Query 5: Trending Videos by Day**
Analyze the number of trending videos over time (trending_date) to identify patterns in video performance.

```sql
-- Count of trending videos by date
SELECT trending_date, COUNT(*) AS trending_videos
FROM youtube_trending_videos
GROUP BY trending_date
ORDER BY trending_date;
```
- **Objective:** Identify if there are specific days or times where more videos are likely to trend.

#### **Query 6: Seasonal Trends**
Examine if certain times of the year (month/quarter) see a higher number of trending videos.

```sql
-- Seasonal trends: count of trending videos by month
SELECT EXTRACT(MONTH FROM trending_date) AS month, 
       COUNT(*) AS trending_videos
FROM youtube_trending_videos
GROUP BY month
ORDER BY month;
```
- **Objective:** Understand whether the popularity of trending videos varies with the season (e.g., festival periods).

---

### **Step 5: Analysis of Tags**

#### **Query 7: Most Frequently Used Tags**
Which tags are most frequently used across trending videos?

```sql
-- Count of most frequently used tags
SELECT tags, COUNT(video_id) AS tag_usage_count
FROM youtube_trending_videos
GROUP BY tags
ORDER BY tag_usage_count DESC;
```
- **Objective:** Identify common trends or topics in the videos that tend to trend.

---

### **Step 6: Channel-Level Analysis**

#### **Query 8: Top Channels by View Count**
Which channels have the most viewership in the trending video list?

```sql
-- Top channels based on total views
SELECT channelTitle, SUM(view_count) AS total_views
FROM youtube_trending_videos
GROUP BY channelTitle
ORDER BY total_views DESC;
```
- **Objective:** Identify the top-performing channels and their contribution to trending content.

#### **Query 9: Channel Engagement**
Evaluate the average engagement (likes, dislikes, comments) per video for each channel.

```sql
-- Average engagement metrics by channel
SELECT channelTitle, 
       AVG(likes) AS avg_likes, 
       AVG(dislikes) AS avg_dislikes, 
       AVG(comment_count) AS avg_comments
FROM youtube_trending_videos
GROUP BY channelTitle;
```
- **Objective:** Compare engagement across channels to see which ones create the most interactive content.

---

### **Step 7: Visualization in Excel**

Once you've completed the analysis using SQL, visualize the results using Excel:

1. **Bar Charts for Category Analysis**:
   - Visualize the most popular categories based on total views.
   - Compare engagement metrics (likes, dislikes, comments) for each category.
   
2. **Time Series Charts for Trending Videos**:
   - Use a line chart to show the number of trending videos over time (daily or monthly).
   
3. **Scatter Plots for Correlation**:
   - Visualize the correlation between likes and views, and dislikes and views using scatter plots.

4. **Channel Comparison Bar Graphs**:
   - Visualize the top-performing channels and their average engagement metrics using bar charts.

5. **Tag Frequency Word Cloud**:
   - Create a word cloud of the most common tags used in trending videos to visually represent common themes.

---

### **Step 8: Conclusion and Recommendations**
- Summarize key insights, such as which content categories are most popular, the time periods when videos are most likely to trend, and how engagement metrics like likes and dislikes impact viewership.
- Provide recommendations for content creators to optimize their chances of making it to the trending list based on data-driven insights.

By following these steps, you'll perform a comprehensive analysis of YouTube's trending videos in India.
