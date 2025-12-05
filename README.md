# Intermediate PostgreSQL - Synthetic Training Dataset

Synthetic movie ratings database for PostgreSQL training exercises covering advanced data types, indexing, query optimization, and performance tuning.

## Files Included

1. **movies_dataset.sql** - Creates schema and loads ~80,000 records of movie data
2. **seed_monitoring_data.sql** - Populates query performance statistics

## How to Run

Run these scripts **in order** on each database instance:

### Step 1: Load the dataset
```bash
psql -d your_database_name -f movies_dataset.sql
```
**Time**: ~3-5 minutes
**Creates**: All tables, ~500 movies, ~5000 users, ~75,000 ratings

### Step 2: Seed monitoring data
```bash
psql -d your_database_name -f seed_monitoring_data.sql
```
**Time**: ~30 seconds
**Creates**: Query statistics in pg_stat_statements for performance exercises

## Verify Setup

```sql
-- Check record counts
SELECT
    (SELECT COUNT(*) FROM movies) AS movies,
    (SELECT COUNT(*) FROM users) AS users,
    (SELECT COUNT(*) FROM ratings) AS ratings;

-- Expected: ~500 movies, 5000 users, ~75,000 ratings
```

## Adjusting Dataset Size (Optional)

If the dataset is too large or load times are too long, you can reduce the size by editing `movies_dataset.sql`:

**Line 292** - Reduce number of users:
```sql
FROM generate_series(1, 5000) AS gs  -- Change 5000 to 2000 for smaller dataset
```

**Line 430** - Reduce ratings per movie:
```sql
LIMIT (100 + (floor(random() * 101))::INT)  -- Change to (20 + (floor(random() * 81))::INT) for smaller dataset
```

**Smaller settings** (2000 users, 20-100 ratings/movie):
- Creates ~30,000 ratings instead of ~75,000
- Load time: ~1-2 minutes per database
- Total setup: ~20-40 minutes for 20 databases

**Note:** The larger dataset (75K ratings) is recommended for clearer performance demonstrations, but the smaller version will work if needed.

## What Gets Created

**8 Tables:**
- movies, people, users, ratings, genres, movie_cast, movie_genres, watchlist, popularity_cache

**Key Features:**
- JSONB metadata, TEXT[] arrays, HSTORE preferences, TSVECTOR for full-text search
- Realistic data patterns (popularity bias, rating correlations, recency effects)
- Intentional data quality issues for teaching
- Pre-populated pg_stat_statements for monitoring exercises

**Extensions Used:**
- hstore, pg_stat_statements

---

That's it! Students will have all data pre-loaded and ready for class.
