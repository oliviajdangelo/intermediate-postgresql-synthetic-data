# Intermediate PostgreSQL - Synthetic Training Dataset

Realistic movie ratings database with ~140,000 rows designed for intermediate PostgreSQL training: advanced data types, indexing, query optimization, and performance tuning. All content is synthetically generated and safe for corporate training use.

## What Gets Created

**9 Tables with ~140,000 total rows (~30-50MB):**

| Table | Rows | Description |
|-------|------|-------------|
| movies | 500 | Movies with JSONB metadata, TEXT[] tags, TSVECTOR search |
| people | 308 | Actors, directors, producers (includes 8 animal actors) |
| genres | 15 | Standard movie genres |
| movie_cast | ~2,750 | 3-8 cast members per movie with genre-aware character names |
| movie_genres | ~1,000 | 1-3 genres per movie |
| users | 5,000 | Users with HSTORE preferences |
| ratings | ~75,000 | Variable ratings per movie (30-300 based on popularity/recency) |
| watchlist | ~56,000 | 5-20 movies per user watchlist |
| popularity_cache | 500 | Cached aggregates for UPDATE contention demos |

**Key Features:**
- Advanced data types: JSONB, TEXT[], HSTORE, TSVECTOR
- Realistic patterns: popularity bias, rating correlations, temporal trends
- Reproducible: Same data across all instances (uses random seed)
- Intentional data quality issues for teaching
- Extensions: hstore, pg_stat_statements (auto-installed)

## Prerequisites

- PostgreSQL 16+ (tested on PostgreSQL 17)
- ~50MB disk space per database instance
- Database creation privileges and extension installation permissions
- Setup time: 3-5 minutes per database

## Quick Start

**Recommended database name:** `movies_db`

```bash
# Step 1: Load dataset (required)
psql -d movies_db -f movies_dataset.sql

# Step 2: Seed monitoring data (optional but recommended)
psql -d movies_db -f seed_monitoring_data.sql
```

The monitoring script populates pg_stat_statements with slow query patterns for performance tuning exercises. While optional, it's helpful for demonstrating EXPLAIN ANALYZE and optimization techniques.

**Verify setup:**
```sql
SELECT
    (SELECT COUNT(*) FROM movies) AS movies,
    (SELECT COUNT(*) FROM users) AS users,
    (SELECT COUNT(*) FROM ratings) AS ratings;
-- Expected: 500 movies, 5000 users, ~75,000 ratings
```

## Advanced Options

**Reduce dataset size:** If 3-5 minute load times are too long, edit `movies_dataset.sql`:
- **Line 321**: Change `generate_series(1, 5000)` to `generate_series(1, 2000)` for fewer users
- **Line 488**: Change rating limit from `100-200` to `20-100` per movie
- **Line 610**: Change watchlist limit from `5-20` to `2-10` per user (reduces ~56,000 to ~30,000 watchlist entries)

This creates ~30,000 ratings and ~30,000 watchlist entries instead of ~75,000 and ~56,000 (load time: 1-2 minutes). The larger dataset is recommended for clearer performance demonstrations.
